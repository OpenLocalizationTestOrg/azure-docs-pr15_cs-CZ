<properties
   pageTitle="Konfigurace upgrade aplikace služby struktury | Microsoft Azure"
   description="Zjistěte, jak nakonfigurovat nastavení upgradu služeb struktury aplikace pomocí technologie Microsoft Visual Studio."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Konfigurace upgrade aplikace služby struktury ve Visual Studiu

Visual Studio nástroje pro struktury služby Azure podporují upgradu publikování na místní nebo vzdálené clusterů. Existují dvě výhody upgradu na novější verzi místo nahrazení aplikace během testování a ladění aplikace:

- Data aplikace neztratí během upgradu.
- Dostupnost zůstane vysoký a nebudou žádné přerušení poskytování služeb během upgradu, pokud jsou tu uvedené, že dost instancí služby rozšířit mezi upgradu domény.

Testuje poběží proti aplikace během probíhá upgrade.

## <a name="parameters-needed-to-upgrade"></a>Parametry potřebné k upgradu

Můžete si vybrat z dva typy nasazení: normálního nebo upgradu. Běžná nasazení se vymažou všechny předchozí informace o instalaci a data clusteru, během upgradu nasazení a zachovat přitom ho. Při upgradu služeb struktury aplikace ve Visual Studiu, je třeba zadat, že upgrade parametry aplikace a stav najdete zásady. Upgrade parametry aplikace pomáhají určit upgradu, zatímco zásady Kontrola stavu zjistit, zda upgrade podaří. Najdete v článku [upgrade aplikace služby struktury: upgrade parametry](service-fabric-application-upgrade-parameters.md) další podrobnosti.

Existují tři upgradu režimy: *sledované* *UnmonitoredAuto*a *UnmonitoredManual*.

  - Upgrade sledované automaticky upgradu a kontrola stavu aplikace.

  - UnmonitoredAuto upgrade automaticky upgradu, ale přeskočí Kontrola stavu aplikace.

  - Když provedete UnmonitoredManual upgrade, budete muset ručně upgrade každé upgradu domény.

Každý upgradu režim vyžaduje různými sadami parametrů. V tématu Další informace o dostupných možnostech upgrade [aplikace upgradu parametry](service-fabric-application-upgrade-parameters.md) .

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Upgrade aplikace služby struktury ve Visual Studiu

Pokud používáte nástroje struktury služby Visual Studio k upgradu služeb struktury aplikace, můžete určit, publikovat procesu upgradu, nikoli na běžná nasazení zaškrtnutím políčka **upgradovat aplikaci** .

### <a name="to-configure-the-upgrade-parameters"></a>Konfigurace upgradu parametrů

1. Klikněte na tlačítko **Nastavení** vedle zaškrtávacího políčka. Zobrazí se dialogové okno **Upravit Upgrade parametry** . Dialogové okno **Upravit Upgrade parametry** podporuje upgradu režimy sledované, UnmonitoredAuto a UnmonitoredManual.

2. Vyberte režim upgradu, který chcete použít a potom vyplňte mřížce parametr.

    Každý parametr má výchozí hodnoty. Volitelný parametr *DefaultServiceTypeHealthPolicy* trvá vstupní tabulka hash. Tady je příklad formátu vstupní tabulka hash *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* je jiné volitelný parametr, který má vstupní tabulka hash ve formátu následující:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Tady je příklad reálné:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Pokud vyberete režim upgradu UnmonitoredManual, je třeba spustit ručně konzoly PowerShell pokračovat a dokončete proces upgradu. V nápovědě k [upgrade aplikace služby struktury: rozšířené témata](service-fabric-application-upgrade-advanced.md) se dozvíte, jak ruční upgrade funguje.

## <a name="upgrade-an-application-by-using-powershell"></a>Upgrade aplikace pomocí prostředí PowerShell

Pomocí rutin prostředí PowerShell k upgradu služeb struktury aplikace. Podrobné informace najdete v článku [upgrade kurz aplikace služby struktury](service-fabric-application-upgrade-tutorial.md) a [Start ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) .

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Zadání zásad Kontrola stavu do soubor aplikace

Každou službu v aplikaci služby struktury může mít vlastní stav zásad parametrů, které buď přijměte výchozí hodnoty. Můžete zadat tyto hodnoty parametrů v aplikaci soubor.

Následující příklad ukazuje, jak použít i zásady zaškrtněte jedinečné stavu pro každou službu v manifestu aplikace.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Další kroky
Další informace o nasazení aplikace najdete v článku [nasazení existující aplikace v struktury služby Azure](service-fabric-deploy-existing-app.md).
