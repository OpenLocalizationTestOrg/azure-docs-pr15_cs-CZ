<properties
pageTitle="GitHub | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. GitHub je webová libovolná úložišti hostitelské služby. Všechny revize distribuované ovládacího prvku a zdroje kód management (Správce služeb) funkce libovolná stejně jako přidávání vlastní funkcí nabízí."
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

# <a name="get-started-with-the-github-connector"></a>Začínáme s GitHub spojnice

GitHub je webová libovolná úložišti hostitelské služby. Všechny revize distribuované ovládacího prvku a zdroje kód management (Správce služeb) funkce libovolná stejně jako přidání vlastní funkce nabízí.

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2015 08 01 náhled schématu verze. 

Můžete začít s vytvářením logiky aplikaci najdete v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

Konektor GitHub mohou sloužit jako akce; má které aktivační. Všechny spojnice podporují dat ve formátu JSON a XML. 

 Konektor GitHub obsahuje následující akce nebo které aktivační k dispozici:

### <a name="github-actions"></a>GitHub akce
Můžete provést tyto akce:

|Akce|Popis|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Vytvoří problém|
### <a name="github-triggers"></a>Aktivace GitHub
Můžete poslouchat tyto události:

|Aktivační událost | Popis|
|--- | ---|
|Při otevření chybu|Otevření problém|
|Pokud je zavřený problém|Problém je zavřený|
|Když přiřazených problému|Přiřazených problému|


## <a name="create-a-connection-to-github"></a>Vytvoření připojení k GitHub
Vytváření aplikací pro použití logických operátorů s GitHub, můžete musíte nejdřív vytvořit **připojení** a pak zadejte podrobnosti pro následující vlastnosti: 

|Vlastnost| Povinné|Popis|
| ---|---|---|
|Tokenu|Ano|Pověření GitHub|
Po vytvoření připojení můžete k provedení akce a poslouchat aktivace popisované v tomto článku. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] Toto připojení můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="reference-for-github"></a>Odkaz pro GitHub
Platí pro verze: 1.0

## <a name="createissue"></a>CreateIssue
Vytvoření problému: vytvoří problém 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|repositoryOwner|řetězec|Ano|Cesta|žádná|Vlastník úložiště|
|repositoryName|řetězec|Ano|Cesta|žádná|Název úložiště|
|issueBasicDetails| |Ano|textu|žádná|Podrobnosti o problému|

#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněnému|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="issueopened"></a>IssueOpened
Při otevření problém: problém upraveného 

```GET: /trigger/issueOpened``` 

Neexistují žádné parametry pro tento hovor
#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněnému|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="issueclosed"></a>IssueClosed
Pokud je zavřený problém: problém je zavřený 

```GET: /trigger/issueClosed``` 

Neexistují žádné parametry pro tento hovor
#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněnému|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="issueassigned"></a>IssueAssigned
Když na přiřazení problému: přiřazených problému 

```GET: /trigger/issueAssigned``` 

Neexistují žádné parametry pro tento hovor
#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Ok|
|400|Chybný požadavek|
|401|Neoprávněnému|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě|
|Výchozí|Operace se nezdařila.|


## <a name="object-definitions"></a>Definice objektů 

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Název|řetězec|Ano |
|textu|řetězec|Ano |
|osobě|řetězec|Ano |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Název vlastnosti | Datový typ | Povinné |
|---|---|---|
|Název|řetězec|Ano |
|textu|řetězec|Ano |
|osobě|řetězec|Ano |
|číslo|řetězec|Ne |
|Stav|řetězec|Ne |
|created_at|řetězec|Ne |
|repository_url|řetězec|Ne |


## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiku](../app-service-logic/app-service-logic-create-a-logic-app.md)