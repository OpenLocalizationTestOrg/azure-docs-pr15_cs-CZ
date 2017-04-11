<properties
   pageTitle="Použití logických operátorů aplikace obsahu zadejte zpracování | Microsoft Azure"
   description="Vysvětlení, jak použití logických operátorů aplikace zabývá typy obsahu v návrhovém a modulu runtime"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Typ obsahu aplikace logiky zpracování

Existuje mnoha různých typů obsahu, který můžete postupují logiky aplikací – včetně JSON, XML, textových souborů a binárními daty.  Během všechny typy obsahu jsou podporovány, některé nativně srozumitelné modul logiky aplikace a ostatní můžou vyžadovat hlasováním nebo převodů podle potřeby.  Následující článek popisuje způsob, jakým modul zpracovává různé typy obsahu a jak budou správně máte podle potřeby.

## <a name="content-type-header"></a>Záhlaví typu obsahu

Zahájíte jednoduché Pojďme se podívat na dvou `Content-Types` , nevyžadovat při převodu formátu nebo hlasováním budou používat v rámci logiky aplikací – `application/json` a `text/plain`.

### <a name="applicationjson"></a>Aplikace/json

Modul pracovní postup závisí na `Content-Type` záhlaví z HTTP volá k určení odpovídající zpracování.  Jakékoliv žádosti o s typem obsahu `application/json` budou uloženy a zpracována jako objekt JSON.  Kromě toho můžete JSON obsah analyzovat ve výchozím nastavení aniž by musel všechny hlasováním.  Takže požadavek, který obsahuje záhlaví typu obsahu `application/json ` takto:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

může analyzovat v pracovním postupu pomocí výrazu jako `@body('myAction')['foo'][0]` získání hodnoty (v tomto případě `bar`).  Není nutná žádná další hlasováním.  Pokud pracujete s daty, která je JSON, ale neměli záhlaví určené, můžete můžete ručně jej převést pomocí JSON `@json()` funkci (například: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Text nebo prostý

Podobně jako `application/json`, HTTP přijaté zprávy s `Content-Type` záhlaví `text/plain` budou uloženy v původním formátu.  Kromě toho, pokud součástí následné akce bez jakékoli hlasováním žádost budou přesměrovány s `Content-Type`: `text/plain` záhlaví.  Třeba při práci s plochému souboru zobrazí následující obsahu HTTP:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  Další akce Odeslat Pokud jste ho v těle zprávy jiné žádosti o (`@body('flatfile')`), budou mít žádosti `text/plain` záhlaví typu obsahu.  Při práci s daty, která je ve formátu prostého textu, ale neměli záhlaví určené, můžete můžete ručně jej převést na text pomocí `@string()` funkci (například: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Aplikace/xml a aplikace/osmičkové stream a převaděče funkcí

Modul logiky aplikace se vždy zachová `Content-Type` , která byla přijata na žádost HTTP nebo odpověď.  Co to znamená, pokud obsahu dostali s `Content-Type` z `application/octet-stream`, včetně následné akce se žádné hlasováním povede žádost odchozí s `Content-Type`: `application/octet-stream`.  Tímto způsobem modul nelze guaruntee data budou ztracena pohyb v celém pracovního postupu.  Však stav akce (vstupů a výstupů) jsou uloženy v objektu JSON jako toky v celém pracovního postupu.  Znamená to, že chcete zachovat některé typy dat, modul bude obsah na binární ve formátu Base 64 zakódovaný řetězec s příslušnou metadata, která obě zachová `$content` a `$content-type` – který se automaticky převedou.  Můžete taky ručně převést mezi typy obsahu pomocí integrované funkce převaděče:

* `@json()`-přetypuje dat`application/json`
* `@xml()`-přetypuje dat`application/xml`
* `@binary()`-přetypuje dat`application/octet-stream`
* `@string()`-přetypuje dat`text/plain`
* `@base64()`-obsahu převede na řetězce ve formátu Base 64
* `@base64toString()`-Převede řetězec kódovaný ve formátu Base 64`text/plain`
* `@base64toBinary()`-Převede řetězec kódovaný ve formátu Base 64`application/octet-stream`
* `@encodeDataUri()`-kódování řetězci jako pole bajtů dataUri
* `@decodeDataUri()`-dešifruje dataUri do pole bajtů

Pokud jste obdrželi žádost HTTP se například `Content-Type`: `application/xml` části:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Lze převést a později pomocí nějak `@xml(triggerBody())`, nebo v rámci jedné funkce jako `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Typy dalšího obsahu

Další typy obsahu jsou podporované a pracovat s aplikací použití logických operátorů, ale může vyžadovat ručně načítání zprávě dekódovacím `$content`.  Například, pokud se byly aktivaci vypnout `application/x-www-url-formencoded` požadavku, který vypadalo následující:

```
CustomerName=Frank&Address=123+Avenue
```

protože to není prostého textu nebo JSON, uloží se v této akci jako:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Kde `$content` je datové kódovaný jako řetězec ve formátu Base 64 zachovat všechna data.  Od momentálně není k dispozici nativní funkci pro dat formuláře, můžu může pořád nepoužívá tato data v rámci pracovní postup ručně přístup k datům pomocí funkce, jako například `@string(body('formdataAction'))`.  Pokud je potřeba Můj odchozí žádost o taky `application/x-www-url-formencoded` záhlaví typu obsahu, může jenom přidám ho na text bez jakékoli hlasováním jako `@body('formdataAction')`.  Však to funkční pouze v případě textu je parametr pouze v `body` zadávání.  Pokud se pokusíte dělat `@body('formdataAction')` uvnitř `application/json` požadavku, zobrazí se chyba za běhu jako odešle zakódovaný textu.
