<properties
    pageTitle="Vytvořit web appu Java v aplikaci služby Azure | Microsoft Azure"
    description="Tento kurz se dozvíte, jak nasazení aplikace od Java webové aplikace služby Azure."
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-java-web-app-in-azure-app-service"></a>Vytvořit web appu Java v aplikaci služby Azure

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Tento kurz ukazuje, jak vytvořit Java [web app v aplikaci služby Azure] pomocí [Portálu Azure]. Portál Azure je webového rozhraní, které můžete použít ke správě Azure prostředků.

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Microsoft Azure. Pokud nemáte účet, můžete [aktivovat své Visual Studio účastnická výhody] nebo [zaregistrovat bezplatnou zkušební verzi].
>
> Pokud chcete začít pracovat s aplikaci služby Azure před registraci účet Azure, přejděte na [Zkuste aplikaci služby]. Můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby; požaduje žádné platební kartou a bez závazky.

## <a name="java-application-options"></a>Možnosti aplikace Java

Existuje několik způsobů, jak můžete nastavit aplikace Java ve webové aplikaci pro aplikaci služby. 

1. Vytvoření aplikace a potom i konfiguraci **Nastavení aplikace**.

    Aplikace služby nabízí několik Tomcat a molo verzí výchozí konfigurace. Pokud aplikace, který je hostitelem fungovat s některou z předdefinovaných verzí, tento způsob nastavení kontejneru web je nejjednodušší a je ideální, když chcete udělat stačí odeslání souboru war kontejneru web. Pro tento způsob vytvoření aplikace pro na portálu Azure a potom přejděte k části **Nastavení aplikace** zásuvné aplikace a vyberte si verzi jazyka Java spolu s požadovaný web kontejner Java. Pokud použijete tento způsob Java a váš web kontejner spustit z Program Files. Další metody a jsou vloženy kontejneru web potenciálně JVM do místa na disku. Když použijete tento model, nebudete mít přístup k úpravě souborů v této části systému souborů. To znamená, že nemůžete dělat třeba nakonfigurovat *server.xml* soubor nebo umístěte souborů knihovny do *složky/lib* . Další informace najdete v tématu [vytvořit a nakonfigurovat webovou aplikaci Java](#appsettings) uvedenou dál v tomto kurzu.
    
2. Pomocí šablony z webu Azure Marketplace.

    Azure Marketplace obsahuje šablony, které automaticky vytvořit a nakonfigurovat Tomcat nebo molo web kontejnery Java webových aplikacích. Vytvořené šablony kontejnery web lze konfigurovat. Další informace naleznete v části [použití Java šablony z webu Azure Marketplace](#marketplace) tohoto kurzu.
  
3. Vytvoření aplikace pro a potom ručně kopírovat a upravovat soubory konfigurace 

    Můžete hostovat vlastní aplikaci Java, která není nasazení v některém z webu kontejnery poskytovaná aplikace. Příklad:
    
    * Aplikace Java vyžaduje verzi Tomcat nebo molo, který není přímo nepodporuje aplikaci služby nebo obsažených v galerii.
    * Aplikace Java zabírá požadavků HTTP a nasazení jako WAR do kontejneru stávající web.
    * Chcete konfigurovat kontejneru webu úplně od začátku sami. 
    * Chcete použít verzi jazyka Java, který není podporován v aplikaci služby a chcete ho nahrajte sami.

    Pro takovýchto případech můžete vytvořit aplikaci portálu Azure a zadejte příslušný runtime soubory ručně. V tomto případě soubory, které se mají být spočítány proti kvóty prostoru úložiště u vašeho plánu aplikaci služby. Další informace najdete v tématu [nahrání vlastní webovou aplikaci Java na Azure].

## <a name="portal"></a>Vytváření a konfigurace Java web appu

Tato část popisuje můžete vytvořit web appu a nakonfigurovat jazyka Java pomocí **Nastavení aplikace** zásuvné na portálu.

1. Přihlaste se k [portálu Azure].

2. Klikněte na **Nový > Web + mobilní > webové aplikace**.

    ![Nové webové aplikace][newwebapp]

4. Zadejte název pro web app v poli **v prohlížeči** .

    Tento název musí být jedinečný v doméně azurewebsites.net, protože bude adresa URL webové aplikace {název}. azurewebsites.net. Není-li zadané jméno jedinečné, zobrazí se v textovém poli červený vykřičník.

5. **Pole Skupina zdroje** vyberte nebo vytvořte nový účet.

    Další informace o skupinách prostředků najdete v článku [Azure portálu pro správu Azure prostředků].

6. Vyberte **Aplikaci služby plán/umístění** nebo vytvořte nový účet.

    Další informace o různých plánech aplikaci služby najdete v článku [Přehled plány aplikace služby Azure].

7. Klikněte na **vytvořit**.

    ![Vytvoření webové aplikace][newwebapp2]
 
8. Po vytvoření web appu klikněte na tlačítko **Web Apps > {webovou aplikaci}**.
 
    ![Vyberte v prohlížeči][selectwebapp]

9. V zásuvné **Web appu** klikněte na **Nastavení**.

10. Klikněte na **Nastavení aplikace**.

11. Vyberte požadovanou **verzi jazyka Java**. 

12. Volba požadovaného **jazyka Java podverze**. Pokud vyberete možnost **od nejnovějšího**, použije aplikace nejnovější dílčí verzí, která je dostupná v aplikaci služby pro danou Java hlavní verzi. Položka **od nejnovějšího** je jedinečné pro aplikace Java vytvořené v **Nastavení aplikace**. Pokud vytvoříte Java aplikace z galerie, budete muset Správa kontejner a JVM změny. 

12. Vyberte požadovaný **Web kontejner**. Pokud jste vybrali kontejneru s názvem začínajícím **od nejnovějšího**, aplikace bude nacházet v nejnovější verzi dané web kontejneru hlavní verze, která je dostupná v aplikaci služby. 

    ![Web kontejneru verze][versions]

13. Klikněte na **Uložit**.

    Ve chvíli nepůjde webovou aplikaci na základě Java a nakonfigurovaný tak, aby použít kontejneru web, který jste vybrali.

14. Klikněte na **webové aplikace > {novou webovou aplikaci}**.

15. Klikněte na adresu **URL** umožňuje přecházet na nový web.

    Na webovou stránku potvrzuje, že jste vytvořili základě Java v prohlížeči.

## <a name="marketplace"></a>Použít šablonu Java z Azure Marketplace

Tato část popisuje umožňuje vytvořit web appu Java Azure Marketplace. Stejné obecné tok lze také vytvořit na základě Java mobilní telefon nebo rozhraní API aplikace. 

1. Přihlaste se k [portálu Azure]

2. Klikněte na **Nový > Marketplace**.

    ![Nové Marketplace][newmarketplace]

3. Klikněte na **Web + Mobile**.

    Možná bude potřeba Posunout vlevo zobrazíte zásuvné **Marketplace** , kde můžete vybrat **Web + Mobile**.

4. Do textového pole Hledat zadejte název aplikační server Java, například **Apache Tomcat** nebo **molo**a stiskněte klávesu Enter.

5. Ve výsledcích hledání klikněte na aplikační server Java.

    ![Web mobilních molo][webmobilejetty]

6. V první **Apache Tomcat** nebo **molo** zásuvné klikněte na **vytvořit**.

    ![Přístavních mol portálu zásuvné][jettyblade]

7. V dalším **Apache Tomcat** nebo **molo** zásuvné zadejte název pro web app v poli **v prohlížeči** .

    Tento název musí být jedinečný v doméně azurewebsites.net, protože bude adresa URL webové aplikace {název}. azurewebsites.net. Není-li zadané jméno jedinečné, zobrazí se v textovém poli červený vykřičník.

8. **Pole Skupina zdroje** vyberte nebo vytvořte nový účet.

    Další informace o skupinách prostředků najdete v článku [Azure portálu pro správu Azure prostředků].

9. Vyberte **Aplikaci služby plán/umístění** nebo vytvořte nový účet.

    Další informace o různých plánech aplikaci služby najdete v článku [Přehled plány aplikace služby Azure].

10. Klikněte na **vytvořit**.

    ![Vytvoření přístavních mol portálu][jettyportalcreate2]

    Ve chvíli, obvykle méně než jednu minutu Azure skončí, vytvoření nové webové aplikace.

11. Klikněte na **webové aplikace > {novou webovou aplikaci}**.

12. Klikněte na adresu **URL** umožňuje přecházet na nový web.

    ![Přístavních mol adresy URL][jettyurl]

    Tomcat je dodávána se sadou výchozí stránky. Pokud jste se rozhodli Tomcat, uvidí stránku podobně jako v následujícím příkladu.

    ![Použití Apache Tomcat v prohlížeči][tomcat]

    Pokud jste se rozhodli molo, uvidí stránku podobně jako v následujícím příkladu. Molo nemá výchozí sadu stránky, aby stejné JSP, který se používá pro prázdný web Java tady opětovné použití.

    ![Použití molo v prohlížeči][jetty]

Teď, když jste vytvořili web appu s kontejnerem aplikace, naleznete v části [Další kroky](#next-steps) informace o tom, jak nahrát aplikaci pro web appu.

## <a name="next-steps"></a>Další kroky

Nyní máte aplikační server Java spuštěné ve vaší webové aplikaci služby Azure aplikace. Abyste mohli nasadit vlastní kód k web appu, najdete v článku [Přidat aplikaci nebo webové stránky do webové aplikace Java].

Další informace o vývoji aplikací Java v Azure naleznete v článku [Středisko pro vývojáře Java].

<!-- URL List -->

[Přidání aplikace nebo webové stránky do webové aplikace Java]: ./web-sites-java-add-app.md
[Azure aplikaci služby plány jednotného zasílání zpráv – přehled]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[Azure portálu]: https://portal.azure.com/
[Aktivujte své výhody odběratele Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=623901
[zaregistrovat bezplatnou zkušební verzi]: http://go.microsoft.com/fwlink/?LinkId=623901
[Vyzkoušejte aplikaci služby]: http://go.microsoft.com/fwlink/?LinkId=523751
[v prohlížeči v aplikaci služby Azure]: http://go.microsoft.com/fwlink/?LinkId=529714
[Středisko pro vývojáře Java]: /develop/java/
[Pomocí portálu Azure ke správě Azure prostředků.]: ../azure-portal/resource-group-portal.md
[Uložit vlastní webové aplikace Java Azure]: ./web-sites-java-custom-upload.md

<!-- IMG List -->

[newwebapp]: ./media/web-sites-java-get-started/newwebapp.png
[newwebapp2]: ./media/web-sites-java-get-started/newwebapp2.png
[selectwebapp]: ./media/web-sites-java-get-started/selectwebapp.png
[versions]: ./media/web-sites-java-get-started/versions.png
[newmarketplace]: ./media/web-sites-java-get-started/newmarketplace.png
[webmobilejetty]: ./media/web-sites-java-get-started/webmobilejetty.png
[jettyblade]: ./media/web-sites-java-get-started/jettyblade.png
[jettyportalcreate2]: ./media/web-sites-java-get-started/jettyportalcreate2.png
[jettyurl]: ./media/web-sites-java-get-started/jettyurl.png
[tomcat]: ./media/web-sites-java-get-started/tomcat.png
[jetty]: ./media/web-sites-java-get-started/jetty.png
