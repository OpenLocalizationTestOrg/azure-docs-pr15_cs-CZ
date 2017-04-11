<properties 
    pageTitle="Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ | Microsoft Azure" 
    description="Tento kurz se dozvíte, jak používat tuto sadu nástrojů Azure pro IntelliJ při vytváření Ahoj světě webové aplikace pro Azure." 
    services="app-service\web" 
    documentationCenter="java" 
    authors="selvasingh" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="create-a-hello-world-web-app-for-azure-in-intellij"></a>Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ

Tento kurz ukazuje, jak vytvářet a publikovat základní Vítáme aplikace Azure jako do webových aplikací pomocí [Nástrojů Azure pro IntelliJ]. Pro zjednodušení jsou uvedeny základní JSP příkladu, ale velmi podobným způsobem vhodné pro Java servlet jde Azure nasazení.

Po dokončení tohoto kurzu aplikace bude vypadat podobně jako na následujícím obrázku, když zobrazíte ve webovém prohlížeči:

![][01]
 
## <a name="prerequisites"></a>Zjistit předpoklady pro

* Java vývojář Kit (JDK), v 1,8 nebo novější.
* IntelliJ NÁPAD Ultimate Edition. Můžete stáhnout z <https://www.jetbrains.com/idea/download/index.html>.
* Rozdělení na základě Java webový server nebo aplikační server, například Apache Tomcat nebo molo.
* Azure předplatné, které lze získat z <https://azure.microsoft.com/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure sada nástrojů IntelliJ Další informace najdete v tématu [instalace Azure sada nástrojů pro IntelliJ].

## <a name="to-create-a-hello-world-application"></a>Vytvoření aplikace Ahoj prezentace

Nejdřív budete Začneme s vytvářením Java projektu.

1. Zahájení IntelliJ a v nabídce klikněte na **soubor**a potom na **Nový**a potom klikněte na **projekt**.

   ![][02]

1. V dialogovém okně Nový projekt vyberte **Java**, pak **Webové aplikace**a klikněte na tlačítko **Další**.

   ![][03a]

   Po zobrazení výzvy pokračujte bez SDK přiřazené, klikněte na **Ano**.

   ![][03b]

1. Pro účely tohoto kurzu název projektu **Java – Web-aplikace-na-Azure**a potom klikněte na **Dokončit**.

   ![][04]

1. V zobrazení na IntelliJ Průzkumník projektu rozbalte **Java – Web-aplikace-na-Azure**, a pak rozbalte **web**a potom poklepejte **index.jsp**.

   ![][05]

1. Při otevření souboru index.jsp v IntelliJ přidejte v dynamicky zobrazený text **Vítáme!** ve stávající `<body>` prvek. Vaše aktualizované `<body>` obsahu by měla vypadat podobně jako v následujícím příkladu:

   `<body><b><% out.println("Hello World!"); %></b></body>` 

1. Uložení index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Nasazení aplikace na kontejner Azure Web App

Existuje několik způsobů, kterými můžete nasadit aplikaci web Java Azure. Tento kurz popisuje nějaká nejjednodušší: aplikace má být nasazené na kontejner Azure Web App – potřeby žádný typ zvláštní projektu ani další nástroje. JDK a příslušný software kontejneru web vám poskytne vám Azure, tak, aby je potřeba nahrát vlastní; potřebujete stačí webovou aplikaci Java. V důsledku toho publikování aplikace bude trvat sekund, ne minut.

1. V Průzkumníkovi na IntelliJ projektu klikněte pravým tlačítkem myši **Java – Web-aplikace-na-Azure** projektu. Po zobrazení místní nabídky vyberte **Azure**a potom klikněte na **Publikovat jako Azure Web App....**

   ![][06]

1. Pokud jste to ještě již přihlásili k Azure z IntelliJ, zobrazí se výzva k přihlášení k účtu Azure:

   ![][07]

   Poznámka: Pokud máte víc účtů Azure pokynů během procesu přihlášení může zobrazeny některé víckrát, přestože zobrazují stejné. Důvody tohoto chování, pokračujte po znaménku v pokyny.

1. Po úspěšném podepsání do účtu Azure se zobrazí dialogové okno **Spravovat předplatná** seznam předplatných, která jsou přidružena svoje přihlašovací údaje. Pokud jsou uvedené víc předplatných a chcete pracovat s konkrétní dílčí z nich, které může taky zrušit zaškrtnutí políčka těch, které chcete použít. Pokud jste vybrali předplatného, klikněte na tlačítko **Zavřít**.

   ![][08]

1. Když se zobrazí dialogové okno **nasazení Azure webové aplikace kontejneru** , zobrazí žádné kontejnery webové aplikace, které jste dříve vytvořili; Pokud jste nevytvořili žádné kontejnery, bude seznamu prázdné.   

   ![][09]

1. Pokud jste nevytvořili kontejneru Azure webové aplikace před nebo pokud chcete publikovat aplikace do nového kontejneru, použijte následující kroky. V opačném vyberte kontejneru existující Web App a přejděte ke kroku 6 níže.

  1. Klikněte na**+**

        ![][10]

  1. Dialogové okno **Nový Web App kontejner** zobrazí se, které bude sloužit k další několik kroků.

        ![][11]

  1. Zadejte **štítek DNS** pro váš Web App kontejner; to bude formuláře popisek DNS listu adresa URL hostitele vaší webové aplikace v Azure Poznámka: Název musí být k dispozici a souladu s požadavky na pojmenování Azure Web Appu.

  1. V nabídce **Web kontejneru** rozevíracího seznamu vyberte příslušný software pro aplikaci.

        V současné době můžete z Tomcat 8, Tomcat 7 nebo molo 9. Poslední distribuce vybrané softwaru vám poskytne Azure a se spustí na poslední rozdělení JDK 8 vytvořené Oracle a poskytovanou Azure.

  1. V rozevírací nabídce **předplatné** vyberte předplatné, které chcete použít pro toto nasazení.

  1. V rozevírací nabídce **Pole Skupina zdroje** vyberte skupina zdroje, ke kterému chcete přidružit webovou aplikaci.

        Poznámka: Azure skupiny zdrojů umožňují seskupení související materiály tak, aby, například, můžete je odstranit společně.

        Můžete vybrat existující pole Skupina zdroje (Pokud máte některý) a vynechat, můžete přecházet g dole nebo pomocí následujících kroků k vytvoření nové skupiny prostředků následující:

      * Klikněte na **nové...**

      * Zobrazí se dialogové okno **Nová skupina zdroje** :

            ![][12]

      * V textového pole **název** zadejte název nové skupiny prostředků.

      * V **oblast** rozevírací nabídce vyberte odpovídající Azure datového centra umístění skupina zdroje.

      * Klikněte na **OK**.

  1. Rozevírací nabídky **Aplikace služby plánování** uvádí plány aplikace služby, které jsou přidružené ke skupině zdroje, který jste vybrali.

        Poznámka: Služba aplikace plánování určuje informace jako je místo webovou aplikaci, ceny osy a velikosti instance výpočetním. Jeden plán služeb aplikací se dá použít pro více webových aplikací Web Apps, a proto je udržovat zvlášť z konkrétního nasazení Web Appu.

        Vyberte stávající aplikace služby plánování (Pokud máte některý) a přeskočit můžete přecházet h dole nebo můžete použít následující následujících kroků k vytvoření nového plánu služby aplikace:

      * Klikněte na **nové...**

      * Zobrazí se dialogové okno **Nové aplikace služby plán** :

            ![][13]

      * V textového pole **název** zadejte název nové aplikace služby plánu.

      * V **umístění** rozevírací nabídce vyberte odpovídající Azure datového centra umístění pro plán.

      * V rozevírací nabídce **Ceny osy** , vyberte odpovídající ceny pro plán. Pro účely testování můžete **Free**.

      * V Instance rozevírací nabídky **Velikost** , vyberte příslušnou instanci změnit velikost pro plán. Pro účely testování můžete **malých**.

  1. Jakmile dokončíte všechny výše uvedené kroky, dialogové okno nový Web App kontejner by měl vypadat na následujícím obrázku:

        ![][14]

  1. Klikněte na **OK** dokončete vytváření nové kontejneru v prohlížeči.

        Počkejte několik sekund, než seznam kontejnery Web Appu tak možné je aktualizovat, a měly by být vybrány váš kontejner nově vytvořený web app v seznamu.

1. Nyní jste připraveni k dokončení počáteční nasazení webové aplikace Azure; Klikněte na **OK** pro nasazení aplikace Java do kontejneru vybrané v prohlížeči.

    ![][15]

    Poznámka: Ve výchozím nastavení aplikace má být nasazené jako podadresáře aplikační server. Pokud se má být nasazené jako kořenové aplikace, zaškrtněte políčko **nasadit do kořene** před kliknutím na **OK**.

1. Další byste měli vidět zobrazení **Protokolu Azure činnosti** , které bude indikaci stavu nasazení svojí webové aplikace.

    ![][16]

    Proces nasazení webovou aplikaci Azure brát jenom několik sekund. Při vaší připravených aplikace se zobrazí odkaz s názvem **publikované** ve sloupci **Stav** . Po kliknutí na odkaz přejdete na nasazeném webovou aplikaci na domovskou stránku nebo pomocí postupu v následující části umožňuje přecházet na web appu.

## <a name="browsing-to-your-web-app-on-azure"></a>Procházení do webové aplikace na Azure

Chcete brows do webové aplikace na Azure můžete použít zobrazení **Průzkumníka Azure** .

Pokud zobrazení **Průzkumníka Azure** ještě není otevřený, otevřete kliknutím na potom nabídky **zobrazení** v IntelliJ, a pak klikněte na **Panel nástrojů**a potom klikněte na položku **Průzkumník služby**. Pokud jste to ještě přihlášeni dříve, zobrazí výzvu k tomu nevyzve.

Při zobrazení **Průzkumníka Azure** použití takto ukončíte webovou aplikaci: 

1. Rozbalte položku **Azure** .

1. Rozbalte položku **Web Apps** . 

1. Klikněte pravým tlačítkem požadovaný Web Appu.

1. Po zobrazení v místní nabídce klikněte na **Otevřít v prohlížeči**.

    ![][17]

## <a name="updating-your-web-app"></a>Aktualizace webovou aplikaci

Aktualizace existující systém Azure Web Appu je rychlý a snadný proces a máte dvě možnosti pro aktualizaci:

* Nasazení aplikace existující Web Java můžete aktualizovat.
* Můžete publikovat další aplikace Java do kontejneru stejné Web App.

V obou případech procesu je stejný a trvá jenom pár sekund:

1. V podokně IntelliJ projektu klikněte pravým tlačítkem myši Java aplikace, které chcete přidat do kontejneru existující Web aplikace nebo aktualizovat.

1. Po zobrazení místní nabídky vyberte **Azure** a zvolte **Publikovat jako Azure Web App....**

1. Protože se už přihlásili dříve, zobrazí se seznam stávajících kontejnerů v prohlížeči. Vyberte tu, kterou chcete publikovat nebo znova publikovat Java aplikaci a klikněte na **OK**.

Několik sekund, než později **Protokolu činnosti Azure** se zobrazí aktualizované nasazení jako **publikované** a budete moct ověření aktualizované aplikace ve webovém prohlížeči.

## <a name="starting-or-stopping-an-existing-web-app"></a>Spuštění nebo zastavení existující Web Appu

Spuštění nebo zastavení existující kontejner Azure v prohlížeči, (včetně všech nasazeném aplikace Java v nich), můžete použít zobrazení **Průzkumníka Azure** .

Pokud zobrazení **Průzkumníka Azure** ještě není otevřený, otevřete kliknutím na potom nabídky **zobrazení** v IntelliJ, a pak klikněte na **Panel nástrojů**a potom klikněte na položku **Průzkumník služby**. Pokud jste to ještě přihlášeni dříve, zobrazí výzvu k tomu nevyzve.

Při zobrazení **Průzkumníka Azure** použití tímto postupem zahájení nebo ukončení webovou aplikaci: 

1. Rozbalte položku **Azure** .

1. Rozbalte položku **Web Apps** . 

1. Klikněte pravým tlačítkem požadovaný Web Appu.

1. Po zobrazení v místní nabídce klikněte na **Spustit** nebo **Zastavit**. Všimněte si, že nabídku možností kontextu vědět, abyste je mohli pouze zastavit spuštěný web appu a začněte do webových aplikací, které neběží.

    ![][18]

## <a name="next-steps"></a>Další kroky

Další informace o sadách Azure pro Java IDEs najdete v následujících tématech:

- [Azure sada nástrojů pro Eclipse]
  - [Instalace Azure sada nástrojů pro Eclipse]
  - [Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění]
  - [Co je nového v Azure sada nástrojů pro Eclipse]
- [Azure sada nástrojů pro IntelliJ]
  - [Instalace Azure sada nástrojů pro IntelliJ]
  - *Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ (Tento článek)*
  - [Co je nového v Azure sada nástrojů pro IntelliJ]

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

Další informace o vytváření Azure Web Apps najdete v článku [Přehled webových aplikacích].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure sada nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse.md
[Azure sada nástrojů pro IntelliJ]: ../azure-toolkit-for-intellij.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Instalace Azure sada nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Instalace Azure sada nástrojů pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Co je nového v Azure sada nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse-whats-new.md
[Co je nového v Azure sada nástrojů pro IntelliJ]: ../azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java Azure]: https://azure.microsoft.com/develop/java/
[Přehled webových aplikací]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-No-SDK-Specified.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
