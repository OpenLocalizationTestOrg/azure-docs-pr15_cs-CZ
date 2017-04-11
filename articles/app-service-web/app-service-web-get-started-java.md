<properties 
    pageTitle="Nasazení webovou aplikaci první Java Azure pět minut | Microsoft Azure" 
    description="Zjistěte, jak je snadné spuštění webových aplikací web apps v aplikaci služby nasazením ukázkové aplikace. Spusťte rychle udělat skutečný rozvoj a okamžitě zobrazit výsledky." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/13/2016" 
    ms.author="cephalin"
/>
    
# <a name="deploy-your-first-java-web-app-to-azure-in-five-minutes"></a>Nasazení webovou aplikaci první Java Azure pět minut

Tento kurz vám pomůže publikovat do jednoduchých webových aplikací Java [aplikace](../app-service/app-service-value-prop-what-is.md)služby Azure.
Aplikace služby slouží k vytváření webových aplikací web apps, [mobilní aplikace zpět koncích](/documentation/learning-paths/appservice-mobileapps/)a [rozhraní API aplikace](../app-service-api/app-service-api-apps-why-best-platform.md).

Udělejte toto: 

- Vytvořte web appu v aplikaci služby Azure.
- Nasazení aplikace od Java vzorku.
- Zobrazit kód spuštěný žijí výroby.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- Pokud potřebujete FTP/FTPS klientů, jako je [FileZilla](https://filezilla-project.org/).
- Zřízení účtu Microsoft Azure. Pokud nemáte účet, můžete [Registrace bezplatnou zkušební verzi](/pricing/free-trial/?WT.mc_id=A261C142F) nebo [Aktivujte své výhody odběratele Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Pokud nepoužíváte účet Azure můžete [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751) . Vytvoření aplikace starter a přehrát s ním pro až jednu hodinu – bez platební kartou povinné žádné závazky.

<a name="create"></a>
## <a name="create-a-web-app"></a>Vytvoření webové aplikace

1. Přihlaste se k [portálu Azure](https://portal.azure.com) Azure účtem.

2. V nabídce nalevo klikněte na **Nový** > **Web + Mobile** > **Web Appu**.

    ![](./media/app-service-web-get-started-languages/create-web-app-portal.png)

3. V aplikaci vytváření zásuvné použijte následující nastavení pro novou aplikaci:

    - **Název aplikace**: Zadejte jedinečný název.
    - **Pole Skupina zdroje**: vyberte **vytvořit nový** a zadejte název skupiny zdrojů.
    - **Plán/umístění aplikace služby**: klikněte na něj ke konfiguraci a potom klikněte na **Vytvořit nový** nastavení název, umístění a ceny osy plán služeb aplikací. Neváhejte použít **Free** ceny osy.

    Až budete hotovi, zásuvné vytváření vaše aplikace by měl vypadat takto:

    ![](./media/app-service-web-get-started-languages/create-web-app-settings.png)

3. Klikněte na **vytvořit** dole. Klikněte na **oznamovací** ikony umístěné nahoře najdete v článku průběh.

    ![](./media/app-service-web-get-started-languages/create-web-app-started.png)

4. Po dokončení nasazení byste měli vidět toto oznámení. Klikněte na zprávu otevřete zásuvné vašeho nasazení.

    ![](./media/app-service-web-get-started-languages/create-web-app-finished.png)

5. V zásuvné **úspěšném nasazení** klikněte na odkaz **zdroj** otevřete zásuvné svůj nový web appu.

    ![](./media/app-service-web-get-started-languages/create-web-app-resource.png)

## <a name="deploy-a-java-app-to-your-web-app"></a>Nasazení aplikace od Java do webové aplikace

Teď Pojďme nasazení aplikace od Java Azure pomocí FTPS.

5. Ve webové aplikaci zásuvné posuňte se dolů na **Nastavení aplikace** nebo aplikaci vyhledejte ho a pak klikněte na něj. 

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

6. **Verze Java**vyberte **Java 8** a na tlačítko **Uložit**.

    ![](./media/app-service-web-get-started-languages/set-java-application-settings.png)

    Až se dostanete oznámení **úspěšně aktualizovat nastavení web appu**, přejděte na http://*&lt;název_aplikace >*. azurewebsites.net zobrazíte servlet JSP výchozí akce.

7. Zpátky v zásuvné web app posuňte se dolů na **pověření nasazení** nebo aplikaci vyhledejte ho a potom klikněte na něj.

8. Nastavení přihlašovacích údajů nasazení a klikněte na **Uložit**.

7. Po návratu do webové aplikace zásuvné klikněte na **Přehled**. Vedle **FTP/nasazení uživatelské jméno** a **název FTPS hostitele**klikněte na tlačítko **Kopírovat** a zkopírujte tyto hodnoty.

    ![](./media/app-service-web-get-started-languages/get-ftp-url.png)

    Teď jste připraveni Nasaďte aplikaci Java s FTPS.

8. Ve vašem klientovi FTP/FTPS přihlaste k serveru FTP Azure webovou aplikaci pomocí hodnoty, které jste zkopírovali v posledním kroku. Použijte nasazení heslo, které jste dříve vytvořili.

    Následující obrázek ukazuje přihlášení pomocí FileZilla.

    ![](./media/app-service-web-get-started-languages/filezilla-login.png)

    Může se zobrazit upozornění zabezpečení pro nerozpoznaný certifikát SSL od Azure. Pojďte dále a pokračovat.

9. Klikněte na [Tento odkaz](https://github.com/Azure-Samples/app-service-web-java-get-started/raw/master/webapps/ROOT.war) na WAR budou moct soubor stáhnout do počítače.

9. Ve vašem klientovi FTP/FTPS přejděte na **/site/wwwroot/webapps** vzdálený server a přetáhněte stažený soubor WAR na místním počítači do této vzdálený adresář.

    ![](./media/app-service-web-get-started-languages/transfer-war-file.png)

    Klikněte na **OK** přepsat souboru v Azure.

    >[AZURE.NOTE] V souladu s Tomcat je výchozí chování název_souboru **ROOT.war** /site/wwwroot/webapps vám kořenový web appu (http://*&lt;název_aplikace >*. azurewebsites.net) a název souboru ** * &lt;anyname >*.war** vám do pojmenované webových aplikací (http://*&lt;název_aplikace >*.azurewebsites.net/*&lt;anyname >*).

Je to! Aplikace Java teď běží žijí Azure. V prohlížeči přejděte na http://*&lt;název_aplikace >*. azurewebsites.net zobrazíte v akci. 

## <a name="make-updates-to-your-app"></a>Provést aktualizace aplikace

Pokaždé, když potřebujete udělat aktualizaci, stačí nové WAR soubor nahrajte stejný vzdálený adresář s klientem FTP/FTPS.

## <a name="next-steps"></a>Další kroky

[Vytvořit web appu Java ze šablony v Azure Marketplace](web-sites-java-get-started.md#marketplace). Můžete získat vlastní plně přizpůsobitelného kontejneru Tomcat a najděte známé rozhraní správce. 

Ladění Azure webovou aplikaci, přímo v [IntelliJ](app-service-web-debug-java-web-app-in-intellij.md) nebo [zatmění](app-service-web-debug-java-web-app-in-eclipse.md).

Nebo další s webovou aplikaci první. Příklad:

- Vyzkoušejte [Další způsoby nasazení kódu Azure](../app-service-web/web-sites-deploy.md). 
- Používání aplikace Azure vyšší úrovni. Ověřování uživatelů. Škála podle služba. Nastavte si některé výstrahy výkonu. Všechny několika kliknutími. V tématu [Přidání funkcí do prvního webové aplikace](app-service-web-get-started-2.md).

