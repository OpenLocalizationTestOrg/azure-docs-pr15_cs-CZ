<properties
   pageTitle="Azure architektura referenční informace – IaaS: Vytváření doménové zdroje služby Active Directory v Azure | Microsoft Azure"
   description="Jak vytvořit důvěryhodné Active Directory domain Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Vytvoření struktury zdrojů Active Directory adresáře služeb (přidá) v Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tento článek popisuje, jak vytvořit Active Directory domain Azure, která je nezávislý, ale důvěryhodné, domén v místním doménové.

> [AZURE.NOTE] Azure obsahuje dva různé nasazení modely: [Správce prostředků] [ resource-manager-overview] a klasické. Tento odkaz architektura používá správce zdroje, který Microsoft doporučuje nové nasazení.

Organizace, která běží Active Directory (AD) místní pravděpodobně strukturu zahrnující mnoho různých domén. Například můžete vytvořit jednotlivé domény pro různá oddělení nebo suborganizations nebo důsledku pořízení nebo spojení z jiných organizací může byly přidány nové domény. Domény můžete použít k poskytnutí izolace mezi funkčních oblastí, které jsou průběžně samostatné, případně z bezpečnostních důvodů, ale můžete sdílet informace mezi doménami vytvořením vztahy důvěryhodnosti.

Organizace, která používá oddělené domény můžete využívat Azure přemístíte jednu nebo víc z těchto domén do cloudu. Organizace může můžete taky chtít zachovat všechny zdroje cloudu logicky odlišné od těch uloženými místní a obsahují informace o zdrojích cloudu v svůj adresář v doméně taky uskutečňuje v cloudu.

Využijete služby Active Directory v Azure několika různými způsoby podle popisu v článcích [Rozšíření Active Directory, aby Azure] [ extending-ad-to-azure] a [Provádění Azure Active Directory][implementing-aad]. V tomto dokumentu se zaměřuje na jeden konkrétním scénáři: vytvoření domény v cloudu, která se liší od všechny uskutečňuje místní domény, ale, které mají vztah důvěryhodnosti s místní domény. 

Typické použití případů pro tuto architekturu patří:

- Správa zabezpečení oddělování objektů a identit v cloudu.

- Migrace jednotlivé domény z místního do cloudu.

## <a name="architecture-diagram"></a>Diagram architektury

Následující diagram zvýrazní důležitá součástí tato architektura (*klikněte na zvětšit*). Další informace o prvcích šedě, přečtěte si [Implementace zabezpečeného hybridní architektura sítě v Azure] [ implementing-a-secure-hybrid-network-architecture] a [Implementace zabezpečeného hybridní architektura sítě s přístupem k Internetu v Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **V místní síti.** V místní síti obsahuje vlastní AD struktury a domén.

- **Servery AD.** Toto jsou řadiče domény provádění directory services (AD DS) spuštěný jako VMs v cloudu. Tyto servery hostují strukturu obsahující jednu nebo víc domén, nezávislá na tyto umístěn místní.

- **Jednosměrný vztah důvěryhodnosti.** V diagramu příklad jednosměrný vztah důvěryhodnosti mezi doménu v cloudu pro místní domény. Tento vztah umožňuje místní přístup k prostředkům domény v cloudu, ale ne další způsob, jakým kolem. Toto je běžný případ. Pokud uživatelé cloudu taky potřebovat přístup k místním zdrojům však můžete vytvořit obousměrný vztah důvěryhodnosti.

- **Active Directory podsítě.** Servery služby AD DS jsou umístěny v samostatné podsítě. Pravidla NSG chránit servery služby AD DS a může poskytovat bránu firewall proti z neočekávané zdrojů.

- **Web osy podsítě**, **podsítě osy Business**a **Data úroveň podsítě**. Tyto podsítě hostovat servery a součástí, které spuštění aplikace v cloudu. Další informace najdete v tématu [Spuštění VMs N-vrstvy architektury na Azure][running-VMs-for-an-N-tier-architecture-on-Azure]. Zdroje a VMs do této podsítě jsou obsaženy v cloudu domény.

- **Azure brány**. Azure brány obsahuje spojení mezi místní síti a Azure VNet. Může to být [připojení VPN] [ azure-vpn-gateway] nebo [Azure ExpressRoute][azure-expressroute]. Další informace najdete v tématu [implementace architektura sítě zabezpečené hybridní v Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Doporučení

Tato část obsahuje seznam doporučení podle základní součásti požadované provádět základní architektury. Těmito doporučeními zabývat těmito oblastmi:

- Nastavení přidá a

- Důvěřujte konfigurace relace.

Máte další nebo různé požadavky zde popsané. Položky v této části můžete použít jako výchozí bod pro vzhledem k tomu, jak přizpůsobit architektura vlastní systému.

### <a name="adds-recommendations"></a>Přidá doporučení

Konkrétní doporučení pro implementaci služby Active Directory v cloudu, odkazovat na dokument [Rozšíření Active Directory, aby Azure][extending-ad-to-azure]. V článku [pokyny pro nasazení systému Windows služby Active Directory Server na virtuálních počítačích Azure] [ ad-azure-guidelines] obsahuje další podrobné informace.

### <a name="trust-recommendations"></a>Zabezpečení doporučení

Místní domény jsou obsaženy v jiné struktuře domén v cloudu. Chcete-li povolit ověřování místních uživatelů v cloudu, musí domén v cloudu důvěřovat doménu ve struktuře místní. Podobně pokud cloudu poskytuje přihlašovací doména pro externí uživatele, může být nutné pro místní domény s informacemi o důvěryhodnosti domény cloudu.

Je možné vytvořit vztahy důvěryhodnosti služby na úrovni doménové vytvořením [vztahy důvěryhodnosti][creating-forest-trusts], nebo na úrovni domény tak, že [vytvoříte externí vztahy důvěryhodnosti][creating-external-trusts]. Úrovně zabezpečení doménové vytvoříte relaci mezi všemi doménami ve dvou strukturami. Úrovně zabezpečení externí domény jenom vytvoříte relaci mezi dvěma zadaný domény. Měli byste pouze vytvořit externí domény úrovně vztahy mezi domén v různých doménových.

Vztahy důvěryhodnosti mohou být jednosměrné (jednosměrné) nebo obousměrný (mezi dvěma stranami):

- Jednosměrný vztah důvěryhodnosti umožňuje uživatelům v jedné doméně struktuře či (označované jako *příchozí* domény nebo doménové) získat přístup ke zdrojům v jiné ( *odchozí* domény nebo doménové). 

- Obousměrný vztah důvěryhodnosti umožňuje uživatelům v doméně nebo doménové přístupu k prostředkům ve druhé.

Následující tabulka shrnuje zabezpečení konfigurací některých jednoduché scénářích:

| Scénář | Místní zabezpečení | Shluk zabezpečení |
|----------|-------------------|-------------|
| Místní uživatelé potřebovat přístup ke zdrojům v cloudu, ale ne číslování na odrážky | Jednosměrný příchozí pošty | Jednosměrný, odchozí |
| Uživatelé v cloudu vyžadují přístup do prostředky umístěné v místním nasazení, ale ne číslování na odrážky | Jednosměrný, odchozí | Jednosměrný příchozí pošty |
| Uživatelé v cloudu a místní vyžaduje přístup ke zdrojům v cloudu a místní | Mezi dvěma stranami, příchozí a odchozí | Mezi dvěma stranami, příchozí a odchozí |

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Úrovně vztahy důvěryhodnosti jsou přenosné. Pokud vytvoříte doménové úrovně vztah důvěryhodnosti mezi místní struktury a strukturu v cloudu, tento důvěryhodnosti rozšířit na jiné nové domény vytvořené v obou doménových. Pokud používáte domén poskytnout oddělení z bezpečnostních důvodů, zvažte vytvoření vztahy důvěryhodnosti služby na úrovni domény jenom. Úrovně vztahy důvěryhodnosti domény jsou jiné přenosné.

Otázky bezpečnosti pro konkrétní AD, naleznete v části *otázky bezpečnosti pro* [Rozšíření Active Directory, aby Azure][extending-ad-to-azure].

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Implementace aspoň dva řadiče domény u každé domény. Díky automatické replikace mezi servery. Vytvoření dostupné pro VMs budou sloužit jako servery AD zpracování všech domén. Ujistěte se, že jsou v sadě aspoň dva servery. 

Zvažte taky, jestli označením jeden nebo více serverů v každé domény jako [předlohy úsporném operace] [ standby-operations-masters] v případě, že připojení k serveru budou sloužit jako roli FSMO se nezdaří.

## <a name="scalability-considerations"></a>Škálovatelnost: co byste měli zvážit

AD je automaticky přizpůsobit pro řadiče domény, které jsou součástí tu samou doménu. Žádosti o jsou rozvržena všechny řadiče domény. Přidání dalšího řadiče domény a automaticky synchronizuje s doménou. Konfigurovat při vyrovnávání zatížení samostatné směrování adres řadiče v rámci domény. Zajištění všechny řadiče domény dostatečné paměti a úložiště prostředky pro zpracování databáze domény. Aby všechny řadiče domény VMs stejnou velikost.

## <a name="management-and-monitoring-considerations"></a>Správa a sledování co byste měli zvážit

Informace o správě a sledování aspektech najdete v tématu odpovídající částech [Rozšíření Active Directory, aby Azure][extending-ad-to-azure]. 

Další informace najdete v tématu [Sledování Active Directory][monitoring_ad]. Instalace nástrojů, jako jsou [Microsoft System Center] [ microsoft_systems_center] sledování v na serveru správy podsítě můžete provést tyto úlohy. 

## <a name="solution-components"></a>Součásti řešení

Ukázkový skript řešení [Nasazení ReferenceArchitecture.ps1][solution-script], neexistuje, které můžete provádět architektura, který následuje doporučení popisované v tomto článku. Tento skript používá správce prostředků Azure šablony. Šablony jsou k dispozici jako sady základní stavební bloky, z nichž každá provádí konkrétní akce, jako jsou vytváření VNet a konfigurace NSG. Účelem skript je organizovat nasazení šablony.

Šablony jsou s parametry, s parametry v samostatných souborech JSON. Můžete změnit parametrů v těchto souborů, které chcete konfigurovat nasazení vlastní požadavkům. Nemusíte změnit šablony sami. Schémata objektů v souborech parametru nesmí změnit.

Při úpravách šablony vytvořit objekty, které následují za konvence podle [Doporučené názvů zdrojů Azure][naming-conventions].

Ukázka řešení vytvoří a konfiguruje prostředí v cloudu implementující doménu s názvem *treyresearch.com*. Toto prostředí zahrnuje přidá podsítě a servery DMZ, web osy, obchodní osy a komponenty Accessu osy dat, virtuální privátní sítě brány a správa osy. Ukázka řešení obsahuje taky volitelná konfigurace pro vytvoření simulovaný místního prostředí s vlastní doménou, *contoso.com*. Řešení zahrnuje skripty, které mají vztah důvěryhodnosti mezi tyto domény, který umožňuje místních uživatelů pro přístup k objektům v doméně *treyresearch.com* v cloudu.

Následující části popisují prvky místní i cloudových konfigurace.

### <a name="on-premises-components"></a>Místní součásti

>[AZURE.NOTE] Tyto součásti nejsou hlavní výběr architektury popsané v tomto dokumentu a jsou k dispozici pouze vám umožní otestovat prostředí cloudu, ne pomocí skutečné provozním prostředí. Z tohoto důvodu v této části pouze shrnuje klíčové parametr soubory. Můžete změnit nastavení, například velikosti VMs nebo IP adresy, ale je vhodné ponechat spoustu dalších parametrů beze změny.

Toto prostředí zahrnuje doménové AD doménu *contoso.com* . Doména obsahuje dva přidá servery s IP adresy 192.168.0.4 a 192.168.0.5. Tyto dva servery taky spustit službu DNS. S názvem místního účtu správce na obou VMs `testuser` heslem `AweS0me@PW`. Kromě toho konfigurace nastaví Brána VPN připojit se k VNet v cloudu. Konfigurace můžete změnit úpravou následující JSON soubory umístěné ve [**parametry/místní edici**] [ on-premises-folder] složky:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Tento soubor definuje sítě adresní prostor pro místního prostředí.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Tento soubor obsahuje konfiguraci místního VMs hostitelské služby přidá. Ve výchozím nastavení jsou vytvářeny dvě *Standardní DS3 v2* VMs.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** a ** [connection.parameters.json][on-premises-connection-parameters]**. Tyto soubory podržte nastavení pro připojení VPN k bráně Azure VPN v cloudu, včetně sdílené klávesy použít k ochraně přenosy procházení brány.

Zbývající soubory ve složce obsahují informace o konfiguraci použitých k vytvoření místní domény (*contoso.com*), pomocí tohoto infrastruktury. Používat k instalaci přidá a nastavení DNS a vytvořte struktuře místní.

Řešení taky používá následující skript s názvem [příchozí trust.ps1][incoming-trust], která běží na počítači v místní domény:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Tento skript přidá IP adresy služby AD DS servery v cloudu (viz následující části) místní služby DNS a potom pomocí [netdom] [ netdom] příkaz Vytvořit příchozí jednosměrný vztah důvěryhodnosti domény v cloudu (*treyresearch.com*).

### <a name="cloud-components"></a>Součásti cloudu

Tyto součásti tvoří základní tato architektura. Nastavení infrastrukturu pro doménu *treyresearch.com* a vytvořte vztahy důvěryhodnosti s místní domény *contoso.com* . [**Parametry/azure**] [ azure-folder] složka obsahuje následující soubory parametr pro konfiguraci tyto součásti:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Tento soubor definuje strukturu VNet VMs a jiné součásti v cloudu. Obsahuje nastavení, například název, adresní prostor, podsítí a adresy všech serverů DNS povinné. Adresy DNS ukazuje tento příklad odkaz IP adresy místní servery DNS a výchozí server Azure DNS. Změna tyto adresy Pokud nepoužíváte místního prostředí ukázkové neodkazuje nastavení DNS:
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Tento soubor nakonfiguruje VMs systém přidá v cloudu. Nastavení se skládá ze dvou VMs. Změnit správu uživatelské jméno a heslo v `virtualMachineSettings` část notebooku a můžete upravit velikost OM, aby odpovídala požadavky na domény:

    Další informace najdete v článku [Rozšíření Active Directory, aby Azure][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [Přidat přidá domény controller.parameters.json][add-adds-domain-controller-parameters]**. Tento soubor obsahuje nastavení pro vytváření *treyresearch.com* domény trvající přidá servery. Použití vlastní příponami vytvořte domény a přidání přidá serverů. Není-li vytvořit další přidá servery (v tomto případě je, aby měli přidat `vms` matice), změnit jejich jména z výchozího nebo chcete vytvořit doménu s jiným názvem nemusíte upravit tento soubor.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Tento soubor obsahuje nastavení slouží k vytváření brána Azure VPN na cloud používá pro připojení k místní síti. Změňte `sharedKey` hodnota argumentu `connectionsSettings` oddílu, aby odpovídal, místní VPN zařízení. Další informace najdete v tématu [implementace hybridní architektura sítě s Azure a místních VPN][hybrid-azure-on-prem-vpn].

- ** [dmz private.parameters.json] [ dmz-private-parameters] ** a ** [dmz public.parameters.json ] [ dmz-public-parameters] **. Tyto soubory konfigurace příchozí (veřejné) a odchozí (soukromé) straně VMs, které tvoří DMZ ochrana servery v cloudu. Další informace o těchto prvků a jejich konfigurace v tématu [implementace DMZ mezi Azure a Internetu][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer web.parameters.json][loadBalancer-web-parameters]**, ** [loadBalancer biz.parameters.json][loadBalancer-biz-parameters]**, a ** [loadBalancer data.parameters.json][loadBalancer-data-parameters]**. Tyto soubory parametry obsahují OM specifikace pro úrovní přístupu pro web, business a data a konfigurace vyrovnávání zatížení pro každou úroveň. Toto jsou VMs, které hostovat webových aplikací web apps a databází a provádění úloh business pro organizaci. VMs ve vrstvě web se přidají do domény v cloudu pomocí nastavení zadané v ** [web OM domény join.parameters.json] [ web-vm-domain-join-parameters] ** soubor.

    Každý soubor obsahuje dvě sady parametry konfigurace. `virtualMachineSettings` Část definuje VMs hostujících službu služby AD FS v cloudu. Ve výchozím nastavení skript vytvoří dva z těchto VMs ve stejnou sadu dostupnosti. Následující fragmenty zobrazovat příslušných částí *loadBalancer web.parameters.json* souboru:

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    Můžete změnit velikost a počet VMs v každé osy podle vašich požadavků.

    `loadBalancerSettings` Část obsahuje popis Vyrovnávání zatížení pro tyto VMs. Vyrovnávání zatížení předává přenosy v síti, která se zobrazí na porty 80 (HTTP) a port 443 (HTTPS) jednoho nebo několika VMs. 

    >[AZURE.NOTE] Pravidlo pro port 80 používá pro připojení TCP nikoli HTTP. Důvodem je instalace služby IIS na úrovni webu konfigurace podporuje ověřování systému Windows pouze. Anonymní přístup je vypnutá. Pokusíte *ping* port 80 přes připojení HTTP se nezdaří 401 (neoprávněným) chybu, že pouze pomocí připojení TCP zjistí, zda je číslo portu aktivní:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Tento soubor obsahuje konfiguraci pole a Správa odkazů VMs. Je možné získat přihlášení a přístup pro správu k VMs web, business a úrovní dat v poli odkaz. Ve výchozím nastavení vytvoří skript jednoho *Standard_DS1_v2* OM, ale můžete upravit tento soubor vytvořit VMs zvětšit nebo další, pokud je pravděpodobně významné pracovní zátěž správy.

Konfigurace taky používá [odchozí trust.ps1] [ outgoing-trust] skript ukázáno v následujícím příkladu k vytvoření jednosměrné odchozí vztah důvěryhodnosti s doménou *contoso.com* :

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Tento skript je podobný *příchozí trust.ps1* skript popsané výše. Přidá IP adresy místní servery služby AD DS místní služby DNS a potom pomocí [netdom] [ netdom] příkaz vytvořit odchozí vztah důvěryhodnosti.

## <a name="solution-deployment"></a>Nasazení řešení

Řešení předpokládá následující požadavky:

- Máte existující Azure předplatné, ve kterém můžete vytvořit skupiny zdrojů.

- Jste stáhli a nainstalovali nejnovější sestavení Azure Powershellu. Najdete [tady] [ azure-powershell-download] pokyny.

Spuštění skript, který nasadí řešení:

1. Přesunutí do pohodlný složky na místním počítači a vytvořte následující podsložky:

    - Skriptů

    - Skripty a parametrů

    - Skripty/parametry/místní edici

    - Skripty/parametry/Azure

2. Stažení [Nasazení ReferenceArchitecture.ps1] [ solution-script] souboru do složky skriptů

3. Stáhnout obsah [místní parametry/edici] [ on-premises-folder] skripty/parametry/místní edici složky:

4. Stáhnout obsah [Parametry/azure] [ azure-folder] skripty/parametry/Azure složky.

5. Upravte soubor nasazení ReferenceArchitecture.ps1 ve složce skripty a změňte následující čar a umožňuje určit, které by měly být vytvořené nebo používá k ukládání prostředků vytvořené skriptem skupiny zdroje:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. Upravte soubory parametrů v skripty/parametry/místní edici a skripty/parametry/Azure složek. Aktualizujte odkazy skupina zdroje v těchto souborech podle názvů skupin zdrojů přiřazených k proměnné v souboru ReferenceArchitecture.ps1 nasazení. Následující tabulka uvádí soubory, které parametr odkazovat skupiny jako zdroje. Skupiny zdrojů *Vzdálená pomoc služby AD FS pracovní zátěž rg* *Vzdálená pomoc služby AD FS zabezpečení rg*, *Vzdálená pomoc-služby AD FS přidá rg*, *Vzdálená pomoc služby AD FS služby AD FS rg*a *Vzdálená pomoc služby AD FS proxy rg* se používají v skript Powershellu a nedojde v soubory parametrů.

  	|Pole Skupina zdroje|Soubory parametrů|
  	|--------------|--------------|
  	|Vzdálená pomoc adtrust místní edici rg|parameters\onpremise\connection.Parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.Parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\onpremise\virtualNetwork.Parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON
  	|Vzdálená pomoc adtrust sítě rg|parameters\onpremise\connection.Parameters.JSON<br />parameters\azure\dmz-Private.Parameters.JSON<br />parameters\azure\dmz-Public.Parameters.JSON<br />parameters\azure\loadBalancer-Biz.Parameters.JSON<br />parameters\azure\loadBalancer-data.Parameters.JSON<br />parameters\azure\loadBalancer-Web.Parameters.JSON<br />parameters\azure\virtualMachines-ADDS.Parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.Parameters.JSON<br />parameters\azure\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\azure\virtualNetwork.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON (*dvou výskytů*)

    Navíc nastavení místním i cloudových součásti, jak je uvedeno v [Součásti řešení] [ solution-components] oddíl.

7. Otevřete okno Azure Powershellu, přejděte do složky skripty, spusťte tento příkaz:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Nahrazení `<subscription id>` pomocí svého ID Azure předplatného.

    Pro `<location>`, zadejte Azure oblast, jako například `eastus` nebo `westus`.

    `<mode>` Parametr můžete mít jednu z následujících hodnot:

    - `Onpremise`, k vytvoření simulovaný místního prostředí.

    - `Infrastructure`, a vytvořte infrastruktury VNet přejít pole v cloudu.

    - `CreateVpn`, sestavovat Azure virtuální sítě brány a připojte ji k místní síti.

    - `AzureADDS`, vytvářet VMs budou sloužit jako přidá servery, nasadit služby Active Directory do těchto VMs a vytvoření domény v cloudu.

    - `WebTier`, která vytvoří webu úroveň VMs a služba Vyrovnávání zatížení.

    - `Prepare`, která provádí všechny předchozí úkoly. **Pokud vytváříte zcela novou nasazení a nemáte infrastruktury místní jde doporučené možnost.**

    - `Workload`Vytvoření vrstvy business a data VMs a vyrovnávání zatížení. Tyto VMs nejsou součástí `Prepare` možnost.

    >[AZURE.NOTE] Pokud používáte `Prepare` možnost skript trvat několik hodin.

8.  Pokud používáte konfiguraci místního vzorku:

    1. Připojení odkazů (*Vzdálená pomoc adtrust Správa vm1* ve skupině prostředků *Vzdálená pomoc adtrust zabezpečení rg* ). Přihlaste se jako *testuser* heslem *AweS0me@PW*.

    2.  V poli odkaz otevřít relaci RDP na první OM v doméně *contoso.com* (místní domény). Tento OM adresou IP 192.168.0.4. Uživatelské jméno je *contoso\testuser* heslem *AweS0me@PW*.

    3. Stáhněte si [příchozí trust.ps1] [ incoming-trust] skriptování a ho spusťte a vytváří příchozí vztah důvěryhodnosti mezi *treyresearch.com* domény.

9. Pokud používáte vlastní místní infrastrukturu:

    1. Stáhněte si [příchozí trust.ps1] [ incoming-trust] skriptu.

    2. Upravte skript a nahraďte hodnotu `$TrustedDomainName` proměnná název vaší vlastní domény.

    3. Spusťte skript.

10. V poli odkaz připojte k první OM v *treyresearch.com* domain (doména v cloudu). Tento OM adresou IP 10.0.4.4. Uživatelské jméno je *treyresearch\testuser* heslem *AweS0me@PW*.

11. Stáhněte si [odchozí trust.ps1] [ outgoing-trust] skriptování a ho spusťte a vytváří příchozí vztah důvěryhodnosti mezi *treyresearch.com* domény. Pokud používáte vlastní místního počítače, klepněte napřed upravujete skript. Nastavit `$TrustedDomainName` proměnná Název místní domény a zadejte IP adresy služby AD DS servery u této domény v `$TrustedDomainDnsIpAddresses` proměnné.

12. V místním počítači, postupujte podle pokynů uvedených v článku [ověření vztah důvěryhodnosti] [ verify-a-trust] a zjistit, zda vztah důvěryhodnosti má správně nakonfigurované mezi *contoso.com* a *treyresearch.com* domény. Budete muset počkat několik minut po dokončení předchozích kroků nedá před můžete ověřením důvěřovat.

Zbývající volitelné kroky ukazují, jak zjistit, zda důvěryhodnosti domény funguje očekávaným způsobem. Tyto kroky nutné, abyste měli přístup k počítači vývoj s Visual Studio nainstalovaný.

1.  V okně Azure PowerShell spusťte tento příkaz Vytvořit web osy, pokud nebyly nastavením dříve (pomocí `Prepare` možnost):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    Tento příkaz vytvoří webová vrstva a přidá VMs *treyresearch.com* doménu.

2. Pomocí aplikace Visual Studio ve vývojovém počítači, vytvořte webovou aplikaci ASP.NET s názvem *TreyResearchWebApp*. Použití .NET Framework 4.5.2.

3. Vyberte šablonu *MVC* a změňte ověřování ověřování *Systému Windows*. Nevytvářejte služby aplikace v cloudu.

3. Vytvoření a spuštění aplikace otestovat ověřování. Zkontrolujte, že vaše aktuální uživatelské jméno systému Windows vypadá v řádku nabídek v horní části stránky směrem doprava.

4. Ukončete aplikaci Internet Explorer.

5. V okně *Průzkumník řešení* klikněte pravým tlačítkem myši TreyResearchWebApp projektu, klikněte na *Publikovat*.

6. V okně *Publikovat Web* klikněte na *vlastní*. Vytvoření vlastního profilu s názvem *TreyResearchWebApp*.

7. Na stránce *připojení* nastavení *Metoda publikování* k *Systému souborů* a zadejte do složky nazvané *TreyResearchWebApp*, nachází na vhodné místo ve vašem počítači vývoj.

8. Na stránce *Nastavení* nastavte *konfiguraci* *verzi*.

9. Na stránce *náhledu* klikněte na *Publikovat*.

10. Připojení ke každé OM ve vrstvě web zase (přes pole přeskakování) a provádět následující úkoly. IP adresy webu osy VMs jsou 10.0.1.4 a 10.0.1.5. Uživatelské jméno pro obě VMs je *treyresearch\testuser* heslem *AweS0me@PW*:

    1. Zkopírujte *TreyResearchWebApp* složku a její obsah z vývojového počítače do složky *C:\inetpub* .

    2. Pomocí konzoly Správce Internetové informační služby (IIS), přejděte na *webu servery\Výchozí* v počítači.

    3. V podokně *akcí* klikněte na *Základní nastavení*a změna fyzické cesty webu *%SystemDrive%\inetpub\TreyResearchWebApp*.

    4. V podokně *Zobrazení funkcí* poklikejte na položku *ověřování*. Zkontrolujte, že je zapnuté *Ověřování Windows* zakázat *Anonymní přístup* .

11. z místního počítače spusťte aplikaci Internet Explorer a přejděte na web na http://10.0.1.254 (Toto je adresa Vyrovnávání zatížení osy web).

12. V dialogovém okně *Zabezpečení systému Windows* zadejte přihlašovací údaje uživatele v místní doméně. Zadejte uživatelské jméno, který neexistuje v doméně *treyresearch* . Pokud používáte simulovaný místního prostředí nejprve vytvořit uživatele v doméně *contoso* a zadejte přihlašovací údaje tohoto uživatele.

13. Když se zobrazí na domovské stránce, zkontrolujte, že správné doméně a uživatelské jméno se mají zobrazit v řádku nabídek v horní části stránky směrem doprava.

## <a name="next-steps"></a>Další kroky

- Přečtěte si doporučené postupy pro [rozšíření místních domény přidá do Azure][adds-extend-domain]

- Přečtěte si doporučené postupy pro [vytváření infrastruktury služby AD FS] [ adfs] v Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Zabezpečené hybridní architektura sítě s doménami samostatné AD"