<properties 
    pageTitle="Úvod k aplikaci služby na Linux | Microsoft Azure" 
    description="Informace o aplikaci služby na Linux." 
    keywords="Služba Azure aplikací, linux, oss"
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

# <a name="introduction-to-app-service-on-linux"></a>Úvod k aplikaci služby na Linux
Služba aplikací na Linux právě Public Preview a nativně podporuje pracovního webových aplikací web apps na Linux. 

## <a name="overview"></a>Základní informace ##
Zákazníci můžete aplikaci služby na Linux hostitele web apps nativně na Linux podporovaných aplikací štosech. V následující části funkce seznamy aktuálně podporovaných aplikací štosech.

## <a name="features"></a>Funkce ##
Služba aplikací na Linux v současné době podporuje následující balíčky aplikace

- Node.js
- PHP

Zákazníci mohli nasadit žádostí o použití

- FTP.
- Místní libovolná.
- GitHub nebo BitBucket.

Pro změnu velikosti aplikace


- Zákazníci můžete změnit jejich web appu nahoru a dolů změnou osy v jejich plán služeb aplikací. 
- Zákazníci můžete rozšiřování žádostí o a spusťte jejich aplikace napříč několika výskyty textu v rámci jejich SKU.

Pro Kudu některé základní funkce budou fungovat

- Prostředí.
- Nasazení.
- Základní konzoly.

## <a name="limitations"></a>Omezení ##

Na portálu Správa Azure jenom zobrazit aktuálně podporované funkce pro aplikaci služby na Linux a zbytek skryli. Jako náš tým povolení dalších funkcí jsme bude mít uvědomění si to na portálu Správa. Některé funkce, jako je VNET integrace a AAD / ověřování třetích stran a rozšíření webů Kudu nefungují aktuálně. Ale jako jsme to práce get budeme aktualizovat naše si přečtěte následující dokumentaci a blog o změnách.

V tomto veřejné náhledu je aktuálně dostupná jenom v těchto oblastech

-   Západ USA.
-   Západní Europe.
-   Jihovýchodní Asie.

V prohlížeči na Linux je podporována pouze v snaží o aplikaci služby plány jednotného zasílání zpráv a nemá Free nebo sdílený osy. Aplikace služby plány pro běžné a Linux web apps se také vylučují, tak do webových aplikací Linux nelze vytvořit v plánu není Linux aplikace služby.

V prohlížeči na Linux je vytvořit v skupina zdroje, který neobsahuje-Linux webové aplikace ve stejné oblasti.

Z důvodu nedostatku překrývající recyklace z těchto webových aplikací zákazníci očekávat, že máte nerestartuje malé prostoje v případě do webových aplikací. 

## <a name="next-steps"></a>Další kroky ##

Postupujte podle následujících odkazů na článek Začínáme s aplikací služby na Linux. Na [naše fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview)zveřejňují otázky a problémy.

* [Vytváření webových aplikací Web Apps v aplikaci služby na Linux](./app-service-linux-how-to-create-a-web-app.md)
* [Pomocí konfigurace PM2 pro Node.js ve webových aplikacích na Linux](./app-service-linux-using-nodejs-pm2.md)

