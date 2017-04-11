<properties 
   pageTitle="Poznámky k verzi Azure SDK pro .NET 2.6" 
   description="Poznámky k verzi Azure SDK pro .NET 2.6" 
   services="app-service/web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

 
# <a name="azure-sdk-for-net-26-release-notes"></a>Poznámky k verzi Azure SDK pro .NET 2.6

Tento dokument obsahuje poznámky k verzi pro SDK Azure .NET 2.6 verzi. 

S Azure SDK 2.6 můžete vyvíjet cloudové služby aplikací (PaaS) .NET 4.5.2 nebo .NET 4.6 za předpokladu, že ruční instalace cílové .NET Framework rolí služby cloudu. V tématu [instalace .NET rolí služby cloudu](http://go.microsoft.com/fwlink/?LinkID=309796).


##<a name="service-bus-updates"></a>Aktualizace služby Bus

- Rozbočovače událostí: 

    - Teď můžete ovládat cílové aplikace access při odesílání události vystavením koncový bod další aplikace publisher pro rozbočovače události.
    - Další stability a zlepšování přidána do funkce rozbočovače události.
    - Přidání podpory Amqp protokolu přes WebSocket pro zasílání zpráv a rozbočovače události.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>Aktualizuje HDInsight Tools for Visual Studio

- **Rozšíření technologie IntelliSense**: návrh vzdálené metadat

    HDInsight Tools for Visual Studio nyní podporuje dosáhnout toho, aby vzdálené metadat při úpravách podregistru skriptu. Například, můžete zadat * *Vyberte* od** a zobrazí se všechny názvy tabulek. Také názvy sloupců se zobrazí po zadání tabulky.

- **Podpora emulátoru HDInsight**

    HDInsight Tools for Visual Studio podporuje připojení k emulátoru Hdinsightu, tak, aby mohl vyvíjíte skriptů podregistru místně bez představení náklady, potom spustit tyto skripty proti clusterů HDInsight. 

    Další informace naleznete v [této příručce](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

- **HDInsight Tools for Visual Studio podpory obecný Hadoop clusterů** (Verze preview)

    HDInsight Tools for Visual Studio podporuje obecný Hadoop clusterů, takže HDInsight Tools for Visual Studio můžete provést následující akce:

    - připojení k svůj cluster 
    - zápis podregistru dotazu s Rozšířená podpora technologie IntelliSense/automatického dokončování 
    - Zobrazte všechny úlohy v clusteru s intuitivní uživatelské rozhraní. 

    Další informace naleznete v [této příručce](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

##<a name="in-role-cache-updates"></a>Aktualizace v roli mezipaměti

- **V roli mezipaměti** aktualizovaném používat **Microsoft Azure úložiště SDK** verze 4.3. Až používal **v roli mezipaměti** Azure úložiště SDK verzi 1.7.

    Zákazníci pomocí 2,5 SDK Azure nebo pod by měl aktualizovat Azure SDK 2.6 a přesunutí novou verzi Azure úložiště SDK. 

    V současné době úložišti Azure verze 2011-08-18 plánuje odeberou 1 srpen 2016. Všechny migrací v roli mezipaměti z Azure SDK 2,5 nebo nižší pro 2.6 musí být úplný tak, že tentokrát. Další informace o vyřazování webů Azure úložiště verze 2011-08-18 najdete v tématu [aktualizace verze odebrání úložiště služby Microsoft Azure: rozšíření 2016](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]Jsme jste oznamující vyřazování webů 30 listopad 2016 služba spravovaných mezipaměti Azure a Azure v roli mezipaměti. Doporučujeme migrovat do mezipaměti Redis Azure při přípravě na tento důchodové pojištění. Další informace o kalendářních dat a migrace pokyny najdete v tématu [Nabízení Azure mezipaměti které je pro mě nejlepší?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Nástroje služby Azure aplikace

Byly aktualizovány následující položky ve verzi Azure SDK 2.6.

- Azure publikování rozšířeného zahrnout Azure rozhraní API aplikace jako cíl nasazení.
- Rozhraní API aplikace zřizování funkce umožňuje uživatelům s funkcí vytváření a zřízení rozhraní API aplikace.
- Průzkumník serveru změní tak, aby odrážely novou aplikaci služby uzel s aplikací pro Web, mobilní telefon a rozhraní API seskupené podle pole Skupina zdroje.
- Přidání gesto Azure klientského rozhraní API aplikace přidá na většině C# projekty, které vám umožní automatické generování Swagger s podporou rozhraní API aplikace spuštěné v Azure předplatnému uživatele.
- Rozhraní API aplikace nástrojů a aplikací služby uzlů v Průzkumníku serveru jsou k dispozici ve Visual Studiu 2013 pouze. 

##<a name="azure-resource-manager-tools-updates"></a>Azure aktualizace nástroje pro správu zdrojů

Byly aktualizovány nástrojů Správce prostředků Azure včetně šablon na virtuálních počítačích, sítě a úložiště. Byl aktualizován JSON možnosti úprav zahrnout nové zobrazení osnovy šablony a možnost upravovat šablony pomocí fragmenty JSON. Šablony nasazeny z aplikace Visual Studio pomocí skriptu prostředí PowerShell součástí projektu, aby všechny změny provedené skript se použije ve Visual Studiu.

##<a name="diagnostics-improvements-for-cloud-services"></a>Vylepšení diagnostických nástrojů pro Cloudovým službám

Azure SDK 2.6 přináší zpět podpora shromáždění protokolů Diagnostika v Azure výpočetním emulátoru a přenos k základnímu úložišti vývoj. Všechny diagnostiky protokoly (včetně trasování aplikace protokoly událostí trasování protokoly systému Windows (trasování událostí pro Windows), výkonnosti, infrastruktura protokoly událostí windows a protokolování) generovaného při spuštění aplikace v emulátor může dojít ke ztrátě určitého k základnímu úložišti vývoj k ověření, že protokolování diagnostiky pracuje na místním počítači. 

Účet úložiště diagnostiky můžete nyní zadané v souboru konfigurace (.cscfg) služby snadnější použití různých diagnostiky úložiště účtů na jiném prostředí. Existuje několik důležitých rozdíly mezi jak připojovací řetězec v Azure SDK 2.4 a Azure SDK 2.6 fungoval. Další informace o používání připojení diagnostiky úložiště řetězce a jak ovlivňuje vašich projektů najdete v tématu [Konfigurace diagnostiky cloudové služby Azure](http://go.microsoft.com/fwlink/?LinkID=532784).

##<a name="breaking-changes"></a>Zrušení změn

###<a name="azure-resource-manager-tools"></a>Azure nástroje Správce prostředků 

- Typ projektu **Projekty nasazení Cloud** k dispozici v Azure SDK 2,5 přejmenoval na **Azure pole Skupina zdroje**.
- **Projekty nasazení cloudu** typ projekty vytvořené v Azure SDK 2,5 lze použít v 2.6 ale nasazení šablony z aplikace Visual Studio se nezdaří. Však nasazení s skript Powershellu bude dál fungovat stejně jako dříve.  Informace o používání **Cloudové nasazení projekty** v 2.6 přečtěte si tento [příspěvek](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Známé problémy

- Shromáždění protokolů Diagnostika v emulátor vyžaduje operační systém 64bitová verze. Protokolování diagnostiky nebude vyberou spuštěná 32bitová verze operačního systému. Další funkce emulátoru netýká. 

- Azure SDK 2.6 vydána 4/29/2015 má dvě otázky: 

    - Univerzální aplikace nelze načíst ve Visual Studiu 2015 při Azure SDK 2.6 byl nainstalovaný v počítači.
    - Ladění projektu cloudové služby selže ve Visual Studiu 2013 a Visual Studio 2015, kde Visual Studio přestane reagovat a dojde k chybě při zobrazování dialogové okno se zprávou "Konfigurace diagnostiky emulátoru".
    
    Aktualizaci Azure SDK 2.6 byla vydána 5/18/2015. Aktualizovanou verzi je 2.6.30508.1601; obsahuje opravy dva problémy, jsme je popsali výše. Při zjišťování sestavení SDK pomocí ovládacích panelů -> programy a funkce -> nástroje Microsoft Azure pro Microsoft Visual Studio 2013 – v 2.6. Sloupec verze se zobrazí číslo sestavení.

    Pokud se stále protilehlé výše uvedených problémů, nainstalujte nejnovější verzi Azure 2.6 SDK [a 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [2013 a](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) nebo [a 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).
 
##<a name="see-also"></a>Viz taky

[Podpora a vyřazování webů informace o Azure SDK pro .NET a rozhraní API](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
