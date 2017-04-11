<properties
pageTitle="Informační | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Project Online je flexibilní online řešení pro správu portfolia projektů (PPM) a od Microsoftu každodenní práci. Doručeno prostřednictvím Office 365, Project Online umožňuje organizacím rychle začít s možnostmi správy výkonné projektu na plán, určovat priority a správa projektů a investic projektového portfolia – prakticky odkudkoli, kde na téměř libovolném zařízení."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-projectonline-connector"></a>Začínáme s informační spojnice

Project Online je flexibilní online řešení pro správu portfolia projektů (PPM) a od Microsoftu každodenní práci. Doručeno prostřednictvím Office 365, Project Online umožňuje organizacím rychle začít s možnostmi správy výkonné projektu na plán, určovat priority a správa projektů a investic projektového portfolia – prakticky odkudkoli, kde na téměř libovolném zařízení.

>[AZURE.NOTE] Tuto verzi článku platí pro verze schématu 2015 08 01 náhled logiky aplikace. 

Můžete začít s vytvářením logiky aplikaci najdete v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

Konektor informační mohou sloužit jako akce; má které aktivační. Všechny spojnice podporují dat ve formátu JSON a XML. 

 Konektor informační obsahuje následující akce nebo které aktivační k dispozici:

### <a name="projectonline-actions"></a>Informační akce
Můžete provést tyto akce:

|Akce|Popis|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Seznamy projekty na web služby project online|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Vytvoří nový projekt v online web projektu|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Vytvoří nový úkol v projektu můžete|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|Vytvoří zdroje organizace v online web projektu|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|Seznamy publikované úkolů v projektu|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|Rezervuje projektu na webu|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Vrácení se změnami a publikovat s existujícím projektu na webu|
### <a name="projectonline-triggers"></a>Informační aktivačních událostí
Můžete poslouchat tyto události:

|Aktivační událost | Popis|
|--- | ---|
|Vytvoření nového projektu|Spustí tok při vytvoření nového projektu|
|Vytvoření nového zdroje|Spustí nový tok, když se vytvoří nový prostředek|
|Vytvoření nového úkolu|Spustí tok, když se vytvoří nový úkol|


## <a name="create-a-connection-to-projectonline"></a>Vytvoření připojení k informační
Vytváření aplikací pro použití logických operátorů s informační, můžete musíte nejdřív vytvořit **připojení** a pak zadejte podrobnosti pro následující vlastnosti: 

|Vlastnost| Povinné|Popis|
| ---|---|---|
|Token|Ano|Informační pověření|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="reference-for-projectonline"></a>Odkaz pro informační
Platí pro verze: 1.0

## <a name="onnewproject"></a>OnNewProject
Vytvoření nového projektu: aktivuje tok při vytvoření nového projektu 

```GET: /trigger/_api/ProjectData/Projects``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="onnewresource"></a>OnNewResource
Vytvoření nového zdroje: spustí nový tok, když se vytvoří nový prostředek 

```GET: /trigger/_api/ProjectData/Resources``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="onnewtask"></a>OnNewTask
Když se vytvoří nový úkol: spustí tok, když se vytvoří nový úkol 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="listprojects"></a>ListProjects
Seznam projekty: seznamy projekty na web služby project online 

```GET: /_api/ProjectServer/Projects``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="createproject"></a>CreateProject
Vytvoří nový projekt: vytvoří nový projekt v online web projektu 

```POST: /_api/ProjectServer/Projects``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|
|proj| |Ano|textu|žádná|Vytvoření nového projektu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="createtask"></a>CreateTask
Vytvoří nový úkol: vytvoříte nový úkol v projektu můžete 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|řetězec|Ano|Cesta|žádná|Jedinečné ID projektu přidáte úkol|
|úkol| |Ano|textu|žádná|Nový úkol přidat do projektu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="createresource"></a>CreateResource
Vytvoření nového zdroje: vytvoří zdroje organizace v online web projektu 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|
|zdroje| |Ano|textu|žádná|Nový zdroj organizace přidáte do projektu|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="listtasks"></a>ListTasks
Seznamy úkolů: seznamy publikované úkolů v projektu 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|řetězec|Ano|Cesta|žádná|Jedinečné ID projektu, abyste vzdáleně používali úkoly|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="checkoutproject"></a>CheckoutProject
Rezervace projektu: rezervuje projektu na webu 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|řetězec|Ano|Cesta|žádná|Jedinečné ID projektu přidáte úkol|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="publishproject"></a>PublishProject
Vrácení se změnami a publikování projektu: vrácení se změnami a publikovat s existujícím projektu na webu 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|siteUrl|řetězec|Ano|dotaz|žádná|Kořenové adresy url webu webu vašeho projektu (Příklad: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|řetězec|Ano|Cesta|žádná|Jedinečné ID aplikace project k vrácení se změnami|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="object-definitions"></a>Definice objektů 

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="triggerproject"></a>TriggerProject


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ProjectStartDate|řetězec|Ne |
|ProjectFinishDate|řetězec|Ne |
|ProjectCreatedDate|řetězec|Ne |
|ProjectId|řetězec|Ne |
|ProjectModifiedDate|řetězec|Ne |
|ProjectType|celé číslo|Ne |
|Název projektu|řetězec|Ne |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="triggerresource"></a>TriggerResource


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ResourceId|řetězec|Ne |
|ResourceBaseCalendar|řetězec|Ne |
|ResourceBookingType|celé číslo|Ne |
|ResourceCanLevel|Logická hodnota|Ne |
|ResourceCostPerUse|číslo|Ne |
|ResourceCreatedDate|řetězec|Ne |
|ResourceEarliestAvailableFrom|řetězec|Ne |
|ResourceEmail|řetězec|Ne |
|ResourceInitials|řetězec|Ne |
|ResourceIsActive|Logická hodnota|Ne |
|ResourceIsGeneric|Logická hodnota|Ne |
|ResourceLatestAvailableTo|řetězec|Ne |
|ResourceModifiedDate|řetězec|Ne |
|ResourceName|řetězec|Ne |
|ResourceStatsuName|řetězec|Ne |
|ResourceType|celé číslo|Ne |
|TypeDescription|řetězec|Ne |
|TypeName|řetězec|Ne |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="triggertask"></a>TriggerTask


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ProjectId|řetězec|Ne |
|TaskId|řetězec|Ne |
|Názevprojektu|řetězec|Ne |
|Název úlohy|řetězec|Ne |
|TaskCreatedDate|řetězec|Ne |
|TaskModifieddate|řetězec|Ne |
|TaskStartDate|řetězec|Ne |
|TaskFinishDate|řetězec|Ne |
|TaskPriority|celé číslo|Ne |
|TaskIsActive|Logická hodnota|Ne |



### <a name="newproject"></a>NewProject


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Jméno|řetězec|Ano |
|Popis|řetězec|Ne |
|Zahájení|řetězec|Ne |



### <a name="newreource"></a>NewReource


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Jméno|řetězec|Ano |
|IsBudget|Logická hodnota|Ne |
|IsGeneric|Logická hodnota|Ne |
|IsInactive|Logická hodnota|Ne |



### <a name="project"></a>Projektu


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|ApprovedStart|řetězec|Ne |
|ApprovedEnd|řetězec|Ne |
|CheckedOutDate|řetězec|Ne |
|CheckOutDescription|řetězec|Ne |
|CheckOutId|řetězec|Ne |
|Datum vytvoření|řetězec|Ne |
|ID|řetězec|Ne |
|IsCheckedOut|Logická hodnota|Ne |
|LastPublishedDate|řetězec|Ne |
|LastSavedDate|řetězec|Ne |
|OptimizerDecision|celé číslo|Ne |
|PlannerDecision|celé číslo|Ne |
|ProjectType|celé číslo|Ne |
|Jméno|řetězec|Ne |
|WinprojVersion|řetězec|Ne |



### <a name="projectswrapper"></a>ProjectsWrapper


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="newtask"></a>Nový úkol


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Parametry|Nedefinováno|Ano |



### <a name="taskparameters"></a>TaskParameters


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Jméno|řetězec|Ano |
|Poznámky|řetězec|Ne |
|Zahájení|řetězec|Ne |
|Doba trvání|řetězec|Ne |



### <a name="enterpriseresource"></a>EnterpriseResource


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|CanLevel|Logická hodnota|Ne |
|Kód|řetězec|Ne |
|CostAccrual|celé číslo|Ne |
|CostCenter|řetězec|Ne |
|Vytvoření|řetězec|Ne |
|DefaultBookingType|celé číslo|Ne |
|E-mailu|řetězec|Ne |
|ExternalId|řetězec|Ne |
|Skupina|řetězec|Ne |
|HireDate|řetězec|Ne |
|ID|řetězec|Ne |
|Pole Iniciály|řetězec|Ne |
|IsActive|Logická hodnota|Ne |
|IsBudget|Logická hodnota|Ne |
|IsCheckedOut|Logická hodnota|Ne |
|IsGeneric|Logická hodnota|Ne |
|IsTeam|Logická hodnota|Ne |
|MaterialLabel|řetězec|Ne |
|Změněno|řetězec|Ne |
|Jméno|řetězec|Ne |
|Pole Výslovnost názvu|řetězec|Ne |
|ResourceType|celé číslo|Ne |
|TerminationDate|řetězec|Ne |



### <a name="taskswrapper"></a>TasksWrapper


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Hodnota|pole|Ne |



### <a name="task"></a>Úkol


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Vytvoření|řetězec|Ne |
|Změněno|řetězec|Ne |
|Zahájení|řetězec|Ne |
|Dokončení|řetězec|Ne |
|Jméno|řetězec|Ne |
|ID|řetězec|Ne |
|Priority (priorita)|celé číslo|Ne |
|Procento dokončení|celé číslo|Ne |
|Poznámky|řetězec|Ne |
|Kontakt|řetězec|Ne |


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)