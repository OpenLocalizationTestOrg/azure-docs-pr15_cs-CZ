<properties
    pageTitle="Přidejte akce dotazu v aplikacích pro použití logických operátorů | Microsoft Azure"
    description="Základní informace o akce dotazu k provádění akcí, jako pole filtru."
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
   ms.date="07/20/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-query-action"></a>Začínáme s akce dotazu

Pomocí akce dotazu můžete pracovat s listy a maticemi provádění pracovní postupy:

- Vytvoření úkolu pro všechny záznamy vysokou prioritou z databáze.
- Uložte všechny přílohy ve formátu PDF pro e-maily do objektů blob Azure.

Abyste mohli začít používat akce dotazu v aplikaci logiku, v tématu [Vytvoření logiku aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-query-action"></a>Použití akce dotazu

Akce je operaci, která provádí pracovního postupu, která je definovaná v aplikaci logiku. [Další informace o akce](connectors-overview.md).  

Akce dotazu právě najednou, s názvem pole filtru, který se zobrazí v návrháři. Díky dotazu matice a vrátí sadu filtrovaných výsledků.

Tady je, jak můžete ho přidat v aplikaci použití logických operátorů:

1. Klikněte na tlačítko **Nový krok** .
2. Zvolte **Přidat akci**.
3. Do pole Hledat akce zadejte **Filtr** zobrazíte **pole filtru** akci.

    ![Vyberte akce dotazu](./media/connectors-native-query/using-action-1.png)

4. Vyberte pole k filtrování. (Následující obrázek ukazuje o matici výsledků vyhledávání na Twitter.)
5. Vytvoření podmínky pro hodnocení u každé položky. (Následující snímek obrazovky filtruje tweety od uživatelů, kteří mají víc než 100 sledující.)

    ![Provedení akce dotazu](./media/connectors-native-query/using-action-2.png)

    Akce Výstup nové pole, které obsahuje jenom výsledky, které došlo ke splnění požadavky na filtr.
6. Klikněte na levém horním rohu panelu nástrojů na Uložit a aplikace pro použití logických operátorů bude jak uložit a publikovat (aktivovat).

## <a name="query-action"></a>Akce dotazu

Tady jsou podrobnosti o akci, která podporuje tato spojnice. Spojnice obsahuje jeden možných akcí.

|Akce|Popis|
|---|---|
|Pole Filtr|Vyhodnotí podmínky pro každou položku matice a vrátí výsledek|

## <a name="action-details"></a>Podrobnosti o akci

Akce dotazu je součástí jedné možných akcí. Následující tabulka popisuje povinných a nepovinných vstupních polí pro různé akce a výstup podrobností o odpovídající, které jsou spojené s použitím akce.

### <a name="filter-array"></a>Pole Filtr
Následují vstupních polí pro akce, která zajišťuje odchozí žádost HTTP.
A * znamená, že požadované pole.

|Zobrazované jméno|Název vlastnosti|Popis|
|---|---|---|
|Z *|z|Pole Filtr|
|Podmínka *|kde|Podmínka vyhodnotit pro každou položku|
<br>

### <a name="output-details"></a>Podrobnosti výstupu

Následují výstup podrobnosti pro odpověď HTTP.

|Název vlastnosti|Datový typ|Popis|
|---|---|---|
|Filtrované matice|pole|Pole obsahující objektu pro každou filtrovaný výsledek|

## <a name="next-steps"></a>Další kroky

Teď vyzkoušejte si platformu a [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md). Prozkoumání dalších dostupné spojnice v aplikacích pro použití logických operátorů prohlédnete [seznam rozhraní API](apis-list.md).
