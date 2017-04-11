<properties
    pageTitle="Nasazení internetového Vyrovnávání zatížení řešení se IPv6 použít šablonu | Microsoft Azure"
    description="Postup pro nasazení podpora protokolu IPv6 u Azure nástroj pro vyrovnávání zatížení a vyrovnávání zatížení VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="protokol IPv6 Vyrovnávání zatížení azure, duální zásobníku, veřejnou ip, nativní protokol ipv6, mobilní telefon, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="deploy-an-internet-facing-load-balancer-solution-with-ipv6-using-a-template"></a>Nasazení řešení ve Vyrovnávání zatížení internetového se IPv6 použít šablonu?

> [AZURE.SELECTOR]
- [Prostředí PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure rozhraní příkazového řádku](./load-balancer-ipv6-internet-cli.md)
- [Šablony](./load-balancer-ipv6-internet-template.md)

Vyrovnávání zatížení Azure je Vyrovnávání zatížení vrstvy-4 při instalaci (TCP, UDP). Vyrovnávání zatížení zajišťuje dostupnost distribuce příchozí komunikaci mezi správný služby instancí služby cloudu nebo virtuálních počítačích v sadě Vyrovnávání zatížení. Azure Vyrovnávání zatížení můžete zobrazit tyto služby ve více portů a více IP adres.

## <a name="example-deployment-scenario"></a>Příklad scénáře

Následující obrázek znázorňuje řešení vyrovnávání zatížení nasazení pomocí šablony příklad popisované v tomto článku.

![Scénář Vyrovnávání zatížení](./media/load-balancer-ipv6-internet-template/lb-ipv6-scenario.png)

V tomto scénáři vytvoří následující Azure zdroje:

- virtuální sítě rozhraní pro každou OM s IPv4 a IPv6 adresa přiražená
- Vyrovnávání zatížení internetového IPv4 a IPv6 veřejnou IP adresu
- dvě načíst vyrovnávání pravidla přiřadit veřejné virtuální privátní koncové body
- Nastavení dostupnost, obsahující dvě VMs
- dva virtuálních počítačích (VMs)

## <a name="deploying-the-template-using-the-azure-portal"></a>Nasazení šablony pomocí portálu Azure

Tento článek odkazuje na šablonu, která je publikovaných v galerii [Šablon rychlý úvod Azure](https://azure.microsoft.com/documentation/templates/201-load-balancer-ipv6-create/) . Můžete stáhnout šablonu z Galerie nebo spuštění zavedení v Azure přímo z galerie. Tento článek předpokládá, že jste stáhli šablony na místním počítači.

1. Otevřete Azure portálem a přihlaste se pomocí účtu, který má oprávnění k vytváření VMs a sociální sítě zdroje v Azure předplatného. Pokud používáte existujících zdrojů, účet musí oprávnění k vytvoření skupiny zdrojů a účet úložiště.

2. Klikněte na "+ nový" z nabídky a typ "šablony" do vyhledávacího pole. Ve výsledcích hledání vyberte "Nasazení šablony".

    ![LB ipv6 portál kroku2](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step2.png)

3. V všechno, co zásuvné, klikněte na "Šablony nasazení."

    ![LB-ipv6-portál-ke kroku 3](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step3.png)

4. Klikněte na "Vytvořte."

    ![LB ipv6 portál step4](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step4.png)

5. Klikněte na "Šablonu upravovat přímo." Odstraňte existující obsah a zkopírovat a vložit celý obsah soubor šablony (Pokud chcete zahrnout zahájení a ukončení {}) a potom klikněte na "Uložit".

    > [AZURE.NOTE] Pokud používáte Microsoft Internet Explorer, při vložení se zobrazí dialogové okno s žádostí o povolení přístupu do schránky systému Windows. Klikněte na možnost "Povolit přístup."

    ![LB ipv6 portál step5](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step5.png)

6. Klikněte na "Upravit parametry." V zásuvné parametry zadejte hodnoty podle návodu v části šablona parametry a potom na "Uložit" ukončíte zásuvné parametry. V zásuvné nasazení vlastní vyberte předplatné, existující skupiny zdrojů nebo ho vytvořit. Pokud vytváříte skupina zdroje, vyberte umístění pro skupiny zdrojů. Pak **právní podmínky**klepněte **nákupní** právní podmínky. Nasazení zdrojů, nebude zahájen Azure. Trvá několik minut nasazení všechny zdroje.

    ![LB ipv6 portál step6](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step6.png)

    Další informace o těchto parametrů naleznete v části [šablony parametry a proměnné](#template-parameters-and-variables) dál v tomto článku.

7. Najdete v tématech vytvořené pomocí šablony, klikněte na Procházet, pomocí posuvníku dolů v seznamu zobrazit "Zdroje skupiny" a potom klikněte na něj.

    ![LB ipv6 portál step7](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step7.png)

8. Na zásuvné skupiny zdrojů klikněte na název skupiny zdrojů, které jste zadali v kroku 6. Zobrazí se seznam všech zdrojů, která byla nasazená. Pokud všechny se dobře, je vhodné zadat "Byl úspěšný" v části "Poslední nasazení." Pokud ne, zkontrolujte, že účet, který používáte nainstalovaný oprávnění k vytváření potřebné zdroje.

    ![LB ipv6 portál step8](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step8.png)

    > [AZURE.NOTE] Procházet skupiny zdrojů bezprostředně po dokončení kroku 6 "Poslední nasazení" zobrazí stav "Zavedení" při zavedení zdroje.

9. Klikněte na "myIPv6PublicIP" v seznamu zdrojů. Vidíte, že má adresy IPv6 IP adresa a jeho název DNS je hodnota, kterou jste zadali pro parametr dnsNameforIPv6LbIP v kroku 6. Je zdroj veřejné IPv6 adresy a hostitele název, který bude přístupný pro klienty sítě Internet.

    ![LB ipv6 portál step9](./media/load-balancer-ipv6-internet-template/lb-ipv6-portal-step9.png)

## <a name="validate-connectivity"></a>Ověřte připojení

Po úspěšném nasazení šablony můžete ověřit připojení dokončení tyto věci:

1. Přihlaste se k portálu Azure a připojte k jednotlivým VMs vytvořil nasazení šablony. Pokud jste nainstalovali systému Windows Server OM, spusťte ipconfig/všechny z příkazového řádku. Uvidíte, že VMs adres protokolu IPv4 a IPv6. Nasazení Linux VMs potřebujete konfigurovat OS Linux přijímat dynamické adresy IPv6 pro pomocí pokynů pro rozdělení Linux.
2. Z připojeného k Internetu IPv6 klienta spusťte připojení k veřejné IPv6 address Vyrovnávání zatížení. Abyste si ověřili, že vyrovnávání zatížení vyrovnávání mezi dvěma VMs, můžete nainstalovat na webový server jako internetové informační služby (IIS) ve všech VMs. Výchozí webové stránky na všech serverech může obsahovat text "Server0" nebo "Server1" jednoznačně identifikovat. Potom otevřete internetového prohlížeče na připojeného k Internetu IPv6 klienta a přejděte do název hostitele zadané pro parametr dnsNameforIPv6LbIP Vyrovnávání zatížení potvrďte připojení pomocí protokolu IPv6 začátku do konce každý v angličtině. Pokud se zobrazí jenom na webovou stránku z pouze jeden server, budete muset vymazat mezipaměť prohlížeče. Otevřete víc soukromé relace prohlížeče. Měli byste vidět odpověď z každé serveru.
3. Z připojeného k Internetu IPv4 klienta spusťte připojení k veřejné adresy IPv4 Vyrovnávání zatížení. Potvrďte, že je Vyrovnávání zatížení dvou VMs Vyrovnávání zatížení může otestujte pomocí služby IIS, jak je uvedeno v kroku 2.
4. Od každého OM zahajte odchozí připojení na zařízení s IPv6 nebo připojení IPv4 Internet. V obou případech je zdroj IP prezentací, které zařízení cíl veřejné adresy IPv4 nebo IPv6 Vyrovnávání zatížení.

>[AZURE.NOTE]
ICMP pro IPv4 a IPv6 blokován v Azure sítě. V důsledku toho ICMP nástroje jako ping vždy fungovat. Pokud chcete testovat připojení, použijte TCP alternativní TCPing ATP rutiny prostředí PowerShell Test-NetConnection. Všimněte si, že IP adresy v diagramu příklady hodnot, které se můžou objevit. Protože adresy IPv6 přiřazené dynamicky, adresy, které dostanete se budou lišit a se může lišit podle oblasti. Je také běžné veřejné adresy IPv6 na Vyrovnávání zatížení budete chtít začít předponu jiné než soukromé adresy IPv6 ve fondu back-end.

## <a name="template-parameters-and-variables"></a>Parametry šablony a proměnných

Správce prostředků Azure šablona obsahuje více proměnných a parametrů, které si můžete přizpůsobit a vašim potřebám. Proměnné se používají pro pevnými hodnotami, které nechcete uživateli změnu. Parametry používáme pro hodnoty, že chcete uživatelům poskytnout při nasazení šablony. Příklad šablony nakonfigurovaný pro scénář popisované v tomto článku. To si můžete přizpůsobit potřebám prostředí.

Příklad šablony použitých v tomto článku obsahuje následující proměnné a parametry:

| Parametr / proměnná | Poznámky |
|-----------|-------|
| adminUsername | Zadejte název účtu správce použitá k přihlášení k virtuálních počítačích s. |
| adminPassword | Zadejte heslo k účtu správce použitá k přihlášení k virtuálních počítačích s. |
| dnsNameforIPv4LbIP | Zadejte název hostitele DNS, který chcete označit jako veřejný název Vyrovnávání zatížení. Tento název se změní veřejné adresy IPv4 Vyrovnávání zatížení. Název musí být malá písmena a odpovídat regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. |
| dnsNameforIPv6LbIP | Zadejte název hostitele DNS, který chcete označit jako veřejný název Vyrovnávání zatížení. Tento název se změní Vyrovnávání zatížení veřejné IPv6 address. Název musí být malá písmena a odpovídat regex: ^ [a-z][a-z0-9-]{1,61}[a-z0-9]$. To je stejný název jako adres protokolu IPv4. Pokud klient odešle DNS dotazu pro tento název, Azure vrátí A a AAAA záznamů, kdy se budou sdílet název. |
| vmNamePrefix | Zadejte název předponu OM. Šablona připojí číslo (0, 1, atd.) na název při vytvoření VMs. |
| nicNamePrefix | Zadejte název předponu rozhraní sítě. Šablona připojí číslo (0, 1, atd.) na název při vytvoření rozhraní sítě. |
| storageAccountName | Zadejte název existujícího účtu úložiště nebo zadejte název novou vytvořené pomocí šablony. |
| availabilitySetName | Potom zadejte název dostupnost nastavení pro použití se VMs |
| addressPrefix | Předpona adresa použitá k definování rozsahu adres virtuální sítě |
| subnetName | Název podsítě v vytvoří VNet |
| subnetPrefix | Slouží k definování rozsahu adres podsítě předponu adresy |
| vnetName | Zadejte název pro VNet používaný VMs. |
| ipv4PrivateIPAddressType | Pole přidělení metodou pro soukromé IP adresu (statického nebo dynamického) |
| ipv6PrivateIPAddressType | Metoda přidělení použitá pro soukromou IP adresu (dynamické). Dynamické přidělení pouze podporuje protokol IPv6. |
| numberOfInstances | Počet instancí rozloženy nasazených pomocí šablony |
| ipv4PublicIPAddressName | Zadejte název DNS, který chcete použít ke komunikaci s veřejné adresy IPv4 Vyrovnávání zatížení. |
| ipv4PublicIPAddressType | Pole přidělení metodou na veřejnou IP adresu (statického nebo dynamického) |
| Ipv6PublicIPAddressName | Zadejte název DNS, který chcete použít ke komunikaci s veřejné adresy IPv6 Vyrovnávání zatížení. |
| ipv6PublicIPAddressType | Metoda přidělení použitá na veřejnou IP adresu (dynamické). Dynamické přidělení pouze podporuje protokol IPv6. |
| lbName | Zadejte název Vyrovnávání zatížení. Tento název je zobrazena v portálu nebo při odkazu do ní pomocí příkazu rozhraní příkazového řádku nebo Powershellu. |

Zbývající proměnných v šabloně obsahovat odvozené hodnoty, které jsou přiřazené Azure vytvoří zdroje. Neměňte těchto proměnných.
