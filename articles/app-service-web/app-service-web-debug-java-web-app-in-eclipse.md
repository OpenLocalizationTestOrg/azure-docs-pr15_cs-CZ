<properties 
    pageTitle="Ladění Java Web Appu na Azure v zatmění | Microsoft Azure" 
    description="Tento kurz se dozvíte, jak používat tuto sadu nástrojů Azure pro Eclipse ladění do webových aplikací Java spuštěna Azure." 
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
    ms.date="09/20/2016" 
    ms.author="asirveda;robmcm"/>

# <a name="debug-a-java-web-app-on-azure-in-eclipse"></a>Ladění Java Web Appu na Azure v zatmění

Tento kurz ukazuje, jak ladění spuštěna Azure pomocí [Nástrojů Azure pro Eclipse]do jazyka Java webových aplikací. Za účelem zjednodušení základní příklad Java serveru stránky (JSP) bude používat pro účely tohoto návodu, ale kroky by být stejný pro Java servlet při ladění na Azure.

Po dokončení tohoto kurzu při ladění v zatmění bude vypadat podobně jako na následujícím obrázku aplikace:

![][01]
 
## <a name="prerequisites"></a>Zjistit předpoklady pro

* Java vývojář Kit (JDK), v 1,8 nebo novější.
* -Zatmění integrovaném vývojovém prostředí pro vývojáře í Java džínovinu nebo novější. Můžete stáhnout z <http://www.eclipse.org/downloads/>.
* Rozdělení na základě Java webový server nebo aplikační server, například Apache Tomcat nebo molo.
* Azure předplatné, které lze získat z <https://azure.microsoft.com/en-us/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure sada nástrojů pro Eclipse. Další informace najdete v tématu [instalace Azure sada nástrojů pro Eclipse].
* Dynamické projekt webové vytvořené a nasazení aplikace služby Azure; Příklad v tématu [Vytvoření do Ahoj světě webových aplikací pro Azure v zatmění].

## <a name="to-debug-a-java-web-app-on-azure"></a>Ladění Java Web Appu na Azure

K dokončení těchto kroků v této části můžete použít existující dynamické Web projektu, který jste již nainstalovali jako Java Web App na Azure, stáhnout [Ukázkový dynamické Web projektu] a postupujte podle kroků v tématu [Vytvoření do Ahoj světě webových aplikací pro Azure v zatmění] pro nasazení na Azure. 

1. Otevřete zatmění.

1. Konfigurace časové limity relací pro vzdálené ladění:

    1. Klikněte na nabídku **Windows** v zatmění a potom klikněte na **Předvolby**.
    1. Rozbalte položku **Java** a potom vyberte **ladění**.
    1. Nastavení jak **ladění vypršení časového limitu ([ms)** i **spuštění vypršení časového limitu ([ms)** do `120000`.

        ![][02]

    1. Klikněte na **OK** zavřete dialogové okno **předvoleb** .

1. V zobrazení na zatmění Průzkumník projektu klikněte pravým tlačítkem dynamické Web projektu, který jste nainstalovali Azure. Po zobrazení místní nabídky vyberte **Ladění jako**a klikněte na **Azure v prohlížeči**.

    ![][03]

1. Pokud při prvním ladění dynamické Web projektu, otevře se dialogové okno **Konfigurace ladění** ; můžete nechejte výchozí hodnoty, které jsou určeny sada nástrojů na kartě **připojení** . Na kartě **zdroj** klikněte na **Přidat**a pak **Java projektu**, vyberte **Dynamické projekt webu**a klikněte na **OK**. Po dokončení těchto kroků, klikněte na **ladění**.

    ![][04]

1. Po zobrazení výzvy k **Povolení vzdáleného ladění vzdálené Web App teď?**, klikněte na **OK**.

1. Po zobrazení výzvy, který **je nyní připravena pro vzdálené ladění webovou aplikaci**, klikněte na **OK**.

    ![][05]

1. Dialogové okno **Konfigurace ladění** se znovu zobrazí, klepněte na tlačítko **ladění**.

1. Příkazový řádek systému Windows nebo Unix shell otevřete a připravit potřebné připojení pro ladění. potřebujete Počkejte, dokud nebude připojení ke vzdálené Java webové aplikace úspěšně až potom pokračujte. Pokud používáte Windows, bude vypadat jako na následujícím obrázku.

    ![][06]

1. Vložit konec přejděte na stránce JSP a potom otevřete adresu URL pro webovou aplikaci Java v prohlížeči:

    1. Otevřete **Průzkumníka Azure** ve zatmění.
    1. Přejděte na **Web Apps** a chcete ladění aplikace Java Web App.
    1. Klikněte pravým tlačítkem na Web App a klikněte na **Otevřít v prohlížeči**.
    1. Zatmění teď zahájí ladění režimu.

## <a name="next-steps"></a>Další kroky

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

Další informace o vytváření Azure Web Apps najdete v článku [Přehled webových aplikacích].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure sada nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse.md
[Instalace Azure sada nástrojů pro Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Ukázka dynamické Web projektu]: http://go.microsoft.com/fwlink/?LinkId=817337

[Středisko pro vývojáře Java Azure]: https://azure.microsoft.com/develop/java/
[Přehled webových aplikací]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-eclipse/01-debug-java-web-app-in-eclipse.png
[02]: ./media/app-service-web-debug-java-web-app-in-eclipse/02-configure-eclipse-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-eclipse/03-debug-as.png
[04]: ./media/app-service-web-debug-java-web-app-in-eclipse/04-debug-configurations.png
[05]: ./media/app-service-web-debug-java-web-app-in-eclipse/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-eclipse/06-windows-command-prompt-connection-successful-to-remote.png
