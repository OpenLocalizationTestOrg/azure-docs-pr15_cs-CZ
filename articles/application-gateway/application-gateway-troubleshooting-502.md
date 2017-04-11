<properties
   pageTitle="Poradce při potížích aplikace brány Chybná brána (502) | Microsoft Azure"
   description="Zjistěte, jak řešit problémy s aplikací brány 502 chyby"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Poradce při potížích Chybná brána v aplikaci brány

## <a name="overview"></a>Základní informace

Po konfiguraci brány aplikační Azure, chyb, které uživatelé setkat reprodukujte "Chyba serveru: 502 - Webový server obdržel neplatnou odpověď a budou sloužit jako brány nebo proxy server". K tomu může dojít kvůli následující hlavních důvodů:

- Fond back-end Azure aplikace brány není nakonfigurován nebo prázdná.
- Žádná VMs nebo instancí v sadě měřítko OM není správný.
- VMs nebo instance OM měřítko sady back-end neodpovídají zkušební výchozího stavu.
- Neplatné nebo nesprávná konfigurace sond vlastní stavu.
- Požadavky na uživatele požádat o časový limit nebo problémy s připojením.

## <a name="empty-backendaddresspool"></a>Prázdný BackendAddressPool

### <a name="cause"></a>Příčina

Pokud brána aplikace VMs ani OM měřítko vybrána položka konfigurovat fondu back-end adres, nemůže směrovat jakékoliv žádosti o zákazníka a vyvolá chybu Chybná brána.

### <a name="solution"></a>Řešení

Zajistěte, aby byl fondu back-end adres není prázdný. Lze to buď prostřednictvím Powershellu, rozhraní příkazového řádku nebo portál.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

Výstup z předchozího rutina smí obsahovat fondu neprázdné back-end adres. Následuje příklad dvou fondů místo, kam se vracejí kterého budou nakonfigurována plně kvalifikovaný název domény nebo IP adres pro back-end VMs. Zřizovací stav BackendAddressPool musí být "byla úspěšně dokončena".
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Nefunkční instancí v BackendAddressPool

### <a name="cause"></a>Příčina

Jsou-li všechny výskyty BackendAddressPool chybná, aplikace brány neměli všechny databázovým žádost směrovat uživatele na. Může taky k tomu při back-end instance jsou správný, ale nebudete mít požadovaných aplikací nasazena.

### <a name="solution"></a>Řešení

Zajistěte, aby instance jsou správný a správně nakonfigurovat aplikaci. Zkontrolujte, jestli jsou instance back-end moct odpovědět na příkaz ping z jiného OM ve stejném VNet. Pokud nakonfigurována veřejné koncový bod, zajistěte, aby byl požadavek prohlížeče webové aplikace nacházet.

## <a name="problems-with-default-health-probe"></a>Problémy s výchozí stav zkušební

### <a name="cause"></a>Příčina

chyby 502 lze také časté indikátory stavu zkušební výchozí není moct dosáhla VMs back-end. Pokud máte k dispozici instanci aplikace brány, automaticky nakonfiguruje zkušební stavu výchozí pro každou BackendAddressPool pomocí vlastnosti BackendHttpSetting. Zásah uživatele je potřeba nastavit tento zkušební. Konkrétně až nakonfigurovali pravidlo Vyrovnávání zatížení přidružení volání mezi BackendHttpSetting a BackendAddressPool. Výchozí zkušební nakonfigurovaný pro každou z těchto přidružení a aplikace brány zahájí zkontrolujte připojení pravidelných stav každé instanci BackendAddressPool přístavu podle BackendHttpSetting element. Následující tabulka obsahuje hodnoty spojené s zkušební výchozího stavu.


|Probe vlastnost | Hodnota | Popis|
|---|---|---|
| Probe adresy URL| http://127.0.0.1/ | Cesta URL |
| Interval | 30 | Probe interval v sekundách |
| Časový limit  | 30 | Probe časového limitu v sekundách |
| Chybná prahová hodnota | 3 | Probe počet opakování. Databázový server je označen po počet chyb po sobě jdoucí zkušební dosáhne chybná prahová hodnota. |

### <a name="solution"></a>Řešení

- Zajistěte, aby výchozí server nakonfigurovaný a je naslouchají 127.0.0.1.
- Pokud BackendHttpSetting Určuje port než 80, by měl poslouchat této přístavu nakonfigurované výchozí web.
- Volání http://127.0.0.1:port měly vrátit kód výsledku HTTP 200. To má být vrácena v rámci časový limit 30 sekund.
- Zajistěte, že je otevřen port nakonfigurované a jestli list bez pravidla brány firewall nebo skupiny zabezpečení sítě Azure, která blokuje příchozí nebo odchozí přenos na port nakonfigurované.
- Pokud se plně kvalifikovaný název domény nebo veřejnou IP se používají Azure klasické VMs nebo cloudové služby, ujistěte se, že je otevřít odpovídající [koncového bodu](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) .
- Pokud OM nakonfigurovaný prostřednictvím Správce prostředků Azure a je mimo VNet kde nasazení aplikace brány, [Skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) musí být nakonfigurované pro povolení přístupu na požadovaný port.


## <a name="problems-with-custom-health-probe"></a>Problémy s zkušební vlastní stavu

### <a name="cause"></a>Příčina

Vlastní stavu sond povolit vyšší flexibility výchozí zjišťování chování. Pokud chcete použít vlastní sond, uživatelé mohou konfigurovat interval zkušební, adresu URL a cesta k testování a počet neúspěšných odpovědí přijmout před označením instance back-end fondu jako chybné. Jsou přidané následující další vlastnosti.

|Probe vlastnost| Popis|
|---|---|
| Jméno | Název zkušební. Tento název se používá odkázat na zkušební v části Nastavení protokolu HTTP back-end. |
| Protocol (protokol) | Protokol sloužící k odeslání zkušební. Zkušební použije protokolu HTTP nastavení back-end |
| Host (hostitel) |  Název hostitele odeslat zkušební. Použít pouze v případě více webů je nakonfigurovaný na použití brány. Tím se liší od OM název hostitele.  |
| Cesta | Relativní cestu zkušební. Začíná platnou cestu "/". Zkušební odeslaný do \<protokol\>://\<hostitele\>:\<port\>\<cesta\> |
| Interval | Probe interval v sekundách. Toto je časový interval mezi dvěma po sobě jdoucí sond.|
| Časový limit | Probe časového limitu v sekundách. Zkušební je označen jako se nepodařilo není-li platnou odpověď přijata v rámci tohoto časového období. |
| Chybná prahová hodnota | Probe počet opakování. Databázový server je označen po počet chyb po sobě jdoucí zkušební dosáhne chybná prahová hodnota. |


### <a name="solution"></a>Řešení

Ověřte, že stav vlastní Probe správně nakonfigurované jako v předchozí tabulce. Kromě předchozích kroků zajistíte také následující:

- Ujistěte se, že zkušební je správně zadané podle [Průvodce](application-gateway-create-probe-ps.md).
- Pokud aplikace bránu pro jednoho webu, ve výchozím nastavení hostitele je nutné zadat název jako "127.0.0.1", pokud jinak konfigurovat vlastní zkušební.
- Zkontrolujte, že je volání http://\<hostitele\>:\<port\>\<cestu\> vrátí výsledek kód HTTP 200.
- Zajištění Interval, časový limit a UnhealtyThreshold v rámci přijatelné oblastí.

## <a name="request-time-out"></a>Žádost o časový limit

### <a name="cause"></a>Příčina

Po přijetí žádosti uživatele aplikace brána nakonfigurované pravidla platí pro žádosti a přesměrovává instanci fondu back-end. Čeká na Konfigurovat interval dobu odezvy z back-end instance. Ve výchozím nastavení je tento interval **30 sekund**. Pokud aplikace brány není dostanete odpověď od back-end aplikace v tomto intervalu, uvidí požadavek uživatele chybu 502.

### <a name="solution"></a>Řešení

Aplikace brány umožňuje uživatelům toto nastavení prostřednictvím BackendHttpSetting, které se dají pak použít pro různé skupiny. Různých fondů back-end může obsahovat jinou BackendHttpSetting a tedy různé žádosti časový limit nakonfigurované.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Další kroky

Pokud předchozích kroků problém nevyřeší, otevřete [podporují lístek](https://azure.microsoft.com/support/options/).
