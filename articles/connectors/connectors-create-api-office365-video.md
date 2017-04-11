<properties
pageTitle="Použití Office 365 Video Connectoru v aplikacích použití logických operátorů | Microsoft Azure"
description="Začínáme s používáním konektoru Office 365 Video do aplikace Microsoft Azure aplikace služby použití logických operátorů"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office365-video-connector"></a>Začínáme s Office 365 Video spojnice
Připojení k Office 365 Video k získání informací o účtu Office 365 video, zobrazte seznam videa a další. Office 365 Video spojnice lze z:

- Logiku aplikace 

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2015 08 01 náhled schématu verze. Tato spojnice není podporována ve všech předchozích verzí schéma.

V Office 365 Video máte tyto možnosti:

- Vytvoření vaše obchodní toku podle data, která se zobrazí v Office 365 Video. 
- Pomocí akce, které kontrola videa portálu stavu, získáte seznam všech videa v kanálu a další. Tyto akce se odpověď a proveďte umožňující příkazu jiné akce výstup. Můžete například pomocí konektoru Bingu Hledat můžete vyhledávat videa Office 365 a pak pomocí Office 365 video konektor získat informace o dané video. Pokud video odpovídá vašim požadavkům, můžete toto video publikovat na Facebook. 

Operace v aplikacích pro použití logických operátorů přidáte v tématu [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Aktivace a akce

Konektor Office 365 Video má k dispozici následující akce: Existuje žádné aktivačních událostí.

| Aktivace | Akce|
| --- | --- |
| Žádná | <ul><li>Stav videa portal kontroly</li><li>Zobrazí všechny zobrazitelné kanály</li><li>Získání url přehrávání manifestu Azure Media Services pro video</li><li>Získání nosný token abyste získali přístup k dešifrování videa</li><li>Získání informací o konkrétní Office 365 video</li><li>Seznamy všechna vaše Office 365 videa v kanálu</li></ul>

Všechny spojnice podporují dat ve formátu JSON a XML. 

## <a name="create-a-connection-to-office365-video-connector"></a>Vytvoření připojení k Office 365 Video konektor
Tato spojnice přidáte ke svým aplikacím logiku, musíte přihlásit ke svému účtu Office 365 Video a povolit logiku aplikace pro připojení ke svému účtu.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Po vytvoření připojení k zadání vlastností Office 365 videa, jako je název klienta nebo kanálu ID. Tyto vlastnosti popisuje **rozhraní REST API odkazy** v tomto tématu.

>[AZURE.TIP] Toto stejné připojení Office 365 Video můžete použít v jiných aplikacích pro použití logických operátorů.

## <a name="swagger-rest-api-reference"></a>Odkaz swagger rozhraní REST API
Platí pro verze: 1.0.

### <a name="checks-video-portal-status"></a>Stav videa portal kontroly 
Zkontroluje, stav videa portálu zobrazíte, jestli jsou povolené videa služby.  
```GET: /{tenant}/IsEnabled``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|klienta|řetězec|Ano|Cesta|žádná|Název klienta v adresáři uživatel je součástí|


#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|404|Nenalezeno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|



### <a name="get-all-viewable-channels"></a>Zobrazí všechny zobrazitelné kanály 
Získá všechny kanály, které má uživatel přístup k zobrazení.  
```GET: /{tenant}/Channels``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|klienta|řetězec|Ano|Cesta|žádná|Název klienta v adresáři uživatel je součástí|


#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|404|Nenalezeno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Seznamy všechna Office 365 videa v kanálu 
Seznam všechna vaše Office 365 videa v kanálu.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|klienta|řetězec|Ano|Cesta|žádná|Název klienta v adresáři uživatel je součástí|
|channelId|řetězec|Ano|Cesta|žádná|Id kanálu odkud muset načítat videa|


#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|404|Nenalezeno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|




### <a name="gets-information-about-a-particular-office365-video"></a>Získání informací o konkrétní Office 365 video 
Získání informací o konkrétní Office 365 video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|klienta|řetězec|Ano|Cesta|žádná|Název klienta v adresáři uživatel je součástí|
|channelId|řetězec|Ano|Cesta|žádná|Id kanálu|
|videoId|řetězec|Ano|Cesta|žádná|Videa id|


#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|404|Nenalezeno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Získání url přehrávání manifestu Azure Media Services pro video 
Získání url přehrávání manifestu Azure Media Services video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|klienta|řetězec|Ano|Cesta|žádná|Název klienta v adresáři uživatel je součástí|
|channelId|řetězec|Ano|Cesta|žádná|Id kanálu|
|videoId|řetězec|Ano|Cesta|žádná|Videa id|
|streamingFormatType|řetězec|Ano|dotaz|žádná|Typ streamování formátu. 1 – hladký přenos nebo MPEG-přerušované čáry. 0 - HLS datových proudů|


#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|404|Nenalezeno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Získání nosný token abyste získali přístup k dešifrování videa 
Pokud potřebujete nosný token abyste získali přístup k dešifrování video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Jméno| Datový typ|Povinné|Součástí|Výchozí hodnota|Popis|
| ---|---|---|---|---|---|
|klienta|řetězec|Ano|Cesta|žádná|Název klienta v adresáři uživatel je součástí|
|channelId|řetězec|Ano|Cesta|žádná|Id kanálu|
|videoId|řetězec|Ano|Cesta|žádná|Videa id|


#### <a name="response"></a>Odpověď

|Jméno|Popis|
|---|---|
|200|Operace byla úspěšně|
|400|BadRequest|
|401|Neoprávněnému|
|404|Nenalezeno|
|500|Vnitřní chyba serveru|
|Výchozí|Operace se nezdařila.|


## <a name="object-definitions"></a>Definice objektů

#### <a name="channel-channel-class"></a>Kanálu: Třídy kanálu

| Jméno | Datový typ | Povinné|
|---|---|---|
|ID|řetězec|Ne|
|Název|řetězec|Ne|
|Popis|řetězec|Ne|


#### <a name="video"></a>Video 

| Jméno | Datový typ |Povinné|
|---|---|---|
|ID|řetězec|Ne|
|Název|řetězec|Ne|
|Popis|řetězec|Ne|
|Datum vytvoření|řetězec|Ne|
|Vlastník|řetězec|Ne|
|ThumbnailUrl|řetězec|Ne|
|VideoUrl|řetězec|Ne|
|VideoDuration|celé číslo|Ne|
|VideoProcessingStatus|celé číslo|Ne|
|ViewCount|celé číslo|Ne|


## <a name="next-steps"></a>Další kroky
[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).
