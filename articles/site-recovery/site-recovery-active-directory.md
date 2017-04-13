<properties
    pageTitle="Ochrana služby Active Directory a DNS s obnovení Azure webu | Microsoft Azure"
    description="Tento článek popisuje, jak implementovat řešení obnovení havárie pro použití obnovení webu Azure Active Directory."
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="08/31/2016"
    ms.author="pratshar"/>

# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Ochrana služby Active Directory a DNS s obnovení Azure webu

Podnikových aplikací například SharePoint, Dynamics AX a SAP, závisí na služby Active Directory a infrastruktury služby DNS, budou fungovat správně. Když vytvoříte řešení obnovení havárie aplikací, je důležité si pamatovat, že budete muset ochrana a obnovit Active Directory a DNS před další aplikace komponenty, zajistit, že věcí správně fungovat v případě havárie.

Obnovení webu je Azure služba, která zajišťuje havárie obnovení orchestrating replikace, převzetí a obnovení virtuálních počítačích. Obnovení webu podporuje celou řadu scénářů replikace konzistentní chránit a bezproblémové převzetí virtuálních počítačích a aplikací pro soukromé, veřejná nebo hostitele mračnech.

Použití webu obnovení, můžete vytvořit plán obnovení úplné automatické havárie služby Active Directory. Pokud dojde k přerušením, můžete zahájit přepojení vyvolané odkudkoli a vstoupit služby Active Directory zařídit i několik minut. Pokud jste nasazení služby Active Directory pro více aplikací, jako je SharePoint a SAP na webu primární a chcete převzít dokončení webu, může selhat prostřednictvím služby Active Directory nejdřív pomocí obnovení webu a převzít ostatní aplikace, které používají plány pro obnovení aplikace.

Tento článek vysvětluje, jak vytvořit řešení obnovení havárie služby Active Directory, jak provádět plánované a neplánované a test převzetí služeb při selhání pomocí plán obnovy jedním kliknutím, podporované konfigurace a požadavky.  Byste měli znát služby Active Directory a obnovení webu Azure před spuštěním.

Podle složitost prostředí dvěma doporučenými způsoby.

### <a name="option-1"></a>Možnost 1

Pokud máte malým počtem poštovních aplikací a jednoho řadiče domény a chcete převzít celého webu, potom doporučujeme používat obnovení webu replikace řadiče domény na vedlejší web (jestli jste jste selhání přes Azure nebo vedlejší webu). Stejné replikovanou virtuálního počítače se dá použít pro překlopení test příliš.

### <a name="option-2"></a>Možnost 2

Pokud máte velký počet aplikace a v prostředí existuje více než jednoho řadiče domény nebo pokud chcete převzít několik aplikací najednou, doporučujeme, aby kromě replikace domény řadiče domény virtuálního počítače s obnovení webu taky nastavíte dalšího řadiče domény na cílovém webu (Azure nebo vedlejší místního datacentra).

>[AZURE.NOTE] I v případě, že implementace možnost 2, při provádění přepojení test přesto musíte replikovat řadiče domény pomocí obnovení webu. Přečtěte si, [Otestujte převzetí aspektech](#considerations-for-test-failover) Další informace.


Následující části popisují, jak povolit ochranu řadiče domény v obnovení webu a jak nastavit řadiče domény v Azure.


## <a name="prerequisites"></a>Zjistit předpoklady pro

- Místního nasazení služby Active Directory a DNS server.
- Obnovení služby Azure webu trezoru v Microsoft Azure předplatné.
- Pokud jste replikace Azure spustit nástroj Azure virtuálního počítače připravenosti zhodnocení na VMs zajistit jsou kompatibilní se službou Azure VMs a obnovení služby Azure webu.


## <a name="enable-protection-using-site-recovery"></a>Povolte ochranu pomocí obnovení webu


### <a name="protect-the-virtual-machine"></a>Ochrana virtuálního počítače

Povolení ochrany domény řadiče domény a DNS virtuálního počítače v obnovení webu. Konfigurace nastavení obnovení webu v závislosti na typu virtuálního počítače (Hyper-V nebo VMware). Doporučujeme frekvenci pád konzistentní replikace 15 minut.

###<a name="configure-virtual-machine-network-settings"></a>Konfigurace nastavení sítě virtuálního počítače

Na počítači virtuální domény řadiče domény a DNS konfigurovat nastavení sítě v obnovení webu tak, aby OM budou připojena k správné sítě po překlopení. Například pokud jste replikace VMs Hyper-V Azure můžete vybrat OM v cloudu VMM nebo ve skupině zámek konfigurovat nastavení sítě, jak je ukázáno v následujícím příkladu

![Nastavení sítě OM](./media/site-recovery-active-directory/VM-Network-Settings.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Ochrana služby Active Directory s replikace služby Active Directory

### <a name="site-to-site-protection"></a>Ochrana webu na webu

Vytvoření řadiče domény na vedlejší webu a zadejte název doméně, která používá na primární webu převedete serveru a role řadiče domény. Můžete modulu snap-in **sítě a Active Directory Services** ke konfiguraci nastavení na odkaz objektu webu do kterého se přidají weby. Nakonfigurováním nastavení na odkaz webů můžete určit, kdy replikace mezi dvěma nebo více webů a jak často. Další informace najdete v článku [Plánování replikace mezi weby](https://technet.microsoft.com/library/cc731862.aspx) .

###<a name="site-to-azure-protection"></a>Ochrana webu Azure

Postupujte podle pokynů k [Vytvoření řadiče domény v Azure virtuální sítě](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Když převedete serveru a role řadiče domény určete stejný název domény, který se používá na primární webu.

Potom [Nakonfigurujte DNS server pro virtuální sítě](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), používat DNS server Azure.

![Azure sítě](./media/site-recovery-active-directory/azure-network.png)

## <a name="test-failover-considerations"></a>Důležité informace o převzetí test

Test selháním v síti, která obsahuje samostatný z výrobní sítě tak, aby žádný vliv na výrobní úloh.

Většina vyžadovat také aplikace přítomnosti řadiče domény a DNS server budou fungovat, abyste předtím, než selhání aplikace řadiče domény potřebuje vytvořit v izolovaná síť pro překlopení test. Nejjednodušší způsob je zamknout virtuální počítače domény řadiče domény a DNS s obnovení webu a spusťte test přepojení virtuálního počítače, před spuštěním selhání testovací plán pro obnovení aplikace. Tady je, můžete to udělat takto:

1. Povolení ochrany v obnovení webu domény řadiče domény a DNS virtuálního počítače.
2. Vytvoření izolovaná síť. Všechny virtuální síť vytvořené v Azure ve výchozím nastavení je izolovat od jiných sítích. Doporučujeme, abyste je stejné jako u sítě rozsah IP adres pro této sítě. Není povolit připojení k webu v této síti.
3. Zadání adresy DNS IP v síti vytvořili, jako na IP adresu, který očekáváte virtuálního počítače DNS získat. Pokud jste replikace na Azure, zadejte IP adresu pro OM, která se použije na převzetí v **Cílové IP** nastavení v dialogovém okně Vlastnosti OM. Pokud jste replikace do jiného místních webů a používáte DHCP postupujte podle pokynů k [Nastavení DNS a DHCP pro překlopení test](site-recovery-failover.md#prepare-dhcp)

>[AZURE.NOTE] Na IP adresu přiřazených virtuálního počítače při selhání test je shodný s IP adresu, který byste dostali během plánované nebo neplánované selhání, pokud je k dispozici v síti převzetí test na IP adresu. V opačném případě virtuální počítač obdrží různých IP adresu, která je dostupná v síti převzetí testu.

4. V počítači domény řadiče domény virtuální spusťte test selhání ho v izolovaná síť. Pomocí nejnovější dostupnou aplikaci konzistentní obnovení místa virtuálního počítače řadiče domény můžete provádět test překlopení. 
5. Spusťte test přepojení pro plán pro obnovení aplikace.
6. Po dokončení testování označte testovací převzetí úlohy domény řadiče domény virtuálního počítače a obnovení plánu "Hotovo" na kartě **úlohy** na portálu obnovení webu.

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS a domény řadiče domény na různých počítačích

Pokud není DNS na stejném počítači virtuální jako řadiče domény musíte vytvořit OM DNS pro překlopení test. Pokud jsou umístěné na stejné OM, můžete tuto část přeskočit.

Můžete použít nové DNS server a vytvořte požadované zóny. Například pokud je vaše doména služby Active Directory contoso.com, můžete vytvořit zóny DNS s názvem contoso.com. Položky odpovídající ke službě Active Directory musí být aktualizovány v systému DNS, následujícím způsobem:

1. Zajistěte, aby že tato nastavení jsou na místě, než ostatní virtuálního počítače v plánu obnovení se zobrazí:

    - Po název kořenové doménové musí mít název zóny.
    - Zóny musí být záložní soubor.
    - Zóny musí být povolené na aktualizace zabezpečení a úrovně zabezpečení.
    - Překládání domény řadiče domény virtuálního počítače odkazovat na IP adresu počítače virtuální DNS.

2. V počítači virtuální řadiče domény, spusťte tento příkaz:

    `nltest /dsregdns`

3. Přidání zóny na serveru DNS, povolte nezabezpečenou aktualizace a přidejte položku ho na server DNS:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1


## <a name="next-steps"></a>Další kroky

Čtení [Jaké pracovního vytížení je můžete chránit?](../site-recovery/site-recovery-workload.md) zobrazíte další informace o ochraně pracovního vytížení organizace s obnovení webu Azure.
