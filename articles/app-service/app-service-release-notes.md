<properties 
   pageTitle="Poznámky k verzi Azure SDK pro .NET 2.5.1" 
   description="Poznámky k verzi Azure SDK pro .NET 2.5.1" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>Poznámky k verzi Azure SDK pro .NET 2.5.1

Poznámky k verzi Azure SDK pro .NET 2.5.1 obsahuje tento dokument. 

##<a name="azure-sdk-for-net-251-release-notes"></a>Poznámky k verzi Azure SDK pro .NET 2.5.1

Následují novými funkcemi a aktualizací v Azure SDK .NET 2.5.1.

- Nové features\scenarios související s **Rozšíření nástrojů Web**. 

    - Azure weby přejmenovaná aplikace služby Azure. Další informace najdete [aplikaci služby Azure a existující služby Azure](app-service-changes-existing-services.md).
    - Podpora Azure rozhraní API aplikace (verze Preview) je přidán tak, aby zákazníci můžete publikovat ASP.NET projekty jako rozhraní API aplikace a potom použijte Přidat > gesto Azure rozhraní API aplikace klienta v jazyce C# projekty generovat kód podle struktury nasazeném rozhraní API aplikace. 
    - Uzel weby v Průzkumníku serveru se nepoužívá namísto uzlu aplikaci služby Azure, která obsahuje podporu pro zdroje založené na skupinách seskupení Azure rozhraní API aplikace, mobilní aplikace a webových aplikací Web Apps.
    - Podpora Azure mobilní aplikace (verze Preview) je přidán tak, aby zákazníci můžete vytvořit nové projekty mobilní aplikace řadiče mobilní aplikace, publikování projektů a přidejte vzdáleně ladění aplikací.
    - Přidání > Azure rozhraní API aplikace klienta gesto nyní podporuje místní Swagger JSON soubory, aby vývojáři rozhraní API webových mohli NuGets třetí strany jako Swashbuckle generovat Swagger nebo vytvořte ručně. Tímto způsobem klienta mohou vývojáři funkce generování kódu používat všechny Swagger koncového bodu v jazyce C# projekty. 
    - Web App a dialogy publikování rozhraní API aplikace byly rozšířeného podporuje Azure portálu koncepci zdroje seskupování a výběr a vytváření skupin zdrojů Azure a aplikace služby plány jednotného zasílání zpráv představují v nový Web App a rozhraní API aplikace zřizovací dialogu. 
    - Azure uzly rozhraní API aplikace serveru Průzkumníka odkazy na rozhraní API aplikace hloubkové odkaz v portálu Azure, jakož i dalších funkcí, jako je přenos protokolu a vzdálené ladění.

    Známé problémy a aktuální omezení Azure SDK .NET 2.5.1 [Tento](app-service-release-notes.md#known_issues_2_5_1) oddíl níž.


- Nové features\scenarios týkající se **Nástroje HDInsight** ve Visual Studiu jsou povoleny v této verzi. 
    - Místní ověření podregistru skriptů. Kliknutím na tlačítko Ověřit skriptu na panelu nástrojů zobrazíte, pokud je potřeba udělat některé chyby skriptu. 
    - Lepší ladění podregistru úloh. Teď můžete ladění podregistru úlohy přístupem vláken protokolů ve Visual Studiu. Pokud aplikace má problémy s výkonem, vyšetřování vláken protokoly vám poskytne užitečné informace.
    - (Veřejné verze Preview) Automatické dokončování klíčových slov a technologie IntelliSense podporu podregistru. Umožňuje vytvářet skripty podregistru, HDInsight Tools for Visual Studio přidá automatické dokončování klíčových slov a technologie IntelliSense podpory podregistru.
    - Podpora bouře. Teď můžete HDInsight Tools for Visual Studio se dají bouře topologie/Spouts/prvky v jazyce C#. Můžete odeslat vyvinutý topologie clusteru bouře a zobrazení stavu topologie/blesku/hubičky. Protokoly systému a zákazníka slouží k řešení potíží s vaší bouře topologií/prvky/Spouts. Můžete taky stávající aktiva JAVA v bouře na HDInsight.
    
    Další informace najdete v tématu [Začínáme s používáním HDInsight Hadoop Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).



##<a id="known_issues_2_5_1"></a>Azure SDK pro .NET 2.5.1 známé problémy a omezení

- Azure rozhraní API aplikace se zobrazuje jako cíl nasazení pro mobilní aplikace. Webové aplikace by měl být jenom cíl mobilní aplikace do pozdější verzi. 
- Azure zřizování rozhraní API aplikace můžete mít za následek úspěch ale občas může při k chybě aktualizace průběhu v okně aktivitu aplikace služby Azure. Řešení: je kontrola stavu novou aplikaci rozhraní API Azure na portálu Azure. 
- Soubor > Nový Projekt > rozhraní API aplikace > F5 prostředí má za následek chybu HTTP protože neexistuje žádná default/index.html. Řešení: je ručně přejděte na adresu URL /api/values. 
- Přerušovaně Průzkumník serveru ikon sloučený. Restartování a řeší potíže. 
- Pokud je výjimka při zřizování webové aplikace nebo rozhraní API aplikace (například chyby překročení kvóty nebo duplicitní název brány Azure rozhraní API aplikace), chyby zobrazit jako nezpracovaná JSON text. 
- Údaje o času vytvoření chybovému vytvářeného projektu problémy při interpretaci aplikace zaškrtnuté v projektu
- V některých případech kód generovaný Azure rozhraní API aplikace klienta chybí obory názvů, budou muset ručně zahrnout (nebo automaticky importované pomocí aplikace Visual Studio pomůcek) kódem sestavíte. 
- Mobilní aplikace projekty by měl publikovat do webových aplikací, ale musí vyberte web, který jste vytvořili jako mobilní aplikace na portálu Azure od mobilní aplikaci projekty vyžadují databáze. 
- Úvodní stránka mobilní aplikace používá termín "mobilní" služba místo "mobilních aplikací" 
- Vytvářeného projektu mobilní aplikace může trvat až na minutu vytvořit. 
- Během rozhraní API aplikace zřizování (v některých případech) chybu vracejí ze odrážející Azure API, že oprávnění nelze nastavit správně, zatímco rozhraní API aplikace správně zřízení a je připraven k použití. Můžete ručně nastavit oprávnění pomocí portálu Azure.
- Přehledy aplikace není podporována ve rozhraní API aplikace šablony a mobilní aplikaci šablony.
- Rozhraní API aplikace projekty nelze použít v souvislosti s projekty cloudové služby.
- Šablony projektů rozhraní API aplikace jsou dostupné jenom v jazyce C#.
- Rozhraní API aplikace spotřebu prostřednictvím místní nabídku "Přidat Azure rozhraní API aplikace klienta" je podporována pouze v jazyce C#.

 
