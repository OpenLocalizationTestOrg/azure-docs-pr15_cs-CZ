<properties
   pageTitle="Použití Azure funkcí s aplikacemi jiných logiky | Microsoft Azure"
   description="Informace o použití funkce Azure s aplikacemi jiných logiky"
   services="logic-apps,functions"
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

# <a name="using-azure-functions-with-logic-apps"></a>Použití funkcí Azure s aplikacemi jiných logiky

Spustit vlastní fragmenty C# nebo node.js pomocí funkcí Azure z v rámci logiky aplikace.  [Funkce Azure](../azure-functions/functions-overview.md) nabízí serveru bez výpočtu v Microsoft Azure. To je užitečné k provádění tyto věci:

* Rozšířené formátování nebo výpočetních polí v aplikaci použití logických operátorů
* Provádění výpočtů v rámci pracovního postupu
* Rozšíření funkcí aplikace logických funkcí, které podporuje C# nebo node.js

## <a name="create-a-function-for-logic-apps"></a>Vytvoření funkce pro použití logických operátorů aplikace

Doporučujeme, abyste si vytvořili novou funkci na portálu funkce Azure pomocí šablon **Obecný Webhook - uzel** nebo **Obecný Webhook - C#** . To automatického naplní šablonu, která přijímá `application/json` pomocí funkčně logiky aplikace.  Pokud vyberete kartě **Integrace** ve funkcích Azure měli **režimu** **Webhook** a **Typ Webhook** **Obecný JSON**.  Funkce, které používají tyto šablony automaticky zjištění a uvedené v Návrháři logiky aplikace v části **Azure funkcí v oblast.**

Funkce Webhook přijmout žádost a předejte mu do metody prostřednictvím `data` proměnné. Máte přístup k vlastnosti vaše data pomocí tečkami jako `data.foo`.  Jednoduché funkce JavaScript, která se převede hodnotu data a času na řetězec datum bude vypadat jako v následujícím příkladu:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Funkce Azure hovor z aplikace použití logických operátorů

V Návrháři klikněte na nabídku **Akce** můžete vybrat **Azure funkcí v oblast**.  Seznamy na kontejnerech předplatného a umožňuje zvolit, které chcete volat.  

Po výběru funkci se výzva k zadání vstupních datové objekt. Toto je zpráva, která odešle aplikaci použití logických operátorů k této funkci a musí být objekt JSON. Například pokud chcete předat datum **Poslední změny** ze služby Salesforce aktivační událost, datové funkce může vypadat takto:

![Datum posledního upravenou barvou vzhledu][1]

## <a name="trigger-logic-apps-from-a-function"></a>Aktivační událost aplikace logických funkcí

Také je možné aktivovat logiky aplikace z jiné funkce.  K tomuto účelu vytvoření jednoduše aplikace logika s ruční spuštění. Další informace najdete v tématu [Použití logických operátorů aplikace jako možné volat koncové body](app-service-logic-http-endpoint.md).  V rámci vaší funkce generovat HTTP příspěvek na ruční spuštění adresu URL s informacemi, které chcete odeslat do aplikace logiky.

### <a name="create-a-function-from-the-designer"></a>Vytvoření funkce z Návrháře

Můžete taky vytvořit node.js webhook funkce do z návrháře. Nejdřív vyberte **Azure funkcí v oblast** a pak zvolte kontejneru vaší funkce.  Pokud ještě nemáte kontejner, je potřeba vytvořit některé z [funkcí Azure portálu](https://functions.azure.com/signin). Klikněte na **Vytvořit nový**.  

Generovat šablony podle data, která chcete spočítat, zadejte kontextový objekt, který chcete předat funkci. Musí být objekt JSON. Například pokud předávání v obsah souboru z akce FTP datové kontextu bude vypadat takhle:

![Místní data][2]

>[AZURE.NOTE] Protože tento objekt nebyl přetypován řetězec, obsah se přidá přímo do datové JSON. Však se chyba, pokud ještě není token JSON (to znamená řetězec nebo JSON pole nebo objektu). Se jej převést jako řetězec, jednoduše přidáte uvozovky v první obrázku v tomto článku.

Návrhář potom vytvoří, můžete vytvořit vložené šablony (funkce). Proměnné předem vytvořené podle kontextu, který chcete předat funkci.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
