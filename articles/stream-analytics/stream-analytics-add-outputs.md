<properties 
    pageTitle="Postup při konfiguraci dat výstup pro analýzy toku úlohy | Microsoft Azure" 
    description="Konfigurace výstupy pro analýzy toku úlohy | Přehled výukových segmentu cesty."
    keywords="výstup, data přesun dat"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/> 

# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Postup při konfiguraci dat výstup pro analýzy toku úlohy

Azure úlohy toku analýzy můžete připojení k jedné nebo více výstupy dat, které určují připojení k existující jímky data. Práce toku analýzy zpracuje a transformací příchozí data, toku dat výstupu událostí je aby došlo k zápisu výstup vašeho projektu.

Vysílání technologie pro analýzu dat výstupy je možné použít jako zdroj v reálném čase řídicí panely nebo upozornění, spouštění pracovních postupů pohyb dat nebo jednoduše archivace dat pro dávkové zpracování později. Technologie pro analýzu toku má prvotřídní integrace s několika Azure službami, které jsou popsány podrobně.

Přidání výstup do analýzy toku práce:

1. Na portálu klasické Azure klikněte **výstupy** a potom na **Přidat výstup** v toku analýzy práce.

    ![Přidání výstupy](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  

    Na portálu Azure klikněte na dlaždici **výstupy** v toku analýzy práce.

    ![Azure Porta přidat výstupy](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Zadejte typ výstup:

    ![Vyberte datový typ pohybu](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  

    ![Azure portál vyberte datový typ pohybu](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Zadejte popisný název pro tento výstup do pole **Alias výstupu** . Tento název lze použít v dotazu vaše úloha později odkázat na výstupu.  
    
    Vyplňte zbytek vlastnosti požadované připojení pro připojení k výstup.  Tato pole se liší podle typu výstup a jsou definované podrobně tady.  

    ![Přidání dat – vlastnosti výstup](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  

4. V závislosti na typu výstup můžete určit, jak serializovat nebo formátu data. Nastavení konkrétní serializace u jednotlivých typů výstup jsou popsány v tomto poli.

    Vyplňte zbytek vlastnosti vyžaduje připojení k připojení ke zdroji dat. Tato pole se liší podle typu typu zadávání a zdroje a jsou definované podrobně [tady](stream-analytics-create-a-job.md).  

    ![Přidání dat výstupu k rozbočovači události](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  

    ![Výstup Azure portálu dat k rozbočovači události](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [Azure.Note] Všechny výstup prvek přidaný do projektu, musí existovat před zahájení úlohy a události začněte toku. Například pokud používáte úložiště objektů Blob jako výstup, úlohy nevytvoří účet úložiště automaticky. Je třeba vytvořit tak, že uživatel před tím, než ASA projektu.

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
