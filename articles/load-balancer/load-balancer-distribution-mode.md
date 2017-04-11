<properties
   pageTitle="Konfigurace služby Vyrovnávání zatížení rozdělení režimu | Microsoft Azure"
   description="Postup při konfiguraci Azure zatížení vyrovnávání rozdělení režimu podporuje spřažení IP zdroje"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>Konfigurace režimu rozdělení pro vyrovnávání zatížení

## <a name="hash-based-distribution-mode"></a>Distribuce založené na hash režimu

Výchozí algoritmus rozdělení je 5 – řazené kolekce členů (Zdrojová IP adresa, zdrojový port cílová adresa IP cílový port protokol typu) hash namapujte přenosy na servery k dispozici. Poskytuje lepivost pouze v rámci relace přenosu. Paketů ve stejné relaci přesměrovaní na stejné instanci IP (DIP) datacentra za rozloženy koncového bodu. Když klienta spustí novou relaci z stejné IP zdroje, zdrojový port změní a způsobuje komunikaci přejdete na jiný koncový bod DIP.

![Vyrovnávání zatížení hash na základě](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Obrázek 1-5-n-tici rozdělení

## <a name="source-ip-affinity-mode"></a>Režim spřažení IP zdroje

Máme jiný režim rozdělení s názvem zdroje IP spřažení (označovaná taky jako spřažení relace nebo IP spřažení). Azure Vyrovnávání zatížení je možné konfigurovat mapování přenosy na dostupné servery pomocí 2-n-tici (zdroj IP, cílová adresa IP) nebo 3-n-tici (zdroj IP cílová adresa IP Protocol). Pomocí IP zdroj spřažení připojení ze stejného klientský počítač, které iniciuje půjde stejný koncový bod DIP.

Následující obrázek znázorňuje konfigurace 2 řazené kolekce členů. Všimněte si, jak 2-n-tici projde Vyrovnávání zatížení do virtuálního počítače 1 (VM1) které pak zálohování VM2 a VM3.

![spřažení relace](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Obrázek 2-2-n-tici rozdělení

Zdroj IP spřažení řeší nekompatibilita mezi Vyrovnávání zatížení Azure a Vzdálená plocha (RD) brány. Nyní je možné vytvářet farmy služby Brána RD v jedné cloudové službě.

Další použití varianta je médií nahrát kdy ukládání dat se stane, až UDP, ale řídicí nedosáhnete prostřednictvím protokolu TCP:

- Klient nejdřív zahájí relace TCP veřejnou adresu Vyrovnávání zatížení, se přesměrují do určité DIP tohoto kanálu ještě zbývá aktivní, sledování stavu připojení
- Novou relaci UDP ze stejného klientský počítač je zahájená stejné veřejné koncový bod Vyrovnávání zatížení, za předpokladu, že toto připojení také přesměrován stejný koncový bod DIP jako předchozí připojení pomocí protokolu TCP tak, aby odeslat multimédia jde spouštět na vysoký výkon a také zachovat řídicí kanál prostřednictvím protokolu TCP.

>[AZURE.NOTE] Při změně nastavení Vyrovnávání zatížení (odeberete nebo přidání stroje virtuální), distribuci požadavků klientů je přepočítávány. Nelze závisí na nové připojení klientů existující konec stejný server. Navíc pomocí IP zdroj režimu rozdělení příbuznosti tuto chybu může způsobovat nestejné výskyt přenosy. Klienty za proxy servery měly považovat za jeden jedinečný klientské aplikaci.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigurace nastavení spřažení zdroj IP služba Vyrovnávání zatížení

Virtuálních počítačích můžete změnit nastavení časového limitu Powershellu:

Přidejte Azure koncový bod virtuálního počítače a nastavte režimu rozdělení Vyrovnávání zatížení

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

LoadBalancerDistribution můžete nastavit sourceIP 2-n-tici (zdroj IP, cílová adresa IP), Vyrovnávání zatížení, sourceIPProtocol pro vyrovnávání zatížení 3-n-tici (zdroj IP cílová adresa IP protocol) nebo žádný požadovaná výchozí chování Vyrovnávání zatížení 5 n-tici.

Pomocí následujícího postupu načtení koncový bod zatížení vyrovnávání rozdělení režimu konfigurace:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15
    LoadBalancerDistribution : sourceIP

Pokud není k dispozici LoadBalancerDistribution element Vyrovnávání zatížení Azure používá algoritmus 5 n-tici výchozí.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Nastavení režimu rozdělení se sadou koncový bod Vyrovnávání zatížení

Pokud koncové body jsou součástí sady koncový bod Vyrovnávání zatížení, musí být režim rozdělení nastaven na stránce nastavit koncový bod rozloženy:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Konfigurace služby cloudu Změna režimu rozdělení.

Azure SDK pro .NET 2,5 (Chcete-li vydávat v listopadu) můžete využít k aktualizaci cloudové služby. Nastavení koncového bodu služby cloudu se provádí v .csdef. Aktualizujte si režimu rozdělení Vyrovnávání zatížení nasazení cloudové služby, je vyžadován upgrade nasazení.
Tady je příklad .csdef změny nastavení koncového bodu:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
    <InstanceAddress roleName="VMRolePersisted">
      <PublicIPs>
        <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
      </PublicIPs>
    </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="api-example"></a>Příklad rozhraní API

Můžete nakonfigurovat distribuce Vyrovnávání zatížení pomocí rozhraní API pro správu služby. Ujistěte se, pokud chcete přidat `x-ms-version` záhlaví je nastavený na verzi `2014-09-01` nebo vyšší.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Aktualizace konfigurace sady zadaný Vyrovnávání zatížení při nasazení

#### <a name="request-example"></a>Příklad požadavek

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

Hodnota LoadBalancerDistribution může být sourceIP 2 n-tici spřažení, sourceIPProtocol příbuznosti 3-n-tici nebo žádný (bez příbuznosti. tedy 5-n-tici)

#### <a name="response"></a>Odpověď

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Další kroky

[Přehled Vyrovnávání zatížení vnitřní](load-balancer-internal-overview.md)

[Začínáme konfigurace Internetové Vyrovnávání zatížení](load-balancer-get-started-internet-arm-ps.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)
