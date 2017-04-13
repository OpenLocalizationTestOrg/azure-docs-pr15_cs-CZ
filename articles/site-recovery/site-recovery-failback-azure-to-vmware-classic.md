<properties 
   pageTitle="Selhání zpět VMware virtuálních počítačích a fyzické servery k místnímu webu | Microsoft Azure"
   description="Informace o selhání zpět k místnímu webu po převzetí VMware VMs a fyzických serverů Azure." 
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
   ms.workload="required" 
   ms.date="08/22/2016"
   ms.author="ruturajd"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Selhalo: zpět VMware virtuálních počítačích a fyzické servery k místnímu webu

> [AZURE.SELECTOR]
- [Azure portálu](site-recovery-failback-azure-to-vmware.md)
- [Azure Classic portálu](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klasické portál (starší verze)](site-recovery-failback-azure-to-vmware-classic-legacy.md)



Tento článek popisuje, jak selhání zpět Azure virtuálních počítačích z Azure k místnímu webu. Když budete chtít navrácení virtuálních počítačích VMware nebo Windows/Linux fyzické servery poté, co jste selhání z místní webu Azure pomocí tohoto [kurzu](site-recovery-vmware-to-azure-classic.md), postupujte podle pokynů v tomto článku.



## <a name="overview"></a>Základní informace

Tento diagram znázorňuje navrácení architektura tento scénář.

Používejte tato architektura serveru proces se místně a používáte ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Tato architektura použijte, když je proces server na Azure a máte VPN nebo připojení k ExpressRoute.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Pokud chcete zobrazit úplný seznam porty a protokoly navrácení architechture diagram odkazují na následujícím obrázku

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Tady je fungování navrácení:

- Poté, co jste selhání na Azure, po několika krocích se nezdaří zpátky na svůj místní web:
    - **Fázi 1**: reprotect Azure VMs tak, aby začínaly znovu u VMware VMs spuštěná na místním webu. Povolení reprotection slouží k přesunutí OM do skupiny ochranu navrácení vytvořený automaticky při vytvoření původního ochranu skupiny zabezpečení před selháním. Pokud jste přidali ochranu skupiny převzetí obnovení plán potom skupině zámek navrácení byl taky automaticky přidají do plánu.  Během reprotect určit, jak naplánovat selhání zpět.
    - **Fáze 2**: po Azure VMs jsou replikovat na svůj místní web, spusťte selhání selhání zpět z Azure.
    - **Stage 3**: po dat se nezdařila zpátky místního VMs neúspěšných zpátky na reprotect tak, aby začínaly replikace Azure.

> [AZURE.VIDEO enhanced-vmware-to-azure-failback]


### <a name="failback-to-the-original-or-alternate-location"></a>Navrácení původní nebo alternativní umístění

Pokud selhání VMware OM může selhat zpět do stejné zdroje OM, pokud stále existuje místní. V tomto scénáři pouze změny delta se nezdaří zpět. Všimněte si, že:

- Pokud selhání fyzické servery navrácení je vždy nové angličtině VMware.
    - Než znovu fyzické počítače Všimněte si, že
    - Pole fyzicky počítač chráněný budou vracet a virtuálního počítače, když se nepodařilo přes zpět z Azure VMware
    - Ujistěte se, seznamte alespoň jednu předlohu cílové server spolu s potřebné ESX/ESXi hosts, na které je potřeba navrácení.
- Pokud se vám nepovede zpátky na původní OM je potřeba:
    - Pokud OM spravují serverem vCenter cílového předlohy ESX hostitele mají mít přístup k VMs úložiště.
    - Pokud je na hostiteli ESX OM, ale není spravuje vCenter pevný disk OM musí být v úložiště přístupných osobám s postižením hostitelem MT.
    - Pokud je vaše OM na hostiteli ESX a nepoužívá vCenter byste měli dříve, než jste reprotect dokončit zjišťování ESX hostitele MT. Toto nastavení se projeví pokud jste selhání zpátky fyzické servery příliš.
    - Další možností (pokud existuje OM místní) je a vymažete ho dříve, než budete navrácení. Potom navrácení potom vytvoří nový OM na hostiteli stejný jako hlavní cílového ESX hostitele.
    
- Kdy navrácení do jiného umístění dat bude obnovit až stejném úložišti dat a hostiteli ESX jako používaném místního serveru předlohy cílové. 


## <a name="prerequisites"></a>Zjistit předpoklady pro

- Prostředí VMware budete potřebovat k selhání zpět VMware VMs a fyzické servery. Zpět k fyzický server nepodporuje.
- Abyste mohli navrácení by měl nevytvoříte Azure sítě po počátečním nastavení ochrany. Navrácení musí připojení VPN nebo ExpressRoute z Azure sítě, ve kterém jsou umístěny na místní web Azure VMs.
- Požadovaná VMs selhání zpátky na jsou spravovány vCenter serveru, který budete muset Ujistěte se, máte potřebná oprávnění pro zjišťování VMs na serverech vCenter. [Přečtěte si další](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
- Pokud existují snímky na virtuálního počítače se nepovede reprotection. Můžete odstranit snímky nebo discích. 
- Než budete navrácení musíte vytvořit počet součásti:
    - **Vytvoření procesu serveru v Azure**. Toto je Azure OM, které budete potřebovat k vytváření a zachovat spuštění během navrácení. Po dokončení navrácení můžete odstranit počítač.
    - **Vytvořit hlavní cílový server**: server předlohy cílové umožňuje volání a přijímání navrácení data. Vytvoření místní server pro správu je nainstalovaný ve výchozím nastavení předlohy cílové server. Však v závislosti na objem selhalo zpět provozu potřeba vytvořit samostatné předlohy cílový server pro navrácení.
    - Pokud chcete vytvořit další předlohy cílový server spuštěna Linux, musíte nastavit OM Linux před instalací serveru předlohy cílové podle níže uvedeného popisu.
- Konfigurační server je potřeba místně po výběru navrácení. Během navrácení musí existovat virtuálního počítače v databázi konfigurace serveru, když nefunguje, které nelze navrácení byl úspěšný. Zajištění tedy trvat pravidelného zálohování plánované serveru. V případě havárie musíte ji obnovit se stejnou adresou IP tak, aby navrácení budou fungovat.

## <a name="set-up-the-process-server-in-azure"></a>Nastavení serveru obrázku v Azure

Budete potřebovat k instalaci serveru obrázku v Azure tak, aby Azure VMs můžete odeslat data zpátky do místního předlohy cílové serveru. 

1.  Na portálu obnovení webu > **Servery konfigurace** vyberte Přidat nový server obrázku.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)

2.  Zadejte název serveru obrázku a zadejte jméno a heslo, které budete používat pro přístup k OM Azure jako správce. V **Konfigurační Server** vyberte server pro správu místních a určení Azure sítě, ve kterém by měly být nasazeny server procesu. Je vhodné v síti, ve kterém jsou umístěny Azure VMs. Určení jedinečné IP adresu z vyberte podsítě a začněte nasazení.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

    Bude spuštěn úlohy nasazení serveru obrázku

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

    Po nasazení serveru obrázku v Azure přihlášení pomocí přihlašovacích údajů, které jste zadali. Při prvním přihlášení v dialogovém okně proces serveru se spustí. Zadejte IP adresu serveru pro správu místních a jeho heslo. Ponechejte výchozí port 443. Můžete nechat výchozí 9443 port dat replikace není-li toto nastavení změnili zvlášť když nastavíte místního serveru správy. 

    >[AZURE.NOTE] Server se nezobrazí ve skupinovém rámečku **Vlastnosti OM**. Pouze se zobrazuje na kartě **servery** na serveru správy, ke kterému je registrované. Může trvat zhruba 10 až 15 minut pro server proces zobrazovat.


## <a name="set-up-the-master-target-server-on-premises"></a>Hlavní cílové serveru místní nastavení

Hlavní cílový server obdrží navrácení data. Hlavní cílové server je automaticky instalován na serveru správy místních, ale pokud jste zpátky selhání velké množství dat může musíte nastavit další předlohy cílový server. Udělejte to takto:

>[AZURE.NOTE] Pokud chcete nainstalovat Linux předlohy cílový server, postupujte podle pokynů v následujícím postupu.

1. Pokud instalujete předlohy cílovém serveru v systému Windows, otevřete stránku rychlý Start z OM, na kterém při instalaci předlohy cílový server a instalace budou moct soubor stáhnout Průvodce Azure webu obnovení Unified nastavení.
2. Spuštění instalačního programu a v **než začnete** vyberte **Přidat další proces serverů rozšiřování nasazení**.
3. Dokončete Průvodce stejným způsobem, kdy jste [Nastavení serveru pro správu](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Na stránce **Konfigurace serveru podrobnosti** zadejte IP adresu serveru předlohy cílové a heslo pro přístup k OM.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Nastavení Linux OM jako hlavní cílový server
Nastavení serveru pro správu serverem předlohy cílové jako Linux OM, budete muset nainstalovat na Cent) 6.6 S minimálními operačním systému, načíst SCSI ID pro každého pevného záložní instalace některé další balíčky a použít vlastní změny.

#### <a name="install-centos-66"></a>Instalace CentOS 6.6

1.  Nainstalujte CentOS 6.6 minimální operační systém na serveru správy OM. Mějte ISO na disk DVD a spusťte systém. Přeskočte testování média, vyberte Angličtina (USA) na jazyk, vyberte možnost **Základního zařízení úložiště**, zkontrolujte, že pevném disku nemá mít důležitá data a klikněte na **Ano**, zahodit všechna data. Zadejte název hostitele serveru správy a vyberte server síťový adaptér.  V dialogovém okně **Úprava systému** zaškrtněte políčko** automaticky připojit** a přidejte statickou IP adresu, sítě a nastavení DNS. Zadejte časové pásmo a kořenové heslo pro přístup k serveru pro správu. 
2.  Po zobrazení výzvy požadovaný typ instalace libovolný text, vyberte **Vytvořit vlastní rozložení** jako oddíl. Po kliknutí na **Další** **Free** a klikněte na vytvořit. Vytvoření **/**, **/var/crash** a **/ domácí oddíly** s **FS typ:** **ext4**. Vytvoření partion vyměňovat jako **FS typ: vyměňovat**.
3.  Pokud stávající zařízení najdete, že se zobrazí zpráva s upozorněním. Klikněte na **Formát** k formátování jednotky s nastavením oddíl. Klikněte na tlačítko **psát změnit na disk** oddíl změny se projeví.
4.  Vyberte **instalovat spouštěcí zavaděč** > **Další** nainstalovat zavaděč spouštění na kořenový oddíl.
5.  Po dokončení instalace klikněte na **Restartovat**.


#### <a name="retrieve-the-scsi-ids"></a>Načtení identifikátory SCSI

1. Po instalaci načítejte SCSI ID pro každého pevného záložní v OM. Tento vypnout server pro správu OM proveďte ve vlastnostech OM v VMware klikněte pravým tlačítkem myši na položku OM > **Upravit nastavení** > **Možnosti**.
2. Vyberte možnost **Upřesnit** > **Obecné položky** a klikněte na **Konfigurační parametry**. Tuto možnost, budou de-active spuštěné v počítači. Pokud chcete, aby aktivní musí být vypnutí počítače.
3. Pokud řádku **disku. EnableUUID** existuje Přesvědčte se, zda je hodnota nastavena na **True** (malá a velká písmena). Pokud už je můžete zrušit a testovat příkazu SCSI uvnitř hostovaný operační systém po. 
4.  Pokud není řádek stávající klikněte na **Přidat řádek** – a přidejte hodnotu **True** . Nepoužívejte dvojitých uvozovek.

#### <a name="install-additional-packages"></a>Instalace dalších balíčků

Budete muset stáhnout a nainstalovat některé další balíčky. 

1.  Zkontrolujte, že hlavní cílový server je připojený k Internetu.
2.  Spusťte tento příkaz stáhnout a nainstalovat 15 balíčků z úložiště CentOS: **# yum instalace – y xfsprogs perl lsscsi rsync wget kexec nástroje**.
3.  Pokud počítačích zdroje máte ochrana používáte Linux programu Reiser nebo XFS souborů kořenové nebo spouštěcí zařízení, potom by měl stáhněte a nainstalujte další balíčky následujícím způsobem:

    - # <a name="cd-usrlocal"></a>/usr/local disk CD
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
    - # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>reiserfs modulu ot – ivh kmod reiserfs 0,0 1.el6.elrepo.x86_64.rpm-utils-3.6.21-1.el6.elrepo.x86_64.rpm
    - # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
    - # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>ot – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Vlastní změny

Provést následující změny se projeví vlastní poté, co jste dokončete po instalaci a nainstalovaný balení:

1.  Zkopírujte RHEL 6-64 Unified Agent binární bude v angličtině. Spusťte tento příkaz untar binární: **cílový – zxvf <file name> **
2.  Spusťte tento příkaz a udělte oprávnění: **# chmod 755./ApplyCustomChanges.sh**
3.  Následujícím způsobem: **#./ApplyCustomChanges.sh**. Skript by měla běžet jenom jednou. Restartujte server po skript funguje správně.


## <a name="run-the-failback"></a>Spuštění navrácení

### <a name="reprotect-the-azure-vms"></a>Reprotect Azure VMs

1.  Na portálu obnovení webu > karta **počítačích** vyberte OM, který je selhání a klikněte na **znovu chránit**.
2.  V **cílové předlohy** a **Proces Server** vyberte místního serveru předlohy cílové a server Azure OM procesu.
3.  Vyberte účet, který jste nakonfigurovali připojit se bude v angličtině.
4.  Vyberte navrácení verzi skupině zámek. Pro příkladu je-li OM zamknut v PG1 pak budete muset vyberte PG1_Failback.
5.  Pokud chcete obnovit do jiného umístění, vyberte jednotku uchovávání informací a nakonfigurován pro server předlohy cílového úložiště. Když vám nepovede zpátky do místního serveru VMware VMs v plánu ochrany navrácení použije stejném úložišti dat jako hlavní cílový server. Pokud chcete obnovit otevřené Azure OM do stejné místní OM OM místní by již nebude ve stejném úložišti dat jako hlavní cílový server. Pokud žádná OM místní novou vytvořen během reprotection.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)

6.  Po klepnutí na tlačítko **OK** zahájíte reprotection úlohy začne replikovat OM z Azure k místnímu webu. Na kartě **úlohy** můžete sledovat průběh.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Spusťte přepojení k místnímu webu

Po reprotection OM se přesune do navrácení verzi jeho ochranu skupiny a automaticky přidají i do obnovení plán, který jste použili pro přepnutí do Azure, pokud existuje.

1.  Na stránce **Obnovení plány jednotného zasílání zpráv** vyberte plán obnovení obsahující navrácení skupinu a klikněte na **převzetí** > **Neplánované převzetí**.
2.  V **Potvrďte převzetí** ověření převzetí směru (Azure) a vyberte obnovení bod, který chcete použít pro překlopení (nejnovější). Pokud povolíte **Více OM** při konfiguraci vlastností replikace můžete obnovit na nejnovější aplikaci nebo selhání konzistentní obnovení bodu. Zaškrtněte políčko Spustit záložní.
3.  Při selhání obnovení webu se vypne Azure VMs. Po zkontrolujte, že tento navrácení dokončil očekávaným způsobem se můžete můžete zkontrolovat, že Azure VMs vypnut podle očekávání.

### <a name="reprotect-the--on-premises-site"></a>Reprotect místních webů

Po dokončení navrácení dat bude zpátky na místních webů, ale nebude chráněn. Spusťte replikace Azure znova, postupujte takto:

1.  Na portálu obnovení webu > vyberte kartu **počítačích** VMs neúspěšných zpět a klikněte na **znovu chránit**. 
2.  Po ověření, že replikace na Azure funguje očekávaným způsobem, v Azure odstraníte VMs Azure (momentálně není spuštěný), které byly zpět se nepodařilo.


### <a name="common-issues-in-failback"></a>Běžné problémy s v navrácení

1. Pokud provést zjišťování vCenter uživatele jen pro čtení a ochrana virtuálních počítačích, je mu to a převzetí funguje. V době Reprotect se nepovede, protože datastores nelze zjistit. Jako příznakem neuvidíte datastores uvedené současně znovu chrání. Pro tento problém vyřešíte můžete aktualizovat pověření vCenter s příslušnou účtu, který má oprávnění a úlohy. [Další informace](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Když navrácení OM Linux a spusťte místní, zobrazí se, že balíček správce sítě odinstalovat z počítače. Je to proto OM bude obnoven v Azure, balíček správce sítě, odebere se.
3. Když OM nakonfigurovaný pomocí statické IP adresy, je se nepodařilo přes Azure IP adresu vzniká prostřednictvím DHCP. Když převzít zpět na místní OM nadále získání adresy IP pomocí DHCP. Je třeba ručně přihlášení do počítače a nastavte IP adresu zpátky na statický adresu případné potíže.
4. Pokud používáte ESXi 5.5 bezplatné edition nebo vSphere 6 hypervisoru bezplatné edition, převzetí by úspěšné, ale navrácení se nezdaří. Budete ned upgradovat na některý z licence k vyzkoušení Povolit navrácení.

## <a name="failing-back-with-expressroute"></a>Selhání pozadí s ExpressRoute

Může selhat zpět připojení prostřednictvím sítě VPN nebo Azure ExpressRoute. Pokud chcete používat poznámky ExpressRoute následující:

- ExpressRoute by měl nastavit na Azure virtuální sítě pro zdroj, který počítačích selhání myší a ve které Azure VMs nacházejí po selháním.
- Data replikovat na účet Azure úložiště ve veřejných koncového bodu. By měl nastavit veřejné prozkoumávání v ExpressRoute pomocí datového centra cílové replikace obnovení webu používat ExpressRoute.



