<properties
   pageTitle="Stavba řetězcích filtru pro návrháře tabulky | Microsoft Azure"
   description="Vytváření filtrů řetězce Návrháři tabulky"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Stavba řetězcích filtru pro návrháře tabulky

## <a name="overview"></a>Základní informace

Filtrování dat v tabulce aplikace Azure, která se zobrazí ve Visual Studiu **Návrhář tabulky**, vytvářet řetězec filtru a zadejte do pole Filtr. Syntaxe řetězce filtr je definován datové služby WCF je podobný klauzuli WHERE jazyka SQL, ale se neodesílají tabulku služba prostřednictvím žádost HTTP. **Návrhář tabulky** pracuje s počátečními kódování za vás, takže pokud chcete filtrovat podle hodnotu požadované vlastnosti, stačí zadat jenom název vlastnosti, relační operátor, kritéria hodnoty a volitelně logická hodnota operátor do pole Filtr. Není potřeba možnost dotazu $filter jako kdybyste byly stavba adresy URL k vytvoření dotazu v tabulce prostřednictvím [Úložiště služby REST API odkaz](http://go.microsoft.com/fwlink/p/?LinkId=400447).

Datové služby WCF vycházejí [Open Data Protocol](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Podrobné možnosti dotaz systém filtr (**$filter**) najdete v článku [specifikace konvence URI OData](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Relační operátory

Pro všechny typy vlastností jsou podporovány následující logických operátorů:

|Logickým operátorem|Popis|Příklad filtr řetězce|
|---|---|---|
|EQ|Znaménko rovná|Město eq "Redmond"|
|gt|Větší než|Gt cenu 20|
|stránku|Větší než nebo rovno|Cena stránku 10|
|lt|Menší než|Lt cenu 20|
|Le|Menší než nebo rovno|Cena le 100|
|Ne|Není rovno|Ne města "Londýn"|
|a|A|Cena le 200 a cena gt 3.5|
|nebo|Nebo|Cena le 3.5 nebo cena gt 200|
|Not|Not|není isAvailable|

Při vytváření řetězec filtru, je důležité následující pravidla:

- Použití logických operátorů k porovnání vlastnost na hodnotu. Všimněte si, že není možné a umožňuje porovnávat vlastnost na hodnotu dynamické; jedno straně výraz musí být konstanta.

- Všechny části Filtr řetězce rozlišují malá a velká písmena.

- Konstantní hodnota musí být stejného typu dat ve vlastnosti filtr, aby se platný. Další informace o podporovaných typů najdete v článku [Principy datového modelu tabulku služba](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filtrování řetězec vlastností

Pokud filtrujete řetězec vlastností, uzavřete do jednoduchých uvozovek řetězcová konstanta.

Následující příklad filtry vlastností **PartitionKey** a **RowKey** ; Další jiných klíč vlastnosti může taky přidá řetězec filtr:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Každý výrazu filtru můžete uzavřít do závorek, i když není nutné:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Všimněte si, že tabulku služba nepodporuje dotazů se zástupnými znaky a nejsou podporovány v návrhovém zobrazení tabulky buď. Však můžete provádět předponu odpovídající pomocí relačních operátorů na požadovanou předponu. Následující příklad vrátí entity s příjmení vlastnost, začínající na písmeno "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filtrování číselných vlastností

Pokud chcete filtrovat podle celé číslo nebo číslo s plovoucí desetinnou čárkou, zadejte číslo bez uvozovek.

Tento příklad k vrácení všech entit s vlastností stáří jejichž hodnota je větší než 30:

    Age gt 30

Tento příklad k vrácení všech entit s vlastností AmountDue jejichž hodnota je menší nebo rovna 100.25:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filtrování logický vlastností

Pokud chcete filtrovat podle logická hodnota, zadejte **hodnotu true** nebo **false** bez uvozovek.

Následující příklad vrátí všechny entity, pokud je vlastnost IsActive nastavena na **true**:

    IsActive eq true

Můžete také napsat tento výraz filtru bez s logickým operátorem. V následujícím příkladu tabulku služba vrátí rovněž všechny entity **které IsActive platí**:

    IsActive

Pokud chcete vrátit všechny entity, kde je NEPRAVDA IsActive, můžete použít není operátor:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filtrování u vlastnosti data a času

Pokud chcete filtrovat podle hodnoty data a času, zadejte klíčové slovo **data a času** , následovaný konstanta datum a čas do jednoduchých uvozovek. Kombinované UTC formát, musí odpovídat konstanta datum a čas podle [Formátování data a času nemovitostí s hodnotou](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Následující příklad vrátí entity, kde je rovno 10 červenec 2008 vlastnost CustomerSince:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
