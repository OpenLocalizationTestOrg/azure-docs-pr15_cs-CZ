<properties
   pageTitle="Správa více prostředí v služby struktury | Microsoft Azure"
   description="Aplikace služby struktury je možné spouštět na clusterů, které oblasti velikost v jednom počítači k tisíce počítačích. V některých případech můžete nakonfigurovat aplikaci jinak na tyto paletami prostředí. Tento článek popisuje, jak definovat parametry jiné aplikaci na prostředí."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Správa parametry aplikace na více prostředí

Vytvoření clusterů struktury služby Azure pomocí kdekoli od jedna ku mnoha tisíce počítačích. Během binární soubory aplikace fungovat beze změny v tomto široké spektru prostředí, často chcete nakonfigurovat aplikaci odlišně v závislosti na počtu počítačů, které jste nasazení.

Jako příklad jednoduchého zvažte `InstanceCount` příslušnosti služby. Při spuštění aplikace v Azure, bude obecně chcete nastavit tento parametr zvláštní hodnotu "-1". Zajistíte tím, že vaše služba běží v každém uzlu clusteru. Této konfiguraci je však není vhodný pro jeden počítač cluster od nesmí obsahovat více procesů listening na stejné koncového bodu v jednom počítači. Místo toho, obvykle nastavíte `InstanceCount` "1".

## <a name="specifying-environment-specific-parameters"></a>Určení parametry specifické prostředí

Řešení pro tento problém s konfigurací je sada parametry výchozích služeb a soubory parametr aplikací zadejte tyto hodnoty parametrů pro dané prostředí. Manifesty aplikace a služby konfigurovat výchozí services a aplikaci parametry. Definice schématu pro soubory ServiceManifest.xml a ApplicationManifest.xml je instalována jako součást SDK struktury služby a nástroje pro *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Výchozí služby

Aplikace služby struktury se skládají z kolekce instancí služby. Je možné vytvořit prázdnou aplikaci a vytvořte všechny instance služby dynamicky, mít většiny aplikací sady core Services, které mají být vytvořeny vždy při vytváření instance aplikace. Tyto označovány jako "výchozí služby". Jsou uvedeny v manifest aplikace se zástupnými symboly pro konfiguraci za prostředí zahrnuté v hranatých závorkách:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Každý z pojmenované parametrů je třeba definovat v rámci elementu parametry manifestu aplikace:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Atribut VýchozíHodnota Určuje hodnotu se nemusí používat v nepřítomnosti parametru informace specifické pro dané prostředí.

>[AZURE.NOTE] Všechny parametry instanci služby jsou vhodné pro konfiguraci za prostředí. Ve výše uvedeném příkladu jsou hodnoty LowKey a HighKey rozdělení schématu služby explicitně definované pro všechny instance služby od oblasti oddílu je funkce dat doména není prostředí.


### <a name="per-environment-service-configuration-settings"></a>Konfigurace nastavení služby za prostředí

[Model služby struktury aplikace](service-fabric-application-model.md) umožňuje služby zahrnout balíčky konfigurace, které obsahují vlastní klíč dvojice, které jsou čitelné za běhu. Hodnoty toto nastavení můžete taky rozdělí prostředím zadáním `ConfigOverride` v manifestu aplikace.

Předpokládejme, že máte následující nastavení v souboru Config\Settings.xml `Stateful1` služby:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Přepsat tuto hodnotu pro konkrétní aplikaci nebo prostředí dvojici, vytvořit `ConfigOverride` při importu manifest služby v manifestu aplikace.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Tento parametr potom je možné konfigurovat tak, že prostředí uvedené výše. Lze provést kliknutím deklarace v části Parametry manifestu aplikace a zadáním prostředí specifické hodnoty v parametru soubory aplikace.

>[AZURE.NOTE] V případě nastavení konfigurace služeb, jsou tři místa, kde můžete nastavit hodnoty klíče: balíček konfigurace služeb, manifest aplikace a parametr souboru aplikace. Služba struktury vždy vybírat souboru parametr aplikace první (Pokud je zadána), potom manifest aplikace a nakonec balíček konfigurace.


### <a name="application-parameter-files"></a>Soubory aplikace parametrů

Projekt aplikace služby struktury můžete zahrnout jeden nebo víc souborů parametr aplikace. Každý z nich definuje určitých hodnot pro parametrům definovaným v manifestu aplikace:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Ve výchozím nastavení novou aplikaci obsahuje dva parametr soubory aplikace s názvem Local.xml a Cloud.xml:

![Soubory aplikace parametrů v Průzkumníku řešení][app-parameters-solution-explorer]

K vytvoření nového souboru parametr, jednoduše zkopírovat a vložit existující úrovně a zadejte nový název.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Identifikace parametry specifické prostředí během nasazení

Při nasazení budete muset zvolte soubor odpovídající parametr použít s aplikací. Lze provést pomocí dialogu Publikovat ve Visual Studiu nebo pomocí Powershellu.

### <a name="deploy-from-visual-studio"></a>Nasazení z aplikace Visual Studio

Můžete ze seznamu dostupných parametr souborů, když publikování aplikace ve Visual Studiu.

![Vyberte soubor parametrů v dialogovém okně Publikovat][publishdialog]

### <a name="deploy-from-powershell"></a>Nasazení z prostředí PowerShell

`Deploy-FabricApplication.ps1` Zahrnut v šabloně aplikace project skript Powershellu přijme profil publikovat jako parametr a PublishProfile obsahuje odkaz na soubor parametry aplikace.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Další kroky

Další informace o některých základní koncepty, které jsou uvedené v tomto tématu najdete v tématu [služby struktury technický přehled](service-fabric-technical-overview.md). Informace o možnostech správy ostatní aplikace, které jsou k dispozici ve Visual Studiu najdete v tématu [Správa aplikací služby struktury ve Visual Studiu](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
