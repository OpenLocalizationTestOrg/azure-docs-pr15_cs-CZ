<properties
    pageTitle="Ladění Azure aplikací v zatmění"
    description="Informace o ladění Azure aplikace, které používají Azure sada nástrojů pro Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690949.aspx -->

# <a name="debugging-azure-applications-in-eclipse"></a>Ladění Azure aplikací v zatmění #

Aplikace můžete ladění pomocí nástrojů Azure zatmění, zda používají v Azure nebo ve výpočetním emulátoru Pokud používáte operační systém Windows. Následující obrázek znázorňuje dialogové okno slouží k povolení vzdáleného ladění vlastnosti **ladění** :

![][ic719504]

Tento kurz se předpokládá, že už máte aplikace, která byla úspěšně vytvořila a se seznámíte s emulátoru výpočetní a nasazení Azure.

Budeme používat aplikaci z kurzu [používání knihovny Runtime služby Azure v JSP][] jako výchozí bod pro v tomto tématu. Než budete pokračovat, vytvořte aplikace, pokud jste tak již neučinili.

## <a name="to-debug-your-application-while-running-in-azure"></a>Ladění aplikace při spuštění v Azure ##

>[AZURE.WARNING] Tato sada aktuální podpory pro vzdálené ladění Java určeny pro nasazení spuštěné v Azure výpočetním emulátoru. Protože ladění připojení není zabezpečené, neměli byste povolit vzdálené ladění ve výrobním nasazení. Pokud potřebujete ladění aplikace spuštěné v Azure cloudu, použijte pracovní nasazení, ale zjistili neoprávněnému přístupu k relaci ladění je možné v případě, že někdo zná IP adresu nasazením cloudu, i když je pracovní nasazení.

1. Vytvoření projektu pro účely testování v emulátor: Průzkumník projektu v zatmění prvku, klikněte pravým tlačítkem myši **MyAzureProject**, klikněte na příkaz **Vlastnosti**, klikněte na **Azure**a nastavte **vytvořit pro** nasazení **do cloudu**.
1. Znovu vytvořte projekt: V nabídce zatmění klikněte na kartu **projekt**a potom klikněte na **Vytvořit všechny**.
1. Nasazení aplikace pro *přípravu* v Azure.
    >[AZURE.IMPORTANT] Výše uvedené, doporučujeme ladění ve výpočetním emulátoru ve většině případů a jenom v případě, že není potřeba další ladění ladění v testovacím prostředí. Doporučujeme není ladění v provozním prostředí.
1. Po instalaci je připraven v Azure, získání názvu DNS pro nasazení na [Portálu Správa Azure][]. Přípravná nasazení má název DNS ve formě http://*&lt;guid&gt;*. cloudapp.net, kde * &lt;guid&gt; * je hodnota GUID přidělil Azure.
1. V Průzkumníkovi na zatmění projektu klikněte pravým tlačítkem myši **WorkerRole1**a **Azure**klikněte na **ladění**.
1. V dialogovém okně **Vlastnosti pro WorkerRole1 ladění** :
    1. Kontrola **Povolení vzdáleného ladění pro tuto roli.**
    1. **Koncový bod vstupní používat**když použijete **ladění (veřejné: 8090, soukromé: 8090)**.
    1. Ujistěte se, že je zrušené zaškrtnutí políčka **Spustit JVM v pozastaveném režimu, čeká se na ladění připojení** .
        >[AZURE.IMPORTANT] Možnost **Začít JVM v pozastaveném režimu, čeká se na připojení ladění** je určená pro pokročilé ladění scénářích výpočetním emulátoru pouze (ne pro nasazení cloudu). Pokud je použita možnost **Spustit JVM v pozastaveném režimu, čeká se na ladění připojení** , bude pozastavit spouštění serveru, dokud ladění zatmění je připojen k jeho JVM. Během ladění relace pomocí emulátoru výpočetním můžete použít tuto možnost, nelze použít pro relaci ladění v cloudu nasazení. Úkol Azure spuštění probíhá inicializace serveru a Azure cloudu není zpřístupnit veřejný koncové body až do dokončení úkolu po spuštění. Proto spuštění procesu nebude úspěšně dokončena Pokud tato možnost je povolena v cloudu nasazení, protože nebude moct přijímat připojení z externích zatmění klienta.
1. Klikněte na **vytvořit ladění konfigurace**.
1. V dialogovém okně **Konfigurace ladění Azure** :
    1. **Projekt Java ladění**vyberte **MyHelloWorld** projekt.
    1. **Konfigurovat ladění pro**najdete v **Azure cloud (pracovní)**.
    1. Zajistěte, aby **že Azure výpočet emulátoru** není zaškrtnuté.
    1. **Host (hostitel)**zadejte název DNS fázované nasazení, ale bez předchozího **http://**. Příklad (podržením GUID místo GUID zde zobrazeném): **4e616d65 6f6e-6 2D 65-6973-526f62657274.cloudapp.net**
1. Klikněte na **OK** zavřete dialogové okno **Konfigurace ladění Azure** .
1. Klikněte na **OK** zavřete dialogové okno **Vlastnosti WorkerRole1 ladění** .
1. Pokud nemáte zarážku už nastavení v index.jsp, nastavte ho:
    1. V rámci jeho zatmění Průzkumník projektu rozbalte **MyHelloWorld**, rozbalte položku **obsah webu**a poklikejte na **index.jsp**.
    1. V rámci index.jsp klikněte pravým tlačítkem myši na modrém panelu vlevo od kódu jazyka Java a klikněte na **Přepínač zarážky**, jak je vidět v následujících tématech: ![][ic551537]
1. V rámci zatmění v nabídce na příkaz **Spustit** a potom na **Ladění konfigurace**.
1. V dialogovém okně **Konfigurace ladění** rozbalte **Vzdálená Java aplikace** v levém podokně, vyberte **Azure Cloud (WorkerRole1)**a klepněte **ladění**.
1. V prohlížeči, spusťte aplikaci fázované **http://***&lt;guid&gt;***.cloudapp.net/MyHelloWorld**, zaokrouhlením GUID od názvu DNS pro * &lt;guid&gt;*. Pokud se zobrazí výzva v dialogovém okně **Potvrďte přepínač perspektivy** , klikněte na **Ano**. Relaci ladění nyní spustit řádek kódu, kde jste nastavili zarážce.

>[AZURE.NOTE] Pokud se při pokusu o spuštění vzdáleného ladění připojení k nasazení, které obsahuje více role instance spuštěna nelze ovládáte aktuálně která instance ladění bude možné původně připojení k, jako Vyrovnávání zatížení Azure vyberte instanci náhodně. Po vašem připojení pomocí této instance, ale, vám budou zasílané ladění stejné instanci. Poznámka: Pokud určité době nečinnosti delší než 4 minut (například pokud jste zastavení na zarážce moc dlouho) se také může Azure ukončit připojení.

## <a name="debugging-a-specific-role-instance-in-a-multi-instance-deployment"></a>Ladění instanci konkrétní rolí v více instancí nasazení ##

Při nasazení běží v cloudu, nejspíš spustíte ho ve více výpočetním, nebo role instance. Díky tomu vám umožní využít výhod Azure 99.95 % dostupnost záruky a rozšiřování aplikace.

V takovém případě budete muset vzdáleně ladění aplikace Java v instanci konkrétní rolí. Ale pokud povolíte pouze běžná vstupní koncový bod pro ladění, Vyrovnávání zatížení Azure bude znemožňují prakticky připojení ladění instanci konkrétní rolí. Místo toho můžete se připojí instanci role, která použije náhodně.

Toto je typ scénář kde využijete vstupní koncové body instance usnadní za vás ladění instanci konkrétní rolí.

Řekněme, že chcete spustit až na 5 role výskyty nasazení. Na stránce vlastností **koncové body** v dialogovém okně Vlastnosti role, vytvoření koncového bodu vstupní instance a přiřadit ji oblasti veřejné porty, spíše než jeden port číslo. V poli vstupní **veřejné port** zadejte **81 85**.

Po nasazení aplikace s tímto koncovým bodem instance Azure přiřadí jedinečné číslo portu z této oblasti všech instancí role. Pokud chcete zjistit, která instance máte přiřazené číslo portu, můžete pomocí proměnnou *InstanceEndpointName***_PUBLICPORT** prostředí (kde *InstanceEndpointName* je název jste přiřadili při vytvoření koncového bodu instance) automaticky konfigurované pomocí nástrojů ve vašem nasazení (například vrácením její hodnotu do zápatí na webovou stránku tak, aby mohl čitelné, když přejdete na ni).

Když víte, jaké číslo portu veřejné přidělené vybraný výskyt, můžete odkázat ho v konfiguraci ladění v zatmění, tak, že připevnění na název hostitele služby. To vám umožní ladění zatmění připojení k této konkrétní instanci a všechny další instance.

## <a name="windows-only-to-debug-your-application-while-running-in-the-compute-emulator"></a>Pouze ve Windows: ladění aplikace při spuštění ve výpočetním emulátoru ##

>[AZURE.NOTE] Azure emulátoru je k dispozici pouze v systému Windows. Tuto část přeskočte, pokud používáte operační systém než Windows. 

1. Vytvoření projektu pro účely testování v emulátor: Průzkumník projektu v zatmění prvku, klikněte pravým tlačítkem myši **MyAzureProject**, klikněte na **Vlastnosti**, klikněte na **Azure**a nastavte **vytvořit pro** k **testování v emulátoru**.
1. Znovu vytvořte projekt: V nabídce zatmění klikněte na kartu **projekt**a potom klikněte na **Vytvořit všechny**.
1. V Průzkumníkovi na zatmění projektu klikněte pravým tlačítkem myši **WorkerRole1**a **Azure**klikněte na **ladění**.
1. V dialogovém okně **Vlastnosti pro WorkerRole1 ladění** :
    1. Kontrola **Povolení vzdáleného ladění pro tuto roli.**
    1. **Koncový bod vstupní používat**když použijete výchozí koncový bod automaticky generované sada nástrojů, uvedené jako **ladění (veřejné: 8090, soukromé: 8090)**.
    1. Zajistěte, že je zrušené zaškrtnutí políčka možnost **Spustit JVM v pozastaveném režimu, čeká se na ladění připojení** .
        >[AZURE.IMPORTANT] Možnost **Začít JVM v pozastaveném režimu, čeká se na připojení ladění** je určená pro pokročilé ladění scénářích výpočetním emulátoru pouze (ne pro nasazení cloudu). Pokud je použita možnost **Spustit JVM v pozastaveném režimu, čeká se na ladění připojení** , bude pozastavit spouštění serveru, dokud ladění zatmění je připojen k jeho JVM. Během ladění relace pomocí emulátoru výpočetní můžete použít tuto možnost, nelze použít pro relaci ladění v cloudu nasazení. Úkol Azure spuštění probíhá inicializace serveru a Azure cloudu není zpřístupnit veřejný koncové body až do dokončení úkolu po spuštění. Proto spuštění procesu nebude úspěšně dokončena Pokud tato možnost je povolena v cloudu nasazení, protože nebude moct přijímat připojení z externích zatmění klienta.
1. Klikněte na **vytvořit ladění konfigurace**.
1. V dialogovém okně **Konfigurace ladění Azure** :
    1. **Projekt Java ladění**vyberte **MyHelloWorld** projekt.
    1. **Konfigurovat ladění pro**najdete v **Azure výpočet emulátoru**.
1. Klikněte na **OK** zavřete dialogové okno **Konfigurace ladění Azure** .
1. Klikněte na **OK** zavřete dialogové okno **Vlastnosti WorkerRole1 ladění** .
1. Nastavte zarážky index.jsp:
    1. V rámci jeho zatmění Průzkumník projektu rozbalte **MyHelloWorld**, rozbalte položku **obsah webu**a poklikejte na **index.jsp**.
    1. V rámci index.jsp klikněte pravým tlačítkem myši na modrém panelu vlevo od kódu jazyka Java a klikněte na **Přepínač zarážky**, jak je vidět v následujících tématech: ![][ic551537]

       Pokud se zobrazí ikona zarážce v rámci modrém panelu vlevo od kódu jazyka Java je nastavena zarážka.
1. Spuštění aplikace ve výpočetním emulátoru kliknutím na tlačítko **Spustit v Azure emulátoru** na Azure nástrojů.
1. V rámci zatmění v nabídce na příkaz **Spustit** a potom na **Ladění konfigurace**.
1. V dialogovém okně **Konfigurace ladění** rozbalte **Vzdálená Java aplikace** v levém podokně, vyberte **Azure emulátoru (WorkerRole1)**a klikněte na **ladění**.
1. Po emulátoru výpočetním označuje, že aplikace běží v prohlížeči spustíte **http://localhost:8080/MyHelloWorld**.
    Pokud se zobrazí výzva v dialogovém okně **Potvrďte perspektivy přepnout** , klepněte na tlačítko **Ano**.
    Relaci ladění teď spustit řádek kódu, kde jste nastavili zarážce.

To ukázal, jak ladění ve výpočetním emulátoru; Následující části se dozvíte, jak ladění aplikace používaný Azure.

## <a name="debugging-notes"></a>Ladění poznámek ##

* Po ladění, můžete přepnout perspektivy z **ladění** **Java** prostřednictvím po kliknutí na nabídku zatmění prvku kliknutím **okno** **Otevřít perspektivy**a výběrem perspektivy, který chcete použít.
* Povolení vzdáleného ladění GlassFish, nepoužívejte pro Eclipse vzdálené ladění funkci Konfigurace sady nástrojů Azure. Místo toho GlassFish ruční konfigurace. Vzhledem ke způsobu GlassFish zpracuje možnosti jazyka Java předdefinované proměnné tuto sadu nástrojů vzdáleného ladění konfigurace funkce nefunguje správně s GlassFish. Jestliže sada nástrojů vzdáleného ladění konfigurace zapnutý, ho můžete zabránit GlassFish spuštění.

## <a name="see-also"></a>Viz taky ##

[Azure sada nástrojů pro Eclipse][]

[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění][]

[Instalace Azure sada nástrojů pro Eclipse][] 

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure][].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Portál Správa Azure]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Použití knihovny Runtime služby Azure v JSP]: http://go.microsoft.com/fwlink/?LinkID=699551

<!-- IMG List -->

[ic719504]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic719504.png
[ic551537]: ./media/azure-toolkit-for-eclipse-debugging-azure-applications/ic551537.png
