<properties
    pageTitle="Doporučené postupy pro aplikaci služby Azure"
    description="Přečtěte si doporučené postupy a odstraňování aplikací služby Azure."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="best-practices-for-azure-app-service"></a>Doporučené postupy pro aplikaci služby Azure

Tento článek shrnuje osvědčené postupy pro používání [Aplikace služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Sdruženém
Pokud jsou v různých oblastech umístěny Azure zdroje psaní řešení jako je web app a databáze efekty můžete patří:

*  Lepší latence komunikaci mezi zdroje
*  Peněžní poplatky pro odchozí data přenášet více oblastí jak je uvedeno na [Azure ceny stránky](https://azure.microsoft.com/pricing/details/data-transfers).

Sdruženém ve stejné oblasti je nejvhodnější pro vytváření řešení jako je web app a účet databáze nebo úložiště používá k ukládání obsahu a dat Azure zdroje. Při vytváření prostředky, které jste měli jsou ve stejné Azure oblasti, pokud nemáte specifickým obchodním nebo návrh důvod, proč je nechcete být. Aplikace pro aplikaci služby můžete přesunout do stejné oblasti jako databáze využitím [aplikaci služby klonováním funkce](app-service-web-app-cloning-portal.md) momentálně dostupné pro Premium aplikace služby plánování aplikace.   

## <a name="memoryresources"></a>Při používání aplikace víc paměti nečekaně
Když zjistíte, aplikace využívá víc paměti očekávaným způsobem, jak je uvedeno prostřednictvím sledování nebo služby doporučení zvažte [retušování aplikace služby automatického funkce](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). Jednu z možností pro funkci Automatické retušování trvá vlastních akcí podle paměti mezní hodnota. Akce zahrnovat dokumentech z e-mailová oznámení vyšetřování prostřednictvím výpisu na omezení rizik na místě recyklací pracovního procesu. Retušování automatického je možné konfigurovat pomocí web.config a pomocí popisné uživatelské rozhraní podle na tento příspěvek blogu pro [Aplikaci služby podpory webu rozšíření](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>Při používání aplikace další procesoru nečekaně
Když zjistíte, že aplikace využívá další procesoru než očekávání nebo dojde k opakuje vzroste využití procesoru, jak je uvedeno prostřednictvím sledování nebo služby doporučení zvažte škálování nebo rozšiřování plán služeb aplikací. Pokud statefull je aplikace, škálování je toto jediná možnost, a pokud je aplikace příslušnosti, změnu měřítka mimo získáte větší flexibilitu a vyšší potenciál měřítko. 

Další informace o aplikacích "příslušnosti" a "statefull" můžete se podívat na toto video: [plánování Scalable začátku do konce vícevrstvé aplikace na Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). Další informace o aplikaci služby možnosti změny velikosti a neobsahovaly text: [Měřítko Web App v aplikaci služby Azure](web-sites-scale.md).  

## <a name="socketresources"></a>Pokud jsou vyčerpány socket zdroje
Běžné důvod, proč se náročné odchozí připojení TCP je použití knihoven klienta, které nejsou implementovaná znovu použít připojení TCP nebo v případě protokolu vyšší úroveň například HTTP – udržování nebyl využít. Zkontrolujte dokumentace pro každou z knihovny odkazováno aplikace v plánu služby aplikace, abyste zajistili nakonfigurovali nebo k nim získat přístup v kódu pro účinné opětovné použití odchozí připojení. Také postupovat podle návodu si přečtěte následující dokumentaci knihovny správné vytváření a vydání nebo vyčištění Chcete-li předejít nevrací připojení. Době, kdy je tato vyšetřování knihoven klienta v průběhu dopad může zmírnit roztažením se k několika instancích spuštěných.  

## <a name="appbackup"></a>Pokud aplikace zálohovat spuštění neúspěšně
Dva nejčastější důvody proč záložní dojde k chybě aplikace jsou: neplatné úložiště nastavení a konfiguraci neplatné databáze. Tyto chyby obvykle dojít, pokud existují změny úložiště nebo databáze zdrojů nebo změny jak získat přístup k tyto materiály (například přihlašovací údaje pro vybrané v nastavení zálohování databáze). Zálohování obvykle spusťte při plánování a vyžaduje přístup k ukládání (pro výstup zálohované souborů) a databáze (pro kopírování a čtení obsah mají být součástí zálohování). Důsledek nezdařené pro přístup k některou z následujících zdrojích bude konzistentní selhání zálohování. 

Když dojít k chybám záložní Zrevidujte posledních výsledky porozumět tomu, jaký typ selhání neděje. V případě chyby přístup úložiště zkontrolujte a upravte nastavení úložiště používaných v záložní konfiguraci. V případě databáze aplikace access selhání prosím zkontrolujte a upravte připojení řetězce jako součást aplikace nastavení. Pokračujte aktualizovat konfigurace zálohování správně zahrnout potřebné databáze. Další informace o aplikaci Zálohování najdete v dokumentaci k [obecnějším údajům web app v aplikaci služby Azure](web-sites-backup.md) .

## <a name="nodejs"></a>Při nasazení nových aplikací Node.js aplikace služby Azure
Azure aplikaci služby výchozí konfigurace aplikace Node.js má nejlépe potřebám nejběžnější aplikací. Pokud konfigurace aplikace Node.js byste měli využívat přizpůsobených optimalizace zvýšení výkonu nebo optimalizaci zdroje využití procesoru a paměti/síťových prostředků, může prohlédněte naši stránku věnovanou osvědčené postupy a řešení potíží. V tomto článku si přečtěte následující dokumentaci popisuje iisnode nastavení může být nutné pro nastavení pro aplikaci Node.js, popisuje různé scénáře nebo problémy, aby vaše aplikace může protilehlé a ukazuje, jak při řešení těchto problémů: [osvědčených postupů a příručka pro řešení potíží pro uzel aplikace v aplikaci služby Azure](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md).   


