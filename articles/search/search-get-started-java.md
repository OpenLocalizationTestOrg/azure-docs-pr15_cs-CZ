<properties
    pageTitle="Začínáme s Azure prohledávat Java | Microsoft Azure | Vyhledávací služby serveru hostovanou cloudu"
    description="Postup vytvoření hostovanou cloudu aplikace Vyhledávací služby na Azure pomocí jazyka Java jako programovací jazyk."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-java"></a>Začínáme s Azure prohledávat Java
> [AZURE.SELECTOR]
- [Portál](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Zjistěte, jak vytvářet vlastní Java vyhledávání aplikace, která používá Azure vyhledejte jeho vyhledávání. Tento kurz používá [Azure hledání služba REST API](https://msdn.microsoft.com/library/dn798935.aspx) pro vytvoření objektů a činnosti použitý v tomto cvičení.

V tomto příkladu spustíte musí mít Azure vyhledávací služby, které se můžete přihlásit k [Portálu Azure](https://portal.azure.com). Podrobné pokyny najdete v článku [Vytvoření Azure vyhledávací služby na portálu](search-create-service-portal.md) .

K vytvoření a otestování v tomto příkladu jsme použili následujícím softwarem:

- [Zatmění integrovaném vývojovém prostředí pro vývojáře í Java](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Ujistěte se, že verzi í stáhnout. Jeden z kroků ověření vyžaduje funkci, která se nachází pouze v tomto edition.

- [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Informace o data

Tato ukázková aplikace použije data z [USA geologických služby (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), filtrované na stav Rhode Island zmenšit velikost datovou sadu. Tyto údaje použijeme k vytvoření aplikace Vyhledávací služby, který vrací orientačních bodů budovy například nemocnice a školy, jakož i geologických funkcí, jako je datových proudů jezera a schůzek.

V této aplikaci **SearchServlet.java** program vytvoří a načte index pomocí [indexování](https://msdn.microsoft.com/library/azure/dn798918.aspx) konstrukce, načítání filtrované datovou sadu USGS z veřejné databáze SQL Azure. Předdefinované přihlašovací údaje a informace o připojení ke zdroji dat online jsou uvedeny v programového kódu. Pokud jde o přístup k datům je nutné žádnou další konfiguraci.

> [AZURE.NOTE] Společnost Microsoft použije filtru v této sadě dat dodržovat předpisy ve skupinovém rámečku limit 10 000 dokumentů bezplatné ceny osy. Pokud používáte standardní osy, toto omezení, nebudou mít vliv a můžete upravit tento kód můžete zvětšit datovou sadu. Podrobnosti o kapacity pro každou ceny osy najdete v tématu [limity a omezení](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>Informace o typy souborů

Následující seznam popisuje soubory, které jsou důležité pro tento příklad.

- Search.jsp: Poskytuje uživatelské rozhraní
- SearchServlet.java: Poskytuje metody (podobný řadiči MVC)
- SearchServiceClient.java: Požadavky na úchyty HTTP
- SearchServiceHelper.java: Pomocné třídy, která poskytuje statické metody
- Document.Java: Obsahuje datový model
- config.Properties: Nastaví adresy URL služby hledání a rozhraní api klíč
- Pom.XML: Maven závislostí

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Najděte název služby a rozhraní api klávesami Azure vyhledávací služby

Všechny rozhraní REST API zavolá Azure hledání vyžadují, zadejte adresu URL služby a rozhraní api klíče. 

1. Přihlaste se k [portálu Azure](https://portal.azure.com).
2. Na panelu odkaz klikněte na možnost **Vyhledávací služby serveru** zobrazíte seznam všech služby Azure hledání zřízení pro vaše předplatné.
3. Vyberte službu, kterou chcete použít.
4. Na řídicím panelu služby uvidíte dlaždice základní informace i na ikonu klíče pro přístup ke klíči správce.

    ![][3]

5. Zkopírujte adresu URL služby a správu klíče. Budete potřebovat je později, když je přidáte do souboru **config.properties** .

## <a name="download-the-sample-files"></a>Stažení ukázkových souborů

1. Přejděte na [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) na Github.

2. Klikněte na tlačítko **Stáhnout ZIP**, uložte soubor ZIP na disk a pak extrahovat všechny soubory, které obsahuje. Zvažte extrahování souborů do pracovního prostoru Java usnadnit vyhledání projekt později.

3. Ukázkové soubory jsou jen pro čtení. Klikněte pravým tlačítkem vlastnosti složky a vymažte atribut jen pro čtení.

Před soubory v této složce budou všechny změny následujících souboru a spuštění příkazy.  

## <a name="import-project"></a>Import projektu

1. V zatmění, zvolte **soubor** > **Import** > **Obecné** > **Existující projekty do pracovního prostoru**.

    ![][4]

2. V seznamu **vyberte kořenový adresář**přejděte do složky obsahující ukázkové soubory. Vyberte složku, která obsahuje složce Project. Projekt by se měly v seznamu **projekty** jako vybrané položky.

    ![][12]

3. Klikněte na **Dokončit**.

4. Prohlížení a úpravy souborů pomocí **Průzkumníka projektu** . Pokud ještě není otevřený, klikněte na **okno** > **Zobrazit zobrazení** > **Průzkumník projektu** nebo použití na zástupce a otevřete ho.

## <a name="configure-the-service-url-and-api-key"></a>Konfigurace adresy URL služby a rozhraní api klíč

1. V programu **Průzkumník projektu**poklikejte na **config.properties** upravit nastavení obsahující název serveru a rozhraní api klíč.

2. Podívejte se do kroků uvedených dříve v tomto článku, kde jste našli adresy URL služby a rozhraní api klíč na [Portál Azure](https://portal.azure.com), chcete-li získat hodnoty, které teď zadáte do **config.properties**.

3. V **config.properties**nahraďte "Rozhraní Api klíč" klávesu rozhraní api pro službu. Pak název služby (první součást http://servicename.search.windows.net adresy URL) slouží k nahrazení "název služby" ve stejném souboru.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Konfigurace projekt, vytvořit a modulu runtime prostředí

1. V zatmění v Průzkumník projektu, klikněte pravým tlačítkem Projekt > **Vlastnosti** > **Fasetami projektu**.

2. Vyberte **dynamických modul**, **Java**a **JavaScript**.

    ![][6]

3. Klikněte na tlačítko **použít**.

4. **Okno**vybrat > **Předvolby** > **serveru** > **Runtime prostředí** > **přídavné.**.

5. Rozbalte Apache a vyberte verzi serveru Apache Tomcat, který jste již nainstalovali. V našem systému jsme nainstalovanou verzí 8.

    ![][7]

6. Na další stránce zadejte Tomcat instalační adresář. Na počítači s Windows bude pravděpodobně C:\Program Files\Apache Software Foundation\Tomcat *verze*.

6. Klikněte na **Dokončit**.

7. **Okno**vybrat > **Předvolby** > **Java** > **nainstalovaný JREs** > **Přidat**.

8. **Přidání JRE**vyberte **Standardní OM**.

10. Klikněte na tlačítko **Další**.

11. Definice JRE v JRE Domů klikněte na **adresář**.

12. Přejděte na **Program Files** > **Java** a vyberte JDK dříve nainstalována. Je důležité vyberte JDK jako JRE.

13. V nainstalované JREs zvolte **JDK**. Nastavení by měla vypadat podobně jako následující snímek.

    ![][9]

14. Volitelně můžete vybrat **okno** > **Webový prohlížeč** > **Internet Exploreru** otevřete aplikaci v okně Externí prohlížeče. Použití externích prohlížeče poskytuje lepší webové aplikace.

    ![][8]

Teď jste dokončili konfigurace úkoly. Potom se vytvoření a spuštění projektu.

## <a name="build-the-project"></a>Vytvoření projektu

1. V Průzkumníku projektu klikněte pravým tlačítkem myši na název projektu a klikněte na **Spustit jako** > **Maven sestavit...** konfigurace projektu.

    ![][10]

8. Úprava konfigurace cíle zadejte "instalaci" a klikněte na **Spustit**.

Zprávy o stavu jsou výstup do okna konzoly. Měli byste vidět ÚSPĚŠNOSTI sestavení označující projekt vytvořený bez chyb.

## <a name="run-the-app"></a>Spusťte aplikaci

V tomto posledním kroku spustíte aplikaci v prostředí runtime místního serveru.

Pokud jste ještě nezadali runtime prostředí serveru v zatmění, musíte nejdřív udělat.

1. V okně Průzkumník projektu rozbalte **obsah webu**.

5. Klikněte pravým tlačítkem myši **Search.jsp** > **spustili** > **Spustit na serveru**. Vyberte server Apache Tomcat a potom klikněte na **Spustit**.

> [AZURE.TIP] Pokud jste použili jiné výchozí pracovní prostor pro uložení projektu, musíte změnit **Spustit konfigurace** přejděte na umístění projektu, aby se zabránilo spuštění chyba serveru. V Průzkumníku projektu klikněte pravým tlačítkem myši **Search.jsp** > **Spustit jako** > **Spustit konfigurace**. Vyberte server Apache Tomcat. Klikněte na **argumenty**. Nastavte klepnutím na **pracovní prostor** nebo **Systému souborů** do složky obsahující projektu.

Při spuštění aplikace byste měli vidět okně prohlížeče, poskytnutí vyhledávacího pole pro zadání podmínek.

Počkejte asi minutu před kliknutím na **Hledat** a udělte tak služby času vytvoření a načíst index. Pokud se zobrazí chyba HTTP 404, musíte jenom čekal trochu déle opakováním.

## <a name="search-on-usgs-data"></a>Hledání dat USGS

Uvedenou množinu dat USGS obsahuje záznamy, které jsou důležité pro stát Rhode Island. Pokud kliknete na tlačítko **Hledat** na prázdný vyhledávací pole, zobrazí se vám horní 50 položek, což je výchozí.

Zadání hledaný výraz vám umožní vyhledávací něco přejít. Zadejte název místní. "Roger čáslavka" byla první guvernér Rhode Island. Po mu jsou s názvem spoustu parky budovy a školy.

![][11]

Může taky zkusit některou z těchto podmínek:

- Pawtucket
- Pembroke
- husí + standardní

## <a name="next-steps"></a>Další kroky

Toto je první kurz Azure hledání na základě Java a USGS datovou sadu. V průběhu času rozšíříme kurzu, který ukazují další vyhledávací funkce, které můžete chtít použít ve vlastní řešení.

Pokud máte nějaké základní v Azure hledání, můžete v tomto příkladu jako springboard pro další pokusů, případně zvýšit [stránky hledání](search-pagination-page-layout.md)nebo provádění [působí navigace](search-faceted-navigation.md). Na stránce výsledků hledání můžete také zvýšit přidáním počty a dávky dokumentů tak, aby uživatelé můžou procházení výsledků.

Začínáte s Azure hledání? Doporučujeme vyzkoušení jiných výukové programy pro se dají pochopení co můžete vytvořit. Navštivte naše [si přečtěte následující dokumentaci stránky](https://azure.microsoft.com/documentation/services/search/) na Najít další zdroje informací. Můžete také zobrazit odkazy na naše [videa a výuková seznamu](search-video-demo-tutorial-list.md) zobrazíte další informace.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
