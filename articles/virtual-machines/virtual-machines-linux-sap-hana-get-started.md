<properties
   pageTitle="Průvodce rychlým startem pro ruční instalace SAP HANA na Azure VMs | Microsoft Azure"
   description="Průvodce rychlým startem pro ruční instalace SAP HANA na Azure VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="quickstart-guide-for-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Průvodce rychlým startem pro ruční instalace jednoduchou SAP HANA na Azure VMs

## <a name="introduction"></a>Úvod

Tento průvodce rychlým startem vám pomůže nastavit jednoduchou SAP HANA prototypů/ukázku systému na Azure VMs ruční instalace SAP NetWeaver 7.5 a SAP HANA SP12.
Průvodce předpokládá, že je čtenáře znáte základy Azure IaaS jako nasazení virtuálních počítačích nebo virtuální sítí buď přes Azure portál nebo prostředí Powershell/rozhraní příkazového řádku včetně možnost použití šablon json. Kromě očekává se, že je čtenáře znají SAP HANA, SAP NetWeaver a jak ji nainstalovat místní.

Očekává se, že čtenáře známa obecné SAP Azure dokumentaci uvedené v oddílu obecné informace na konci tohoto článku.

Z důvodu omezení testovacím systémů tato příručka nezahrnuje témata jako HA, zálohování DR, vysoký výkon nebo jinak důležité.

Hotový ukázkový nastavení pomocí dvou virtuálních počítačích provádění distribuované instalace SAP NetWeaver přes správce prostředků Azure modelu jako SAP Linux Azure je podporována pouze pro správce prostředků Azure a ne klasické modelu. Odkazy na další informace o správce prostředků Azure najdete v části Obecné informace na konci tohoto článku.

Byly VMs dvou zkušebních slouží k instalaci vzorku:

* Hana appsrv (typ DS3) hostovat NW 7.5 ASC instancí + PAS
* Hana-dbsrv (typ GS4) na hostitele HANA SP12
* obě VMs patřilo jedné Azure virtuální sítě (azure-hana-test-vnet)
* OS v obou případech byla SLES 12 SP1


Mějte na paměti skutečnosti ale, že k červenec 2016 SAP HANA je pouze plně podporovány pro systémy výrobní OLAP (pásma) na Azure OM typu GS5. Pro testování kde jednu není očekává SAP podporu je v pořádku něco menší jako je například GS4 používat.
Co by se měl HANA SAP na Azure je Azure premium úložiště pro HANA dat a souborů protokolu - naleznete v tématu Nastavení"disku" oddílu Další dolů. Pokud jde o další podrobnosti o které SAP produkty jsou podporovány na Azure zkontrolujte, zda oddílu obecné informace na konci tohoto článku.


Průvodce popisuje dvěma různými způsoby, jak ručně nainstalovat SAP HANA na Azure VMs:

* instalace SAP HANA prostřednictvím SAP Software zřizování správce (SWPM) jako součást distribuované instalace NetWeaver v kroku "instance databázového"
* Nainstalujte SAP HANA pomocí nástroje hdblcm HANA životního cyklu správce a potom NetWeaver později

Také je možné použít SWPM a nainstalujte všechny komponenty (SAP HANA, SAP aplikační server, ASC instance SAP grafické) v jedné jednoho OM. Tato možnost není popisované v tomto článku ale položek, které mají být vzít v úvahu zůstávají stejné.

Před spuštěním instalace oddílu za seznamy úkolů pod o nastavení VMs Azure test obsaženou pro omezení několik základních chyb, které se stane při použití výchozí Azure OM konfigurace.


## <a name="checklist-sap-hana-installation-via-sap-swpm"></a>Kontrolní seznam SAP HANA instalace prostřednictvím SAP SWPM

Toto je jednoduchého kontrolního seznamu klíč, který položky vztahující se k ruční instalace SAP HANA jednoduchou pro ukázku nebo vytváření prototypů pursposes prostřednictvím SAP SWPM způsobem distribuované 7.5 NW SAP nainstalovat. Jednotlivé položky jsou vysvětleny a zobrazená ve formuláři snímky obrazovek podrobnější v následujícím článku:

* Vytvoření Azure virtuální sítě, která bude obsahovat dva test VMs 
* nasazení dvou VMs Azure s operačním systémem SLES 12 SP1 prostřednictvím modelu správce prostředků Azure 
* Připojte dva disků standardní úložiště na server aplikace OM (například 75GB a 500)
* přiložení čtyř disků HANA databázový server OM - 2 standardní úložiště disků jako pro server aplikace OM + 2 premium disků úložiště (například 2x512GB)
* v závislosti na velikosti a/nebo výkon požadavky připojit více disků a vytvořte prokládané svazky buď pomocí lvm nebo mdadm na úrovni OS uvnitř OM
* Vytvoření systémy XFS souborů připojených disků / logické svazky
* Připojte novou systémy souborů XFS na úrovni s operačním systémem. Použití jednoho systému souborů k synchronizaci SAP software a druhý například sapmnt adresář a možná zálohy. Na SAP HANA DB server připojení XFS systémy na discích úložiště premium jako /hana a /usr/sap, který je to všechny nezbytné Chcete-li předejít, že kořenovou souborů, který není příliš velký na Linux Azure VMs zaplní souborů
* Zadejte místní ip adresy testu VMs/atd/hosts
* zadání parametru nofail v /etc/fstab
* nastavení parametrů jádra podle poznámku HANA SLES 12 SAP (viz podrobnosti níž v části prospívání jádra)
* Přidání prostoru vyměňovat
* Pokud je potřeba - na test VMs nainstalujte grafické plochy. V opačném případě použijte vzdálené sapinst instalace
* Stažení softwaru SAP z marketplace služeb SAP
* Nainstalujte na server aplikace OM instancí SAP ASC
* sdílení adresáři sapmnt prostřednictvím NFS mezi test VMs (server aplikace OM je NFS server)
* instalace instance databázového včetně HANA prostřednictvím SWPM na databázový server OM
* POSLEDNÍCH nainstalovat aplikaci serveru OM
* Spusťte SAP MC a připojte se například pomocí grafického rozhraní SAP / HANA Studio 



## <a name="checklist-sap-hana-installation-via-hdblcm"></a>Kontrolní seznam SAP HANA instalace prostřednictvím hdblcm

Toto je jednoduchého kontrolního seznamu klíč, který položky vztahující se k ruční instalace SAP HANA jednoduchou pro ukázku nebo vytváření prototypů pursposes prostřednictvím SAP SWPM způsobem distribuované 7.5 NW SAP nainstalovat. Jednotlivé položky jsou vysvětleny a zobrazená ve formuláři snímky obrazovek podrobnější v následujícím článku:

* Vytvoření Azure virtuální sítě, která bude obsahovat dva test VMs 
* nasazení dvou VMs Azure s operačním systémem SLES 12 SP1 prostřednictvím modelu správce prostředků Azure 
* Připojte dva disků standardní úložiště na server aplikace OM (například 75GB a 500)
* přiložení čtyř disků HANA databázový server OM - 2 standardní úložiště jako pro server aplikace OM + 2 premium disků úložiště (například 2x512GB)
* v závislosti na velikosti a/nebo výkon požadavky připojit více disků a vytvořte prokládané svazky buď pomocí lvm nebo mdadm na úrovni OS uvnitř OM
* Vytvoření systémy XFS souborů připojených disků / logické svazky
* Připojte novou systémy souborů XFS na úrovni s operačním systémem. Použití jednoho systému souborů k synchronizaci SAP software a druhý například sapmnt adresář a možná zálohy. Na SAP HANA DB server připojení XFS systémy na discích úložiště premium jako /hana a /usr/sap, který je to všechny nezbytné nechcete, aby kořenové souborů, který není příliš velký na Linux Azure VMs zaplní souborů
* Zadejte místní ip adresy testu VMs/atd/hosts
* zadání parametru nofail v /etc/fstab
* nastavení parametrů jádra podle poznámku HANA SLES 12 SAP (viz podrobnosti níž v části prospívání jádra)
* Přidání prostoru vyměňovat
* Pokud je potřeba - na test VMs nainstalujte grafické plochy. V opačném případě použijte vzdálené sapinst instalace
* Stažení softwaru SAP z marketplace služeb SAP
* Vytvoření skupiny "sapsys" s id skupiny 1001 na OM HANA DB serveru
* instalace SAP HANA na databázový server OM pomocí hdblcm
* Nainstalujte na server aplikace OM instance SAP ASC
* sdílení adresáři sapmnt prostřednictvím NFS mezi test VMs (server aplikace OM je NFS server)
* instalace instance databázového včetně HANA prostřednictvím SWPM na databázový server OM
* POSLEDNÍCH nainstalovat aplikaci serveru OM
* Spusťte SAP MC a připojte se například pomocí grafického rozhraní SAP / HANA Studio 




## <a name="prepare-azure-vms-for-manual-installation-of-sap-hana"></a>Příprava Azure VMs ruční instalace SAP HANA

Tato kapitola o přípravě Azure VMs pro ruční instalace SAP HANA se skládá z pěti oddílů, které najdete v těchto tématech:

* Nastavení disku
* Parametry jádra
* Filesystems
* / atd/hosts
* / atd/fstab


### <a name="disk-setup"></a>Nastavení disku

Kořenové souborů v Linux OM na Azure je omezené velikosti. Proto je nezbytné k připojení další místo na disku virtuálního počítače pro spuštění SAP. V případě SAP server aplikace, použité v prostředí čistého prototypů/ukázku OM je v pořádku používat disků Azure standardní úložiště. Zatímco pro SAP HANA DB souborů dat a protokolu - disků Azure Premium úložiště se má používat i v testovacím na šířku.

Některé informace o tom, jak připojit disků Linux OM [tady](virtual-machines-linux-add-disk.md)

Když přijde na Azure diskové mezipaměti - musí se použít "Žádná" disků, které se použijí k ukládání protokolů transakcí HANA. HANA datové soubory, které je v pořádku můžete přečíst v ukládání do mezipaměti. Stejně jako HANA databázi v paměti, který závisí na celkové způsob použití kolik čtení mezipaměť na úrovni Azure disku zvýšit výkon (například uzavřená HANA do tématu data z disku do paměti).

Zobrazit podrobnosti o úložišti Premium Azure [tady](../storage/storage-premium-storage.md)

[Tady](https://github.com/Azure/azure-quickstart-templates) najdete ukázkové šablony json vytvořit VMs.
"101om jednoduché – linux" ukazuje, jak vypadá včetně části úložiště, která přidá disk dat 100GB základní šablonu.

[Tento článek](virtual-machines-linux-sap-on-suse-quickstart.md) obsahuje některé informace o tom, jak najít SUSE obrázek pomocí prostředí Powershell nebo rozhraní příkazového řádku a důležitost, kterou chcete připojit disk prostřednictvím UUID.


V závislosti na velikosti požadavky na systém a výkon může být nutné připojit více disků místo jedné a novějším vytvořit pruh nastavení těchto disků na úrovni s operačním systémem. Jsou dva aspekty jednu proč byste si vytvořili pruh nastavit různých Azure disků:

* zvýšit výkon
* potřebujete jednoho systému souborů > 1TB je aktuální omezení velikosti Azure disku 1TB (od 2016 červenec)


Další informace o dvou hlavních nástrojích konfigurace prokládání najdete tady:

[Článek o použití mdadm ke konfiguraci Linux software raid na OM Azure](virtual-machines-linux-configure-raid.md)

[Článek o konfiguraci Správce logických hlasitosti na OM Azure Linux](virtual-machines-linux-configure-lvm.md)



![](./media/virtual-machines-linux-sap-hana-get-started/image003.jpg)

Ve sloupci podmínka byly prostředí dvou Azure standardní úložiště discích připojené k aplikaci SAP server OM. Bylo použito pro uložení všech SAP software pro instalaci (například NetWeaver 7.5, SAP grafické SAP HANA... ) a druhý na dostatek místa pro něco jiného, co můžou být nutné (například zálohovat, testovací data) i v adresáři sapmnt (například profily SAP) mají být sdíleny všechny VMs, které patří do stejné SAP orientace na šířku.

![](./media/virtual-machines-linux-sap-hana-get-started/image004.jpg)

Na rozdíl od aplikace serveru OM čtyři disků připojenou SAP HANA Server OM. Jako než dvě disků byly použity pro zachování SAP software (jednu můžou taky sdílet SAP softwaru přes NFS) a máte dost místa například zálohování. Další dvou discích byly disků úložiště Azure Premium zachovat SAP HANA dat a souborů protokolu, jakož i adresáři /usr/sap.


### <a name="kernel-parameters"></a>Parametry jádra


SAP HANA vyžaduje specifická nastavení jádra Linux, které nejsou součástí standardní Azure Galerie obrázků a potřeba nejdřív nastavit ručně. Existuje určité poznámky SAP, který popisuje nastavení. 


SAP Poznámka SAP HANA DB: Doporučené nastavení OS pro SLES 12 / SLES SAP aplikací 12: [2205917 poznámku SAP](https://launchpad.support.sap.com/#/notes/2205917)

Jeden další téma reagrding-mezipaměti stránky týkající se systémem SAP HANA SLES najdete [tady](https://www.suse.com/documentation/sles_for_sap/singlehtml/sles_for_sap_guide/sles_for_sap_guide.html#sec.s4s.configure.page-cache) v kapitola 6.1 jádra: Limit mezipaměti stránky

Je taky SAP Poznámka týkající se limit mezipaměti stránky [1557506 poznámku SAP](https://service.sap.com/sap/support/notes/1557506)

SLES 12 má nového nástroje, které nahrazují nástroj staré sapconf. Není k dispozici zvláštní profilu SAP HANA je "správně zapnuli Admin". Jednu můžete najít podrobnější informace o tomto nástroji následující dva odkazy dole.

SLES dokumentace o profilu správně zapnuli adm sap-hana najdete [tady](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html)

SLES dokumentace o správně zapnuli adm profilu sap-hana - kapitola 6.2 optimalizace systémů pro systémy SAP úloh s správně zapnuli adm najdete [tady](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf)


![](./media/virtual-machines-linux-sap-hana-get-started/image005.jpg)

Tady jednu můžete najdete v článku jak "správně zapnuli Admin" změnil transparent_hugepage, jakož i numa_balancing hodnot podle požadovaná nastavení SAP HANA.


Trvalé nastavení jádra SAP HANA jednu obsahují používání grub2 na SLES 12. Další informace o grub2 najdete [tady](https://www.suse.com/documentation/sled-12/book_sle_admin/data/sec_grub2_file_structure.html)


![](./media/virtual-machines-linux-sap-hana-get-started/image006.jpg)

Tento snímek obrazovky ukazuje, jak byly jádra nastavení změnit v souboru konfigurace a potom "kompilovaný" prostřednictvím grub2 mkconfig


![](./media/virtual-machines-linux-sap-hana-get-started/image007.jpg)

Další možností bude změnit nastavení prostřednictvím Yast a nastavení spuštění zavaděč jádra parametrů.


### <a name="filesystems"></a>Filesystems 

![](./media/virtual-machines-linux-sap-hana-get-started/image008.jpg)

Tady jednu vidí systémy dvou souborů, které byly vytvořené na serveru aplikace SAP OM nad dvou discích připojených Azure standardní úložiště. Obě filesystems jsou typu XFS a připojit /sapdata a /sapsoftware.

Není povinný postupovat takto. Různé způsoby jak se nezdařilo místa na disku.
Nejdůležitější poměr je vyhnout se, že kořenovou souborů dostatek místa. 


![](./media/virtual-machines-linux-sap-hana-get-started/image009.jpg)

Pokud jde o OM DB HANA SAP je důležité vědět, že během instalace databáze prostřednictvím sapinst (swpm) a používáte možnost Jednoduchá "typické" instalace nainstalujte obsahu ve výchozím nastavení v části /hana a /usr/sap. Výchozí nastavení pro zálohování protokolu SAP HANA je například v části /usr/sap.
Jako předtím, než je zkratka, která nechcete souborů kořenové dostatek místa. Proto jednu zkontrolujte, že je před instalací SAP HANA prostřednictvím swpm dostatek místa v části /hana a /usr/sap.

[Tento článek](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm) z SAP popisuje standardní souborů rozložení SAP HANA 


![](./media/virtual-machines-linux-sap-hana-get-started/image010.jpg)

Po instalaci SAP NetWeaver na obrázku standardních Galerie SLES 12 Azure budete mít zprávu, že je bez vyměňovat mezery. Zbavit tato zpráva jednu může například ručně přidat odkládací soubor popsanou v tomto dokumentu přes dd, mkswap a swapon. Jenom vyhledejte "Přidání zaměnit soubor ručně" v [tomto článku](https://www.suse.com/documentation/sled-12/book_sle_deployment/data/sec_yast2_i_y2_part_expert.html)

Další možností je třeba nakonfigurovat odkládací prostor prostřednictvím agenta OM Linux. Další informace najdete [tady](virtual-machines-linux-agent-user-guide.md)


### <a name="etchosts"></a>/ atd/hosts

![](./media/virtual-machines-linux-sap-hana-get-started/image011.jpg)

Jiné důležitým aspektem před spuštěním nainstalovat SAP je zahrnout názvy hostitelů a IP adresy SAP VMs/etc/hosts soubor. Jeden by měl nasazení všechny VMs SAP v rámci jedné Azure virtuální sítě a potom použijte vnitřní IP adresy.

### <a name="etcfstab"></a>/ atd/fstab

![](./media/virtual-machines-linux-sap-hana-get-started/image000c.jpg)

Testování fázi ho vypadá je dobré přidat parametr nofail fstab. Pokud se vyskytne problém s disků OM by pořád zapínal a není reagovat v procesu spouštění. Ale už pozor na stejně jako v tomto případě další místo na disku nemusí být k dispozici a procesy můžou zaplnit kořenové souborů. V případě, že by chybět /hana SAP HANA nespustí, když vůbec.


## <a name="install-graphical-gnome-desktop-on-sles-12"></a>Grafické plochy Gnome nainstalovat SLES 12

Tato kapitola se skládá ze dvou secitions, které najdete v těchto tématech:

* Instalace aplikace plochy Gnome a xrdp na SLES 12
* Spuštění na základě Java SAP MC prohlížeči Firefox SLES 12

Jednu také použít alternativy jako Xterminal VNC ale od září 2016 Tento článek popisuje pouze xrdp.

### <a name="installation-of-gnome-desktop-and-xrdp-on-sles-12"></a>Instalace aplikace plochy Gnome a xrdp na SLES 12

Zejména pro uživatele, kteří mají Microsoft Windows pozadí a chcete použít grafický plochy přímo v prostředí SAP Linux VMs ke spuštění Firefox, Sapinst, SAP grafické, SAP MC nebo HANA Studio a možná připojení k OM prostřednictvím RDP z počítače s Microsoft Windows je jednoduchý způsob, jak dosáhnout. Když to nemusí být vhodné například databázovým serverem výrobní je ok prostředí čistého prototypů/ukázku. Tady je postup nainstalovat plochy Gnome OM Azure SLES 12:

Nainstalujte na plochu gnome tento příkaz (například v okně nátěrové):

   zypper ve vzorku -t gnome basic

Nainstalujte xrdp povolit připojení k OM prostřednictvím RDP:

   zypper v xrdp

Upravte /etc/sysconfig/windowmanager a nastavte výchozí správce systému windows na Gnome:

   DEFAULT_WM = "gnome"

Spusťte chkconfig, abyste měli jistotu, že tento xrdp spustí automaticky po restartování počítače: 

  chkconfig-xrdp na úrovni 3

v případě, že by měl být problém s připojením RDP zkuste restartování (možná mimo nátěrové okno):

  restartování /etc/xrdp/xrdp.SH

v případě, že restartování xrdp jako výše uvedené nebude fungovat zaškrtněte, pokud soubor .pid a odeberte ho:

  Zaškrtněte políčko /var/run a vyhledejte xrdp.pid   
  Odeberte ho a pak zkuste restartování



### <a name="sap-mc"></a>SAP MC


Spusťte grafické MC o SAP na základě Java mimo aplikaci Azure SLES 12 OM po instalaci Gnome Firefox bude plochy podle popisu v předchozí části, jednu dojde k chybě kvůli chybějící modul plug-in Java prohlížeče.

Adresa URL zahájíte SAP MC je <server>: 5 < instance_number > 13

Další informace najdete [tady](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm)


![](./media/virtual-machines-linux-sap-hana-get-started/image013.jpg)

Na snímek výše jednu můžete vidět, jak může vypadat chybové zprávy při modulu plug-in prohlížeče Java chybí. 

![](./media/virtual-machines-linux-sap-hana-get-started/image014.jpg)

Jednou z možností problém vyřešit je jednoduše nainstalovat chybějící modul plug-in prostřednictvím Yast.

![](./media/virtual-machines-linux-sap-hana-get-started/image015.jpg)

Opakující se adresa URL SAP MC zobrazíte nějakou dobu první dialogové okno s žádostí o aktivaci modulu plug-in.


Jeden další problém, který může objevovat se zobrazí chybová zpráva o chybějících souborů: javafx.properties to je velmi pravděpodobné týkající se instalace Java 1.8, který je potřebný pro 7.4 grafické SAP

Verze IBM Java vidět prostřednictvím Yast nezahrnuje tento soubor. Řešení je Java stahování Oracle.
Článek, který pojednává o tohoto problému najdete [tady](https://scn.sap.com/thread/3908306)



## <a name="manual-sap-hana-installation-via-swpm-as-part-of-a-netweaver-75-installation"></a>Ruční instalace SAP HANA prostřednictvím SWPM jako součást NetWeaver 7.5 instalace


V následujícím seznamu snímky obrazovek vidíte základní kroky instalace SAP NetWeaver 7.5 a SAP HANA SP12 prostřednictvím SWPM (sapinst). Jako součást NW 7.5 instalace SWPM má funkce taky nainstalovat HANA databáze jako jedna instance.


![](./media/virtual-machines-linux-sap-hana-get-started/image012.jpg)

Při zkoušce vzorku byl nainstalovaný prostředí jediným ABAP aplikace server. Možnost "Distributed systém" byla použita k instalaci instance ASC a instance serveru primární aplikace v jednom Azure OM a SAP HANA jako databáze systému do jiného OM Azure.


![](./media/virtual-machines-linux-sap-hana-get-started/image016.jpg)

Jakmile instance ASC máte nainstalované na serveru aplikaci OM a je nastavena na "zeleným" v SAP MC sapmnt directory, která obsahuje například v adresáři profilu SAP má mají být sdíleny se serverem SAP HANA DB OM.
Krok instalace DB potřebuje přístup k těmto informacím. Nejlepší způsob, jak je používat NFS, které můžou být konfigurují Yast.


![](./media/virtual-machines-linux-sap-hana-get-started/image017b.jpg)

V aplikaci by měl server OM adresáři sapmnt sdílet prostřednictvím NFS pomocí možnosti "rw" i "no_root_squash". Výchozí hodnota je "ro" a "root_squash", což může vést k problémy při instalaci instance databázového.


![](./media/virtual-machines-linux-sap-hana-get-started/image018b.jpg)

Na databázový HANA SAP server OM sapmnt sdílet ze serveru aplikace, které OM nakonfigurovat přes "Klientovi NFS" (například pomocí Yast)


![](./media/virtual-machines-linux-sap-hana-get-started/image019.jpg)

Potom je nutné přihlásit k serveru SAP HANA DB OM a začněte SWPM provádění dalším krokem distribuované instalace NW 7.5 - "Instance databázového".


![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Týkající se instalace SAP HANA není k dispozici ve skutečnosti příliš mnoho k zadání po výběru "typické" instalace. Kromě cestu k installatiom médií jednu musí zadejte ID zabezpečení databáze, název hostitele, číslo instance a hesla správce systémových DB.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Dalším krokem je zadat heslo pro schématu DBACOCKPIT.



![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Poté přejde otázky k zadání hesla schématu SAPABAP1.


![](./media/virtual-machines-linux-sap-hana-get-started/image023.jpg)

Na konci by měl být jenom zelené značky zaškrtnutí před každý fáze procesu instalace DB a pole malé zprávy, které překryvné okno by měl vyslovte příkaz "spuštění aplikace: Instance databázového dokončil".

 
![](./media/virtual-machines-linux-sap-hana-get-started/image024.jpg)

Po úspěšném instalaci by měl zobrazit SAP MC taky instance odpis.ZRYCH "zeleným" a úplný seznam procesů SAP HANA (například hdbindexserver, hdbcompileserver)


![](./media/virtual-machines-linux-sap-hana-get-started/image025.jpg)

Tento snímek obrazovky znázorňuje díly struktura souboru v části /hana/shared, která SWPM vytvořená během instalace HANA. Došlo lze zadat jinou cestu. Proto je tak důležitý k připojení další místo na disku ve skupinovém rámečku /hana před instalací SAP HANA prostřednictvím SWPM Chcete-li předejít souborů kořenové dojdou volného místa.


![](./media/virtual-machines-linux-sap-hana-get-started/image026.jpg)

Tady jednu vidí totéž popsané před /usr/sap adresáře.


![](./media/virtual-machines-linux-sap-hana-get-started/image027b.jpg)

Posledním krokem distribuované instalace ABAP je "Primární serveru Instance aplikace"


![](./media/virtual-machines-linux-sap-hana-get-started/image028b.jpg)

Jakmile máte nainstalovaný PAS, stejně jako SAP grafické jednu zaškrtněte prostřednictvím transakce "dbacockpit", která všechno vypadá přímo k instalaci SAP HANA.

 
![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Naposledy může kroku 1 instalace SAP HANA Studio server aplikace SAP OM a připojte k SAP HANA instance spuštěna databázový server OM.




## <a name="manual-sap-hana-installation-via-hana-life-cycle-manager-tool-hdblcm"></a>Ruční instalace SAP HANA prostřednictvím HANA životního cyklu správce nástroj hdblcm


Kromě instalaci SAP HANA jako součást distribuované instalace prostřednictvím SWPM je také uvede první instalaci samostatného HANA pomocí hdblcm a pak nainstalujte například SAP NetWeaver 7.5 později. V seznamu snímky obrazovek dole vidíte, jak to funguje.

Tady jsou tři zdroje informací o nástroji hdblcm HANA:

[Výběr správné SAP HDBLCM HANA úkolu](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)

[Nástroje pro správu SAP HANA životního cyklu](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)

[SAP HANA serveru Průvodci instalací a aktualizace](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)



![](./media/virtual-machines-linux-sap-hana-get-started/image030.jpg)

Aby se zabránilo spuštění na pozdější s výchozí nastavení id skupiny pro problémy \<ID HANA zabezpečení\>adm uživatele (vytvořená pomocí nástroje hdblcm) jednu měli definovat nové skupiny nazvané "sapsys" s id skupiny 1001 před instalací SAP HANA pomocí hdblcm.




![](./media/virtual-machines-linux-sap-hana-get-started/image031.jpg)

Počáteční čas hdblcm první, bude nabídky jednoduché start, kde je nutné vybrat položku 1 "instalace nového systému"



![](./media/virtual-machines-linux-sap-hana-get-started/image032.jpg)

Na tento snímek jednu v tématu klíčové možnosti, které byly vloženy před. Důležité – adresářů kterého byly s názvem HANA protokolu a data svazky, jakož i instalační cesta (/ hana/sdílené v tomto příkladu) a/usr/sap by měly není součástí systému souborů kořenové, ale patří discích Azure dat, které jsou připojené k OM podle popisu v části Nastavení OM Azure. Zabrání toto riziko, že kořenovou souborů může docházet prostor.
Jednu uvidíte taky, že uživatel správce HANA id uživatele 1005 a je součástí skupiny sapsys (id 1001), která byla definována před instalací.



![](./media/virtual-machines-linux-sap-hana-get-started/image033.jpg)

Jednu můžete zkontrolovat HANA \<ID HANA zabezpečení\>podrobné informace o uživateli adm (azdadm v tomto příkladu) v/atd/hesel



![](./media/virtual-machines-linux-sap-hana-get-started/image034.jpg)

Po instalaci SAP HANA pomocí hdblcm lze zobrazit v SAP HANA Studio. SAPABAP1 schéma, které patří například všemi SAP NetWeaver tabulkami ještě není dostupné.



![](./media/virtual-machines-linux-sap-hana-get-started/image035b.jpg)

Po instalaci SAP HANA jednu můžete nainstalovat SAP NetWeaver sobě. V tomto příkladu bylo provedeno použití "distribuované instalace" prostřednictvím SWPM způsobem popsaným v odpovídající části výše.
Při instalaci instance databázového prostřednictvím SWPM jednu jenom musí zadat stejná data jako před s hdblcm (například hostname, ID HANA zabezpečení číslo instance). SWPM potom použití stávající instalace HANA a přidejte další schémata.



![](./media/virtual-machines-linux-sap-hana-get-started/image036b.jpg)

Toto je obrázek krok instalace SWPM kde je nutné zadávat data týkající se schématu DBACOCKPIT.


![](./media/virtual-machines-linux-sap-hana-get-started/image037b.jpg)

Potom SWPM předpokládá, že zadání dat o schématu SAPABAP1.


![](./media/virtual-machines-linux-sap-hana-get-started/image038b.jpg)

Po dokončení instalace instance SWPM databáze jednu najdete v článku schématu SAPABAP1 v HANA Studio.



![](./media/virtual-machines-linux-sap-hana-get-started/image039b.jpg)

A nakonec po instalaci serveru aplikací SAP a SAP grafické jedna by měla být ověřit instance HANA DB s transakce "dbacockpit".




## <a name="general-information-related-to-sap-azure-certifications-running-sap-hana-on-azure-and-sap-software-download"></a>Obecné informace související s SAP Azure certifikace SAP HANA spuštěna Azure a SAP stažení softwaru

* Obecné doku SAP Azure o aplikaci SAP na Azure s operačním systémem Windows klasický režim: [Pomocí SAP na virtuálních počítačích Windows v Azure](virtual-machines-windows-classic-sap-get-started.md)

* informace o existující šablony SAP pro použití zákazníci: [Azure rychlý úvod šablony pro SAP](https://blogs.msdn.microsoft.com/saponsqlserver/2016/05/16/azure-quickstart-templates-for-sap/)

* Obecné doku SAP Azure o spuštění SAP na Azure s operačním systémem Linux v modelu správce prostředků Azure: [Pomocí SAP na Linux virtuálních počítačích (VMs)](virtual-machines-linux-sap-get-started.md)

* certifikované adresáře hardwaru SAP HANA, které je uvedené typy Azure OM, které jsou podporovány pro výrobní: [Certifikované HANA SAP® hardwaru adresáře](https://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)

* informace o velikosti virtuálního počítače zejména u pracovního vytížení Linux: [velikost virtuálních počítačích v Azure](virtual-machines-linux-sizes.md)

* SAP poznámky, které jsou uvedeny všechny podporované SAP produkty na Azure a podporovaných typů Azure OM SAP: [1928533 poznámku SAP](https://launchpad.support.sap.com/#/notes/1928533/E)

* SAP upozornění o SAP "rozšířeného sledování" s Linux VMs Azure: [2191498 poznámku SAP](https://launchpad.support.sap.com/#/notes/2191498/E)

* SAP HANA nabízející na Azure "Velké instance". Je třeba porozumět tomu, že se nejedná o tom, jak SAP HANA na Azure VMs, ale v hybridním prostředí kde servery aplikace SAP v Azure VMs ale SAP HANA běží na servery: [2316233 poznámku SAP](https://launchpad.support.sap.com/#/notes/2316233/E)

* SAP poznámky s informacemi o SAPOSCOL na Linux: [1102124 poznámku SAP](https://launchpad.support.sap.com/#/notes/1102124/E)

* Základní sledování metriky SAP na Microsoft Azure: [SAP Poznámka 2178632](https://launchpad.support.sap.com/#/notes/2178632/E)

* Informace o správce prostředků Azure: [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md)

* Informace o nasazení Linux VMs pomocí šablony: [nasazení a správu virtuálních počítačích pomocí Správce prostředků Azure šablony a rozhraní příkazového řádku Azure](virtual-machines-linux-cli-deploy-templates.md)

* Porovnání nasazení modely mezi správce prostředků Azure a klasické: [Správce prostředků Azure porovnání klasické nasazení: princip modelů nasazení a stav zdroje](../resource-manager-deployment-model.md)

* Stažení NetWeaver 7.5 pro Linux/HANA z Marketplace služeb SAP:![](./media/virtual-machines-linux-sap-hana-get-started/image001.jpg)

* Stažení HANA SP12 platformy Edition z Marketplace služeb SAP:![](./media/virtual-machines-linux-sap-hana-get-started/image002.jpg)

