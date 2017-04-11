<properties 
    pageTitle="Přidání aplikace Java k Azure aplikace služby Web Apps" 
    description="Tento kurz se dozvíte, jak přidat na stránku nebo aplikací k instanci Azure aplikace služby webových aplikací Web Apps, které je už nakonfigurovali používat Java." 
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

# <a name="add-a-java-application-to-azure-app-service-web-apps"></a>Přidání aplikace Java k Azure aplikace služby Web Apps

Jakmile jste inicializovali webovou aplikaci Java v [Aplikaci služby Azure][] , jak je uvedeno na [vytvořit web app Java v aplikaci služby Azure](web-sites-java-get-started.md), můžete nahrávat aplikace tak, že zaškrtnete vaší WAR ve složce **webapps** .

Navigační cestu ke složce **webapps** liší v závislosti na nastavení instance Web Apps.

- Pokud jste nastavili webovou aplikaci pomocí Azure Marketplace, cestu ke složce **webapps** je ve formuláři **d:\home\site\wwwroot\bin\application\_server\webapps**, kde **aplikace\_serveru** je název serveru aplikace v podstatě instance Web Apps. 
- Pokud jste nastavili webovou aplikaci pomocí Azure konfigurace uživatelského rozhraní, cestu ke složce **webapps** je ve formuláři **d:\home\site\wwwroot\webapps**. 

Všimněte si, že k nahrání aplikace nebo webové stránky, včetně [scénáře průběžné integrace](app-service-continuous-deployment.md)můžete používat ovládacího prvku zdroje. FTP je také možnost pro odesílání aplikace nebo webové stránky.

Poznámka pro Tomcat webové aplikace: po nahrání souboru WAR ke složce **webapps** aplikační server Tomcat zjistí, kterou jste přidali a bude automaticky načíst. Všimněte si, že pokud zkopírujete (jiné než soubory WAR) do kořenového adresáře, aplikační server bude potřeba restartovat před použitím těchto souborů. Funkce autoload pro webové aplikace Tomcat Java spuštěna Azure je založena na nový soubor WAR vás někdo přidá, nebo nové soubory nebo adresáře přidána do složky **webapps** . 

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře Java](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[Azure aplikace služby]: http://go.microsoft.com/fwlink/?LinkId=529714
