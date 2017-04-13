<properties
    pageTitle="Azure správce prostředků podpora pro správce přenosy | Microsoft Azure "
    description="Pomocí prostředí PowerShell pro správce přenosy v síti s Azure správce prostředků"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure správce prostředků podpora pro správce dopravy Azure

Azure správce prostředků je rozhraní upřednostňovaného správy služby v Azure. Azure profily přenosy správce můžete spravovat pomocí nástroje a na základě správce prostředků Azure rozhraní API.

## <a name="resource-model"></a>Model prostředků

Azure správce přenosy se konfigurují sada nastavení s názvem profil přenosy správce. Tento profil obsahuje nastavení, nastavení směrování přenosu, nastavení sledování koncového bodu a seznam služby koncové body, ke kterým je směrovat přenosy DNS.

Každý profil správce přenosy představuje zdrojem typu "TrafficManagerProfiles". Na úrovni rozhraní REST API URI u každého profilu vypadá takto:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Porovnání s klasické rozhraní API Azure přenosy správce

Správce prostředků Azure podpora pro správce přenosy používá jiná terminologie, než modelu klasické nasazení. V následující tabulce jsou uvedeny rozdíly mezi správce zdrojů a klasické podmínek:

| Správce prostředků termínů | Klasický termínů |
|-----------------------|--------------|
| Metody směrování přenosu | Metody vyrovnávání zatížení |
| Metoda priority (priorita) | Metoda překlopení |
| Váženého metody | Metoda kruhového |
| Metoda výkonu | Metoda výkonu |

Na základě reakcí zákazníků jsme změnili terminologie pro zvýšení přehlednosti a zkrácení běžné nedorozuměním. Není žádný rozdíl funkčnosti.

## <a name="limitations"></a>Omezení

Při odkazování na koncový bod typu "AzureEndpoints" pro Web App, koncové body přenosy správce může odkazovat pouze na výchozí (výroba) [úsek v prohlížeči](../app-service-web/web-sites-staged-publishing.md). Vlastní sloty nejsou podporované. Jako alternativu vlastní sloty lze nastavit pomocí typu "ExternalEndpoints".

## <a name="setting-up-azure-powershell"></a>Nastavení prostředí PowerShell Azure

Tyto pokyny pomocí prostředí PowerShell Microsoft Azure. Následující článek vysvětluje, jak nainstalovat a nakonfigurovat Azure Powershellu.

- [Instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md)

V příkladech v tomto článku se předpokládá, že máte existující skupiny zdrojů. Můžete vytvořit skupinu zdrojů pomocí následujícího příkazu:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure správce prostředků vyžaduje, aby všechny skupiny prostředků umístění. Toto umístění slouží jako výchozí pro zdroje vytvořené v dané skupině zdroje. Však od přenosy správce profilu zdroje jsou globální, ne místní, výběr umístění skupiny zdrojů nemá žádný vliv na Azure přenosy správce.

## <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu přenosy správce

Vytvoření profilu přenosy správce, použijte rutinu New-AzureRmTrafficManagerProfile:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Následující tabulka popisuje parametry:

| Parametr | Popis |
|-----------|-------------|
| Jméno | Název zdroje pro zdroj profilu přenosy správce. Profily ve stejné skupině zdroje musí mít jedinečné názvy. Tento název je nezávislý název DNS dotazy DNS.|
| ResourceGroupName | Název skupiny zdrojů obsahující daný zdroj profilu.|
| TrafficRoutingMethod | Určuje metodu směrování přenosu požívaný k určení, které koncový bod je vrácena odpověď dotaz DNS. Možné hodnoty jsou "Výkonu", "Vážená" nebo "Priorita".|
| RelativeDnsName | Určuje název hostitele část názvu DNS poskytovanou profil přenosy správce. Tato hodnota je společně s názvem domény DNS managera tak, že Azure přenosy tvoří plně kvalifikovaný název domény (FQDN) profilu. Například nastavení hodnoty "contoso" bude "contoso.trafficmanager.net."|
| HODNOTA TTL | Určuje DNS Time-to-Live (TTL) v sekundách. Tato hodnota TTL informuje překládání DNS místní a jak dlouho odpovědi mezipaměť DNS pro tento profil správce přenosy klienti DNS.|
| MonitorProtocol | Určuje protokol sloužící k monitorování stavu koncového bodu. Možné hodnoty jsou "HTTP" a "HTTPS".|
| MonitorPort | Určuje port TCP použitý sledování stavu koncového bodu.|
| MonitorPath | Určuje cestu vzhledem k názvu domény koncový bod slouží k probe pro stav koncového bodu.|

Rutiny vytvoří profil správce přenosy v Azure a vrátí odpovídající profil objekt k Powershellu. V tomto okamžiku profil neobsahuje žádné koncové body. Další informace o tom, že koncové body profil správce přenosy najdete v článku [Přidání koncové body přenosy správce](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Získání profil správce dopravy

Pokud chcete načíst existující objekt profil správce přenosy, získáte pomocí rutiny Get-AzureRmTrafficManagerProfle:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Tato rutina vrací objekt profilu přenosy správce.

## <a name="update-a-traffic-manager-profile"></a>Aktualizace profilu přenosy správce

Změna profilů přenosy správce spočívá 3 krocích:

1. Načtení profil pomocí Get-AzureRmTrafficManagerProfile nebo můžete použít vrácené AzureRmTrafficManagerProfile nový profil.
2. Upravte profil. Můžete přidat a odebrat koncové body nebo změna parametrů koncový bod nebo profilu. Tyto změny se offline operace. Pouze měníte místní objekt v paměti, který představuje profilu.
3. Uložte provedené změny pomocí rutinu Set-AzureRmTrafficManagerProfile.

Všechny vlastnosti profilu můžete změnit kromě RelativeDnsName profilu. Pokud chcete změnit RelativeDnsName, je potřeba odstranit profilu a nový profil pod novým názvem.

Následující příklad ukazuje, jak změna TTL na profil:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Existují tři typy koncové body přenosy správce:

1. **Azure koncové body** jsou službám v Azure
2. **Externí koncové body** jsou služby hostovaný mimo Azure
3. **Koncové body vnořené** se používají k vytvoření vnořené hierarchie profilů přenosy správce. Vnořené koncové body povolit rozšířené směrování přenosu konfigurace složitých aplikací.

Ve všech případech tři koncové body lze přidat dvěma způsoby:

1. Použití 3 krocích popsaných dříve. Výhodou tento způsob je to, že můžete v jednu aktualizaci provedeno několik změn koncového bodu.
2. Použití rutinu New-AzureRmTrafficManagerEndpoint. Tato rutina přidá koncový bod do existujícího profilu přenosy správce v jedné operace.

## <a name="adding-azure-endpoints"></a>Přidání Azure koncové body

Azure koncové body odkazovat službám v Azure. Tři typy Azure koncové body nejsou podporované:

1. Azure Web Apps
2. Klasický"cloudových služeb, (které může obsahovat služby PaaS nebo IaaS virtuálních počítačích)
3. Azure zdroje PublicIpAddress (které může být připojen Vyrovnávání zatížení nebo virtuálního počítače NIC). PublicIpAddress musí mít přiřazené k použít ve Správci přenosy název DNS.

V obou případech:

- Služba není zadán pomocí parametr "targetResourceId" Přidat AzureRmTrafficManagerEndpointConfig nebo nový AzureRmTrafficManagerEndpoint.
- "Cílový" a "EndpointLocation" je obsaženo v TargetResourceId.
- Zadání "tloušťky" je nepovinný krok. Gramáží se používá pouze v případě profilu nakonfigurovaný tak, aby použijte metodu směrování přenosu "Vážená". V opačném jsou ignorovány. Pokud je zadaná, hodnotu musí být číslo od 1 do 1 000. Výchozí hodnota je "1".
- Zadání "Priorita" je nepovinné. Priority se používá pouze v případě profilu nakonfigurovaný tak, aby použijte metodu směrování přenosu "Priorita". V opačném jsou ignorovány. Platné hodnoty jsou 1 až 1000 s nižšími hodnotami označující vyšší prioritu. Pokud zadané pro jeden koncový bod, musí být zadán pro všechny koncové body. Pokud není zadáno, použijí se výchozí hodnoty počínaje '1' v pořadí, ve kterém koncové body uvádí.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Příklad 1: Přidání pomocí přidat AzureRmTrafficManagerEndpointConfig koncové body v prohlížeči

V tomto příkladu jsme vytvořit profil správce přenosy a přidejte dva koncové body v prohlížeči pomocí rutiny AzureRmTrafficManagerEndpointConfig přidat.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Příklad 2: Přidání koncový bod služby "klasické" cloudu pomocí nový AzureRmTrafficManagerEndpoint

V tomto příkladu "klasické" koncový bod služby cloudu přibude profil přenosy správce. V tomto příkladu jsme určení profilu pomocí názvy skupin profilu a zdroje, spíše než předávání objekt profilu. Oba přístupy jsou podporovány.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Příklad 3: Přidání koncový bod publicIpAddress pomocí nový AzureRmTrafficManagerEndpoint

V tomto příkladu je veřejné prostředek IP adresy přidán do profilu přenosy správce. Na veřejnou IP adresu musí mít název DNS nakonfigurované a vázat NIC virtuálního počítače nebo při vyrovnávání zatížení.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Přidání externích koncové body

Přenosy správce používá externí koncové body směrování adres ke službám hostovaný mimo Azure. Stejně jako u Azure koncové body, můžete přidat externí koncové body buď pomocí přidat AzureRmTrafficManagerEndpointConfig následuje sadu AzureRmTrafficManagerProfile nebo nový AzureRMTrafficManagerEndpoint.

Při zadávání externí koncové body:

- Název koncového bodu domény je nutné zadat pomocí parametru "Cílový"
- Pokud se použije směrování přenosu metoda "Výkonu", "EndpointLocation" není potřeba. V opačném případě je nepovinný krok. Hodnota musí být [platná Azure oblasti název](https://azure.microsoft.com/regions/).
- "Hmotnost" a "Priorita" jsou volitelné.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Příklad 1: Přidání externího koncové body pomocí přidat AzureRmTrafficManagerEndpointConfig a nastavení AzureRmTrafficManagerProfile

V tomto příkladu jsme vytvořit profil správce přenosy přidat dvěma externí koncové body a potvrďte změny.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Příklad 2: Přidání externího koncové body pomocí nový AzureRmTrafficManagerEndpoint

V tomto příkladu jsme přidat externí koncový bod do existujícího profilu. Profil je určen pomocí názvy skupin profilu a zdroje.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>Přidání koncové body "Vnořené"

Každý profil správce přenosy Určuje jednu metodu směrování přenosu. Můžou ale nastat scénáře, které vyžadují složitější směrování přenosu než směrování poskytovanou jeden profil přenosy správce. Můžete do sebe vnořit profily přenosy správce umožňují kombinovat výhody více než jednu metodu směrování přenosu. Vnořené profily umožňují změnit výchozí chování přenosy správce podpora větších a složitější nasazení aplikace. Více podrobných příkladů naleznete v tématu [vnořené správce přenosy profily](traffic-manager-nested-profiles.md).

Konfigurace vnořené koncové body v nadřazené profil pomocí konkrétní typ "NestedEndpoints". Při zadávání vnořené koncové body:

- Koncový bod je nutné zadat pomocí parametru "targetResourceId"
- Pokud se použije směrování přenosu metoda "Výkonu", "EndpointLocation" není potřeba. V opačném případě je nepovinný krok. Hodnota musí být [platná Azure oblasti název](http://azure.microsoft.com/regions/).
- "Hmotnost" a "Priorita" jsou volitelné jako Azure koncové body.
- Parametr "MinChildEndpoints" je nepovinné. Výchozí hodnota je "1". Počet dostupných koncové body, které patří pod této mezní hodnoty, považuje profilu nadřazené profilu podřízené "sníženou kvalitu." a přesměruje umožnění datových přenosů do jiných koncové body v nadřazené profilu.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Příklad 1: Přidání vnořené koncové body pomocí přidat AzureRmTrafficManagerEndpointConfig a nastavení AzureRmTrafficManagerProfile

V tomto příkladu jsme vytvořit nové podřízené přenosy správce a nadřazené profily: Přidání podsložky jako vnořené koncový bod do nadřazené a potvrďte změny.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Neopakujeme v tomto příkladu jsme nepřidali jiných koncové body k profilům podřízené nebo nadřízenou.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Příklad 2: Přidání vnořené koncové body pomocí nový AzureRmTrafficManagerEndpoint

V tomto příkladu jsme přidat existující profil podřízené jako vnořené koncový bod do existujícího profilu nadřazené. Profil je určen pomocí názvy skupin profilu a zdroje.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Aktualizace koncový bod přenosy správce

Existují dva způsoby, jak aktualizovat existující koncový bod přenosy správce:

1. Získání profil správce přenosy pomocí Get-AzureRmTrafficManagerProfile, vlastnosti koncového bodu v profilu aktualizovat a potvrďte změny pomocí nastavení AzureRmTrafficManagerProfile. Tento způsob zahrnuje výhod bude možné aktualizovat víc koncový bod jedné operace.
2. Získání koncový bod přenosy správce pomocí Get-AzureRmTrafficManagerEndpoint, aktualizovat vlastnosti koncového bodu a potvrďte změny pomocí nastavení AzureRmTrafficManagerEndpoint. Tento způsob je jednodušší a od nevyžaduje indexování do pole koncové body v profilu.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Příklad 1: Aktualizace pomocí Get-AzureRmTrafficManagerProfile a nastavení AzureRmTrafficManagerProfile koncové body

V tomto příkladu jsme změnit jejich priority na dvě koncové body do existujícího profilu.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Příklad 2: Aktualizace pomocí Get-AzureRmTrafficManagerEndpoint a AzureRmTrafficManagerEndpoint nastavit koncový bod

V tomto příkladu jsme upravit tloušťky jeden koncový bod do existujícího profilu.

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Povolení nebo zakázání koncové body a profily

Přenosy Manager umožňuje jednotlivé koncových bodů ho povolit nebo zakázat a povolení povolení nebo zakázání celý profilů.
Tyto změny může provést nastavením začíná/aktualizace/koncový bod nebo profilu zdrojů. Zjednodušení tyto společná operace, budou také podporují prostřednictvím vyhrazené rutiny.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Příklad 1: Povolení nebo zakázání profil správce dopravy

Povolit profil správce přenosy, použijte AzureRmTrafficManagerProfile povolit. Profil lze zadat použití objektu profilu. Objekt profilu můžete předávat prostřednictvím příležitosti nebo pomocí "-TrafficManagerProfile" parametr. V tomto příkladu jsme zadáním profilu na profil a zdroje název skupiny.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Jak zakázat profil přenosy správce:

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Rutina zakázat AzureRmTrafficManagerProfile vyzve k potvrzení. Tento dotaz můžete potlačit pomocí "-platnost" parametr.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Příklad 2: Povolení nebo zakázání koncový bod přenosy správce

Povolit správce přenosy koncový bod, použijte AzureRmTrafficManagerEndpoint povolit. Existují dva způsoby, jak zadat koncový bod

1. Pomocí objektu TrafficManagerEndpoint předaných prostřednictvím příležitosti nebo "-TrafficManagerEndpoint" parametrů
2. Použití název koncového bodu, typ koncového bodu, název profilu a název skupiny zdrojů:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Podobně zakázat přenosy správce koncového bodu:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Stejně jako u zakázat-AzureRmTrafficManagerProfile rutinu zakázat AzureRmTrafficManagerEndpoint vyzve k potvrzení. Tento dotaz můžete potlačit pomocí "-platnost" parametr.

## <a name="delete-a-traffic-manager-endpoint"></a>Odstranění koncového bodu přenosy správce

Pokud chcete odebrat jednotlivé koncové body, získáte pomocí rutiny AzureRmTrafficManagerEndpoint odebrat:

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Tato rutina vyzve k potvrzení. Tento dotaz můžete potlačit pomocí "-platnost" parametr.

## <a name="delete-a-traffic-manager-profile"></a>Odstranění profilu přenosy správce

K odstranění profil správce přenosy použijte rutinu odebrat AzureRmTrafficManagerProfile zadání názvu skupiny profilu a zdroje:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Tato rutina vyzve k potvrzení. Tento dotaz můžete potlačit pomocí "-platnost" parametr.

Profil odstraněno je možné také zadat pomocí objekt profilu:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Tato řada lze také přesměrovat:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Další kroky

[Sledování přenosy správce](traffic-manager-monitoring.md)

[Důležité informace o výkonu přenosy správce](traffic-manager-performance-considerations.md)
