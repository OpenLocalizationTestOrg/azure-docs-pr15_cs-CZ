
<properties
    pageTitle="Přidání HTTP + Swagger akcí v aplikacích pro použití logických operátorů | Microsoft Azure"
    description="Základní informace o HTTP + Swagger akce a operace"
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

# <a name="get-started-with-the-http--swagger-action"></a>Začínáme s HTTP + Swagger akce

Pomocí protokolu HTTP + Swagger akce, můžete vytvořit prvotřídní spojnice tak, aby všechny ostatní koncového bodu prostřednictvím [Swagger dokumentu](https://swagger.io). Můžete také rozšířit logiky aplikace volání žádné ZBÝVAJÍCÍ koncový bod prvotřídní možnosti v logických aplikace Návrhář.

Začínáme s HTTP + Swagger akce v aplikaci logiku, najdete v článku [Vytvoření nové aplikace logiku](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## <a name="use-http--swagger-as-a-trigger-or-an-action"></a>Použití HTTP + Swagger jako aktivační událost nebo akce

Nastavit informace HTTP + Swagger aktivační událost akce fungovat stejná jako [Akce nastavit informace HTTP](connectors-native-http.md) a poskytnout lepší návrh zobrazením tvar rozhraní API a výstupy v Návrháři z [Swagger metadata](https://swagger.io). Kromě toho můžete použít HTTP + Swagger jako aktivační událost. Pokud chcete provádět aktivační hlasování, řiďte se hlasování vzorku, který je popsáno v tématu [Vytvoření vlastní rozhraní API pro práci s aplikací použití logických operátorů](../app-service-logic/app-service-logic-create-api-app.md#polling-triggers).

[Další informace o použití logických operátorů aplikace aktivačními událostmi a akce.](connectors-overview.md)

Tady je příklad toho, jak používat HTTP + Swagger operace akce v pracovním postupu v aplikaci použití logických operátorů.

1. Klikněte na tlačítko **Nový krok** .
2. Vyberte **Přidat akci**.
3. Do pole Hledat akce zadejte **swagger** do seznamu HTTP + Swagger akce.

    ![Vyberte HTTP + Swagger akce](./media/connectors-native-http-swagger/using-action-1.png)

4. Zadejte adresu URL dokumentu Swagger:
    - Vycházet z aplikace Návrhář použití logických operátorů, musí být tato adresa koncového HTTPS a povolili CORS.
    - Pokud dokument Swagger nesplňuje tohoto požadavku, můžete uložit dokument [Úložišti Azure s CORS povolené](#hosting-swagger-from-storage) .
5. Klikněte na tlačítko **Další** a vykreslování z dokumentu Swagger.
6. Přidáte do všech parametrů, které jsou potřeba pro volání HTTP.

    ![Dokončení akce HTTP](./media/connectors-native-http-swagger/using-action-2.png)

1. V levém horním rohu panelu nástrojů a vaše logiky aplikace ušetří obou klikněte na **Uložit** a publikovat (aktivovat).

### <a name="host-swagger-from-azure-storage"></a>Host (hostitel) Swagger z Azure úložiště

Můžete chtít odkazovat Swagger dokumentu, který není hostitelem nebo, která je nesplňuje zabezpečení a křížově origin požadavky na tvůrce. Tento problém můžete vyřešit, můžete uložit dokument Swagger v úložišti Azure a povolení CORS neodkazuje dokument.  

Tady je postup vytvoření, konfigurace a ukládání dokumentů Swagger v úložišti Azure:

1. [Vytvořit účet Azure úložiště s úložiště objektů Blob Azure](../storage/storage-create-storage-account.md). (K tomuto účelu nastavit oprávnění pro **přístup k veřejným**.)
2. V objektů blob povolte CORS. [Tento skript Powershellu](https://github.com/logicappsio/EnableCORSAzureBlob/blob/master/EnableCORSAzureBlob.ps1) můžete používá ke konfiguraci toto nastavení automaticky.
3. Nahrání souboru Swagger do objektů blob. Můžete to provést z [Azure portál](https://portal.azure.com) nebo z nástroje jako [Průzkumníka úložišť Azure](http://storageexplorer.com/).
1. Odkaz na odkaz HTTPS do dokumentu v úložišti objektů Blob Azure. (Odkaz formát `https://*storageAccountName*.blob.core.windows.net/*container*/*filename*`.)



## <a name="technical-details"></a>Podrobné technické informace

Tady jsou podrobnosti týkající se aktivace a akce, které tato HTTP + Swagger podporuje spojnice.

## <a name="http--swagger-triggers"></a>Nastavit informace HTTP + Swagger aktivačních událostí

Aktivační událost je u události, mohou sloužit k spuštění pracovního postupu, který je definován v aplikaci použití logických operátorů. [Další informace o aktivačních událostí.](connectors-overview.md) Nastavit informace HTTP + Swagger spojnice obsahuje jednu aktivační událost.

|Aktivační událost|Popis|
|---|---|
|Nastavit informace HTTP + Swagger|Hovor HTTP a vraťte se obsah odpověď|

## <a name="http--swagger-actions"></a>Nastavit informace HTTP + Swagger akce

Akce je operaci, která provádí pracovního postupu, která je definovaná v aplikaci použití logických operátorů. [Další informace o akce.](connectors-overview.md) Nastavit informace HTTP + Swagger spojnice obsahuje jeden možných akcí.

|Akce|Popis|
|---|---|
|Nastavit informace HTTP + Swagger|Hovor HTTP a vraťte se obsah odpověď|

### <a name="action-details"></a>Podrobnosti o akci

Nastavit informace HTTP + Swagger spojnice je součástí jedné možných akcí. Toto jsou informace o všech akcí, jejich povinných a nepovinných vstupních polí a odpovídající výstup podrobnosti, které jsou přidružené k jejich použití.

#### <a name="http--swagger"></a>Nastavit informace HTTP + Swagger

Vytvoření žádosti o odchozí HTTP pomoci Swagger metadata.
Hvězdička (*) znamená požadované pole.

|Zobrazované jméno|Název vlastnosti|Popis|
|---|---|---|
|Metoda *|Metoda|Příkaz HTTP používat.|
|IDENTIFIKÁTOR URI *|identifikátor URI|Identifikátor URI protokolu HTTP požadavku na.|
|Záhlaví|záhlaví|Objekt JSON záhlaví HTTP zahrnout.|
|Textu|textu|Požadavek HTTP.|
|Ověřování|ověřování|Ověřování pro účely žádost. [Další podrobnosti najdete v tématu HTTP](./connectors-native-http.md#authentication).|

**Podrobnosti výstupu**

Odpověď HTTP

|Název vlastnosti|Datový typ|Popis|
|---|---|---|
|Záhlaví|objekt|Záhlaví odpovědí|
|Textu|objekt|Objekt odpovědi|
|Stavový kód|Funkce INT|Nastavit informace HTTP stavový kód|

### <a name="http-responses"></a>Odpovědi na HTTP

Při volání na různé akce, můžou některé odpovědi. Následuje tabulka, která obsahuje odpovídající odpovědi a popisy.

|Jméno|Popis|
|---|---|
|200|Ok|
|202|Přijaté|
|400|Chybný požadavek|
|401|Neoprávněnému|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě.|

---

## <a name="next-steps"></a>Další kroky

Vyzkoušejte si platformu a [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md) . Prozkoumání dalších spojnice k dispozici v aplikacích pro použití logických operátorů prohlédnete [seznam rozhraní API](apis-list.md).
