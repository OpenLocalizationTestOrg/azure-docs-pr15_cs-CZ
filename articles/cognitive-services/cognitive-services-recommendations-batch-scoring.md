
<properties
    pageTitle="Získání doporučení v dávkách: počítače výukové rozhraní API doporučení | Microsoft Azure"
    description="Azure počítače výukové doporučení – začíná doporučení v listech"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Doporučení vstoupit listy

>[AZURE.NOTE] Získání doporučení v dávkách je složitější než začíná doporučení jeden po druhém. Zkontrolujte rozhraní API pro informace o tom, jak získat doporučení pro jeden požadavek:

> [Doporučení pro položku](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Doporučení položky uživatele](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Dávkové bodování jde použít pouze pro sestavení, které jsou vytvořené po 21 červenec 2016.


Existují situace, kdy budete potřebovat sadu doporučení pro více položek najednou. Například nejspíš vás bude zajímat vytváření doporučení mezipaměti nebo dokonce analýza typy doporučení, která se zobrazují.

Dávkové bodování operace, jak jsme je volají, jsou asynchronní operace. Potřebujete odeslat žádost, počkejte na dokončení operace a shromáždění výsledky.  

Je další přesné, Tyhle postupujte podle těchto kroků:

1.  Vytvoření kontejneru Azure úložiště, pokud už nemáte.
2.  Nahrání vstupní souboru, který obsahuje popis všech vašich požadavků doporučení k úložišti objektů Blob Azure.
3.  Zahájení debat bodování dávku.
4.  Počkejte na dokončení asynchronní operace.
5.  Po dokončení operace shromážděte výsledky z úložiště objektů Blob.

Podívejme se každý z těchto kroků.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Vytvoření kontejneru úložiště, pokud už nemáte

Přejděte na [portál Azure](https://portal.azure.com) a vytvoření nového účtu úložiště, pokud už nemáte. K tomuto účelu přejděte na **Nový** > **dat** + **úložiště** > **Účtu úložiště**.

Až budete mít účet úložiště, je potřeba vytvořit kontejnery objektů blob kam bude ukládat vstupní a výstupní spuštění dávky.

Nahrání vstupní soubor, který popisuje všech vaší doporučení žádosti k úložišti objektů Blob – Pojďme zavolejte na linku input.json soubor tady.
Až budete mít kontejneru, budete muset nahrát soubor, který obsahuje popis požadavků, které potřebujete provést služby doporučení.

Listu můžete provádět pouze jeden typ žádosti o z konkrétní Tvůrce dotazů. Jsme vysvětluje, jak definovat tyto informace v další části. Prozatím Předpokládejme, že jsme budete provádět doporučení položky mimo určitý Tvůrce dotazů. Vstupní soubor pak s informacemi vstupní (v tomto případě počáteční hodnota položky) pro jednotlivá pole žádosti.

Toto je příklad souboru input.json vypadá jako:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Jak vidíte, soubor je JSON souboru, kde má každá žádostí o informace, které je třeba pošlete žádost o doporučení. Vytvoření souboru podobné JSON pro požadavky, které je potřeba splnit a zkopírujte ho do kontejneru, který jste vytvořili v úložišti objektů Blob.

## <a name="kick-start-the-batch-job"></a>Zahájení debat úlohy

Dalším krokem je odešlete novou dávku. Další informace najdete v [rozhraní API odkaz](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

Žádost o textu rozhraní API musí definovat umístění, kde vstupní výstup a soubory s chybovými zprávami nutné uložit. Také musí definovat její přihlašovací údaje, které jsou potřebné pro přístup k těmto umístění. Kromě toho je nutné zadat některé parametry, které se vztahují k celé skupině (druh doporučení požádat o modelu/Tvůrce dotazů můžete použít, počet výsledků na volání atd.)

Toto je příklad požadavku by měl vypadat takto:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Tady několik důležitých věcí je potřeba pamatovat:

-   V současné době **authenticationType** měli vždy nastavit **PublicOrSas**.

-   Potřebujete k získání token přidružení sdílené podpis aplikace Access (zabezpečení) Pokud chcete povolit rozhraní API doporučení pro čtení a zápis ze/ke svému účtu úložiště objektů Blob. Další informace o tom, jak generovat tokeny přidružení zabezpečení najdete na [stránce rozhraní API doporučení](../storage/storage-dotnet-shared-access-signature-part-1.md).

-   Pouze **apiName** aktuálně podporovaný je **ItemRecommend**, které se používá pro doporučení položek se. Dávkové, které nejsou podporovány aktuálně doporučení položky uživatele.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Počkejte na dokončení asynchronní operace

Po spuštění operace dávku odpověď vrátí operace umístění záhlaví, které vám informace, které je třeba sledovat operaci.
Operace sledovat pomocí [Načíst API stav operace]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da), stejně jako pro sledování operace operace Tvůrce dotazů.

## <a name="get-the-results"></a>Získání výsledků

Po dokončení operace za předpokladu, že došlo k žádné chyby, můžete získat výsledky z výstupu úložiště objektů Blob.

V příkladu níže zobrazit výstup může vypadat takto. V tomto příkladu jsme zobrazující výsledky listu se jenom dva požadavky (neopakujeme).

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Další informace o omezeních

-   Jedinou dávku můžete volat na jedno předplatné najednou.
-   Dávkový soubor úlohu vstupní nesmí být více než 2 MB.
