<properties
    pageTitle="Přehled rozhraní API mobilní zapojení exportu"
    description="Základní informace o exportu dat jako nezpracovaná generovaných zařízení uživatele můžete využít do vlastního nástroje"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Přehled rozhraní API mobilní zapojení exportu

## <a name="introduction"></a>Úvod

V tomto dokumentu se dozvíte základní informace o exportu jako nezpracovaná dat generovaných zařízení uživatele můžete využít do vlastního nástroje.

## <a name="pre-requisites"></a>Předpoklady

Export nezpracovanými daty z mobilních zapojení vyžaduje:

- Vzhled rozhraní API ověřování a nebudou moct používat rozhraní API (viz [Ruční nastavení ověřování](mobile-engagement-api-authentication-manual.md))
- Použití rozhraní REST API nebo [.net SDK](mobile-engagement-dotnet-sdk-service-api.md)
- Účet Azure úložiště.

>[AZURE.NOTE] Také, doporučujeme pracovníků [Microsoft Azure úložiště Explorer](http://storageexplorer.com/)aspoň fázi vývoj poskytuje snadno se použije uživatelského rozhraní pro komunikaci s Azure úložiště.

## <a name="what-can-be-exported"></a>Co je možné vyexportovat?

Zapojení Mobile umožňuje jeho shromáždit mnoho typů dat a proto má několik typů export hodí k těmto typům jiná data.
Existují 2 základní typy exportovat:

- Snímek: používá obvykle export dat, který znázorňuje stav a jehož zapojení Mobile, které neobsahují historie. Jedná se o značky (informace app), tokeny nebo nabízených kampaně názory například. Tyto typy export v důsledku netýkají k určitému datu.
- Historie: Tento typ exportu se používá pro data, která například shromáždí v čase ATP aktivity události.

V následující tabulce jsou podrobně všechny možné exporty:

| Exportujte typ | Datový typ | Popis                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Snímek    | Připínáčku      | Vygeneruje export nabízená kampaní názory na základě za deviceid nebo ID uživatele                                                              |
| Snímek    | Značka       | Vygeneruje export značek (informace app) přidružený ke každé zařízení                                                                       |
| Snímek    | Zařízení    | Vygeneruje export většiny údajů o zařízení například technicals (modelu, národní prostředí, podle časového pásma,...), značky, příručky poprvé... |
| Snímek    | Tokenu     | Vygeneruje export platné tokenů                                                                                                 |
| Historie  | Aktivita  | Vygeneruje export všech činností pro každého zařízení na daného období                                                           |
| Historie  | Události     | Vygeneruje export všech činností pro každého zařízení na daného období                                                           |
| Historie  | Úlohy       | Vygeneruje export všech projektů pro každého zařízení na daného období                                                                 |
| Historie  | Chyba     | Vygeneruje export všechny chyby pro každého zařízení na daného období                                                               |

## <a name="how-does-it-work"></a>Jak to funguje?

Export jsou dlouhé spuštění úkoly, které můžou vytvářet rozsáhlé datové soubory. Z tohoto důvodu nejde se dovolat okamžitě vrátit soubor ke stažení.
Abyste mohli exportovat data z zapojení Mobile, budete muset vytvořit **Úlohy exportu** prostřednictvím rozhraní API kterém nastavujete, obecně:

- Typ požadovaného exportu (snímek, nebo historických)
- Datový typ
- **Kontejner úložiště Azure** (včetně platné přidružení s přístup pro zápis) místo, kam se měly zapisovat výsledek exportovat.

Dejte pozor, aby může trvat několik minut práce jej lze spustit a potom může spusťte z několik sekund, než miniaturní aplikace několik hodin aplikací s velkým množstvím uživatele nebo aktivity.

Po vytvoření projektu je možné zkontrolovat jeho stav je spuštěné nebo zda byl dokončen.

Jakmile úlohy úspěšný, výsledný datový soubor neexistuje na kontejner zadané úložiště.
