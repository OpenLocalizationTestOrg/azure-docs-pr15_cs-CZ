<properties 
   pageTitle="Vytvoření zóny DNS a záznam sady v Azure DNS pomocí .NET SDK | Microsoft Azure" 
   description="Jak se dá vytvořit zóny DNS a sady záznamů u Azure DNS panelů .NET SDK." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Vytvoření zóny DNS a sady záznamů pomocí .NET SDK

Můžete automatizovat operace vytvoření, odstranění nebo aktualizace DNS zones sady záznamů a záznamů pomocí DNS SDK s knihovnou správy DNS .NET. Úplné projekt aplikace Visual Studio je k dispozici [zde.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Vytvořit účet služby základní

Obvykle programové přístup k Azure zdrojů, která přes účet vyhrazené místo vlastní přihlašovací údaje uživatele pro. Tyto vyhrazené účty se označují jako účty služby základní. Používat project ukázkové Azure DNS SDK, musíte nejdřív vytvořit účet služby základní a přiřadit ji správná oprávnění.

1. Postupujte podle [těchto pokynů](../resource-group-authenticate-service-principal.md) a vytvořte účet služby základní (ukázkový projekt Azure DNS SDK předpokládá ověřování hesla.)

2. Vytvoření skupiny prostředků ([to jak](../azure-portal/resource-group-portal.md)).

3. Umožňuje Azure RBAC oprávnění účet služby základní "DNS Zone přispěvatelů" k skupina zdroje ([to jak](../active-directory/role-based-access-control-configure.md).)

4. Používáte-li ukázkový projekt Azure DNS SDK, upravte soubor "program.cs" následujícím způsobem:
    * Vložení správné hodnoty tenantId, clientId (označovaná taky jako ID účtu), heslo, služby základní účtu a subscriptionId jako našli v kroku 1.
    * Zadejte název skupiny zdroje vybrané v kroku 2.
    * Zadejte název zóny DNS podle svého výběru.

## <a name="nuget-packages-and-namespace-declarations"></a>Balíčky NuGet a deklarace oboru názvů

Použít Azure DNS .NET SDK, musíte nainstalovat balíček NuGet **Knihovnou správy DNS Azure** a další požadované Azure balíčky.
 
1. Ve **Visual Studiu**otevřete projekt nebo nový projekt. 

2. Přejděte na **Nástroje** **>** **Správce NuGet balíčků** **>** **Správa NuGet balíčků... řešení**. 

3. Klikněte na tlačítko **Procházet**, **Zahrnout zkušební** zaškrtávací políčko Povolit a do vyhledávacího pole zadejte **Microsoft.Azure.Management.Dns** .

4. Vyberte balíček a na tlačítko **nainstalovat** ho přidat do aplikace Visual Studio projektu.
 
5. Opakujte výše uvedené taky nainstalovat následující balíčky proces: **Microsoft.Rest.ClientRuntime.Azure.Authentication** a **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Přidání deklarace oboru názvů

Přidejte následující deklarace oboru názvů

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>Inicializace klient správy DNS

*DnsManagementClient* obsahuje metody a vlastnosti potřebné pro správu zóny DNS a sady záznamů.  Následující kód přihlášení k účtu služby základní a vytvoří DnsManagementClient objekt.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Vytvoření nebo aktualizace zóny DNS

Pokud chcete vytvořit zóny DNS, nejdřív "Zóna" je vytvořen objekt obsahovat parametry zóny DNS. Protože zóny DNS nejsou propojeny do konkrétní oblasti, umístění nastavenou "globální". V tomto příkladu [Správce prostředků Azure "značku"](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) taky přidá do zóny.

Ve skutečnosti vytvoření nebo aktualizace zóny Azure DNS, zóny objekt obsahující parametry zóny předána metodu *DnsManagementClient.Zones.CreateOrUpdateAsyc* .

>[AZURE.NOTE] DnsManagementClient podporuje tři způsoby operace: synchronní ("CreateOrUpdate"), asynchronní ("CreateOrUpdateAsync"), nebo asynchronní s přístupem k odpovědi na HTTP (CreateOrUpdateWithHttpMessagesAsync).  Můžete vybrat libovolný z těchto režimů podle vašich potřeb aplikace.

Azure DNS podporuje optimistické souběžné s názvem [Etags](dns-getstarted-create-dnszone.md). V tomto příkladu určující "*" záhlaví "If-žádné časově porovnat" zobrazuje Azure DNS vytvořit zóny DNS, pokud už neexistuje.  Hovor se nezdaří, pokud zóna se zadaným názvem už existuje ve skupině daného zdroje.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>Vytvoření sady záznamů DNS a záznamy

Jako sada záznamů správy záznamů DNS. Sada záznamů je sada záznamů se stejným názvem a typ záznamu v rámci zóny.  Sada záznamů název je relativní název zóny, nejsou plně kvalifikovaný název DNS.

Vytvoření nebo aktualizace záznamu nastavit, "Záznamů" vytvořené a předán *DnsManagementClient.RecordSets.CreateOrUpdateAsync*parametry objektu. S zóny DNS jsou tři způsoby operace: synchronní ("CreateOrUpdate"), asynchronní ("CreateOrUpdateAsync"), nebo asynchronní s přístupem k odpovědi na HTTP (CreateOrUpdateWithHttpMessagesAsync).

Stejně jako u zóny DNS na sady záznamů samostatných zahrnují podporu optimistické souběžné.  V tomto příkladu protože byla vyplněná "If POZVYHLEDAT" ani "If-žádné časově POZVYHLEDAT", sady záznamů se vždy vytvoří.  Volání přepíše všechny existující sady záznamů se stejným názvem a typ záznamu v této zóně DNS.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Získání zón a sady záznamů

Metody *DnsManagementClient.Zones.Get* a *DnsManagementClient.RecordSets.Get* načíst jednotlivé zón a sady záznamů. Sady záznamů jsou označeny jejich typ, název a skupina zóny a zdroje, které jsou v. Zóny jsou označeny jeho jméno a skupina zdroje, které jsou v.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Aktualizace stávající sada záznamů

Pokud chcete aktualizovat existující sady záznamů DNS, první načtení sady záznamů a potom aktualizaci obsahu sada záznamů, odešlete změnit.  V tomto příkladu jsme zadejte "Etag" ze sady načtený záznam v parametru "If POZVYHLEDAT". Hovor se nezdaří, pokud operace souběžné změnil mezitím sady záznamů.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Seznam zón a sady záznamů

Seznam zóny, použijte metody *DnsManagementClient.Zones.List...* , které podporují výpis všech pásma v dané skupiny prostředků nebo všechny zóny v dané Azure předplatného (v rámci skupiny zdrojů.) Seznam sady záznamů, použijte *DnsManagementClient.RecordSets.List...* metody, které podporují výpis všech sady záznamů v dané oblasti nebo jenom ty sady záznamů určitého typu.

Poznámka: při výpisu zón a sady záznamů, které výsledky mohou být čísla stránek vložena.  Následující příklad ukazuje, jak iteraci stránky výsledků. (Velikost uměle malé stránky "2" slouží k vynucení stránkování; v praxi by měl být tento parametr vynechán a použít výchozí velikost stránky)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Další kroky

Stáhněte si [Azure DNS .NET SDK ukázkový projekt](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), který obsahuje další příklady použití .NET SDK Azure DNS, včetně příkladů pro jiné typy záznamů DNS.
