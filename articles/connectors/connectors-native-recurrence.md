<properties
    pageTitle="Přidání aktivační události opakování v aplikacích pro použití logických operátorů | Microsoft Azure"
    description="Základní informace o aktivační události opakování a jak pomocí aplikace Azure logiky."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-recurrence-trigger"></a>Začínáme s aktivační události opakování

Pomocí aktivační události opakování můžete vytvořit výkonné pracovních postupů v cloudu.

Například máte tyto možnosti:

- Naplánování pracovní postup spustit uložené procedury SQL každý den.
- E-mailové souhrn všech tweety minulý týden o některých značka.

Začít používat opakování aktivační události v aplikaci použití logických operátorů, najdete v článku [Vytvoření aplikace použití logických operátorů](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-a-recurrence-trigger"></a>Použijte aktivační událost opakování

Aktivační událost je u události, mohou sloužit k spuštění pracovního postupu, který je definován v aplikaci použití logických operátorů. [Další informace o aktivačních událostí](connectors-overview.md).

Tady je příklad řadu jak nastavit opakování aktivační události v aplikaci použití logických operátorů:

1. Cílem prvního kroku v aplikaci použití logických operátorů přidejte aktivační události **opakování** .
2. Vyplňte parametry intervalu opakování.

Použití logických operátorů aplikace spustí spustit po jednotlivých časový interval.

![Nastavit informace HTTP aktivační událost](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a>Podrobnosti o aktivační událost

Aktivační události opakování má následující vlastnosti, které můžete konfigurovat.

Použití logických operátorů aplikace se aktivuje po zadaný časový interval.
A * znamená, že požadované pole.

|Zobrazované jméno|Název vlastnosti|Popis|
|---|---|---|
|Četnost *|četnosti|The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.|
|Interval *|interval|Interval dané frekvenci opakování.|
|Časové pásmo|podle časového pásma|Pokud čas začátku bez časový posun časové pásmo se použijí.|
|Spuštění|čas spuštění|Počáteční čas ve [formátu ISO 8601 formátu](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).|
<br>


## <a name="next-steps"></a>Další kroky

Teď vyzkoušejte si platformu a [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md). Prozkoumání dalších dostupné spojnice v aplikacích pro použití logických operátorů prohlédnete [seznam rozhraní API](apis-list.md).
