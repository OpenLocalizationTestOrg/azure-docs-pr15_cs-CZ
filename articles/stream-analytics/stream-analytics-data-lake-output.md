<properties
    pageTitle="Výstup toku technologie pro analýzu dat jezera úložiště | Microsoft Azure"
    description="Konfigurace ověřování a povolení úložiště Azure dat jezera úlohy toku analýzy"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Výstup toku technologie pro analýzu dat jezera úložiště

Úlohy analýzy toku podporují několik výstup metod, jeden je [Úložiště jezera dat Azure](https://azure.microsoft.com/services/data-lake-store/). Úložiště jezera dat Azure je úložiště celopodnikové hyper měřítko pro analytický pracovního vytížení velký data. Úložiště jezera dat umožňuje ukládat data z libovolné velikost, typ a požití rychlost provozní a průzkumné analýzy.

## <a name="authorize-a-data-lake-store-account"></a>Povolte účet jezera úložiště dat

1.  Úložiště jezera dat vybrán jako výstup na portálu Správa Azure, zobrazí se výzva k ověření použití existujícího datového jezera úložiště nebo požádat o přístup k náhled dat jezera úložiště prostřednictvím portálu klasické Azure.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  Pokud už máte přístup k úložišti jezera, klepněte na tlačítko "povolte" a stručný čas stránky bude objevovat označující "Přesměrování na povolení...". Na stránce se automaticky zavře a zobrazí na stránce, která by umožňovala nakonfigurovat úložiště jezera dat výstupu.

Pokud jste ještě nemáte svůj náhled dat jezera úložiště, můžete kliknutím na odkaz "Zaregistrujte" zahajte žádost nebo postupujte podle [Začínáme pokyny](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="configure-the-data-lake-store-output-properties"></a>Konfigurace vlastností výstup jezera úložiště dat

Až budete mít ověření účtu úložiště jezera dat, lze konfigurovat vlastnosti úložiště jezera dat výstupu. Následující tabulce je seznam názvů vlastností a jejich popis nakonfigurovat úložiště jezera dat výstupu.

<table>
<tbody>
<tr>
<td><B>NÁZEV VLASTNOSTI</B></td>
<td><B>POPIS</B></td>
</tr>
<tr>
<td>Výstup Alias</td>
<td>Toto je používána v dotazech ke směrování výstupu dotazu pro toto úložiště jezera popisný název.</td>
</tr>
<tr>
<td>Účet jezera úložiště dat</td>
<td>Název účtu úložiště kde odesíláte výstup. Zobrazí se rozevírací seznam účtů jezera úložiště pro které má uživatel přihlášení k portálu přístup k.</td>
</tr>
<tr>
<td>Cesta předpona vzorek [<I>volitelné</I>]</td>
<td>Cesta k souboru použít k vytvoření souborů v rámci zadaný účtu úložiště jezera Data. <BR>{date}, {čas}<BR>Příklad 1: složka1/protokoly / {dat} / {čas}<BR>Příklad 2: složka1/protokoly / {dat}</td>
</tr>
<tr>
<td>Formát Datum [<I>volitelné</I>]</td>
<td>Pokud token datum používá v parametru path předponu, můžete vybrat formát data, ve kterém jsou uspořádány souborů. Příklad: Rrrr/MM/DD</td>
</tr>
<tr>
<td>Formát času [<I>volitelné</I>]</td>
<td>Pokud je token čas použitý v parametru path předponu, zadejte formát času, ve kterém jsou uspořádány souborů. Aktuálně podporované hodnota je pouze HH.</td>
</tr>
<tr>
<td>Formát serializace události</td>
<td>Formát serializace výstupní data. JSON, CSV a Avro jsou podporovány.</td>
</tr>
<tr>
<td>Kódování</td>
<td>Pokud formátu CSV nebo JSON kódování je nutné zadat. Jediný podporovaný formát kódování UTF-8 je v současné době.</td>
</tr>
<tr>
<td>Oddělovače</td>
<td>Platí pouze pro serializaci CSV. Technologie pro analýzu toku podporuje několik běžných oddělovače serializaci data ve formátu CSV. Podporované hodnoty jsou čárku, středník, místa, tab a svislá čára.</td>
</tr>
<tr>
<td>Formát</td>
<td>Platí pouze pro serializaci JSON. Čára oddělené označuje, že výstup zformátuje tím, že každý objekt JSON odděleni nový řádek. Pole určuje výstup bude naformátovat jako pole objektů JSON.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Obnovení povolení jezera úložiště dat

V současné době není omezení kde ověřovací token je třeba ručně aktualizovat každé 90 dnů pro všechny úlohy pomocí úložiště jezera dat výstupu. Bude taky muset znovu ověření účtu úložiště jezera dat při změně hesla od vaše úloha byla vytvořená nebo poslední ověření. Příznakem tohoto problému je žádné výstupu projektu a chybu v protokolech operace označující potřebu opětovné povolení.

Tento problém můžete vyřešit, zastavte spuštěná úloha a přejděte na úložiště jezera dat výstupu. Klikněte na odkaz "Obnovení se tak mohli ověřovat" a stručný čas stránky bude objevovat označující "Přesměrování na povolení...". Na stránce automatické uzavře a v případě úspěchu výskyt znamená "Se tak mohli ověřovat byl úspěšně obnoven". Můžete pak potřeba v dolní části stránky klikněte na "Uložit" a můžete pokračovat restartováním práce z zastavit naposledy Chcete-li předejít ztrátě dat.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)
