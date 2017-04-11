<properties 
    pageTitle="Ladění Java Web Appu na Azure v IntelliJ | Microsoft Azure" 
    description="Tento kurz se dozvíte, jak používat tuto sadu nástrojů Azure pro IntelliJ ladění do webových aplikací Java spuštěna Azure." 
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

# <a name="debug-a-java-web-app-on-azure-in-intellij"></a>Ladění Java Web Appu na Azure v IntelliJ

Tento kurz ukazuje, jak ladění spuštěna Azure pomocí [Nástrojů Azure pro IntelliJ]do jazyka Java webových aplikací. Za účelem zjednodušení základní příklad Java serveru stránky (JSP) bude používat pro účely tohoto návodu, ale kroky by být stejný pro Java servlet při ladění na Azure.

Po dokončení tohoto kurzu při ladění v IntelliJ bude vypadat podobně jako na následujícím obrázku aplikace:

![][01]
 
## <a name="prerequisites"></a>Zjistit předpoklady pro

* Java vývojář Kit (JDK), v 1,8 nebo novější.
* IntelliJ NÁPAD Ultimate Edition. Můžete stáhnout z <https://www.jetbrains.com/idea/download/index.html>.
* Rozdělení na základě Java webový server nebo aplikační server, například Apache Tomcat nebo molo.
* Azure předplatné, které lze získat z <https://azure.microsoft.com/en-us/free/> nebo <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure sada nástrojů IntelliJ Další informace najdete v tématu [instalace Azure sada nástrojů pro IntelliJ].
* Dynamické projekt webové vytvořené a nasazení aplikace služby Azure; Příklad v tématu [Vytvoření do Ahoj světě webových aplikací pro Azure v IntelliJ].

## <a name="to-debug-a-java-web-app-on-azure"></a>Ladění Java Web Appu na Azure

K dokončení těchto kroků v této části můžete použít existující dynamické Web projektu, který jste již nainstalovali jako Java Web App na Azure, stáhnout [Ukázkový dynamické Web projektu] a postupujte podle kroků v tématu [Vytvoření do Ahoj světě webových aplikací pro Azure v IntelliJ] pro nasazení na Azure. 

1. Otevřete projekt Java v prohlížeči, který používaný Azure v IntelliJ.

1. V nabídce **Spustit** a potom klikněte na **Upravit konfigurace**.

    ![][02]

1. Když se otevře dialogové okno **Spustit/ladění konfigurace** : 

    1. Vyberte **Azure Web App**.
    1. Klikněte na **+** přidáte novou konfiguraci.
    1. Zadejte **název** pro konfiguraci.
    1. Přijměte zbývající výchozí hodnoty, které jsou navržené tak, že Azure nástrojů a klikněte na **OK**.

        ![][03]

1. Vyberte konfiguraci ladění Azure v prohlížeči, který jste vytvořili v pravé horní části nabídky a klepněte na **ladění**

    ![][04]

1. Po zobrazení výzvy k **Povolení vzdáleného ladění vzdálené Web App teď?**, klikněte na **OK**.

1. Po zobrazení výzvy, který **je nyní připravena pro vzdálené ladění webovou aplikaci**, klikněte na **OK**.

    ![][05]

1. Vyberte konfiguraci ladění Azure v prohlížeči, který jste vytvořili v pravé horní části nabídky a pak klepněte na **ladění**.

1. Příkazový řádek systému Windows nebo Unix shell otevřete a připravit potřebné připojení pro ladění. potřebujete Počkejte, dokud nebude připojení ke vzdálené Java webové aplikace úspěšně až potom pokračujte. Pokud používáte Windows, bude vypadat jako na následujícím obrázku:

    ![][06]

1. Vložit konec přejděte na stránce JSP a potom otevřete adresu URL pro webovou aplikaci Java v prohlížeči:

    1. Otevřete **Průzkumníka Azure** ve IntelliJ.
    1. Přejděte na **Web Apps** a chcete ladění aplikace Java Web App.
    1. Klikněte pravým tlačítkem na Web App a klikněte na **Otevřít v prohlížeči**.
    1. IntelliJ se teď zadejte do režimu ladění.

## <a name="next-steps"></a>Další kroky

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

Další informace o vytváření Azure Web Apps najdete v článku [Přehled webových aplikacích].

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Azure App Service]: http://go.microsoft.com/fwlink/?LinkId=529714
[Azure sada nástrojů pro IntelliJ]: ../azure-toolkit-for-intellij.md
[Instalace Azure sada nástrojů pro IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Ukázka dynamické Web projektu]: http://go.microsoft.com/fwlink/?LinkId=817337

[Středisko pro vývojáře Java Azure]: https://azure.microsoft.com/develop/java/
[Přehled webových aplikací]: ./app-service-web-overview.md

<!-- IMG List -->

[01]: ./media/app-service-web-debug-java-web-app-in-intellij/01-debug-java-web-app-in-intellij.png
[02]: ./media/app-service-web-debug-java-web-app-in-intellij/02-configure-intellij-remote-debug.png
[03]: ./media/app-service-web-debug-java-web-app-in-intellij/03-debug-configuration.png
[04]: ./media/app-service-web-debug-java-web-app-in-intellij/04-select-debug.png
[05]: ./media/app-service-web-debug-java-web-app-in-intellij/05-ready-for-remote-debugging.png
[06]: ./media/app-service-web-debug-java-web-app-in-intellij/06-windows-command-prompt-connection-successful-to-remote.png
