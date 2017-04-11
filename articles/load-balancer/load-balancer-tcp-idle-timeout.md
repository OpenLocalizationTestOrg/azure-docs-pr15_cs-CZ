<properties
   pageTitle="Konfigurace nečinnosti TCP Vyrovnávání zatížení | Microsoft Azure"
   description="Konfigurace nečinnosti TCP Vyrovnávání zatížení"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Konfigurace nastavení nečinnosti TCP pro vyrovnávání zatížení Azure

Ve výchozím nastavení Vyrovnávání zatížení Azure má nečinnosti 4 minut. Pokud je delší než hodnotu časového limitu určité době nečinnosti, není nijak zaručené udržované relace TCP a HTTP mezi klientem a cloudové služby.

Když je zavřený připojení vaší klientské aplikace může zobrazit tato chybová zpráva: "zavřený podkladového připojení: připojení očekávaného zůstat aktivní zavřel serveru."

Běžné postup je použít TCP udržování. Tento postup pořád připojení aktivní delší dobu. Další informace najdete v tématu tyto [příklady .NET](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Udržování povoleno, pakety jsou odeslány době nečinnosti na připojení. Udržování pakety zajistit, aby nečinnosti nikdy limit a připojení trvá dlouho.

Toto nastavení funguje pro příchozí připojení. Chcete-li předejít ztrátě připojení, musíte nakonfigurovat interval menší než nastavení nečinnosti TCP udržování nebo prodloužit nedělá časový limit. K podpoře scénářů, jsme přidali podporu, která dokáže nahradit nečinnosti. Teď se můžete nastavit po dobu 4 až 30 minut.

TCP udržování vhodný pro scénáře životností není-li omezení. Není vhodné pro mobilní aplikace. Pomocí protokolu TCP udržování v mobilní aplikaci může dojít battery zařízení k rychleji.

![Časový limit TCP](./media/load-balancer-tcp-idle-timeout/image1.png)

Následující části popisují, jak změnit nastavení nečinnosti ve virtuálních počítačích a cloudových služeb.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>Nastavení limitu TCP pro veřejnou IP úrovni instance až 15 minut

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`je nepovinný krok. Pokud není nastavena, výchozí časový limit je 4 minut. Rozsah přijatelné časový limit je 4 až 30 minut.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Při vytváření Azure koncového bodu na počítač virtuální nastavit časový limit nečinnosti

Změna nastavení časového limitu koncový bod, následujícím způsobem:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

K načtení nečinnosti konfigurace, použijte tento příkaz:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
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

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Nastavení limitu TCP se sadou Vyrovnávání zatížení koncový bod

Pokud se koncové body jsou součástí sady Vyrovnávání zatížení koncový bod, musí být časový limit TCP nastavená na stránce nastavit koncový bod Vyrovnávání zatížení. Příklad:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Změna nastavení časového limitu služby cloudu

Azure SDK slouží k aktualizaci cloudové služby. Můžete nastavit koncový bod služby cloudu v souboru .csdef. Aktualizace TCP časový limit pro nasazení do cloudové služby vyžaduje upgradu na nasazení. Není-li časový limit TCP zadán pouze pro veřejné IP má výjimku. Nastavení na veřejném IP jsou v souboru .cscfg a můžete aktualizovat prostřednictvím nasazení aktualizace a upgrade.

Jsou .csdef změny nastavení koncového bodu:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

Jsou .cscfg změny nastavení časového limitu na veřejnou IP adresy:

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

## <a name="rest-api-example"></a>Příklad rozhraní REST API

Konfigurace nečinnosti TCP pomocí rozhraní API pro správu služby. Ujistěte se, že `x-ms-version` záhlaví je nastavený na verzi `2014-06-01` nebo novější. Aktualizujte konfiguraci zadaný Vyrovnávání zatížení vstupní koncové body na všechny virtuálních počítačích v nasazení.

### <a name="request"></a>Žádost o

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Odpověď

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Další kroky

[Přehled Vyrovnávání zatížení vnitřní](load-balancer-internal-overview.md)

[Začínáme konfigurace vyrovnávání zatížení internetového](load-balancer-get-started-internet-arm-ps.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)
