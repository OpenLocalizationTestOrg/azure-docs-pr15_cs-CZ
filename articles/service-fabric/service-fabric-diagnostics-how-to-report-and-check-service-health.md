<properties
   pageTitle="Vykazovat a kontrola stavu s struktury služby Azure | Microsoft Azure"
   description="Zjistěte, jak posílání sestav stavu z kód služby a jak se dá zjistit stavu služby pomocí nástroje pro sledování stavu, které poskytuje struktury služby Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Sestavy a zkontrolujte stav služby
Když služby docházet k problémům, možnost Odpovědět a oprava událostí a výpadků závisí na možnost rychle zjišťování problémů. Pokud hlášení problémů a chyb stavu vedoucímu struktury služby Azure z kód služby můžete použít standardní stav sledování nástroje, které služby struktury poskytuje pro kontrolu stavu.

Existují dva způsoby, aby mohli vykazovat stav služby:

- Pomocí [oddílu](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) nebo [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) objektů.  
Můžete použít `Partition` a `CodePackageActivationContext` objekty vykazování stavu prvky, které jsou součástí aktuální kontext. Například kód spuštěný jako součást otevřené můžete vykázat stavu pouze tento otevřené oddílu, který patří a aplikace, která je součástí.

- Použití `FabricClient`.   
Můžete použít `FabricClient` na stav sestavy z kód služby nejsou-li clusteru [zabezpečené](service-fabric-cluster-security.md) nebo pokud se službou s oprávněními správce. To nebude platit ve většině případů reálný. S `FabricClient`, je možné vykazovat stav ve osobě, která je součástí clusteru. V ideálním případě však kód služby měli jenom odesílat zprávy, které se vztahují k vlastním stavu.

Tento článek vás provede příklad sestav stavu z kód služby. Také příklad použití nástroje, které poskytuje struktury služby pro kontrolu stavu. Tento článek je určený je rychlý úvod a možnosti služby struktury sledování stavu. Podrobnější informace si můžete přečíst řadu článků podrobné informace o stavu, které začínají na odkaz na konci tohoto článku.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Je nutné mít nainstalovaný následující:

   * Visual Studio 2015
   * Služba struktury SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Vytvoření místní zabezpečené vývojáře obrázku
- Otevřete prostředí PowerShell s oprávněními správce a následující příkazy.

![Příkazy, které ukazují, jak vytvořit zabezpečené vývojáře obrázku](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Nasazení aplikace a kontrola jeho stavu

1. Otevřete aplikaci Visual Studio jako správce.

2. Vytvoření projektu pomocí šablony **Stavová služba** .

    ![Vytvoření aplikace služby struktury s stavová služba](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Stisknutím klávesy **F5** spustit aplikaci v režimu ladění. Aplikace má být nasazené na místní obrázku.

4. Po spuštění aplikace klikněte pravým tlačítkem myši na ikonu místní clusteru správce v oznamovací oblasti a vyberte **Spravovat místní Cluster** v místní nabídce otevřete Průzkumníka struktury služby.

    ![Spusťte Průzkumníka struktury služby z oznamovací oblasti](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. Aplikace stavu mají být zobrazeny jako tento obrázek. V současné době by měl být aplikace správný bez chyb.

    ![Správný aplikace v Průzkumníku struktury služby](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. Můžete taky zaškrtnout stavu pomocí prostředí PowerShell. Můžete použít ```Get-ServiceFabricApplicationHealth``` pro kontrolu stavu aplikace který můžete použít ```Get-ServiceFabricServiceHealth``` zkontrolovat stav služby. Sestava stavu pro stejný aplikací v prostředí PowerShell je na tomto obrázku.

    ![Správný aplikací v prostředí PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Chcete-li přidat vlastní stavu události kód služby
Šablony projektů služby struktury ve Visual Studiu obsahovat ukázkový kód. Následující postup, jak můžete nahlásit vlastní stavu události z kód služby. Tyto zprávy se automaticky objeví v standardní nástroje pro sledování stavu, že služba struktury obsahuje například služba struktury Průzkumníka, zobrazení Azure portálu stavu a Powershellu.

1. Aplikaci, kterou jste vytvořili dříve ve Visual Studiu znovu otevřete nebo vytvořte novou aplikaci pomocí šablony aplikace Visual Studio **Stavová služba** .

2. Otevřete soubor Stateful1.cs a vyhledejte `myDictionary.TryGetValueAsync` volání `RunAsync` metody. Uvidíte, že tato metoda vrátí `result` , který obsahuje aktuální hodnotu čítače, protože klíčové logika v této aplikaci je zachovat počet spuštěný. Pokud to bylo reálné aplikaci a chybějící výsledek se nepovede, by chcete označit příznakem dané události.

3. Vykazování stavu událost nastavit jako když chybějící výsledek představuje se nepovede, přidejte následující kroky.

    na. Přidejte `System.Fabric.Health` názvů Stateful1.cs soubor.

    ```csharp
    using System.Fabric.Health;
    ```

    b. Přidejte následující kód po `myDictionary.TryGetValueAsync` volání

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Jsme Generovat otevřené stavu, protože vykazuje stavové služby. `HealthInformation` Parametr informacemi o stavu problém, který vykazuje.

    Pokud jste vytvořili příslušnosti služby, použijte následující kód

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Pokud vaše se službou s oprávněními správce nebo pokud clusteru není [bezpečný](service-fabric-cluster-security.md), můžete taky použít `FabricClient` na sestavy stavu podle následujících kroků.  

    na. Vytvoření `FabricClient` instance po `var myDictionary` deklarace.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    b. Přidejte následující kód po `myDictionary.TryGetValueAsync` volání.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Pojďme simulovat tuto chybu a zobrazit v nástroji Sledování stavu se zobrazují. Tak, aby napodobily nepovede, komentář, první řádek v kódu vytváření sestav stavu, který jste přidali výše. Po komentář první řádek kód může vypadat jako v následujícím příkladu.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Tento kód bude teď platit tato sestava stavu pokaždé, když `RunAsync` spustí. Po provedení změn, stiskněte klávesu **F5** ke spuštění aplikace.

6. Po spuštění aplikace otevřete Průzkumníka struktury služby a kontrola stavu aplikace. Tentokrát služby struktury Explorer řádku se zobrazí, že aplikace je chybná. Toto je kvůli chybu, která byla nahlášené z kód jsme přidali dříve.

    ![Nefunkční aplikace v Průzkumníku struktury služby](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Pokud vyberete primární otevřené ve stromovém zobrazení Průzkumníka struktury služby, uvidíte **Stavu** příliš označuje k chybě. Taky zobrazí podrobnosti sestavy stavu, které jste přidali do služby struktury Explorer `HealthInformation` parametr v kódu. Zobrazí se stejnými sestavami stavu v prostředí PowerShell a portálu Azure.

    ![Stav otevřené v Průzkumníkovi struktury služby](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Tuhle sestavu zůstanou v správce stavu, dokud se nahradí jiné sestavy nebo dokud se neodstraní tento otevřené. Protože jsme nenastavil `TimeToLive` této sestavy stavu `HealthInformation` objektu, nikdy nevyprší sestavy.

Doporučujeme, aby stavu vykazuje se na úrovni nejčastěji podrobného, které v tomto případě je otevřené. Také je možné vykazovat stav ve `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Do sestavy stavu na `Application`, `DeployedApplication`, a `DeployedServicePackage`, použijte `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Další kroky
[Hloubkové postupy na stav služby struktury](service-fabric-health-introduction.md)
