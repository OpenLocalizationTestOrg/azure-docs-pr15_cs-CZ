<properties
    pageTitle="Přidání zadávání dat k projektům toku analýzy | Microsoft Azure"
    description="Zjistěte, jak připojit zdroj dat k práci toku analýzy jako datových proudů zadávání dat z rozbočovače událost nebo odkaz na data z úložiště blogu."
    keywords="Vstupní, data streaming dat"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Přidání datových proudů dat vstupní nebo odkaz data projektu toku analýzy

Zjistěte, jak připojit zdroj dat k práci toku analýzy jako datových proudů zadávání dat z rozbočovače událost nebo odkaz na data z úložiště objektů Blob.

Azure úlohy toku analýzy můžete být připojeni k zadávání dat jeden nebo více, z nichž každá definice připojení existujícího zdroje dat. Podle data odeslaný k tomuto zdroji dat, je využívané úlohy toku technologie pro analýzu a zpracovávají v reálném čase jako datových proudů data. Technologie pro analýzu toku má prvotřídní integrace se službou [Azure události rozbočovače](https://azure.microsoft.com/services/event-hubs/) a [úložiště objektů Blob Azure](../storage/storage-dotnet-how-to-use-blobs.md) uvnitř a mimo předplatné projektu.

Tento článek se kroku [cesta výuky toku analýzy](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Zadávání dat: přenos dat a referenčních dat

Existují dva typy různých vstupů v toku analýzy: datové proudy a referenčních dat.

- **Datové proudy**: úlohy analýzy toku musí obsahovat alespoň jeden dat toku předávat na vstupu spotřebované množství a transformaci podle projektu. Azure úložiště objektů Blob a Azure události rozbočovače jsou podporované jako vstupní zdroje dat proudu. Azure rozbočovače události slouží k události proudy shromažďovat připojená zařízení, služby a aplikace. Úložiště objektů Blob Azure mohou sloužit jako vstupní zdroje pro ingesting hromadné dat pomocí datového proudu.  
- **Referenční data**: toku analýzy podporuje druhého typu pomocné vstupní jen referenční data.  Protikladu k hledání dat v pohybu tato data jsou statické nebo zpomaluje změny.  Obvykle se používá k provádění look-ups a korelace s datovými proudy k vytvoření bohatší datové sady.  Úložiště objektů Blob Azure momentálně pouze podporované vstupní zdrojem dat odkaz.  

Chcete-li přidat vstup do analýzy toku práce:

1. Na portálu Azure klikněte **vstupů** a klikněte na tlačítko **Přidat vstup** v toku analýzy práce.

    ![Azure klasické portál – přidání vstup.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    Na portálu Azure klikněte na dlaždici **vstupy** v toku analýzy práce.  

    ![Azure portál – přidání zadávání dat.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. Zadejte vstupní: **toku dat** nebo **dat odkaz**.

    ![Přidání správná data pro zadávání, streamují nebo odkaz](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![Přidání správná data pro zadávání, streamují nebo odkaz](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. Při vytváření vstupní toku dat, zadejte zdroj pro vstupní.  Tento krok může přeskočilo při vytváření referenčních dat jako pouze objektů Blob úložiště je v současné době podporované.

    ![Přidání zadávání dat toku dat](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![Přidání zadávání dat toku dat](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. Zadejte popisný název tohoto zadávání do pole vstupní Alias.  Tento název se použije v dotazu vaše úloha později odkázat na vstup.

    Vyplňte zbytek vlastnosti vyžaduje připojení k připojení ke zdroji dat. Tato pole se liší podle typu typu zadávání a zdroje a jsou definované podrobně [tady](stream-analytics-create-a-job.md).  

    ![Přidání události centrální zadávání dat](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. Určení nastavení serializace pro zadávání dat:
    - Abyste měli jistotu svoje dotazy fungují, který očekáváte, zadejte **Formát serializace události** příchozí dat.  Podporované serializačních formátů jsou JSON, CSV a Avro.
    - Ověřte **kódování** pro data.  Jediný podporovaný formát kódování UTF-8 je v současné době.

    ![Nastavení serializace dat pro zadávání dat](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![Nastavení serializace dat pro zadávání dat](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. Po dokončení zadávání vytváření, toku analýzy ověří, můžete připojit ke zdroji zadávání.  V centru oznámení můžete zobrazit stav operace testovat připojení.

    ![Test připojení streamování dat při zadávání](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![Test připojení streamování dat při zadávání](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Pokud potřebujete pomoc s streamování zadávání dat
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
