<properties
   pageTitle="Začít vytvářet internetové Vyrovnávání zatížení v klasické nasazení modelu pomocí služby cloudu | Microsoft Azure"
   description="Naučte se vytvářet internetové Vyrovnávání zatížení v modelu klasické nasazení služby cloudu"
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
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Začít vytvářet internetové Vyrovnávání zatížení služby cloudu

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tento článek popisuje modelu klasické nasazení. Můžete také [Naučte se vytvářet internetové Vyrovnávání zatížení pomocí Správce prostředků Azure](load-balancer-get-started-internet-arm-cli.md).

Cloudovým službám se konfigurují s Vyrovnávání zatížení a lze jej přizpůsobit pomocí modelu služby.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Vytvoření Vyrovnávání zatížení pomocí souboru definice služby

Můžete využít SDK Azure pro .NET 2,5 aktualizovat cloudové služby. Nastavení koncového bodu služby cloudu jsou určené v souboru .csdef [definice služby](https://msdn.microsoft.com/library/azure/gg557553.aspx).

Následující příklad ukazuje konfiguraci souboru servicedefinition.csdef cloudu nasazení:

Kontrola snipet .csdef souboru generovaného při nasazení cloudu, zobrazí se externí koncový bod nakonfigurovaný na používání porty protokolu HTTP na portu 10000, 10001 a 10002.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Kontrola stavu Vyrovnávání zatížení služby cloudu


Následující obrázek je příkladem zkušební stavu:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

Vyrovnávání zatížení kombinuje informace koncového bodu a informací o zkušební vytvořit adresu URL ve formě http://{DIP VM}:80/Probe.aspx, který slouží k vytvoření dotazu stav služby.

Služba rozpozná pravidelných sond z jedné IP adresy. Toto je žádost o zkušební stav pocházejících z hostitele uzel kterém běží virtuální počítač.
Služba obsahuje odpoví HTTP 200 stavový kód pro vyrovnávání zatížení k se předpokládá, že služba není správný. Všechny ostatní stavový kód HTTP (například 503) přímo trvá virtuálního počítače mimo otočení.

Definice zkušební také řídí četnost zkušební. V našem případě nahoře je Vyrovnávání zatížení zjišťování koncový bod každé 5 sekundy. Pokud žádná kladné odpověď přijato 10 sekund (intervaly dva zkušební), zkušební pokládá dolů a virtuální počítač, se považuje mimo otočení. Podobně jestliže službu je mimo otočení v prostoru a dostali odpovědi na kladné, službu se vloží zpět otočení hned. Pokud službu kolísající mezi správný a chybná, můžete se rozhodnout Vyrovnávání zatížení, zpozdit opětovné zavedení služby otočení v prostoru, dokud bylo správný počet sond.

Kontrola služby definice schématu [stavu zkušební](https://msdn.microsoft.com/library/azure/jj151530.aspx) Další informace.

## <a name="next-steps"></a>Další kroky

[Začínáme konfigurace vyrovnávání zatížení vnitřní](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace režimu rozdělení Vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)

