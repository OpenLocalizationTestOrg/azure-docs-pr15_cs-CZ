<properties 
   pageTitle="Toku analýzy omezení tabulky"
   description="Popisuje limity systému a doporučené velikost součásti toku technologie pro analýzu a připojení."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Identifikátor URI limit | Limit       | Komentáře |
|----------------- | ------------|--------- |
| Maximální počet jednotek, datových proudů jedno předplatné na oblast | 50 | Žádost o zvýšení streamování jednotek pro vaše předplatné za 50 půjde kontaktováním [Podpory společnosti Microsoft](https://support.microsoft.com/en-us). |
| Maximální výkon streamování jednotky | 1MB / s * | Maximální výkon SH závisí na scénáře. Skutečné výkon pravděpodobně nižší a závisí na složitost dotazu a rozdělení. Další podrobnosti najdete v článku [Měřítko Azure toku analýzy úlohy zvýšit výkon](../articles/stream-analytics/stream-analytics-scale-jobs.md) . |
| Maximální počet vstupů na projekt | 60 | Existuje pevné omezení 60 vstupů pro analýzy toku projekt. |
| Maximální počet výstupy na projekt | 60 | Existuje pevný limit 60 výstupy na projekt toku analýzy. |
| Maximální počet funkcí na projekt | 60 | Existuje pevné omezení 60 funkcí na projekt toku analýzy. |
| Maximální počet úloh jednotlivých oblastech | 1 500 | Každého předplatného může mít až 1 500 projekty podle zeměpisnou oblast. |