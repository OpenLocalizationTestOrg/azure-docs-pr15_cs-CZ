<properties 
   pageTitle="Selhání zpět VMware virtuálních počítačích a fyzických serverů z Azure VMware (starší verze) | Microsoft Azure" 
   description="Tento článek popisuje, jak navrácení VMware virtuální počítač, který je replikovat na Azure s obnovení webu Azure." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="storage-backup-recovery" 
   ms.date="10/05/2016"
   ms.author="ruturajd@microsoft.com"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Selhání obráceně VMware virtuálních počítačích a fyzických serverů z Azure VMware s obnovení webu Azure (starší verze)

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-failback-azure-to-vmware.md)
- [Azure klasické portálu](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klasický portál (starší verze)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


Obnovení webu Azure služby přispívá k strategie firmy kontinuitu a havárie obnovení (BCDR) tak, že orchestrating replikace, převzetí a obnovení virtuálních počítačích a fyzické servery. Počítačích replikovat Azure nebo vedlejší místního datovém centru. Rychlý přehled číst [Co je obnovení webu Azure?](site-recovery-overview.md)

## <a name="overview"></a>Základní informace

Tento článek popisuje, jak selhání obráceně VMware virtuálních počítačích a Windows/Linux fyzické servery z Azure na svůj místní web, když jste replikovat z místních webů na Azure.

>[AZURE.NOTE] Tento článek popisuje scénáři starší verze. Pokud replikovat na Azure pomocí [těchto starších pokynů](site-recovery-vmware-to-azure-classic-legacy.md), by měl použít jenom pokynů v tomto článku. Pokud se nastavení replikace pomocí [rozšířeného nasazení](site-recovery-vmware-to-azure-classic-legacy.md) pak postupujte podle pokynů v [tomto článku](site-recovery-failback-azure-to-vmware-classic.md) selhání zpět. 


## <a name="architecture"></a>Architektura

Tento diagram znázorňuje scénář překlopení a překlopení zpět. Modrá řádky jsou připojení používaný během překlopení. Červené podtržení jsou připojení používaný během navrácení. Řádky se šipkami přejděte přes internet.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Než začnete 

- Má mít nebyl úspěšný přes VMware VMs nebo pole fyzicky servery a by měly být spuštěny v Azure.
- Je třeba si uvědomit, že jenom může selhat zpět VMware virtuálních počítačích a Windows/Linux fyzických serverů, z Azure VMware virtuálních počítačích na místní primární webu.  Pokud jste zpátky selhání fyzické počítače, převezme Azure budou převedeny na Azure OM a navrácení VMware budou převedeny na VMware OM.

Tady je, jak nastavit navrácení:

1. **Nastavení navrácení součásti**: budete muset nastavit vContinuum server místní a přejděte na konfigurační server OM v Azure. Také nastavíte serveru obrázku jako Azure OM odeslání dat zpátky do místního serveru předlohy cílové. Registrace serveru proces s konfigurační server, který vyřizoval záložní. Nainstalujte místní předlohy cílový. Pokud potřebujete systému Windows server předlohy cílové ní nastavili automaticky při instalaci vContinuum. Pokud potřebujete Linux musíte jej nastavit ručně na samostatném serveru.
2. **Povolení ochrany a navrácení**: když jste nastavili součásti, v vContinuum budete muset povolit ochranu pro selhání Azure VMs. Budete provádět kontrolu připravenosti na VMs a spusťte přepojení z Azure na svůj místní web. Po dokončení navrácení reprotect místního počítače, aby začínaly replikace Azure.



## <a name="step-1-install-vcontinuum-on-premises"></a>Krok 1: Instalace vContinuum místní

Musíte si nainstalovat serveru vContinuum místně a přejděte na konfigurační server.

1.  [Stáhněte si vContinuum](http://go.microsoft.com/fwlink/?linkid=526305). 
2.  Pak budou moct stáhněte [vContinuum aktualizace](http://go.microsoft.com/fwlink/?LinkID=533813) verze.
3. Nainstalujte nejnovější verzi vContinuum. Na stránce **Vítejte** klikněte na **Další**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  Na první stránce průvodce zadejte IP adresu serveru CX a port serveru CX. Vyberte **použít HTTPS**.

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  Najděte konfigurační server IP adresu na karta **řídicí panel** konfigurační server OM, v Azure.
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  Najděte konfigurační server veřejné port HTTPS na kartě **koncové body** konfigurační server OM v Azure.

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. Na stránce **Podrobnosti CS heslo** zadejte heslo, které je uvedeno dolů při registraci konfigurační server. Pokud si nepamatujete ho vrácení se změnami **C:\\Program Files (x86)\\InMage systémy\\soukromé\\connection.passphrase** na konfigurační server OM.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  Na stránce **Vyberte cílové umístění** zadejte místo, kam chcete nainstalovat vContinuum server a klikněte na tlačítko **nainstalovat**.

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. Po dokončení instalace, můžete spustit vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## <a name="step-2-install-a-process-server-in-azure"></a>Krok 2: Instalace serveru obrázku v Azure 

Budete potřebovat k instalaci serveru obrázku v Azure tak, aby VMs v Azure můžete odeslat data zpátky do místního předlohy cílový. 

1.  Na stránce **Konfigurace servery** v Azure vyberte Přidat nový server obrázku.

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  Zadejte server proces název a název a heslo pro připojení k virtuálního počítače jako správce. Vyberte konfigurační server, ke kterému chcete zaregistrovat server procesu. To by měl být stejný server, které používáte k ochraně a nepovede přes virtuálních počítačích.
3.  Určení Azure sítě, ve kterém by měly být nasazeny server procesu. Je třeba stejné síti jako konfigurační server. Určení jedinečné podsítě IP adresu vybranou a začněte nasazení.

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  Úlohy se spustí pro nasazení serveru obrázku.

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  Po nasazení serveru obrázku v Azure se přihlásit k serveru pomocí přihlašovacích údajů, které jste zadali. Registrace serveru proces stejným způsobem jako registrované místního serveru obrázku. 

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] Servery registrované během navrácení se nezobrazí ve skupinovém rámečku vlastnosti OM v obnovení webu. Jsou viditelné na kartě **servery** konfigurační server, ke kterému jste registrované. Může to trvat asi 10 až 15 minut až poté, co, které se zobrazí na kartě server procesu.


## <a name="step-3-install-a-master-target-server-on-premises"></a>Krok 3: Instalace předlohy cílový místní

V závislosti na zdrojovém virtuálních počítačích operačního systému budete potřebovat k instalaci Linux nebo Windows server předlohy cílové místních.

### <a name="deploy-a-windows-master-target-server"></a>Nasazení systému Windows server předlohy cíl

Hlavní cílové Windows je už součástí vContinuum nastavení. Při instalaci vContinuum hlavní server také nasazeném ve stejném počítači a registrován konfigurační server.

1.  Začněte nasazení, vytvořte prázdný počítače místních na hostiteli ESX, do kterého chcete obnovit VMs z Azure.

2.  Zajistit, aby byly aspoň dva disků připojené k OM – jeden slouží k operačního systému a druhý slouží k jednotce uchovávání informací.

3.  Nainstalujte operačního systému.

4.  Nainstalujte vContinuum na serveru. To také dokončení instalace předlohy cílového serveru.

### <a name="deploy-a-linux-master-target-server"></a>Nasazení serveru předlohy cílové Linux

1.  Začněte nasazení, vytvořte prázdný počítače místních na hostiteli ESX, do kterého chcete obnovit VMs z Azure.

2.  Zajistit, aby byly aspoň dva disků připojené k OM – jeden slouží k operačního systému a druhý slouží k jednotce uchovávání informací.

3.  Nainstalujte operační systém Linux. Hlavní cílový systém Linux nepoužívejte LVM pro kořenovou nebo uchovávání informací úložiště mezery. Linux předlohy cílové server nakonfigurovaný Chcete-li předejít LVM oddíly/disků zjišťování ve výchozím nastavení.
4.  Oddíly, které můžete vytvořit:

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  Provádět níže zveřejňují pokynů k instalaci před zahájením předlohy cílové instalaci.


#### <a name="post-os-installation-steps"></a>Postup instalace příspěvek OS

Zobrazíte SCSI ID pro každého pevného záložní ve počítače virtuální Linux povolte parametr "disk. EnableUUID = TRUE "následujícím způsobem:

1. Vypnutí virtuálního počítače.
2. Klikněte pravým tlačítkem myši na položku OM v levém panelu > **Upravit nastavení**.
3. Klikněte na kartu **Možnosti** . Vyberte **Upřesnit\>obecné položku** > **Parametry konfigurace**. **Parametry konfigurace** možnost je dostupná jenom při vypnutí počítače.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. Zkontroluje, zda řádek s **disku. EnableUUID** existuje. Pokud znamená a je nastavena na **hodnotu False** potom ho nastavit na **hodnotu True** (ne malá a velká písmena). Pokud existuje a je nastavena na hodnotu true, klikněte na tlačítko **Storno** a po spuštění si vyzkoušet příkazu SCSI uvnitř hostovaný operační systém. Pokud neexistuje klikněte na **Přidat řádek**.
5. Přidání disku. EnableUUID ve sloupci **název** . Nastavte jeho hodnotu jako PRAVDA. Nepřidávat výše uvedených hodnot spolu s dvojitých uvozovek.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>Stažení a instalace dalších balíčků

Poznámka: Zkontrolujte, jestli že má systém před stažení a instalaci dalších balíčků připojení k Internetu.

\#YUM instalace -y xfsprogs perl lsscsi rsync wget kexec nástroje

Tento příkaz tyto 15 balíčky z úložiště CentOS 6.6 a soubory ke stažení nainstaluje je:

BC 1.06.95 1.el6.x86\_64. ot

busybox 1.15.1 20.el6.x86\_64. ot

elfutils knihoven 0.158 3.2.el6.x86\_64. ot

kexec nástroje 2.0.0 280.el6.x86\_64. ot

lsscsi 0.23 2.el6.x86\_64. ot

lzo 2.03 3.1.el6\_5.1.x86\_64. ot

Perl 5.10.1 136.el6\_6.1.x86\_64. ot

Perl modul-připojitelné-3.90-136.el6\_6.1.x86\_64. ot

Perl-Pod-návrat-1.04-136.el6\_6.1.x86\_64. ot
    
Perl-Pod-jednoduché – 3.13-136.el6\_6.1.x86\_64. ot

Perl knihoven 5.10.1 136.el6\_6.1.x86\_64. ot

Perl verze 0,77 136.el6\_6.1.x86\_64. ot

RSync 3.0.6 12.el6.x86\_64. ot

Tenhle 1.1.0 1.el6.x86\_64. ot

wget 1.12 5.el6\_6.1.x86\_64. ot

Poznámka: Pokud zdroj počítač používá Reiser nebo XFS systému souborů pro zařízení kořenové nebo operačních systémů, pak následující balíčky by měl být stáhnout a nainstalovat do předlohy cílové Linux před zámek.

\#/usr/local disk CD

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#ot - ivh kmod-reiserfs 0,0 1.el6.elrepo.x86\_64. ot reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64. ot

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#ot - ivh xfsprogs-3.1.1-16.el6.x86\_64. ot

#### <a name="apply-custom-configuration-changes"></a>Použití vlastní konfigurace změn

Před použitím těchto změn Ujistěte se, že po dokončení předchozího oddílu a potom postupujte podle těchto kroků:


1. Zkopírujte do nově vytvořený OS RHEL 6-64 Unified Agent binární.

2. Spusťte tento příkaz untar binární: **cílový - zxvf \<název souboru\> **

3. Spusťte tento příkaz a udělte oprávnění: \# **chmod 755./ApplyCustomChanges.sh**

4. Následujícím způsobem: ** \# ./ApplyCustomChanges.sh**. Skript jenom jednou se spouštějí na serveru. Po spuštění skript, restartujte server.



### <a name="install-the-linux-server"></a>Instalace Linux server


1. [Stáhnout](http://go.microsoft.com/fwlink/?LinkID=529757) instalační soubor.
2. Zkopírujte soubor do počítače virtuální cílové Linux předlohy pomocí nástroj klienta sftp podle svého výběru. Střídavě přihlášení Linux předlohy cílové virtuálního počítače a pomocí wget můžete stáhnout instalační balíček ze zadaného odkazu
3. Přihlášení do na Linux předlohy cílové serveru virtuálního počítače pomocí ssh klienta podle svého výběru
4. Pokud jste připojení k síti Azure dnem nasazení serveru Linux předlohy cílové prostřednictvím připojení VPN pomocí interní IP adresu serveru, který můžete najít v karta **řídicí panel** virtuálního počítače a port 22 se připojit k předlohy cílové Linux serveru pomocí zabezpečené prostředí.
5. Pokud se připojujete k serveru předlohy cílové Linux přes veřejné připojení k Internetu pomocí serveru předlohy cílové Linux veřejné virtual IP adresu (na kartě **řídicího panelu** virtuálních počítačích) a veřejné koncový bod vytvoří ssh se přihlásit k Linux servder.
6. Extrahujte soubory z archivu algoritmem gzip Linux předlohy cílové serveru instalační program vkládání spuštěním: *"cílový – xvzf funkci Automatické obnovení systému Microsoft\_UA\_8.2.0.0\_RHEL6 64\"* z adresáře, který obsahuje instalační soubor.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. Pokud extrahovali Instalační služby systému souborů na jinou složku změnit do adresáře, aby byly extrahovaných které obsah vkládání archivovat. Z tohoto adresář spustit "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. Po zobrazení výzvy vyberte primární roli vyberte **2 (cílový předlohy)**. Další možnosti instalace interaktivní opustit na výchozí hodnoty.
9. Počkejte instalace pokračovat a rozhraní konfigurace hostitele zobrazovat. Konfigurace hostitele nástroj pro předlohy sarget Linux Server je nástroj příkazového řádku. Nechcete změnit velikost ssh okno nástroj klienta. Pomocí kláves se šipkami vyberte požadovanou možnost **globální** a stiskněte klávesu ENTER na klávesnici.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)


10. Do pole **IP** zadejte interní IP adresu konfiguračního serveru (najdete ho na **řídicí panel** karet konfigurační server OM) a stiskněte klávesu ENTER. Do pole **Port** zadejte **22** a stiskněte ENTER. 
11.  Nechte **Použití HTTPS** **Ano**a stiskněte klávesu ENTER.
12.  Zadejte heslo, který byl vytvořen konfigurační server. Pokud používáte klienta nátěrové z počítače Windows ssh do virtuálního počítače Linux slouží k vložení obsahu schránky Shift + Insert. Kopírování heslo do místní schránky pomocí kláves Ctrl + C, můžete ho vložte Shift + Insert. Stiskněte klávesu ENTER.
13.  Pomocí klávesy šipka vpravo přejdete do ukončení a stiskněte klávesu ENTER. Počkejte na dokončení instalace.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Pokud z nějakého důvodu se nepodařilo zaregistrovat serveru předlohy cílové Linux konfigurační server máte tak znovu spuštěním hostiteli nástroj pro konfiguraci z /usr/local/ASR/Vx/bin/hostconfigcli (Nejdřív musíte nastavit přístupová oprávnění k němu spuštěním chmod jako super uživatel).


#### <a name="validate-master-target-registration"></a>Ověření registrace předlohy cíl

Můžete ověřit, že hlavní cílový server byla úspěšně na konfigurace serveru registrován obnovení webu Azure trezoru > **Konfigurační Server** > **Podrobnosti o Server**.

>[AZURE.NOTE] Po registraci předlohy cílový server, pokud se zobrazí chyby konfigurace virtuálního počítače mohou někdo odstranil z Azure nebo koncové body nejsou nakonfigurovány správně, je to proto sice konfigurace hlavního cílové rozpoznané službou Azure dndpoints nasazené předlohy cílové v Azure, není to platí pro místní předlohy cílové serveru místního. Neovlivní to navrácení a tyto chyby mohli ignorovat. 



## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Krok 4: Ochrana virtuálních počítačích k místnímu webu

Potřebujete k ochraně VMs k místnímu webu před nepovede zpátky.

### <a name="before-you-begin"></a>Než začnete

Pokud virtuálního počítače se nezdaří přes Azure, přidá další temp jednotky stránky souboru. Toto je navíc jednotku, která je obvykle není potřeba tak, že vaše selhalo přes OM, protože už může obsahovat jednotka snaží souboru stránky. Než začnete obráceném ochrany virtuálních počítačích, je třeba zajistit, že tento jednotka do režimu offline tak, že nejsou chráněny. Udělejte to takto:

1.  Otevřete Správa počítače a klikněte na Správa úložiště tak, že jsou uvedeny disků online a připojené k počítači.
2.  Vyberte dočasné disku připojeného k počítači a zvolte si přenést offline. 

### <a name="protect-the-vms"></a>Ochrana VMs

1. Na portálu Azure zkontrolujte stav virtuálního počítače zajistit, že se nezdařilo přes.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. Spusťte vContinuum ve vašem počítači.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. Klikněte na **Nová ochrana** a vyberte požadovaný typ systému operace 

4.  V novém okně Otevřít vyberte **Typ OS** > **Získat informace** o VMs chcete navrácení. **Podrobnosti o primární server**identifikovat a vyberte virtuálních počítačích, které chcete zamknout. VMs jsou uvedené v části název hostitele vCenter, který byly na před překlopení.
5.  Když vyberete virtuální počítač zamknout (a ji již nelze přes Azure) automaticky otevírané okno nabízí dvě položky pro virtuální počítač. Je to proto konfigurační server rozpozná dvě instance virtuálních počítačích registrované k němu. Potřebujete odstraňte položku pro místní OM tak, že můžete chránit správné OM. K identifikaci správné Azure OM položce tady, můžete se přihlásit k OM Azure a přejděte na C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc. V souboru drscout.conf popsána ID hostitele. V dialogovém okně vContinuum obsahovat záznam, u kterého je používaný ID hostitele OM. Odstranění všech položek. Chcete-li vybrat správný OM můžete odkázat na IP adresu. IP adresu oblast místní budou OM místní.

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. Na serveru vCenter vypnout virtuální počítač. Můžete taky odstranit VMs na pracovišti.
7. Zadejte místního MT serveru, ke kterému chcete zamknout VMs. K tomuto účelu připojte k vCenter serveru, ke kterému chcete navrácení.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. Vyberte předlohy cílové serverové na hostiteli, do kterého chcete obnovit OM.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. Nabízí možnost replikace u každé virtuálních počítačích. Můžete to udělat musíte vybrat úložiště straně obnovení, ke kterému VMs obnovit. Tabulka shrnuje různé možnosti, které potřebujete pro každou OM.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **Možnost** | **Možnost doporučené hodnota**
    ---|---
    IP adresa zpracovat serveru | Vyberte server proces nasazenou v Azure
    Uchovávání informací velikost MB| 
    Hodnota uchovávání informací | 1
    Dny a hodiny | Dny
    Interval konzistence | 1
    Výběr cílového úložiště | Úložišti dat k dispozici na webu obnovení. Úložiště dat by měl dostatek místa a být k dispozici na hostitele ESX, podle kterého chcete obnovit virtuální počítač.


10. Konfigurace vlastností, které virtuální počítač získá po přepnutí místních webů. Vlastnosti jsou shrnuté v následující tabulce.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    **Vlastnost** | **Podrobnosti**
    ---|---
    Konfigurace sítě| Pro každý síťový adaptér zjišťování vyberte ho a klikněte na **změnit** a konfigurace navrácení IP adresa virtuální počítač. 
    Hardwarová konfigurace:| Určete OM procesoru a paměti. Nastavení lze použít pro všechny VMs chcete zamknout. K identifikaci správné hodnoty pro procesoru a paměti, můžete v nápovědě k roli velikost IAAS VMs a zjistit počet jádra a paměti přiřazené.
    Zobrazované jméno | Po navrácení do místního nasazení můžete přejmenovat virtuálních počítačích jako se objeví v seznamu vCenter. Výchozí název je název hostitele počítače virtuálního počítače. K určení názvu OM, může označovat OM seznam ve skupině zámek.
    Konfigurace překladu adres | Popisované píše níže

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>Konfigurace nastavení překladu adres

1. Povolení ochrany virtuálních počítačích, dva kanály komunikace muset vytvořit. První kanál je mezi virtuálního počítače a server procesu. Tento kanál shromáždí data z OM a odesílá na server proces, který odešle data do předlohy cílový server. Pokud server obrázku a virtuálního počítače chráněny jsou ve stejné síti Azure virtuální potom nemusíte používat nastavení překladu adres. V opačném případě určete nastavení překladu adres. Zobrazte veřejnou IP adresu serveru proces Azure. 

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. Druhý kanál je mezi server procesu a předlohy cílový server. Možnost používat překladu síťových adres nebo ne závisí na tom, jestli používáte připojení VPN na základě nebo komunikace přes internet. Nevybírejte překladu síťových adres, pokud používáte VPN, ale jenom v případě, že používáte připojení k Internetu.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. Pokud jste to ještě odstranili virtuálních počítačích místní zadaným způsobem, a pokud úložiště, které jste se nedaří zpátky stále obsahuje staré VMDK budete muset zajistit, aby navrácení vytvořené OM získá na nové místo. Můžete to udělat klikněte na **Upřesnit** nastavení a zadejte alternativní složku obnovení **Nastavení název složky**. Nechte další možnosti ve výchozím nastavení. Použití nastavení název složky do všech serverů.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. Spusťte Kontrola připravenosti zajistit virtuálních počítačích chtít chráněny zpátky do místního nasazení. 

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)


5. Čekat na Dokončit. Pokud je úspěšně na všechny VMs zadáte název plán ochrany. Klepněte na tlačítko **Uzamknout** zahájíte.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)


6. Můžete sledovat průběh v vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. Po dokončení kroku se **Aktivace plánování ochranu** dokončí, můžete sledovat OM ochrany v portálu obnovení webu.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. Zobrazí se přesně stav po kliknutí na OM a sledování průběhu.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Příprava plánu navrácení

Funkce usnadnění můžete připravit plán navrácení pomocí vContinuum tak, že aplikace může být zpátky do místního serveru kdykoli. Tyto obnovení plány se hodně podobají plány obnovy v obnovení webu.

1.  Spusťte vContinuum a vyberte **Spravovat plány** > **obnovit.** Zobrazí se seznam všech plánů, které jste použili k převzetí VMs. Stejné plány můžete obnovit.

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. Vyberte plán ochrany a všechny ostatní VMs, kterou chcete obnovit v nich. Když vyberete jednotlivé OM uvidíte další podrobnosti, například cílový ESX server a zdrojový OM disk. Klikněte na **Další** spustíte Průvodce obnovit a potom vyberte VMs, kterou chcete obnovit.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. Můžete obnovit založené na více možností, ale doporučujeme používat **Nejnovější značky** a vyberte **použít pro všechny VMs** zajistit, že nejnovějšími daty z virtuální počítač se použije.
4. Spustit **kontrolu připravenosti.** To zkontroluje, pokud není nakonfigurován povolit obnovení OM správné parametry. Pokud jsou všechny ověření úspěšné, klikněte na tlačítko **Další** . Pokud není v protokolu a vyřešit chyby.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  V **Konfiguraci OM** ujistěte, že nastavení obnovení jsou správně nastavená. V případě potřeby můžete změnit nastavení OM.

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. Prohlédněte si seznam virtuálních počítačích, které bude možné obnovit a určete pořadí obnovení. Všimněte si, že jsou uvedeny VMs pomocí počítače název hostitele. Může být obtížné namapujte název hostitele počítače virtuálního počítače.
Názvy namapujete přejděte na virtuálních počítačích **řídicího panelu** v Azure a zkontrolujte název hostitele OM.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. Zadejte název plánu a vyberte **Obnovit později**. Doporučujeme, abyste později obnovit, protože počáteční zámek nemusí být úplný. 
11. Klikněte na **Obnovit** uložení plánu nebo obnovení aktivovat, pokud jste vybrali možnost obnovit nyní a nejpozději. Stav využití Chcete-li zobrazit, pokud je plán uložen.

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Obnovení virtuálních počítačích

Po vytvoření plánu je můžete obnovit virtuálních počítačích. Před zkontrolujte, že virtuálních počítačích dokončili synchronizace. Když se při stavu replikace OK zámek dokončení a splnění operace RPO mezní hodnota. Můžete zkontrolovat stav ve vlastnostech OM.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Dříve než zahájíte obnovení vypněte Azure virtuálních počítačích. Zajistíte neexistuje žádná mozku rozděleného a že pouze uživatelé jednu kopii aplikace. 


1.  Spuštění uloženého plánu. V vContinuum vyberte **Monitor** plány. Seznam všech plánů, které jste spustili.

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  **Obnovení** vyberte plán a klikněte na tlačítko **Start**. Obnovení můžete sledovat. Po aktivovali VMs na ve se můžete připojit k nim vCenter.

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Ochrana Azure po navrácení znovu

Po dokončení navrácení budete pravděpodobně chtít znova zamknout virtuálních počítačích. Udělejte to takto:

1.  Zkontrolujte, že pracují virtuálních počítačích místní a aplikace jsou dostupné.
2.  Na portálu obnovení webu Azure vyberte virtuálních počítačích a odstraňte je. Vyberte zakázat ochranu virtuálních počítačích. Zajistíte tak, že VMs žádné další není zamčený.
3.  V Azure odstraňte selhalo přes Azure virtuálních počítačích
4.  Odstraňte staré virtuálního počítače na vSpehere. Toto jsou VMs, které jste dříve nepodařilo přes Azure.
5.  Na portálu obnovení webu Chraňte VMs naposledy neúspěšných myší. Poté, co jste chráněné můžete je přidat do plánu obnovení.
 
## <a name="next-steps"></a>Další kroky



- [Přečtěte si informace o](site-recovery-vmware-to-azure-classic.md) replikace VMware virtuálních počítačích a fyzické servery na Azure pomocí rozšířeného nasazení.

 
