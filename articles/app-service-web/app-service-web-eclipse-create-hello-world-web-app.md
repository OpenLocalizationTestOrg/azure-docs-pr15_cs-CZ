<properties 
    pageTitle="Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění | Microsoft Azure" 
    description="Tento kurz se dozvíte, jak používat tuto sadu nástrojů Azure pro Eclipse při vytváření Ahoj světě webové aplikace pro Azure." 
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
    ms.topic="article" 
    ms.date="08/26/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění

Tento kurz ukazuje, jak vytvářet a publikovat základní Vítáme aplikace Azure jako do webových aplikací pomocí [Nástrojů Azure pro Eclipse]. Pro zjednodušení jsou uvedeny základní JSP příkladu, ale velmi podobným způsobem vhodné pro Java servlet jde Azure nasazení.

Po dokončení tohoto kurzu aplikace bude vypadat podobně jako na následujícím obrázku, když zobrazíte ve webovém prohlížeči:

![Náhled Ahoj prezentace aplikace][01]
 
## <a name="prerequisites"></a>Zjistit předpoklady pro

* Java vývojář Kit (JDK), v 1,8 nebo novější.
* -Zatmění integrovaném vývojovém prostředí vývojářům Java í vzhled Luna nebo novější. Můžete stáhnout z <http://www.eclipse.org/downloads/>.
* Rozdělení na základě Java webový server nebo aplikační server, například Apache Tomcat nebo molo.
* Azure předplatné, které lze získat z <https://azure.microsoft.com/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure sada nástrojů pro Eclipse. Další informace najdete v tématu [instalace Azure sada nástrojů pro Eclipse].

## <a name="to-create-a-hello-world-application"></a>Vytvoření aplikace Ahoj prezentace

Nejdřív budete Začneme s vytvářením Java projektu.

1. Zahájení zatmění, v nabídce klikněte na **soubor**, klikněte na **Nový**a klikněte na **Dynamické Project Web**. (Pokud se nezobrazuje **Dynamické Project Web** uvedené jako projektového plánu dostupné po kliknutí na **soubor** a **Nový**, proveďte následující kroky: klikněte na **soubor**, klikněte na **Nový**, klikněte na **projekt...**, rozbalte **Web**, klikněte na **Dynamické Project Web**a klikněte na tlačítko **Další**.)

1. Pro účely tohoto kurzu zadejte název projektu **MyWebApp**. Obrazovce bude vypadat takto:

    ![Vytvoření nového dynamické Web projektu][02]

1. Klikněte na **Dokončit**.

1. V zobrazení na zatmění Průzkumník projektu rozbalte **MyWebApp**. Klikněte pravým tlačítkem na **obsah webu**, klikněte na **Nový**a potom klikněte na **Soubor JSP**.

1. V dialogovém okně **Nový soubor JSP** , název souboru **index.jsp**zachovat nadřazenou složku jako **MyWebApp/obsah webu**a potom na tlačítko **Další**.

1. V dialogovém okně **Vybrat šablonu JSP** pro účely tohoto kurzu vyberte **Nový soubor JSP (html)**a potom klikněte na **Dokončit**.

1. Při otevření souboru index.jsp v zatmění přidejte v dynamicky zobrazený text **Vítáme!** ve stávající `<body>` prvek. Vaše aktualizované `<body>` obsahu by měla vypadat podobně jako v následujícím příkladu:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Uložení index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Nasazení aplikace na kontejner Azure Web App

Existuje několik způsobů, kterými můžete nasadit aplikaci web Java Azure. Tento kurz popisuje nějaká nejjednodušší: aplikace má být nasazené na kontejner Azure Web App – potřeby žádný typ zvláštní projektu ani další nástroje. JDK a příslušný software kontejneru web vám poskytne vám Azure, tak, aby je potřeba nahrát vlastní; potřebujete stačí webovou aplikaci Java. V důsledku toho publikování aplikace bude trvat sekund, ne minut.

1. V Průzkumníkovi na zatmění projektu klikněte pravým tlačítkem myši **MyWebApp**.

1. V místní nabídce vyberte **Azure**a klikněte na **Publikovat jako Azure Web App....**

    ![Publikovat jako Azure Web Appu][03]
   
    Můžete taky-li vybrán projektu webové aplikace je v okně Průzkumník projektu, můžete na tlačítko **Publikovat** rozevíracího seznamu na panelu nástrojů a vyberte **Publikovat jako Azure Web App** odsud:
   
    ![Publikovat jako Azure Web Appu][14]
   
1. Pokud jste to ještě již přihlásili k Azure z zatmění, zobrazí se výzva k přihlášení k účtu Azure:

    ![Azure Sign In dialogovým oknem][04]
   
    Pokud máte víc účtů Azure, některé pokynů během procesu přihlášení mohou zobrazit více než jednou, i když se zdá být stejné. Důvody tohoto chování, pokračujte po znaménku v pokyny.

1. Po úspěšném podepsání do účtu Azure se zobrazí dialogové okno **Spravovat předplatná** seznam předplatných, která jsou přidružena svoje přihlašovací údaje. Pokud jsou uvedené víc předplatných a chcete pracovat s konkrétní dílčí z nich, které může taky zrušit zaškrtnutí políčka těch, které chcete použít. Pokud jste vybrali předplatného, klikněte na tlačítko **Zavřít**.

    ![Předplatná dialogové okno Spravovat][05]
   
1. Když se zobrazí dialogové okno **nasazení Azure webové aplikace kontejneru** , zobrazí žádné kontejnery webové aplikace, které jste dříve vytvořili; Pokud jste nevytvořili žádné kontejnery, bude seznamu prázdné.

    ![Nasazení Azure webové aplikace kontejneru dialogovým oknem][06]
   
1. Pokud jste nevytvořili kontejneru Azure webové aplikace před nebo pokud chcete publikovat aplikace do nového kontejneru, použijte následující kroky. V opačném vyberte kontejneru existující Web App a přejděte ke kroku 7 pod.

    1. Klikněte na **nové...**

        ![Nasazení Azure webové aplikace kontejneru dialogovým oknem][15]

    1. Zobrazí se dialogové okno **Nový Web App kontejner** :

        ![Dialogové okno nový Web App kontejneru][07a]

    1. Zadejte **štítek DNS** pro váš Web App kontejner; to bude formuláře popisek DNS listu adresa URL hostitele vaší webové aplikace v Azure (Vezměte v úvahu, že název musí být k dispozici a souladu s požadavky na pojmenování Azure Web App.)

    1. V nabídce **Web kontejneru** rozevíracího seznamu vyberte příslušný software pro aplikaci.

        V současné době můžete z Tomcat 8, Tomcat 7 nebo molo 9. Poslední distribuce vybrané softwaru vám poskytne Azure a se spustí na poslední rozdělení JDK 8 vytvořené Oracle a poskytovanou Azure.

    1. V rozevírací nabídce **předplatné** vyberte předplatné, které chcete použít pro toto nasazení.

    1. V rozevírací nabídce **Pole Skupina zdroje** vyberte skupina zdroje, ke kterému chcete přidružit webovou aplikaci. (Azure skupiny zdrojů umožňují seskupení související materiály tak, aby, například, můžete je odstranit společně.)

        Můžete vybrat existující pole Skupina zdroje (Pokud máte některý) a vynechat, můžete přecházet g dole nebo pomocí následujících kroků k vytvoření nové skupiny prostředků následující:

        * Klikněte na **nové...**

        * Zobrazí se dialogové okno **Nová skupina zdroje** :

            ![Dialogové okno Nová skupina zdrojů][08]

        * V textového pole **název** zadejte název nové skupiny prostředků.

        * V **oblast** rozevírací nabídce vyberte odpovídající Azure datového centra umístění skupina zdroje.

        * Volitelné: Ve výchozím nastavení poslední rozdělení Java 8 má být nasazené tak, že Azure automaticky na váš web app kontejner jako svého JVM. Však můžete zadat jinou verzi a distribuce JVM Pokud váš Web Appu vyžaduje. Chcete-li JDK pro webovou aplikaci, klikněte na kartu **JDK** a vyberte jednu z následujících možností:

            * **Nasazení výchozí JDK nabízené služby Azure Web Apps**: tuto možnost, bude nasazení poslední rozdělení Java 8.

            * **Nasazení 3rd narozeninovou párty s motivy JDK k dispozici v Azure**: Tato možnost umožňuje vybrat v seznamu JDKs, které jsou součástí Microsoft Azure.

            * **Nasazení vlastní JDK z tohoto umístění ke stažení**: Tato možnost umožňuje zadat vlastní JDK rozdělením, musí být balíčku ZIP formátu a nahráli veřejně k dispozici ke stažení umístění nebo účet Azure úložiště pro které máte přístup.

            ![Dialogové okno nový Web App kontejneru][07b]

    * Klikněte na **OK**.

    1. Rozevírací nabídky **Aplikace služby plánování** uvádí plány aplikace služby, které jsou přidružené ke skupině zdroje, který jste vybrali. (Plány aplikaci služby zadejte informace jako je místo webovou aplikaci, ceny osy a velikosti instance výpočetním. Jeden plán služeb aplikací lze použít pro více webových aplikací Web Apps, a proto je udržovat zvlášť z konkrétního nasazení v prohlížeči.)

        Vyberte stávající aplikace služby plánování (Pokud máte některý) a přeskočit můžete přecházet h dole nebo můžete použít následující následujících kroků k vytvoření nového plánu služby aplikace:

        * Klikněte na **nové...**

        * Zobrazí se dialogové okno **Nové aplikace služby plán** :

            ![Plánování služby aplikace dialogovém okně Nový][09]

        * V textového pole **název** zadejte název nové aplikace služby plánu.

        * V **umístění** rozevírací nabídce vyberte odpovídající Azure datového centra umístění pro plán.

        * V rozevírací nabídce **Ceny osy** , vyberte odpovídající ceny pro plán. Pro účely testování můžete **Free**.

        * V Instance rozevírací nabídky **Velikost** , vyberte příslušnou instanci změnit velikost pro plán. Pro účely testování můžete **malých**.

    1. Jakmile dokončíte všechny výše uvedené kroky, dialogové okno nový Web App kontejner by měl vypadat na následujícím obrázku:

        ![Dialogové okno nový Web App kontejneru][10]

    1. Klikněte na **OK** dokončete vytváření nové kontejneru v prohlížeči.

        Počkejte několik sekund, než seznam kontejnery Web Appu tak možné je aktualizovat, a měly by být vybrány váš kontejner nově vytvořený web app v seznamu.

1. Teď jste připraveni k dokončení počáteční nasazení webové aplikace Azure:

    ![Nasazení Azure webové aplikace kontejneru dialogovým oknem][11]

    Klikněte na **OK** pro nasazení aplikace Java do kontejneru vybrané v prohlížeči.

    Ve výchozím nastavení má být nasazené aplikace jako podadresáře aplikační server. Pokud se má být nasazené jako kořenové aplikace, zaškrtněte políčko **nasadit do kořene** před kliknutím na **OK**.

1. Další byste měli vidět zobrazení **Protokolu Azure činnosti** , které bude indikaci stavu nasazení svojí webové aplikace.

    ![Protokol Azure činnosti][12]

    Proces nasazení webovou aplikaci Azure brát jenom několik sekund. Při vaší připravených aplikace se zobrazí odkaz s názvem **publikované** ve sloupci **Stav** . Když kliknete na odkaz, je přejdete nasazeném webovou aplikaci na domovskou stránku.

## <a name="updating-your-web-app"></a>Aktualizace webovou aplikaci

Aktualizace existující systém Azure Web Appu je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:

* Nasazení aplikace existující Web Java můžete aktualizovat.
* Můžete publikovat další aplikace Java do kontejneru stejné Web App.

V obou případech procesu je stejný a trvá jenom pár sekund:

1. V podokně zatmění projektu klikněte pravým tlačítkem myši Java aplikace, které chcete přidat do kontejneru existující Web aplikace nebo aktualizovat.

1. Po zobrazení místní nabídky vyberte **Azure** a zvolte **Publikovat jako Azure Web App....**

1. Protože se už přihlásili dříve, zobrazí se seznam stávajících kontejnerů v prohlížeči. Vyberte tu, kterou chcete publikovat nebo znova publikovat Java aplikaci a klikněte na **OK**.

Několik sekund, než později **Protokolu činnosti Azure** se zobrazí aktualizované nasazení jako **publikované** a budete moct ověření aktualizované aplikace ve webovém prohlížeči.

## <a name="stopping-an-existing-web-app"></a>Zastavení existující Web Appu

Ukončení existující Azure Web App kontejneru, (včetně všech nasazeném aplikace Java v nich), můžete použít zobrazení **Průzkumníka Azure** .

Pokud zobrazení **Průzkumníka Azure** ještě není otevřený, otevřete kliknutím na pak nabídku **okna** v zatmění, a pak klikněte na **Zobrazit zobrazení**, a **druhé …**a potom vyberte **Azure**a potom klikněte na položku **Průzkumník Azure**. Pokud jste to ještě přihlášeni dříve, zobrazí výzvu k tomu nevyzve.

Při zobrazení **Průzkumníka Azure** použití takto ukončíte webovou aplikaci: 

1. Rozbalte položku **Azure** .

1. Rozbalte položku **Web Apps** . 

1. Klikněte pravým tlačítkem požadovaný Web Appu.

1. Když se zobrazí na místní nabídku, klepněte na tlačítko **Zastavit**.

    ![Zastavení existující Web Appu][13]

## <a name="next-steps"></a>Další kroky

Další informace o sadách Azure pro Java IDEs najdete v následujících tématech:

- [Azure sada nástrojů pro Eclipse]
  - [Instalace Azure sada nástrojů pro Eclipse]
  - *Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění (Tento článek)*
  - [Co je nového v Azure sada nástrojů pro Eclipse]
- [Azure sada nástrojů pro IntelliJ]
  - [Instalace Azure sada nástrojů pro IntelliJ]
  - [Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ]
  - [Co je nového v Azure sada nástrojů pro IntelliJ]

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

Další informace o vytváření Azure Web Apps najdete v článku [Přehled webových aplikacích].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure sada nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure sada nástrojů pro IntelliJ]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Instalace Azure sada nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Instalace Azure sada nástrojů pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Co je nového v Azure sada nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Co je nového v Azure sada nástrojů pro IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java Azure]: https://azure.microsoft.com/develop/java/
[Přehled webových aplikací]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
