
<properties 
   pageTitle="Poznámky k verzi Azure SDK .NET 2.7 a .NET 2.7.1" 
   description="Poznámky k verzi Azure SDK .NET 2.7 a .NET 2.7.1" 
   services="app-service\web" 
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

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Poznámky k verzi Azure SDK .NET 2.7 a .NET 2.7.1

##<a name="overview"></a>Základní informace

Tento dokument obsahuje poznámky k verzi pro SDK Azure .NET 2.7 verzi. 

Dokument taky obsahovat poznámky k verzi Azure SDK pro .NET 2.7.1.

Azure SDK 2.7 je podporována pouze ve Visual Studiu 2015 a Visual Studio 2013. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) je že poslední podporovaný SDK for Visual Studio 2012.

Podrobné informace o této verzi najdete v tématu [Azure SDK 2.7 oznámení zveřejňují](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) a [zveřejňují Azure SDK 2.7.1 oznámení](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>Azure SDK pro .NET 2.7

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Přihlaste se vylepšení for Visual Studio 2015

Azure 2.7 SDK pro Visual Studio 2015 podporuje nové funkce správy identit ve Visual Studiu 2015.  Jedná se o podporu pro účty přístup k Azure pomocí řízení přístupu na základě rolí cloudové řešení poskytovatelů, DreamSpark a dalších typů účtů a předplatného.

Na přihlašovací vylepšení zahrnutý v sadě Azure SDK 2.7 jsou dostupné jenom v Visual Studio 2015. Podpora pro Visual Studio 2013 je součástí Azure SDK 2.7.1.


###<a name="mobile-sdk"></a>Mobilní SDK

Aktualizace **Aplikací Mobile** šablony tak, aby odrážely nejnovější [NuGet balíčku](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) a vzhled obrázku.

###<a name="service-bus"></a>Služba Bus 

Vylepšení a obecné opravy chyb. Podrobnosti o aktualizacích a funkcí získáte poznámky k verzi nejnovější [NuGet Bus služby](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

###<a name="hdinsight-tools"></a>HDInsight nástroje 

V této verzi jste udělali tyto aktualizace. Tyto aktualizace jsou ve verzi preview. Další informace najdete v tématu [Tento blogu](http://go.microsoft.com/fwlink/?LinkId=619108).

- Grafy podregistru podregistru Tez úloh
- Úplné podpory podregistru DML technologie IntelliSense
- Prasátko šablony podpory
- Šablony bouře služby Azure

####<a name="breaking-changes"></a>Zrušení změn

- Při použití této verzi nástroje musí být aktualizovány staré **bouře** projektu. Další informace najdete v tématu [Tento blogu](http://go.microsoft.com/fwlink/?LinkId=619108).
- Visual Studio Web Express se už nepodporuje. Další informace najdete v tématu [Tento blogu](http://go.microsoft.com/fwlink/?LinkId=619108).

###<a name="azure-app-service-tools"></a>Nástroje služby Azure aplikace

V této verzi tyto aktualizace byly provedeny rozšíření nástrojů Web. Další informace najdete v [tomto](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blogu. 

- Podpora pro účty DreamSpark přidané
- Úplné Azure nástroje změny podporu nového API správy zdrojů Azure
- Podpora pro aplikaci služby Azure přidán do [Cloudu Průzkumník](#cloud_explorer)

####<a name="known-issues"></a>Známé problémy

Web App nasazení úsek uzly nezobrazí uzlu sloty v Průzkumníku serveru a v prohlížeči nasazení úsek podřízené uzly nenačítají pod Průzkumníkem cloudu. Tento problém byl vyřešit a připravit pro další SDK verzi. 


###<a name="cloud_explorer"></a>Průzkumník cloudu z aplikace Visual Studio 2015

Azure SDK 2.7 obsahuje cloudu Průzkumníka for Visual Studio 2015, která umožňuje zobrazit Azure zdrojů, zkontrolujte jejich vlastnosti a provádět klíčové vývojář z aplikace Visual Studio. 

Průzkumník cloudu podporuje následující:

- Pole Skupina zdroje a pole Typ zdroje zobrazení Azure prostředků. 
- Hledání zdrojů podle názvu (dostupné v zobrazení pro pole Typ zdroje)
- Podpora předplatného a zdroje, které mají Role na základě přístup ovládacího prvku (RBAC) použitým 
- Integrovaný panel akcí, který zobrazuje specifické pro zaměřený vývojář akce vybrané zdroje. Příklad: připojit vzdálené ladění pro virtuálních počítačích vytvořené pomocí zásobníku správce prostředků zobrazit data diagnostiky virtuálních počítačích atd.
- Integrovaný panel vlastnosti, který zobrazí vlastnosti zaměřené vývojář běžně potřebují při zkoušce vývojáře / 
- Rychlé přepínání účet, který chcete používat, když výčet zdroje (příkazem nastavení na panelu nástrojů) 
- Filtrování předplatná pro použití při vytváření výčtu prostředků (příkazem nastavení na panelu nástrojů) 
- Hloubkové odkazy na portálu Azure správy zdrojů a skupiny zdrojů 
 
 
###<a name="azure-resource-manager-tools"></a>Azure nástroje Správce prostředků 

Správce prostředků nástroje Azure jsme aktualizovali pro práci s rolí na základě přístup ovládacího prvku (RBAC) a nové předplatné.  Možnost používání nových účtů úložiště, kromě klasický úložiště pro ukládání artefakty během nasazení je součástí těchto změn.  

Pokud používáte Azure pole Skupina zdroje projektu z předchozí verze SDK s SDK 2.7, nové skript pro nasazení je potřeba nasadit pomocí nového účtu úložiště místo klasické úložiště.  Zobrazí se výzva před změn do projektu přidat nový skript.  Staré skript bude přejmenovat a budete muset ručně proveďte všechny změny nový skript.
 
 
###<a name="storage-explorer-tools"></a>Nástroje pro úložiště Explorer 

- Podpora pro zobrazení informací o objektů BLOB připojení. Další informace v [tomto příspěvku blogu](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
- Podpora pro zobrazení informací o Premium úložiště účty pomocí Průzkumníka serveru. Průzkumník serveru zobrazí stránka objektů BLOB účet úložiště premium jsou jediný podporovaný typ účet úložiště premium.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Azure datové nástroje Factory for Visual Studio 

Úvodní informace o **Azure Data Factory Tools** for Visual Studio. Tady jsou povolené funkce. Přečtěte si [Tento blogu](http://go.microsoft.com/fwlink/?LinkId=617530) Další informace.

- **Na základě šablony pro vytváření**: Vyberte notaci použití podle šablony, šablony pohyb dat nebo zpracování dat šablony nasazení řešení integrace dat začátku do konce a můžete začít praktických rychle se Data Factory. 
- **Integrace se službou Průzkumníka řešení pro vytváření a nasazení Data Factory entity**: vytvoření a nasazení kanály a související entity jako projekty Visual Studio. 
- **Integrace se službou Diagram zobrazení pro vizuální interakce při vytváření**: vizuálně vytvářet kanály a datové sady s podporou v zobrazení diagramu. 
- **Integrace se službou Průzkumník serveru pro prohlížení a interakci se už nasazeném entity**: využít Průzkumník serveru procházet nasadili dat továrny a odpovídající entity. Importujte factory nasazeném dat nebo subjekt (kanálem k odesílání zpráv, propojené služby datové sady) do projektu. 
- **JSON upravujete dokument společně s ověřováním schématu a bohaté intellisense**: efektivní konfigurace a upravovat dokumenty JSON Data Factory entit s formátovaným technologie intellisense a schématu ověření 
- **Publikování na více prostředí**: publikování autorem potrubí pro vývojáře, test nebo výrobní prostředí vytváření souborů samostatné konfigurace pro každé prostředí.
- **Prasátko, podregistru a .net na základě zpracování dat podpory**: podpora Prasátko a podregistru skriptů v aplikaci project Data Factory. Podpora pro odkazování C# projektu pro správu .net aktivity.

##<a name="azure-sdk-for-net-271"></a>Azure SDK pro .NET 2.7.1

V následující části obsahuje aktualizace byly vydané s Azure SDK pro .NET 2.7.1.
###<a name="hdinsight-tools"></a>HDInsight nástroje 

Další podrobné informace o aktualizacích nástroje HDInsight přečtěte si [Tento blogu](http://go.microsoft.com/fwlink/?LinkId=623831).

- Podregistru úlohy operátor zobrazení (nové funkce)

    Vám pomůže pochopit dotazu podregistru lépe, funkce podregistru zobrazení operátor jsme přidali. Zobrazíte všechny operátory uvnitř vrcholu poklepáním na vrcholy úlohy grafu. Chcete-li zobrazit další podrobnosti o konkrétní operátor, přejděte myší na tento operátor.
- Značka chyby podregistru (nová funkce)

    Aby bylo možné zobrazit gramatické chyby okamžitě, funkci podregistru značka chyby jsme přidali. Navíc byly rozšířeného chybové zprávy a nyní vidíte podrobné gramatické chyby okamžitě (až do této verzi jste měli odešlete podregistru skriptu clusteru a počkejte chvíli než se pustíte podrobnosti o chybové zprávě).  
- Bouře topologie grafu (nová funkce)

    Vizualizace je velmi důležité, když budete chtít podívat, pokud topologie pracuje podle očekávání. V této verzi jsme přidali vizualizace pro bouře grafy. Vizualizace důležité metriky pro topologii (například barvu označuje počasí určité role "něčem pracuje" nebo ne). Můžete taky se poklepáním blesku/hubičky zobrazíte další informace.

- Podpora pro HDInsight clusterů, které jsou vytvořené v portálu Azure (oprava chyb)

    Teď můžete Visual Studio můžete zobrazit a odesílat úlohy všechny clusterů HDInsight bez ohledu na to, kde byly vytvořeny clusteru.

- Další podporu technologie IntelliSense a rychleji podregistru metadat načítání (zlepšení)

    Technologie IntelliSense jsme mít lepší přidáním další návrhy popisný. Třeba aliasu tabulky můžete nyní také budou doporučovány IntelliSense, můžete napsat svůj dotaz snadněji. Navíc jsme mít lepší podregistru metadata načítání tak jenom bude trvat několik sekund, než se seznam všech databází, tabulek a sloupců podregistru metastore.

Další podrobné informace o aktualizacích nástroje HDInsight přečtěte si [Tento blogu](http://go.microsoft.com/fwlink/?LinkId=623831).

###<a name="improvements-in-visual-studio-2013"></a>Vylepšení ve Visual Studiu 2013

- Azure SDK 2.7.1 umožňuje Visual Studio 2013 pro přístup k Azure účty a předplatných pomocí řízení přístupu na základě rolí poskytovatelé cloudové řešení a Dreamspark.
- S Azure SDK 2.7.1 nové cloudu nástroj Průzkumníka je nyní také k dispozici ve Visual Studiu 2013.

###<a name="known-issues"></a>Známé problémy

Instalaci Azure SDK 2.6 nebo 2.7.1 Visual Studio komunity 2013 na jiné – angličtina s operačním systémem se zobrazí upozornění, že se pravděpodobně angličtinu a jiné než anglické prostředky Visual Studio neshodují. Toto upozornění, může být bezpečně odvolán. Pouze se dojde, pokud v počítači neobsahoval předchozí instalace aplikace Visual Studio komunity 2013 a instalujete SDK na jiné – angličtina s operačním systémem. Po příslušnou jazykovou sadu platí pro aplikace Visual Studio RTM zdroje, ale předtím, než je určený 4 aktualizace, zobrazí se upozornění. Zavřením upozornění povolí příslušnou jazykovou sadu pokračovat a dokončete použití verze 4 aktualizace obsahu language pack.

Projekty LightSwitch nejsou compatibile s touto verzí. Tento problém se být přeložena další SDK verze.

##<a name="also-see"></a>Viz také
[Azure SDK 2.7.1 publikovat oznámení](http://go.microsoft.com/fwlink/?LinkId=623850)

[Publikovat oznámení Azure SDK 2.7](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Podpora a vyřazování webů informace o Azure SDK pro .NET a rozhraní API](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
