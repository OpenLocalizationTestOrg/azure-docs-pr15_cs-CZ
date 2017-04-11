<properties
    pageTitle="Základní informace o použití logických operátorů aplikace spojnic | Microsoft Azure"
    description="Základní informace o spojovací čáry, které lze použít v aplikaci použití logických operátorů"
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
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Použití konektorů v aplikaci použití logických operátorů

Spojnice umožňují rychlý přístup k události, data a akce služby, protokoly a platformách.  Úplný seznam spojovací čáry, které podporuje logiku aplikace může [být tady](apis-list.md).  Spojnice mohou sloužit jako aktivační událost nebo akce v aplikaci logiky a může vyžadovat nakonfigurované *připojení* pro použití (například: ověřování Twitter účtu pro přístup k nebo zveřejňují vaším jménem).

## <a name="basics"></a>Základní informace

Spojnice jsou hostovanou služby, které se zobrazí jako součást aplikace logiky integrovat s dalšími službami třeba Dynamics Azure, Salesforce, [a další](apis-list.md).  Jsou nasazeném a spravuje Microsoft, takže je možné vytvářet pracovních postupů integrace s měřítko, výkon a zabezpečení stará.  Spojnice můžete přidat do aplikace logiky hledání a výběrem spojnice akce nebo aktivační událost ve skupinovém rámečku **Zobrazit Microsoft spravované rozhraní API**.

![Nabídka Akce pro výběr aktivační událost][1]

Každé spojnice akce nebo aktivační událost, bude mít jeho sadu vlastností pro nastavení.  Klikněte na tlačítko informace o další informace o akci nebo odkaz jeho si přečtěte následující dokumentaci [Další informace](apis-list.md).

Pokud chcete integrovat se služby nebo rozhraní API, které není dosud spojnici, můžete taky rozšíření logiky aplikace pomocí [vlastní spojnice](../app-service-logic/app-service-logic-create-api-app.md) nebo jenom volání přímo do služby prostřednictvím protokolu jako HTTP.

## <a name="triggers"></a>Aktivace

Některé spojovací čáry mají aktivační událost, což znamená, bude událost z tuto spojnici spustit aplikaci logiky a předat všech dat v rámci aktivační událost.  Aktivační události vždycky je prvním krokem při použití logických operátorů aplikace.  Oblíbené aktivace patří operací, jako je:
 
 * Opakování – spuštění každou hodinu
 * Přijetí žádost HTTP
 * Přidání položky do fronty
 * Přijetí e-mailu
 
Některých aktivačních událostí bude platit pro rychlé, které události se stane, až oznámení k aplikaci použití logických operátorů a ostatní bude nutné intervalu opakování nakonfigurovaný na intervalu aplikaci logiky kontroloval služby pro události (až každých 15 sekund).  

Po přijetí události bude platit aplikaci logiky spustit a spuštění akce v pracovním postupu.  Můžete taky moct přístup ke všem datům z aktivační událost během pracovního postupu (například aktivační událost "na nové tweetu" předá tweetu do spustit).

## <a name="actions"></a>Akce

Většina spojnice mají jednu nebo více akcemi, které můžete v rámci pracovního postupu.  Akce jsou všechny kroky, které se provedou po spustit byla spuštěná z aktivační události.  Chcete přidat klikněte na akce na tlačítko **Nový krok** a vyhledejte spojnice použít.  Po vybraná (a po konfiguraci všechna [připojení](#connections) , který může být nutné) zobrazí se na kartu akce, kterou můžete nakonfigurovat.  Vyberte data z předchozích kroků kliknutím na některou z tokeny výstupy nebo zadejte ostatní konfigurace podle potřeby.

![Konfigurace spojnice akce][2]

## <a name="connections"></a>Připojení

Většina spojnic vyžadují, abyste konfigurace *připojení* , než budete moct použít spojnice.  *Připojení* se všechny přihlášení nebo připojení konfiguraci potřebné pro přístup ke konektoru.  Spojovací čáry, které používají OAuth, vytvořte si připojení znamená přihlašování ke službě (jako je Office 365, Salesforce nebo GitHub) místo, kam můžete přístupový token zašifrovaných a bezpečné uložené v Azure tajné úložiště.  Další spojnic (třeba FTP a SQL) vyžadují připojení, která obsahuje konfigurace jako adresa serveru, uživatelské jméno a heslo.  Podrobné informace o těchto připojení konfigurace jsou také zašifrovaných a uloženy.  Připojení budou moct přistupovat k služby pro službu umožňuje.  Abychom mohli pokračovat aktualizovat přístupový token donekonečna udržovat v Azure Active Directory OAuth připojení (jako je Office 365 a Dynamics).  Další služby dá limity o tom, jak dlouho token můžete používat bez aktualizována.  Obecně určité akce, jako je změna hesla zruší všechny tokeny přístup.  

Připojení můžete zobrazit a spravovat v Azure kliknutím na **Procházet** a vyberte **Rozhraní API připojení**.  Z rozhraní API připojení zdroje můžete zobrazit, upravit, aktualizovat nebo znovu povolit připojení, které jste vytvořili.

## <a name="next-steps"></a>Další kroky

- [Vytvoření aplikace pro první použití logických operátorů](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Přečtěte si společné použití a příklady použití logických operátorů aplikace](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Začínáme s podnikové integrace aktivačních událostí a akce](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png