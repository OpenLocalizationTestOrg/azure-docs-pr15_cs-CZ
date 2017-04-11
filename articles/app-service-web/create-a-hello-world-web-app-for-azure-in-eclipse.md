<properties 
    pageTitle="Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění" 
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
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-eclipse"></a>Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění

Tento kurz ukazuje, jak vytvářet a publikovat základní Vítáme aplikace Azure jako do webových aplikací pomocí [Nástrojů Azure pro Eclipse]. Pro zjednodušení jsou uvedeny základní JSP příkladu, ale velmi podobným způsobem vhodné pro Java servlet jde Azure nasazení.

Po dokončení tohoto kurzu aplikace bude vypadat podobně jako na následujícím obrázku, když zobrazíte ve webovém prohlížeči:

![][01]
 
## <a name="prerequisites"></a>Zjistit předpoklady pro

* Java vývojář Kit (JDK), v 1.7 nebo novější.
* -Zatmění integrovaném vývojovém prostředí pro vývojáře í Java džínovinu nebo novější. Můžete stáhnout z <http://www.eclipse.org/downloads/>.
* Rozdělení na základě Java webový server nebo aplikační server, například Apache Tomcat nebo molo.
* Azure předplatné, které lze získat z <https://azure.microsoft.com/en-us/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure sada nástrojů pro Eclipse. Další informace najdete v tématu [instalace Azure sada nástrojů pro Eclipse].

## <a name="to-create-a-hello-world-application"></a>Vytvoření aplikace Ahoj prezentace

Nejdřív budete Začneme s vytvářením Java projektu.

1. Zahájení zatmění, v nabídce klikněte na **soubor**, klikněte na **Nový**a klikněte na **Dynamické Project Web**. (Pokud se nezobrazuje **Dynamické Project Web** uvedené jako projektového plánu dostupné po kliknutí na **soubor** a **Nový**, proveďte následující kroky: klikněte na **soubor**, klikněte na **Nový**, klikněte na **projekt...**, rozbalte **Web**, klikněte na **Dynamické Project Web**a klikněte na tlačítko **Další**.)

1. Pro účely tohoto kurzu zadejte název projektu **MyHelloWorld**. Obrazovce bude vypadat takto:

    ![][02]

1. Klikněte na **Dokončit**.

1. V zobrazení na zatmění Průzkumník projektu rozbalte **MyHelloWorld**. Klikněte pravým tlačítkem na **obsah webu**, klikněte na **Nový**a potom klikněte na **Soubor JSP**.

1. V dialogovém okně **Nový soubor JSP** zadejte název souboru **index.jsp**. Zachovat nadřazenou složku jako **MyHelloWorld/obsah webu**.

1. V dialogovém okně **Vybrat šablonu JSP** pro účely tohoto kurzu vyberte **Nový soubor JSP (html)**a potom klikněte na **Dokončit**.

1. Při otevření souboru index.jsp v zatmění přidejte v dynamicky zobrazený text **Vítáme!** ve stávající `<body>` prvek. Vaše aktualizované `<body>` obsahu by měla vypadat podobně jako v následujícím příkladu:

    `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Uložení index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Nasazení aplikace na kontejner Azure Web App

Existuje několik způsobů, kterými můžete nasadit aplikaci web Java Azure. Tento kurz popisuje nějaká nejjednodušší: aplikace má být nasazené na kontejner Azure Web App – potřeby žádný typ zvláštní projektu ani další nástroje. JDK a příslušný software kontejneru web vám poskytne vám Azure, tak, aby je potřeba nahrát vlastní; potřebujete stačí webovou aplikaci Java. V důsledku toho publikování aplikace bude trvat sekund, ne minut.

1. V Průzkumníkovi na zatmění projektu klikněte pravým tlačítkem myši **MyHelloWorld**.

1. V místní nabídce vyberte **Azure**a klikněte na **Publikovat jako Azure Web App....**

    ![][03]
   
    Můžete taky-li vybrán projektu webové aplikace je v okně Průzkumník projektu, můžete na tlačítko **Publikovat** rozevíracího seznamu na panelu nástrojů a vyberte **Publikovat jako Azure Web App** odsud:
   
    ![][14]
   
1. Pokud jste to ještě již přihlásili k Azure z zatmění, zobrazí se výzva k přihlášení k účtu Azure:

    ![][04]
   
    Poznámka: Pokud máte víc účtů Azure pokynů během procesu přihlášení může zobrazeny některé víckrát, přestože zobrazují stejné. Důvody tohoto chování, pokračujte po znaménku v pokyny.
1. Po úspěšném podepsání do účtu Azure se zobrazí dialogové okno **Spravovat předplatná** seznam předplatných, která jsou přidružena svoje přihlašovací údaje. Pokud jsou uvedené víc předplatných a chcete pracovat s konkrétní dílčí z nich, které může taky zrušit zaškrtnutí políčka těch, které chcete použít. Pokud jste vybrali předplatného, klikněte na tlačítko **Zavřít**.

    ![][05]
   
1. Když se zobrazí dialogové okno **nasazení Azure webové aplikace kontejneru** , zobrazí žádné kontejnery webové aplikace, které jste dříve vytvořili; Pokud jste nevytvořili žádné kontejnery, bude seznamu prázdné.

    ![][06]
   
1. Pokud jste nevytvořili kontejneru Azure webové aplikace před nebo pokud chcete publikovat aplikace do nového kontejneru, použijte následující kroky. V opačném vyberte kontejneru existující Web App a přejděte ke kroku 7 pod.

    1. Klikněte na **nové...**

    1. Zobrazí se dialogové okno **Nový Web App kontejner** :

        ![][07]

    1. Zadejte **štítek DNS** pro váš Web App kontejner; to bude formuláře popisek DNS listu adresa URL hostitele vaší webové aplikace v Azure Poznámka: Název musí být k dispozici a souladu s požadavky na pojmenování Azure Web Appu.

    1. V nabídce **Web kontejneru** rozevíracího seznamu vyberte příslušný software pro aplikaci.

        V současné době můžete z Tomcat 8, Tomcat 7 nebo molo 9. Poslední distribuce vybrané softwaru vám poskytne Azure a se spustí na poslední rozdělení JDK 8 vytvořené Oracle a poskytovanou Azure.

    1. V rozevírací nabídce **předplatné** vyberte předplatné, které chcete použít pro toto nasazení.

    1. V rozevírací nabídce **Pole Skupina zdroje** vyberte skupina zdroje, ke kterému chcete přidružit webovou aplikaci.

        Poznámka: Azure skupiny zdrojů umožňují seskupení související materiály tak, aby, například, můžete je odstranit společně.

        Můžete vybrat existující pole Skupina zdroje (Pokud máte některý) a vynechat, můžete přecházet g dole nebo pomocí následujících kroků k vytvoření nové skupiny prostředků následující:

        * Klikněte na **nové...**

        * Zobrazí se dialogové okno **Nová skupina zdroje** :

            ![][08]

        * V textového pole **název** zadejte název nové skupiny prostředků.

        * V **oblast** rozevírací nabídce vyberte odpovídající Azure datového centra umístění skupina zdroje.

        * Klikněte na **OK**.

    1. Rozevírací nabídky **Aplikace služby plánování** uvádí plány aplikace služby, které jsou přidružené ke skupině zdroje, který jste vybrali.

        Poznámka: Služba aplikace plánování určuje informace jako je místo webovou aplikaci, ceny osy a velikosti instance výpočetním. Jeden plán služeb aplikací se dá použít pro více webových aplikací Web Apps, a proto je udržovat zvlášť z konkrétního nasazení Web Appu.

        Vyberte stávající aplikace služby plánování (Pokud máte některý) a přeskočit můžete přecházet h dole nebo můžete použít následující následujících kroků k vytvoření nového plánu služby aplikace:

        * Klikněte na **nové...**

        * Zobrazí se dialogové okno **Nové aplikace služby plán** :

            ![][09]

        * V textového pole **název** zadejte název nové aplikace služby plánu.

        * V **umístění** rozevírací nabídce vyberte odpovídající Azure datového centra umístění pro plán.

        * V rozevírací nabídce **Ceny osy** , vyberte odpovídající ceny pro plán. Pro účely testování můžete **Free**.

        * V Instance rozevírací nabídky **Velikost** , vyberte příslušnou instanci změnit velikost pro plán. Pro účely testování můžete **malých**.

    1. Jakmile dokončíte všechny výše uvedené kroky, dialogové okno nový Web App kontejner by měl vypadat na následujícím obrázku:

        ![][10]

    1. Klikněte na **OK** dokončete vytváření nové kontejneru v prohlížeči.

        Počkejte několik sekund, než seznam kontejnery Web Appu tak možné je aktualizovat, a měly by být vybrány váš kontejner nově vytvořený web app v seznamu.

1. Teď jste připraveni k dokončení počáteční nasazení webové aplikace Azure:

    ![][11]

    Klikněte na **OK** pro nasazení aplikace Java do kontejneru vybrané v prohlížeči.

    Poznámka: Ve výchozím nastavení aplikace má být nasazené jako podadresáře aplikační server. Pokud se má být nasazené jako kořenové aplikace, zaškrtněte políčko **nasadit do kořene** před kliknutím na **OK**.

1. Další byste měli vidět zobrazení **Protokolu Azure činnosti** , které bude indikaci stavu nasazení svojí webové aplikace.

    ![][12]

    Proces nasazení webovou aplikaci Azure brát jenom několik sekund. Při vaší připravených aplikace se zobrazí odkaz s názvem **publikované** ve sloupci **Stav** . Když kliknete na odkaz, je přejdete nasazeném webovou aplikaci na domovskou stránku.

## <a name="updating-your-web-app"></a>Aktualizace webovou aplikaci

Aktualizace existující systém Azure Web Appu je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:

* Nasazení aplikace existující Web Java můžete aktualizovat.
* Můžete publikovat další aplikace Java do kontejneru stejné Web App.

V obou případech procesu je stejný a trvá jenom pár sekund:

1. V podokně zatmění projektu klikněte pravým tlačítkem myši Java aplikace, které chcete přidat do kontejneru existující Web aplikace nebo aktualizovat.

2. Po zobrazení místní nabídky vyberte **Azure** a zvolte **Publikovat jako Azure Web App....**

3. Protože se už přihlásili dříve, zobrazí se seznam stávajících kontejnerů v prohlížeči. Vyberte tu, kterou chcete publikovat nebo znova publikovat Java aplikaci a klikněte na **OK**.

Několik sekund, než později **Protokolu činnosti Azure** se zobrazí aktualizované nasazení jako **publikované** a budete moct ověření aktualizované aplikace ve webovém prohlížeči.

## <a name="stopping-an-existing-web-app"></a>Zastavení existující Web Appu

Ukončení existující Azure Web App kontejneru, (včetně všech nasazeném aplikace Java v nich), můžete použít zobrazení **Průzkumníka Azure** .

Pokud zobrazení **Průzkumníka Azure** ještě není otevřený, otevřete kliknutím na pak nabídku **okna** v zatmění, a pak klikněte na **Zobrazit zobrazení**, a **druhé …**a potom vyberte **Azure**a potom klikněte na položku **Průzkumník Azure**. Pokud jste to ještě přihlášeni dříve, zobrazí výzvu k tomu nevyzve.

Při zobrazení **Průzkumníka Azure** použití takto ukončíte webovou aplikaci: 

1. Rozbalte položku **Azure** .

1. Rozbalte položku **Web Apps** . 

1. Klikněte pravým tlačítkem požadovaný Web Appu.

1. Když se zobrazí na místní nabídku, klepněte na tlačítko **Zastavit**.

    ![][13]

## <a name="next-steps"></a>Další kroky

Další informace najdete v následujících tématech:

* [Středisko pro vývojáře Java](/develop/java/).
* [Přehled webových aplikací](app-service-web-overview.md)

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[01]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/01-Web-Page.png
[02]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/02-Dynamic-Web-Project.png
[03]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/03-Context-Menu.png
[04]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/04-Log-In-Dialog.png
[05]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/05-Manage-Subscriptions-Dialog.png
[06]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/06-Deploy-To-Azure-Web-Container.png
[07]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/07-New-Web-App-Container-Dialog.png
[08]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/08-New-Resource-Group-Dialog.png
[09]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/09-New-Service-Plan-Dialog.png
[10]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/11-Completed-Deploy-Dialog.png
[12]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/12-Activity-Log-View.png
[13]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/13-Azure-Explorer-Web-App.png
[14]: ./media/create-a-hello-world-web-app-for-azure-in-eclipse/publishDropdownButton.png