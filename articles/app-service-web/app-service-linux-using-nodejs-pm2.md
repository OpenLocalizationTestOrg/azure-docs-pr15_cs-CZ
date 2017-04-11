<properties 
    pageTitle="Pomocí konfigurace PM2 pro NodeJS ve webových aplikacích na Linux | Microsoft Azure" 
    description="Pomocí konfigurace PM2 pro NodeJS ve webových aplikacích na Linux" 
    keywords="Služba Azure aplikací v prohlížeči, nodejs, pm2, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Pomocí konfigurace PM2 pro Node.js ve webových aplikacích na Linux

Pokud má argument zásobníku aplikace Node.js pro Web Apps na Linux, zobrazí se vám nastavit spouštěcí soubor Node.js, jak je znázorněno na následujícím obrázku.

![][1]

Tímto způsobem můžete buď

-   Zadat skript při spuštění aplikace Node.js (například: /bin/server.js)
-   Určení konfiguračního souboru PM2 používat aplikaci Node.js (například: /foo/process.json)

 >[AZURE.NOTE] Pokud budete potřebovat uzel procesy automaticky restartujte při změně některé soubory, musíte se PM2 konfigurace. V opačném případě aplikace nebude výzvy obdrží upozornění na změnu od například nepřetržitý nasazení při změně kód aplikace.

Node.js [si přečtěte následující dokumentaci soubor obrázku](http://pm2.keymetrics.io/docs/usage/application-declaration/) pro všechny možnosti, ale tady je ukázka byste měli použít jako soubor process.json

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Důležité je potřeba pamatovat v této konfiguraci věcí 

-   Vlastnost "skript" Určuje skript spuštění aplikace.
-   Vlastnost "případy" Určuje, kolik instancí uzel postupuje při spuštění. Pokud používáte aplikaci na větších OM, které mají víc jádra, budete chtít maximalizace svých prostředcích nastavením hodnotu vyšší tady.
-   Pole "sledováním" Určuje všechny soubory, jejichž změny chcete ho restartovat uzel procesů.
-   "Watch_options" aktuálně potřebujete zadat "usePolling" jako PRAVDA vzhledem ke způsobu, který je připojen váš obsah aplikace.


## <a name="next-steps"></a>Další kroky ##

* [Co je aplikace služby na Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png