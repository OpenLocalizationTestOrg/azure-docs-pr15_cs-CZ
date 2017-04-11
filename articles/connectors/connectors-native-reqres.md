<properties
    pageTitle="Použití akcí žádostí a odpovědí | Microsoft Azure"
    description="Základní informace o žádostí a odpovědí aktivační události a akce v aplikaci pro použití logických operátorů Azure"
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

# <a name="get-started-with-the-request-and-response-components"></a>Začínáme s součásti žádostí a odpovědí

Součástí žádostí a odpovědí v aplikaci použití logických operátorů můžete v reálném čase reagovat na události.

Například máte tyto možnosti:

- Odpovězte na žádost HTTP s daty z místního databáze pomocí aplikace logiku.
- Aktivace logiky aplikace z externího webhook události.
- Volání v aplikaci logika s žádostí a odpovědí akcí z v jiné aplikaci použití logických operátorů.

Začít používat akce žádostí a odpovědí v aplikaci použití logických operátorů, najdete v článku [Vytvoření aplikace použití logických operátorů](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-request-trigger"></a>Použijte aktivační událost žádost HTTP

Aktivační událost je u události, mohou sloužit k spuštění pracovního postupu, který je definován v aplikaci použití logických operátorů. [Další informace o aktivačních událostí](connectors-overview.md).

Tady je příklad řadu nastavíte žádost HTTP v návrháři aplikace použití logických operátorů.

1. Přidání aktivační událost **přijetí žádosti o - požadavek při HTTP** v aplikaci použití logických operátorů. Volitelně můžete zadat schématu JSON (pomocí nástroje jako [JSONSchema.net](http://jsonschema.net)) pro požadavku. To umožňuje návrháři generovat tokeny vlastností v žádost HTTP.
2. Přidání jiné akce tak, aby mohli uložit aplikaci použití logických operátorů.
3. Po uložení aplikaci logiku, můžete získat adresu URL žádost HTTP z karty žádost.
4. POST protokolu HTTP (můžete použít nástroj jako [pošťák](https://www.getpostman.com/)) na adresu URL spustí aplikaci logiky.

>[AZURE.NOTE] Pokud nedefinujete akce odpověď `202 ACCEPTED` volající okamžitě je vrácena odpověď. Akce odpověď slouží k přizpůsobení odpověď.

![Odpověď aktivační událost](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>Pomocí akce odpověď HTTP

Tato akce odpověď HTTP je platný pouze při použití v pracovním postupu, který je aktivovány žádost HTTP. Pokud nedefinujete akce odpověď `202 ACCEPTED` volající okamžitě je vrácena odpověď.  Přidejte akce odpověď na libovolný krok v pracovním postupu. Aplikaci logiky pouze udržuje příchozí žádosti otevřít pro jednu minutu odpověď.  Za minutu, pokud žádná odpověď byla odeslána z pracovního postupu (a odpověď akce existuje v definici) `504 GATEWAY TIMEOUT` je vrácena volajícímu.

Tady je postup, jak přidat odpověď HTTP akce:

1. Klikněte na tlačítko **Nový krok** .
2. Zvolte **Přidat akci**.
3. Do pole Hledat akce zadejte **odpověď** na seznam odpovědí akce.

    ![Vyberte akci odpověď](./media/connectors-native-reqres/using-action-1.png)

4. Přidáte do všech parametrů, které jsou potřeba pro zprávu s odpovědí HTTP.

    ![Provedení akce odpověď](./media/connectors-native-reqres/using-action-2.png)

5. Klikněte na levém horním rohu panelu nástrojů na Uložit a aplikace logiku bude jak uložit a publikovat (aktivovat).

## <a name="request-trigger"></a>Žádost o aktivační událost

Tady jsou podrobnosti o aktivační událost, která podporuje tato spojnice. Je aktivační jeden požadavek.

|Aktivační událost|Popis|
|---|---|
|Žádost o|Nastane při přijetí žádost HTTP|

## <a name="response-action"></a>Odpověď akce

Tady jsou podrobnosti o akci, která podporuje tato spojnice. Je jedna odpověď akci, která lze použít pouze v případě obsahuje aktivační žádost o.

|Akce|Popis|
|---|---|
|Odpověď|Vrátí odpověď vzájemném vztahu žádost HTTP|

### <a name="trigger-and-action-details"></a>Podrobnosti o aktivační události a akce

Následující tabulka popisuje vstupních polí pro aktivační události a akce a odpovídající výstup podrobnosti.

#### <a name="request-trigger"></a>Žádost o aktivační událost
Následující obrázek je vstupní pole aktivační události z příchozí žádost HTTP.

|Zobrazované jméno|Název vlastnosti|Popis|
|---|---|---|
|Schéma JSON|schéma|Schéma JSON požadavek HTTP|
<br>

**Podrobnosti výstupu**

Následují výstup podrobnosti o požadavku.

|Název vlastnosti|Datový typ|Popis|
|---|---|---|
|Záhlaví|objekt|Žádost o záhlaví|
|Textu|objekt|Žádost o objektu|

#### <a name="response-action"></a>Odpověď akce

Následují vstupních polí pro odpověď HTTP akce. A * znamená, že požadované pole.

|Zobrazované jméno|Název vlastnosti|Popis|
|---|---|---|
|Stavový kód *|statusCode|Stavový kód HTTP|
|Záhlaví|záhlaví|Objekt JSON všechny odpovědi záhlaví zahrnout|
|Textu|textu|Obsah odpovědí|

## <a name="next-steps"></a>Další kroky

Teď vyzkoušejte si platformu a [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md). Prozkoumání dalších dostupné spojnice v aplikacích pro použití logických operátorů prohlédnete [seznam rozhraní API](apis-list.md).
