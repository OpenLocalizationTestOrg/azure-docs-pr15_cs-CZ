<properties
    pageTitle="Azure aplikaci služby plány jednotného zasílání zpráv hloubkovou přehled | Microsoft Azure"
    description="Zjistěte, jak aplikaci služby plány jednotného zasílání zpráv pro aplikaci služby Azure práci, a jak výhod, které přináší možnostem správy."
    keywords="aplikace služby azure aplikace služby měřítko scalable, plán služeb aplikací, aplikací služby náklady"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Azure aplikaci služby plány jednotného zasílání zpráv hloubkovou přehled#

Plán služeb aplikací představuje sady funkcí a kapacity, kterou můžete sdílet mezi více aplikací. Web Apps, aplikace Mobile, funkce aplikace nebo rozhraní API aplikace v [Aplikaci služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714) všechny spustit plán služeb aplikací. Pět ceny úrovní podporují těchto plánů: *Uvolnit*, *sdílené*, *základní*, *Standardní*a *Premium*. Jednotlivé vrstvy obsahuje vlastní schopnosti a. Aplikace ve stejném předplatném a zeměpisné polohy můžete sdílet na plán. Všechny aplikace, které sdílejí plán můžete použít funkce a funkce, které jsou definovány plánu osy. Všechny aplikace, které jsou přidružené k plánu spouštět na prostředky, které určuje plánu.

Například pokud váš plán je nakonfigurovaný na použití dvě instance "small" v vrstvy standardní služeb, všechny aplikace, které jsou přidružené k této plán v obou případech spustit a mít přístup k funkci standardního služby osy. Plán na kterých jsou spuštěné aplikace je plně spravovaných a snadno dostupné.

Tento článek popisuje klíčové vlastností, jako je třeba osy a měřítka, plán služeb aplikací a jak je možné uplatnit během provádění správy aplikace.

## <a name="apps-and-app-service-plans"></a>Aplikace a plány aplikace služby

Aplikace v aplikaci služby jde přidružit pouze jeden plán služeb aplikací v daném okamžiku.

Aplikace a plány jsou součástí skupiny zdrojů. Skupina zdroje slouží jako hranici životního cyklu pro každý zdroj, který je v něm obsažené. Skupiny zdrojů umožňuje spravovat všechny části aplikace společně.

Protože jeden zdroj skupina může obsahovat více plány aplikaci služby, můžete přidělit různé aplikace k různým fyzickou prostředkům. Můžete třeba oddělit zdrojů mezi prostředí vývojáře test a výroby. Mají zvláštní prostředí pro výrobní a zařízením nebo zkoušení umožňuje izolovat zdroje. Tímto způsobem není zatížení otestování novou verzi aplikace soutěží o stejné zdroje jako výrobní aplikace, které jsou podávání skutečné zákazníky.

Pokud máte víc plány ve skupině jeden zdroj, můžete také definovat aplikace, která se nachází geografických oblastech. Například vysoce dostupné aplikace spuštěné ve dvou oblastí obsahuje aspoň dva plány pro každou oblast a jedné aplikaci přidružené každý plán. V takovém případě se všechny kopie aplikace poté součástí skupiny jeden zdroj. Skupina zdroje s více plány a více aplikací s umožňuje snadno spravovat, řízení a zobrazení stavu aplikace.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Vytvoření plánu aplikace služby nebo použít existující úrovně

Při vytváření aplikace byste měli zvážit vytvoření skupiny zdrojů. Na druhé straně sloužící k vytvoření aplikace je součástí pro větší aplikace, by být tato aplikace vytvořeny v rámci skupiny zdrojů přidělený větší aplikace.

Jestli nová aplikace je úplně novou aplikaci nebo část většího, můžete ho hostovat nebo vytvořte nový účet pomocí existující plán služeb aplikací. Toto rozhodnutí je další otázky kapacity a očekávané načíst.

Pokud toto nové aplikace bude používat více zdrojů a mají různé měřítko faktory z jiných aplikací hostovaný v existující plán, doporučujeme izolovat v samostatném plánu.

Při vytváření plánu můžete přidělit novou sadu zdrojů aplikace a získat větší kontrolu nad přidělení zdrojů, protože každý plán získává své vlastní sadu instancí.

Protože aplikace můžete přesunout plánech, můžete změnit tak, aby zdrojů přiřazených napříč větší aplikací.

Nakonec pokud chcete vytvořit aplikaci pro v jiné oblasti a dané oblasti listu, mezi kterými existující plán, vytvořte plán v dané oblasti mají být k hostování vašeho aplikace.

## <a name="create-an-app-service-plan"></a>Vytvoření plánu aplikace služby

>[AZURE.TIP] Pokud ještě prostředí služby aplikace můžete zkontrolovat dokumentaci specifické pro aplikaci služby prostředí tady: [Vytvoření aplikace aplikace služby plánování v prostředí aplikace služby](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Prázdný plán služeb aplikací můžete vytvořit z prostředí aplikace služby plán procházet nebo jako součást aplikace vytvoření.

V [Azure portál](https://portal.azure.com), klikněte na **Nový** > **Web + mobile**a potom vyberte **V prohlížeči** nebo jiného typu aplikace aplikaci služby.
![Vytvoření aplikace na portálu Azure.][createWebApp]

Můžete pak vyberte nebo vytvořte plán služeb aplikací pro nové aplikace.

 ![Vytvoření plánu aplikaci služby.][createASP]

Pokud chcete vytvořit nový plán služeb aplikací, klikněte na **[+] vytvořit novou**, napište název **plán služeb aplikací** a vyberte správného **umístění**. Klikněte na **ceny vrstvy**a vyberte odpovídající ceny vrstvy pro službu. Když vyberete **Zobrazit vše** zobrazíte další možnosti ceny, například **zdarma** a **sdílené**. Po výběru ceny vrstvy klikněte na tlačítko **Vybrat** .

## <a name="move-an-app-to-a-different-app-service-plan"></a>Přesunutí aplikace na jiný plán aplikace služby

Aplikaci můžete přesunout do jiné aplikace plán služeb [Azure portálu](https://portal.azure.com). Můžete přesouvat aplikace mezi plány, pokud jsou plány ve stejné skupině prostředků a zeměpisnou oblast.

Přesunout aplikace na jiný plán, přejděte do aplikace, který chcete přesunout. V nabídce **Nastavení** vyhledejte **Změnit aplikaci služby plánování**.

**Plán služeb aplikací změnit** otevře voliče **plán služeb aplikací** . V tomto okamžiku můžete vybrat existující plán nebo vytvořte nový účet. Zobrazují se jenom platný plán (stejné pole Skupina zdroje a geografické polohy).

![Výběr plánu aplikaci služby.][change]

Každý plán obsahuje vlastní ceny osy. Třeba při přesunutí webu z bezplatné osy na standardní osy aplikaci teď můžete použít všechny možnosti a prostředky standardní osy.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klonovat aplikace na jiný plán aplikace služby
Pokud chcete přesunout aplikace do jiné oblasti, je jedna alternativa klonováním. V nové nebo existující plán služeb aplikace nebo služby aplikace prostředí v jakékoli oblasti klonováním vytvoří kopii aplikace.

 ![Klonovat aplikace.][appclone]

V nabídce **Nástroje** můžete najít **Klonovat aplikace** .

Kopírování má určitá omezení, které si můžete přečíst o v [aplikaci služby Azure aplikace klonováním Azure portálu](../app-service-web/app-service-web-app-cloning-portal.md).

## <a name="scale-an-app-service-plan"></a>Změna velikosti plán služeb aplikací

Zobrazit plán třemi způsoby:

- **Změna plánu ceny osy**. Například na plán v základní osy lze převést na standardní nebo Premium osy a všechny aplikace, které platí pro plán teď můžete použít funkci, která nabízí nové vrstvy služeb.
- **Změna velikosti instance požadovaný plán**. Jako příklad lze změnit na plán v základní osy, který používá small instance používat velké instance. Všechny aplikace, které platí pro plán teď můžete použít další paměti a procesoru prostředky, které nabízí větší velikost instance.
- **Změnit počet instancí požadovaný plán**. Standardní plán, který je diagramů s měřítky se na tři instance můžete například zachován na 10 instance. Plán Premium je možné měřítko na 20 instance (vyměřené poplatky za jeho dostupnost). Všechny aplikace, které platí pro plán teď můžete použít další paměti a procesoru prostředky, které nabízí větší počet instancí.

Ceny velikost osy a instanci můžete změnit kliknutím na položku **Měřítko nahoru** ve skupinovém rámečku nastavení pro aplikaci nebo plán služeb aplikací. Změny platí pro plán služeb aplikací a ovlivňují všechny aplikace, která je hostitelem.

 ![Nastavení hodnot a ty pak škálování aplikace.][pricingtier]

## <a name="app-service-plan-cleanup"></a>Plánování služby vyčištění aplikace
**Plány aplikaci služby** , který není přidružený k nim aplikace pořád vzniknou poplatky od budou i nadále rezervovat kapacitu výpočetním konfigurovat vlastnosti aplikace služby plán měřítko.
Chcete-li předejít neočekávané poplatky, po odstranění aplikaci poslední použitý ve plán služeb aplikací, je odstraní taky výsledné prázdný plán služeb aplikací.


## <a name="summary"></a>Souhrn

Plány aplikaci služby představují sadu funkcí a kapacity, kterou můžete sdílet mezi aplikací. Aplikace služby plány co flexibilitě přidělit konkrétní aplikací sady zdrojů a dále optimalizovat využití Azure zdrojů. Tímto způsobem, pokud chcete ušetřit na testování prostředí, můžete na plán sdílet ve více aplikací. Můžete taky maximalizaci výkon pro provozním prostředí roztažením ve více oblastech a plány.

## <a name="whats-changed"></a>Co se změnilo

* Průvodce na změnu z webů pro aplikaci služby najdete v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png
