<properties
    pageTitle="Přidání zpoždění v aplikacích pro použití logických operátorů | Microsoft Azure"
    description="Základní informace o zpoždění a zpoždění-až akcí a jejich použití s aplikaci pro použití logických operátorů Azure."
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

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Začínáme s zpoždění a zpoždění-až akce

Pomocí zpoždění a "zpoždění-dokud" akce dokončení pracovního postupu scénáře.

Například máte tyto možnosti:

- Počkejte, dokud den v týdnu pro odesílání aktualizace stavu prostřednictvím e-mailu.
- Zpoždění pracovního postupu, dokud pozvání na HTTP nějaký čas na dokončení obnovení a načítání výsledek.

Abyste mohli začít používat zpoždění akce v aplikaci logiku, v tématu [Vytvoření logiku aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>Použití zpoždění akcí

Akce je operaci, která provádí pracovního postupu, která je definovaná v aplikaci použití logických operátorů. [Další informace o akce](connectors-overview.md).

Tady je příklad řadu používání kroku zpoždění v aplikaci použití logických operátorů:

1. Po přidání aktivační událost, klikněte na **Nový krok** přidejte akce.
2. Vyhledejte **zpoždění** vyvoláte prodleva akce. V tomto příkladu jsme vybereme **zpoždění**.

    ![Zpoždění akce](./media/connectors-native-delay/using-action-1.png)

3. Provedení některé z vlastností akce konfigurace zpoždění.

    ![Konfigurace zpoždění](./media/connectors-native-delay/using-action-2.png)

4. Klikněte na tlačítko **Uložit** na publikování a aktivovat aplikaci logiky.


## <a name="action-details"></a>Podrobnosti o akci

Aktivační události opakování má následující vlastnosti, které lze nakonfigurovat.

### <a name="delay-action"></a>Zpoždění akce

Tato akce zpoždění spuštění pro určitého časového intervalu.
A * znamená, že požadované pole.

|Zobrazované jméno|Název vlastnosti|Popis|
|---|---|---|
|Počet *|počet|Počet časových jednotek zpozdit|
|Jednotky *|jednotky|Výběru časového úseku: `Second`, `Minute`, `Hour`, nebo`Day`|
<br>

### <a name="delay-until-action"></a>Zpoždění-až akce

Tato akce zpoždění spuštění do určité datum a čas.
A * znamená, že požadované pole.

|Zobrazované jméno|Název vlastnosti|Popis|
|---|---|---|
|Rok *|časové razítko|V roce zpozdit až (GMT)|
|Měsíc *|časové razítko|Měsíc zpozdit až (GMT)|
|Den *|časové razítko|Den zpozdit až (GMT)|
<br>


## <a name="next-steps"></a>Další kroky

Teď vyzkoušejte si platformu a [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md). Prozkoumání dalších dostupné spojnice v aplikacích pro použití logických operátorů prohlédnete [seznam rozhraní API](apis-list.md).
