<properties
    pageTitle="Spustit nástroj pro Hyper-V kapacita Plánovač pro obnovení webu | Microsoft Azure"
    description="Tento článek obsahuje pokyny k nástroji Hyper-V kapacita Plánovač pro obnovení webu Azure"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/12/2016"
    ms.author="raynew" />

# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>Spustit nástroj pro Hyper-V kapacita Plánovač pro obnovení webu

Jako součást obnovení webu Azure nasazení musíte zjistit replikace a požadavcích na šířku pásma. Nástroj pro Hyper-V kapacita Plánovač pro obnovení webu umožňuje zjistit vašim požadavkům replikace a šířky pásma pro Hyper-V virtuální počítačů.


Tento článek popisuje, jak spustit nástroj pro Hyper-V kapacita plánování. Tento nástroj by měl použít společně s dalšími nástroje pro plánování kapacity a informace podle [Kapacita plánování obnovení webu](site-recovery-capacity-planner.md).


## <a name="before-you-start"></a>Než začnete

Spusťte nástroj na uzel Hyper-V serveru nebo obrázku na webu primární. Spuštění nástroje musí servery Hyper-V Host (hostitel):

- Operační systém: Windows Server® 2012 nebo Windows Server® 2012 R2
- Paměť: 20 MB (minimálně)
- Procesor: 5 procent režijních (minimálně)
- Místo na disku: 5 MB (minimálně)

Před spuštěním nástroje musíte připravit primární webu. Pokud jste replikace mezi dva místních webů a chcete zkontrolovat šířkou pásma, budete muset Příprava serveru otevřené.


## <a name="step-1-prepare-the-primary-site"></a>Krok 1: Příprava primární webu
1. Na stránce primární vytvořte seznam všech Hyper-V virtuálních počítačích, které chcete replikovat a Hyper-V hosts/clusterů ve kterých jsou umístěny. Nástroj můžete spustit pokaždé, když pro víc samostatných hostitelů, nebo pro jednoho obrázku, ale ne oba společně. Také potřebuje, samostatně pro každý operační systém, abyste měli shromažďovat a poznamenejte si serverech Hyper-V následujícím způsobem:

  - Servery samostatného systému Windows Server® 2012
  - Windows Server® 2012 clusterů
  - Windows Server® 2012 R2 samostatného serverů
  - Windows Server® 2012 R2 clusterů

3. Povolení vzdáleného přístupu ke službě WMI na všem hostitelé Hyper-V a clusterů. Spusťte tento příkaz každý serveru/clusteru, abyste měli jistotu pravidla brány firewall a nastavení oprávnění:

        netsh firewall set service RemoteAdmin enable

5. Povolení sledování výkonu na serverech a clusterů, následujícím způsobem:

  - Otevření brána Windows Firewall s **Pokročilým zabezpečením** modulu snap-in a pak ji povolit následující příchozí pravidla: **COM + přístup k síti (DCOM-IN)** a všechna pravidla **Vzdálená správa protokolu událostí skupiny**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Krok 2: Příprava otevřené serveru (místní replikace místní)

Nemusíte udělat, pokud jste replikace na Azure.

Doporučujeme že nastavíte jednoho hostitele Hyper-V jako obnovení server tak, aby formální OM replikovat do něj zkontrolovat šířky pásma.  Můžete přejít to ale nebude moct změřit šířky pásma, pokud to udělat.

1. Pokud chcete použít clusteru otevřené můžete nakonfigurovat pro Hyper-V otevřené zprostředkovatele:

    - V okně **Správce serveru**otevřete **Správce**.
    - Připojte se k němu, vyberte název obrázku a klikněte na **Akce** > **Konfigurace Role** otevřete Průvodce dostupnost.
    - **Vyberte** roli klikněte na **Hyper-V otevřené zprostředkovatele**. V průvodci zadejte **název NetBIOS** a **IP adresu** má být použit jako spojovací bod clusteru (nazývané přístupový bod klienta). **Zprostředkovatel Hyper-V otevřené** nakonfigurujete, výsledkem klientského přístupu názvu, který je třeba si uvědomit.
    - Ověřte, zda roli Hyper-V otevřené zprostředkovatele úspěšně je online a můžete převzít mezi všemi uzly clusteru. K tomuto účelu klikněte pravým tlačítkem myši roli, přejděte na příkaz **Přesunout**a potom klikněte na **Uzel vyberte**. Vyberte uzel > **OK**.
    - Pokud používáte ověřování na základě certifikát, zkontrolujte jednotlivých uzlech obrázku a klientovi přístupovému bodu, že mají nainstalován certifikát.
2.  Povolení otevřené serveru:

    - Clusteru otevřete Správce clusteru selhání, připojte se k němu a klikněte na položku **role** > Vybrat roli > Nastavení služby **Replikace**> **Povolit tomuto clusteru jako otevřené server**. Všimněte si, že pokud používáte clusteru jako otevřené musíte mají roli Hyper-V otevřené zprostředkovatele prezentovat clusteru na primární webu.
    - Samostatný server otevřete Hyper-V správce. V podokně **akcí** klikněte na **Nastavení pro Hyper-V** pro server, ke kterému chcete povolit a v **Konfiguraci replikace** klikněte na **Povolit tento počítač jako otevřené server**.
3. Nastavení ověřování:

    - **Porty ověřování a** vyberte způsob ověření primární serveru a porty ověřování. Pokud používáte certifikát klikněte na **Vybrat certifikát** , který chcete vybírat. Protokol Kerberos použijte, když Hyper-V hosts primární a obnovení ve stejné domény nebo důvěryhodná. Použití certifikáty pro různé domény nebo pracovní skupiny nasazení.
    - **Autorizace a úložiště** v části povolte **všechny** ověřené (primární) server odešlete data replikace tento server otevřené. Klikněte na **OK** nebo **použít**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - Spuštění, můžete zkontrolovat, jestli je spuštěný posluchače protokol/číslo portu, které jste zadali **netsh http zobrazit servicestate** :  
4. Nastavení brány firewall. Během instalace Hyper-V pravidla brány firewall vzniká povolit přenos na výchozí porty (HTTPS na 443, pomocí protokolu Kerberos na 80). Povolení následujícími pravidly následujícím způsobem:

        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## <a name="step-3-run-the-capacity-planner-tool"></a>Krok 3: Spusťte nástroj Plánovač kapacity

Když jste počítat primární webů a nastavení serveru pro obnovení můžete spustit nástroj.

1. [Stáhněte si](https://www.microsoft.com/download/details.aspx?id=39057) nástroj z Microsoft Download Center.
2. Spuštění nástroje z jednoho z primární servery (nebo jiné uzlů z primární obrázku). Klikněte pravým tlačítkem myši souboru .exe a klikněte na **Spustit jako správce**.
3. V **než začnete** určete jak dlouho chcete shromáždit data. Doporučujeme že spustit nástroj během pracovní doby a ujistěte se, že data je zástupce. Pokud zkoušíte pouze ověřte připojení k síti, můžete shromažďovat parametru minute pouze.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. V **Podrobné informace o primární webu** pro samostatné hostitele zadat název serveru nebo plně kvalifikovaný název domény, nebo pro clusteru zadejte plně kvalifikovaný název domény klienta přijmout bod, název clusteru nebo libovolný uzel clusteru a klikněte na **Další**. Nástroj automaticky zjistí název serveru, ke kterému je spuštěna. Nástroj vyzvedne VMs, které můžete sledovat pro zadané servery.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. V **Otevřené podrobné informace o webu** vyberte Pokud jste replikace na Azure nebo jste replikace na vedlejší datacentra a nenastavili serveru otevřené, **Přeskočit testů týkající se otevřené webu**. Pokud jsou replikace na vedlejší datacentra a nastavili jste typu otevřené v FQDN samostatný server nebo přístupový bod klienta pro cluster v **název serveru (nebo) Hyper-V otevřené zprostředkovatele Zakončení**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. V **Rozšířené podrobností otevřené** povolte **Přeskočit testů týkající se rozšířené otevřené webu**. Nejsou podporovány obnovení webu.
7. V **Vyberte VMs replikace** nástrojů se připojí k serveru nebo obrázku zobrazí VMs a disků na primární serveru, v souladu s nastavení zadaná na stránce **Podrobnosti primární webu** . Všimněte si, že se nepoužívá VMs, kteří mají povolenou už replikace nebo, která se nezobrazí. Vyberte VMs, pro která chcete shromažďovat metriky. Výběr VHD automaticky shromažďuje data pro VMs příliš.
9. Pokud jste nakonfigurovali otevřené serveru nebo clusteru, v **síti informace** zadejte přibližnou sítě WAN pásma, které by podle vás bude použita mezi weby primární a otevřené a vyberte certifikáty, pokud jste nakonfigurovali ověřovací certifikát.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. V **souhrnu** zkontrolujte nastavení a klikněte na **Další** začněte shromažďování metriky. Nástroj průběhu a stavu se zobrazí na stránce **Výpočet kapacity** . Když nástroj dokončí spuštění klikněte na tlačítko **Zobrazit sestavu** přejdete myší výstup. Ve výchozím nastavení sestavy a protokoly jsou uloženy v **%systemdrive%\Users\Public\Documents\Capacity plánování**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## <a name="step-4-interpret-the-results"></a>Krok 4: Interpretaci výsledků
Tady je důležité metriky. Můžete ignorovat metriky, které nejsou uvedené v tomto poli. Nejsou důležité pro obnovení webu.

### <a name="on-premises-to-on-premises-replication"></a>Místní replikace místní
  - Vliv replikace na primární hostiteli využití paměti
  - Vliv replikace na primárním, obnovení hosts úložiště na disku, procesorů
  - Celkové šířky pásma povinné delta replikace (MB /)
  - Pozorovaná šířka pásma mezi hostiteli primární a obnovení hostitele (MB /)
  - Návrh ideální počet aktivní paralelní přenosů mezi dvěma tabulkami hosts a clusterů

### <a name="on-premises-to-azure-replication"></a>Místní Azure replikace
  - Vliv replikace na primární hostiteli využití paměti
  - Vliv replikace primárního hostitele úložiště místa na disku, procesorů
  - Celkové šířky pásma povinné delta replikace (MB /)

## <a name="more-resources"></a>Další materiály

- Podrobné informace o nástroji pro čtení dokumentu, který doprovází ke stažení nástroje.
- Podívejte se na návod nástroje na Dryml Mayer [TechNet blogu](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
- [Výsledků dosáhnete](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) naše výkonu testování místním systémem replikace místní Hyper-V



## <a name="next-steps"></a>Další kroky

Po dokončení plánování kapacity vyjít nasazení obnovení webu:

- [Replikace Hyper-V VMs v VMM mračnech na Azure](site-recovery-vmm-to-azure.md)
- [Replikace Hyper-V VMs (bez VMM) na Azure](site-recovery-hyper-v-site-to-azure.md)
- [Mezi weby VMM replikovat VMs Hyper-V](site-recovery-vmm-to-vmm.md)
- [Mezi weby VMM s SAN replikovat VMs Hyper-V](site-recovery-vmm-san.md)
- [Replikovat hyper-V VMs na jednom VMM serveru](site-recovery-single-vmm.md)