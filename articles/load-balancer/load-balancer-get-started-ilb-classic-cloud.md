<properties
   pageTitle="Vytvoření vyrovnávání interní zavedení služby cloudu v modelu klasické nasazení | Microsoft Azure"
   description="Naučte se vytvářet Vyrovnávání zatížení interní pomocí Powershellu v modelu klasické nasazení"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Chcete vytvořit Vyrovnávání zatížení vnitřní (klasické) služby cloudu

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Konfigurace služby Vyrovnávání zatížení interní služby cloudu

Vyrovnávání zatížení interní je podporovaný pro virtuálních počítačích a cloudovým službám. Koncový bod vyrovnávání interní zatížení vytvořené v do cloudové služby, které zasahují mimo místní virtuální sítě budou mít přístup jenom do cloudové služby.

Konfigurace vyrovnávání zatížení vnitřní musí nastavit při vytváření prvního nasazení do cloudové služby, jak je vidět v následujícím příkladu.

>[AZURE.IMPORTANT] Aby bylo možné postupujte následujícím způsobem je virtuální síť vytvořen nasazení cloudu. Budete potřebovat virtuální síťový název a název podsítě pro vytvoření interního Vyrovnávání zatížení.

### <a name="step-1"></a>Krok 1

Otevřete soubor konfigurace služby (.cscfg) cloudu nasazení ve Visual Studiu a přidejte následující části Vytvoření vnitřní Vyrovnávání zatížení v části poslední "`</Role>`" položky ke konfiguraci sítě.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Přidejte do něj hodnoty konfiguračního souboru sítě zobrazíte, jak bude vypadat. V tomto příkladu se předpokládá, že jste vytvořili podsítě s názvem "test_vnet" se podsítě 10.0.0.0/24 s názvem test_subnet a statické IP 10.0.0.4. Vyrovnávání zatížení bude název testLB.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Další informace o schématu vyrovnávání zatížení najdete v článku [Přidání služba Vyrovnávání zatížení](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Krok 2


Změna soubor definice (.csdef) služby přidáte interní Vyrovnávání zatížení koncové body. Okamžiku, kdy se vytvořit role instance, soubor definice služby přidá do interního Vyrovnávání zatížení instance role.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Po stejné hodnoty v předchozím příkladu přidáte soubor definice služby hodnoty.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Síťová komunikace budou rozloženy pomocí vyrovnávání zatížení testLB pomocí portu 80 pro příchozí žádosti posílat instancí pracovního role taky na portu 80.


## <a name="next-steps"></a>Další kroky

[Konfigurace režimu rozdělení Vyrovnávání zatížení pomocí spřažení IP zdroje](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)