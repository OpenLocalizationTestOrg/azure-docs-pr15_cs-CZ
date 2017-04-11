<properties 
    pageTitle="Jak změnit velikost aplikace v prostředí aplikace služby" 
    description="Změna měřítka aplikace v prostředí aplikace služby" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Změna měřítka aplikací v prostředí aplikace služby #

V aplikaci služby Azure jsou obvykle tři věci, které je možné přizpůsobit:

- ceny plán
- velikost kolegy 
- počet instancí.

V pomocného mechanismu řízení je nutné vybrat nebo změnit ceny plán.  Z hlediska funkce je už na Premium ceny funkce úroveň.  

Pokud jde o velikosti pracovní můžete přiřadit admin pomocného mechanismu řízení velikost výpočetním zdroje pro každý fond pracovního.  To znamená, pracovní skupina 1 můžete mít ke zdrojům výpočetním P4 a pracovní skupina 2 s P1 výpočet zdrojů, pokud budete chtít.  Není nutné mít v pořádku velikost.  Pro podrobnosti o kolem velikosti a jejich ceny najdete v článku dokument tady [Azure aplikace služby ceny][AppServicePricing].  Možnosti měřítka pro web apps a aplikace služeb plány jednotného zasílání zpráv to ponechá v prostředí aplikace služby je:

- Výběr fondu kolegy
- počet výskytů

Změna jedné položky probíhá prostřednictvím uživatelského rozhraní odpovídající zobrazené u svého pomocného mechanismu řízení hostovaný aplikací služby plány jednotného zasílání zpráv.  

![][1]

Nelze škálování ASP za číslo dostupné výpočetním zdroje z fondu pracovníka, která je vaše ASP v.  Pokud potřebujete vypočítat zdroje v tomto fondu pracovníka potřebujete správce pomocného mechanismu řízení si je přidat.  Pro informace kolem znovu konfiguraci vašeho pomocného mechanismu řízení, přečtěte si informace v tomto poli: [jak nakonfigurovat aplikaci služby prostředí][HowtoConfigureASE].  Můžete taky využít funkce Automatické měřítko pomocného mechanismu řízení přidáte kapacita podle plánu nebo metriky.  Získat další informace o konfiguraci automatické měřítko pro pomocného mechanismu řízení prostředí samotné najdete v článku [jak nakonfigurovat automatické měřítko pro prostředí aplikace služby][ASEAutoscale].

Plány můžete vytvářet několika aplikace služby pomocí výpočetním zdroje z fondu různých pracovních nebo můžete použít stejné fondu pracovní.  Pokud například máte (10) k dispozici pro využití prostředků v pracovní skupina 1, můžete zvolit vytvořit jeden plán služeb aplikací pomocí (6) výpočetním zdroje a plán druhé aplikaci služby, který používá (4) výpočet zdroje.

### <a name="scaling-the-number-of-instances"></a>Změna měřítka počet instancí ###

Při prvním vytvoření webovou aplikaci v prostředí aplikace služby začíná 1 instance.  Můžete pak zmenšit se na další instance poskytnout výpočetním další zdroje pro aplikaci.   

Pokud má vaše pomocného mechanismu řízení dost kapacita je to poměrně jednoduché.  Přejděte do App služby plánu obsahující weby, které chcete škálování a vyberte měřítko.  Otevře se v uživatelském rozhraní, kde můžete ručně nastavit měřítko pro vaše ASP nebo nakonfigurujte automatické měřítko pravidla pro vaše ASP.  Ruční měřítko aplikace jednoduše nastavte ***měřítko tak, že*** na ***počet instancí, které mám zadat ručně***.  Tady přetáhněte posuvník na požadovaný počet nebo zadejte do pole vedle posuvníku.  

![][2] 

Automatické měřítko pravidla pro ASP v pomocného mechanismu řízení funguje stejně, jako normálně.  Můžete vybrat ***Procento využití procesoru*** ve skupinovém rámečku ***měřítko tak, že*** a vytvořte pravidla automatické měřítko pro vaše ASP podle procento využití procesoru nebo můžete vytvářet složitější pravidla pomocí ***pravidel pro plán a výkonu***.  Zobrazit další podrobné informace o konfiguraci automatické měřítko pomocí Průvodce tady [Měřítko aplikace v aplikaci služby Azure][AppScale]. 


### <a name="worker-pool-selection"></a>Výběr fondu kolegy ###

Jak je uvedeno dříve, výběr fondu pracovního přístupný z ASP UI.  Otevřete zásuvné ASP, který chcete změnit velikost a vyberte pracovníka fond.  Zobrazí se všechny pracovní skupiny, které jste nakonfigurovali ve vašem prostředí aplikace služby.  Pokud máte jenom jeden pracovní fondu pak uvidíte pouze jeden fond uvedené.  Pokud chcete změnit pracovní fondu vaší ASP je v, jednoduše vyberte fondu pracovníka chcete, aby vaše aplikace služby plánování přejdete k.  

![][3]

Teprve pak přejděte vaší ASP z fondu jeden pracovní je důležité, aby zkontrolovala, jestli že máte dostatečnou kapacitu pro vaše ASP.  V seznamu pracovní skupiny nejen je uveden název fondu pracovní, ale můžete se taky podívat kolik je k dispozici v tomto fondu pracovní.  Ujistěte se, že jsou dost instance dostupné má být plán služeb aplikací.  Pokud potřebujete další výpočet zdroje z fondu kolegy, které chcete přesunout, pak se chyba správce pomocného mechanismu řízení si je přidat.  

> [AZURE.NOTE] Přesunutí že ASP z fondu jeden pracovní způsobí chladírenského spustí aplikací v prostředí ASP.  Může dojít k žádosti o pomaleji je aplikace studenou začít pracovat s nové výpočetním zdroje.  Studený start můžete zabránit pomocí [aplikace teplé nahoru funkce] [ AppWarmup] v aplikaci služby Azure.  Modul inicializace aplikace popsaná v článku funguje taky studenou spouštění protože proces inicializace taky vyvolání po aplikace studenou začít pracovat s nové výpočetním zdroje. 

## <a name="getting-started"></a>Začínáme

Začínáme s aplikací služby prostředí, najdete v článku [Jak k vytvoření aplikace aplikace služby prostředí][HowtoCreateASE]

Další informace o aplikaci služby Azure platformy, najdete v článku [Aplikace služby Azure][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
