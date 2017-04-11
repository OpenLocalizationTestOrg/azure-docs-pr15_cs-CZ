<properties
   pageTitle="Pomocí konektoru Azure zdroje v aplikacích pro použití logických operátorů | Aplikace služby Microsoft Azure"
   description="Postup vytvoření a konfigurace Azure zdroje spojnice nebo rozhraní API aplikace a její použití v aplikaci logiky v aplikaci služby Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="09/01/2016"
   ms.author="stepsic"/>

# <a name="get-started-with-the-azure-resource-connector-and-add-it-to-your-logic-app"></a>Začínáme s konektoru Azure zdroje a přiřadit ji někomu aplikace pro použití logických operátorů
>[AZURE.NOTE] Tuto verzi článku platí pro logiku aplikace 2014-12-01-schématu verzi.

Pomocí konektoru zdroje Azure snadno spravovat zdroje Azure uvnitř logiky aplikace.

## <a name="create-the-azure-resource-connector"></a>Vytvoření spojnice Azure zdroje
Použití aplikace Azure zdroje konektor rozhraní API, je potřeba nejdřív vytvořit její instanci. Stačí buď vložené při vytváření aplikace logiky nebo tak, že vyberete Azure zdroje správce konektor rozhraní API aplikace z webu Azure Marketplace.

Pokud chcete ji nakonfigurovat, musíte je třeba nastavit nahoru hlavní název služby s oprávněními udělat něco jiného, co je, že chcete udělat v Azure. Všechny hovory budou na jménem z tohoto služby objektu, který jste si nastavili. To umožňuje rozsah spojnice tak, aby použít pouze přesně co chcete udělat a nic Další.

David Ebbo napsal [skvělé blogu](http://blog.davidebbo.com/2014/12/azure-service-principal.html) o tom, jak nastavit. Podle pokynů všechny tam a najděte svoje **ID klienta**, **ID klienta** a **tajná**. Tato tři pole plus **ID předplatného**se, co jsou potřeba pro nastavení konektoru.

## <a name="using-the-azure-resource-connector-in-logic-apps-designer"></a>Pomocí konektoru Azure zdroje v návrháři aplikace použití logických operátorů
### <a name="trigger"></a>Aktivační událost
Existují dva aktivačních událostí, které podporuje spojnice:

Jméno | Popis
---- | -----------
Události | Aktivační události při výskytu události zdroji předplatné.
Mezní hodnota ve výkresu protínají metriky |  Aktivační události při metriky splňuje určitou prahovou hodnotu.

### <a name="action"></a>Akce

Stejně tak můžete zadat velkého počtu akce uvnitř Azure předplatné:

Pro **skupiny zdrojů** můžete:

Jméno | Popis
---- | -----------
Seznam zdrojů skupin | Seznam všech skupin zdrojů v předplatného.
Získání pole Skupina zdroje | Skupina zdroje dosáhnout jeho id.
Vytvoření pole Skupina zdroje | Vytvoření nebo aktualizace skupina zdroje.
Odstranění pole Skupina zdroje | Odstranění skupiny zdrojů.

Pro **zdroje** máte tyto možnosti:

Jméno | Popis
---- | -----------
Seznam zdrojů | Seznam zdrojů v předplatném u různých typů filtry.
Získání prostředku | Získání jeden zdroj podle jeho zdroje Id.
Vytvoření nebo aktualizace zdroje | Vytvořit zdroj nebo aktualizovat existující zdroj. Všechny vlastnosti, je nutné zadat pro daný zdroj.
Akce zdroje |  Proveďte jiné akce zdroje. Je potřeba vědět název akce i data, která má tato akce (pokud existuje).
Odstranění zdroje | Odstranění zdroje.

Pro **Zdroje zprostředkovatelé** máte tyto možnosti:

Jméno | Popis
---- | -----------
Seznam zdrojů poskytovatelů | Seznam všech poskytovatelů dostupné zdroje v předplatného.

Nasazení **Skupina zdroje** máte tyto možnosti:

Jméno | Popis
---- | -----------
Seznam nasazení | Seznam všech nasazení ve skupině zdroje.
Získání nasazení | Nasazení šablony dosáhnout jeho id.
Vytvoření nasazení | Vytvoření nové skupiny nasazení zdroje zadáním šablony.

Pro **události** o zdrojích máte tyto možnosti:

Jméno | Popis
---- | -----------
Získání události | Získáte událostí v předplatné nebo zdroje.

Pro **metriky** o zdrojích máte tyto možnosti:

Jméno | Popis
---- | -----------
Získání metriky | Získání metriky zdroje Id.

## <a name="do-more-with-your-connector"></a>Další možnosti s na spojnici
Teď, když se vytvoří spojnice, můžete ho přidat do firmy toku použití logických operátorů aplikace. V tématu [Jaké aplikace logiky?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Pokud chcete začít s aplikacemi jiných Azure logiky před registrací účet Azure, přejděte k [Použití logických operátorů zkuste aplikaci](https://tryappservice.azure.com/?appservice=logic), kde můžete okamžitě vytvořit aplikaci logiky krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

Zobrazení Swagger rozhraní REST API odkazu na [spojnic a rozhraní API aplikace odkaz](http://go.microsoft.com/fwlink/p/?LinkId=529766).

<!--References -->

<!--Links -->
[Creating a Logic app]: app-service-logic-create-a-logic-app.md
