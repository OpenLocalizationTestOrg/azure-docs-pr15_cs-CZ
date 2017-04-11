<properties
pageTitle="Naučte se používat konektoru FTP v aplikacích pro použití logických operátorů | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Připojení k serveru FTP ke správě souborů. Můžete provádět různé akce například nahrát, aktualizovat, získat a odstraňovat soubory ve serveru FTP."
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
ms.date="07/22/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-ftp-connector"></a>Začínáme s FTP spojnice

Pomocí konektoru FTP monitorovat, Správa a vytváření souborů na serveru FTP. 

Pokud chcete použít [libovolnou spojnici](./apis-list.md), musíte nejdřív při vytváření aplikace logiky. Jak začít vytvořením [aplikace logiky nyní](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-ftp"></a>Připojení k FTP

Pokud aplikace logiky získat přístup k jiné služby, musíte nejdřív vytvořit *připojení* ke službě. [Připojení](./connectors-overview.md) umožňuje připojení mezi logiky aplikace a další služby.  

### <a name="create-a-connection-to-ftp"></a>Vytvoření připojení k FTP

>[AZURE.INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]

## <a name="use-a-ftp-trigger"></a>Použijte aktivační událost FTP

Aktivační událost je u události, mohou sloužit k spuštění pracovního postupu v aplikaci logiky definované. [Další informace o aktivačních událostí](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.IMPORTANT]Konektor FTP vyžaduje serveru FTP přístupné z Internetu a je nakonfigurovaný pro práci s pasivní režim. Konektor FTP je také **není kompatibilní s implicitní FTPS (FTP přes připojení SSL)**. Konektor FTP podporuje pouze explicitní FTPS (FTP přes připojení SSL).  

V tomto příkladu se vám ukáže, jak používat aktivační událost **FTP – při přidání nebo změny souboru** zahajte pracovního postupu aplikace použití logických operátorů, když soubor je přidán do nebo změnit na serveru FTP. V příkladu enterprise můžete použít Tato aktivační událost sledování složky FTP pro nové soubory, které představují objednávky zákazníků.  Pak můžete akce spojnice FTP například **získat obsah souboru** získat obsahu objednávky pro další zpracování a úložiště v databázi objednávky.

1. Zadejte *ftp* do vyhledávacího pole v Návrháři logiku aplikací a pak vyberte aktivační událost **FTP – při přidání nebo změny souboru**   
![Obrázek aktivační událost FTP 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
Ovládací prvek **Při přidání nebo změny souboru** se objeví  
![Obrázek aktivační událost FTP 2](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
- Vyberte **...** nachází na pravé straně ovládacího prvku. Otevře se ovládací prvek Výběr složky  
![Obrázek aktivační událost FTP 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
- Vyberte **>** (šipka vpravo), najděte složku, kterou chcete sledovat nové nebo změněné soubory. Vyberte složku a Všimněte si, že složce se nyní zobrazí v ovládacím prvku **složky** .  
![Obrázek aktivační událost FTP 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   


V tomto okamžiku aplikace logiky nakonfigurované pomocí aktivační začne spustit ostatních aktivačních událostí a akce pracovního postupu se po změnili nebo vytvořené v požadované složce FTP souboru. 

>[AZURE.NOTE]Použití logických operátorů aplikace je funkční musí obsahovat alespoň jeden aktivační události a jednu akci. Postupujte podle pokynů v následující části chcete-li přidat akci.  



## <a name="use-a-ftp-action"></a>Použití akce FTP

Akce je operace prováděné definované v aplikaci logiky pracovního postupu. [Další informace o akce](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

Teď, když jste přidali aktivační událost, přidat akci, která se zobrazí obsah souboru nové nebo změněné nalezenou aktivační událost takto:    

1. Vyberte **+ nový krok** přidáte akci, kterou chcete získat obsah souboru na serveru FTP  
- Vyberte odkaz **Přidat akci** .  
![Obrázek akce FTP 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
- Zadejte *FTP* vyhledat všechny akce související s FTP.
- Vyberte **FTP – zobrazí obsah souboru** jako akce provedeny, pokud soubor nové nebo změněné nachází ve složce FTP.      
![Obrázek akce FTP 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
Ovládací prvek **získat obsah souboru** otevře. **Poznámka**: Zobrazí se výzva k ověření aplikace logiky pro přístup k účtu serveru FTP, pokud jste tak dosud neučinili.  
![Obrázek akce FTP 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
- Vyberte ovládací prvek **souboru** (prázdné znaky umístěné pod **soubor***). Tady můžete některé z různých vlastností ze souboru nové nebo změněné na serveru FTP.  
- Vyberte požadovanou možnost **obsah souboru** .  
![Obrázek akce FTP 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
-  Ovládací prvek se aktualizuje označující, že akce **FTP – zobrazí obsah souboru** pošle *souboru obsahu* souboru nové nebo změněné na serveru FTP.      
![Obrázek akce FTP 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
- Uložte práci a přidání souboru do složky FTP otestujte pracovní postup.    

V tomto okamžiku aplikaci logiky nakonfigurované pomocí aktivační události pro sledování složek na serveru FTP a zahájení pracovního postupu, když zjistí, nový soubor nebo změněné soubor na serveru FTP. 

Aplikaci logiku taky nakonfigurované akce zobrazíte obsah souboru nové nebo změněné.

Teď můžete přidat další akci třeba [SQL Server – vložit řádek](./connectors-create-api-sqlazure.md#insert-row) akci, kterou chcete vložit obsah souboru nové nebo změněné do tabulky databáze SQL.  

## <a name="technical-details"></a>Podrobné technické informace

Tady jsou podrobnosti o aktivačních událostí, akce a odpovědi, které podporuje připojení:

## <a name="ftp-triggers"></a>FTP aktivačních událostí

FTP obsahuje následující které aktivační:  

|Aktivační událost | Popis|
|--- | ---|
|[Při přidání nebo změny souboru](connectors-create-api-ftp.md#when-a-file-is-added-or-modified)|Tuto operaci spustí tok, když soubor přidali nebo změnili ve složce.|


## <a name="ftp-actions"></a>FTP akce

FTP obsahuje následující akce:


|Akce|Popis|
|--- | ---|
|[Získejte soubor metadat](connectors-create-api-ftp.md#get-file-metadata)|Tuto operaci získá metadata v souboru.|
|[Aktualizace souboru](connectors-create-api-ftp.md#update-file)|Operace se aktualizuje v souboru.|
|[Odstranění souboru](connectors-create-api-ftp.md#delete-file)|Tuto operaci odstranění souboru.|
|[Získat metadata soubor pomocí cesty](connectors-create-api-ftp.md#get-file-metadata-using-path)|Tuto operaci získá metadat soubor pomocí cesty.|
|[Získání obsahu soubor pomocí cesty](connectors-create-api-ftp.md#get-file-content-using-path)|Tuto operaci získá obsah soubor pomocí cesty.|
|[Zobrazí obsah souboru](connectors-create-api-ftp.md#get-file-content)|Tuto operaci získá obsah souboru.|
|[Vytvoření souboru](connectors-create-api-ftp.md#create-file)|Tuto operaci vytvoří soubor.|
|[Zkopíroval soubor](connectors-create-api-ftp.md#copy-file)|Tuto operaci zkopíruje soubor na serveru FTP.|
|[Seznam souborů ve složce](connectors-create-api-ftp.md#list-files-in-folder)|Operace se seznam souborů a podsložek ve složce.|
|[Seznam souborů v kořenové složce](connectors-create-api-ftp.md#list-files-in-root-folder)|Operace se seznam souborů a podsložky v kořenové složce.|
|[Extrahování složky](connectors-create-api-ftp.md#extract-folder)|Tuto operaci extrahuje souboru archivu do jiné složky (Příklad: .zip).|
### <a name="action-details"></a>Podrobnosti o akci

Tady jsou podrobnosti pro akce a aktivační události pro tento konektor spolu s jejich odpovědi:



### <a name="get-file-metadata"></a>Získejte soubor metadat
Tuto operaci získá metadata v souboru. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|ID *|Soubor|Vyberte soubor|

* Označuje, že není potřeba vlastnost

#### <a name="output-details"></a>Podrobnosti výstupu

BlobMetadata


| Název vlastnosti | Datový typ |
|---|---|---|
|ID|řetězec|
|Jméno|řetězec|
|DisplayName|řetězec|
|Cesta|řetězec|
|Změněno|řetězec|
|Velikost|celé číslo|
|MediaType|řetězec|
|IsFolder|Logická hodnota|
|ETag|řetězec|
|FileLocator|řetězec|




### <a name="update-file"></a>Aktualizace souboru
Operace se aktualizuje v souboru. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|ID *|Soubor|Vyberte soubor|
|textu *|Obsah souboru|Obsah souboru|

* Označuje, že není potřeba vlastnost

#### <a name="output-details"></a>Podrobnosti výstupu

BlobMetadata


| Název vlastnosti | Datový typ |
|---|---|---|
|ID|řetězec|
|Jméno|řetězec|
|DisplayName|řetězec|
|Cesta|řetězec|
|Změněno|řetězec|
|Velikost|celé číslo|
|MediaType|řetězec|
|IsFolder|Logická hodnota|
|ETag|řetězec|
|FileLocator|řetězec|




### <a name="delete-file"></a>Odstranění souboru
Tuto operaci odstranění souboru. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|ID *|Soubor|Vyberte soubor|

* Označuje, že není potřeba vlastnost




### <a name="get-file-metadata-using-path"></a>Získat metadata soubor pomocí cesty
Tuto operaci získá metadat soubor pomocí cesty. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Cesta k *|Cesta k souboru|Vyberte soubor|

* Označuje, že není potřeba vlastnost

#### <a name="output-details"></a>Podrobnosti výstupu

BlobMetadata


| Název vlastnosti | Datový typ |
|---|---|---|
|ID|řetězec|
|Jméno|řetězec|
|DisplayName|řetězec|
|Cesta|řetězec|
|Změněno|řetězec|
|Velikost|celé číslo|
|MediaType|řetězec|
|IsFolder|Logická hodnota|
|ETag|řetězec|
|FileLocator|řetězec|




### <a name="get-file-content-using-path"></a>Získání obsahu soubor pomocí cesty
Tuto operaci získá obsah soubor pomocí cesty. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|Cesta k *|Cesta k souboru|Vyberte soubor|

* Označuje, že není potřeba vlastnost




### <a name="get-file-content"></a>Zobrazí obsah souboru
Tuto operaci získá obsah souboru. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|ID *|Soubor|Vyberte soubor|

* Označuje, že není potřeba vlastnost




### <a name="create-file"></a>Vytvoření souboru
Tuto operaci vytvoří soubor. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|cesta_ke_složce *|Cestu ke složce|Vyberte složku|
|Název *|Název souboru|Název souboru|
|textu *|Obsah souboru|Obsah souboru|

* Označuje, že není potřeba vlastnost

#### <a name="output-details"></a>Podrobnosti výstupu

BlobMetadata


| Název vlastnosti | Datový typ |
|---|---|---|
|ID|řetězec|
|Jméno|řetězec|
|DisplayName|řetězec|
|Cesta|řetězec|
|Změněno|řetězec|
|Velikost|celé číslo|
|MediaType|řetězec|
|IsFolder|Logická hodnota|
|ETag|řetězec|
|FileLocator|řetězec|




### <a name="copy-file"></a>Zkopíroval soubor
Tuto operaci zkopíruje soubor na serveru FTP. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|zdroj *|Adresa url zdroje|Adresa URL pro zdrojový soubor|
|určení *|Cesta k souboru cíl|Cesta k souboru cíle, včetně cílový název souboru|
|Přepsat|Přepsat?|Pokud přepíše cílového souboru nastavena na hodnotu "true"|

* Označuje, že není potřeba vlastnost

#### <a name="output-details"></a>Podrobnosti výstupu

BlobMetadata


| Název vlastnosti | Datový typ |
|---|---|---|
|ID|řetězec|
|Jméno|řetězec|
|DisplayName|řetězec|
|Cesta|řetězec|
|Změněno|řetězec|
|Velikost|celé číslo|
|MediaType|řetězec|
|IsFolder|Logická hodnota|
|ETag|řetězec|
|FileLocator|řetězec|




### <a name="when-a-file-is-added-or-modified"></a>Při přidání nebo změny souboru
Tuto operaci spustí tok, když soubor přidali nebo změnili ve složce. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|ID složky *|Složka|Vyberte složku|

* Označuje, že není potřeba vlastnost




### <a name="list-files-in-folder"></a>Seznam souborů ve složce
Operace se seznam souborů a podsložek ve složce. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|ID *|Složka|Vyberte složku|

* Označuje, že není potřeba vlastnost



#### <a name="output-details"></a>Podrobnosti výstupu

BlobMetadata


| Název vlastnosti | Datový typ |
|---|---|---|
|ID|řetězec|
|Jméno|řetězec|
|DisplayName|řetězec|
|Cesta|řetězec|
|Změněno|řetězec|
|Velikost|celé číslo|
|MediaType|řetězec|
|IsFolder|Logická hodnota|
|ETag|řetězec|
|FileLocator|řetězec|




### <a name="list-files-in-root-folder"></a>Seznam souborů v kořenové složce
Operace se seznam souborů a podsložky v kořenové složce. 


Neexistují žádné parametry pro tento hovor

#### <a name="output-details"></a>Podrobnosti výstupu

BlobMetadata


| Název vlastnosti | Datový typ |
|---|---|---|
|ID|řetězec|
|Jméno|řetězec|
|DisplayName|řetězec|
|Cesta|řetězec|
|Změněno|řetězec|
|Velikost|celé číslo|
|MediaType|řetězec|
|IsFolder|Logická hodnota|
|ETag|řetězec|
|FileLocator|řetězec|




### <a name="extract-folder"></a>Extrahování složky
Tuto operaci extrahuje souboru archivu do jiné složky (Příklad: .zip). 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|zdroj *|Cesta k souboru archivu zdroje|Cesta k souboru archivu|
|určení *|Cílová složka cesta|Cestu ke složce cíl|
|Přepsat|Přepsat?|Přepíše cílové soubory, pokud nastavena na hodnotu "true"|

* Označuje, že není potřeba vlastnost



#### <a name="output-details"></a>Podrobnosti výstupu

BlobMetadata


| Název vlastnosti | Datový typ |
|---|---|---|
|ID|řetězec|
|Jméno|řetězec|
|DisplayName|řetězec|
|Cesta|řetězec|
|Změněno|řetězec|
|Velikost|celé číslo|
|MediaType|řetězec|
|IsFolder|Logická hodnota|
|ETag|řetězec|
|FileLocator|řetězec|



## <a name="http-responses"></a>Odpovědi na HTTP

Jeden nebo více z následujících kódů stav HTTP se můžete vrátit akcí a aktivační události nad: 

|Jméno|Popis|
|---|---|
|200|Ok|
|202|Přijaté|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě.|
|Výchozí|Operace se nezdařila.|







## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)