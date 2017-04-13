<properties 
   pageTitle="Limity systém StorSimple | Microsoft Azure"
   description="Popisuje limity systému a doporučené velikost součástí řady StorSimple 8000 a připojení."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="storsimple-system-limits"></a>Limity StorSimple systému

## <a name="overview"></a>Základní informace

StorSimple poskytuje scalable a flexibilním úložiště pro vaše datacentra. Můžou ale nastat určité datové limity, které je třeba vzít v úvahu při plánování, nasazení a pracovat StorSimple řešení. Následující tabulka popisuje následující omezení a poskytuje pár doporučení, takže můžete nejlépe využijete StorSimple řešení.

| Identifikátor URI limit | Limit | Komentáře |
|----------------- | ------|--------- |
| Maximální počet přihlašovací údaje účtu úložiště | 64 | |
| Maximální počet hlasitost kontejnery | 64 | |
| Maximální počet svazky | 255 | |
| Maximální počet místně připnuté svazky | 32 | |
| Maximální počet plány na šířku pásma šablony | 168 | Plán pro každou hodinu každý den v týdnu (24 * 7). |
| Maximální velikosti doručovaných vrstvené hlasitosti na fyzické zařízeních | 64 TB pro 8100 a 8600 | 8100 a 8600 jsou fyzických zařízení. |
| Maximální velikosti doručovaných vrstvené hlasitosti na virtuální zařízeních v Azure | 30 TB pro 8010 <br></br> 64 TB pro 8020 | 8010 a 8020 jsou virtuální zařízení v Azure, které používají standardní úložiště a Premium úložiště. |
| Maximální velikosti doručovaných místně připnuté hlasitosti na fyzické zařízeních | 8.5 TB pro 8100 <br></br> 22,5 TB pro 8600 | 8100 a 8600 jsou fyzických zařízení. |
| Maximální počet připojení iSCSI | 512 | |
| Maximální počet připojení iSCSI z iniciátory | 512 | |
| Maximální počet záznamů řízení přístupu na zařízení | 64 | |
| Maximální počet svazky za zásady zálohování | 24 | |
| Maximální počet zálohy zachovají za plánem (zásady zálohování) | 64 | |
| Maximální počet plány za zásady zálohování | 10 | |
| Maximální počet snímků libovolný typ, který můžete uchovávat za hlasitosti | 256 | Toto číslo zahrnuje místní snímky a snímky v cloudu. |
| Maximální počet snímky, které mohou být prezentovat v libovolném zařízení | 10 000 | |
| Maximální počet svazky, které můžete provádí souběžně pro zálohování, obnovení nebo klonovat | 16 |<ul><li>Pokud existuje více než 16 svazky, jsou zpracovány postupně při zpracování sloty k dispozici.</li><li>Nové zálohy klonovaný obnovená vrstvené hlasitost nelze projevit až do dokončení operace. Však svazku místní zálohy mohou po hlasitost je online.</li></ul>|
| Obnovení a klonovat obnovit dobu vrstvené svazky | < 2 minuty | <ul><li>Hlasitost je k dispozici v rámci 2 minuty obnovení nebo klonovat operace, bez ohledu na velikost hlasitost.</li><li>Výkon svazku původně může být nižší než normální jako většina data a metadata stále nachází v cloudu. Jako toky dat z cloudu v zařízení StorSimple může zvýšit výkon.</li><li>Celkový čas ke stažení metadat závisí na velikosti přidělené hlasitost. Metadata není automaticky přenesený do zařízení na pozadí sazbou 5 minut za TB dat přidělené hlasitost. Tento kurz může mít vliv na tak, že šířka internetového pásma do cloudu.</li><li>Obnovení nebo operace klonovat dokončení po všechna metadata v zařízení.</li><li>Zálohování nelze provést, dokud nebude na obnovit nebo klonovat dokončení operace plně.|
| Obnovení obnovit dobu místně připnuté svazky | < 2 minuty | <ul><li>Hlasitost je k dispozici ve dvou minut po obnovení, bez ohledu na velikost hlasitost.</li><li>Výkon svazku původně může být nižší než normální jako většina data a metadata stále nachází v cloudu. Jako toky dat z cloudu v zařízení StorSimple může zvýšit výkon.</li><li>Celkový čas ke stažení metadat závisí na velikosti přidělené hlasitost. Metadata není automaticky přenesený do zařízení na pozadí sazbou 5 minut za TB dat přidělené hlasitost. Tento kurz může mít vliv na tak, že šířka internetového pásma do cloudu.</li><li>Na rozdíl od vrstvené svazky pro místně připnuté objemů dat hlasitost taky stažení místně na zařízení. Obnovení je dokončeno při všechna data Hlasitost zařízení.</li><li>Obnovení operace může být dlouhé. Celkový čas na dokončení obnovení závisí na velikosti zřizování místní hlasitost šířky pásma Internetu a existujících dat v zařízení. Zálohování místně připnuté svazku jsou povoleny v průběhu operace obnovení.|
|Zpracování sazbu cloudu snímky| 15 minut/TB | <ul><li>Minimální chvíli, aby cloudu snímku připravené k nahrání na TB přidělené hlasitost dat v rámci zálohování. </li><li> Čas snímku celkové cloudu počítá přidáním tentokrát nahrát čas snímku, který bude to mít vliv tak, že šířka internetového pásma do cloudu.
| Maximální klienta pro čtení i zápis výkon (je-li podávané množství od osy SSD) * | 920/720 MB/s rozhraním jedné sítě 10 GbE | Až 2 x s MPIO dvě rozhraní a sítě. |
| Maximální klienta pro čtení i zápis výkon (je-li podávané množství od osy pevný disk) * | 120/250 MB/s |
| Maximální klienta pro čtení i zápis výkonu (je-li podávané množství od osy mraků)* pro aktualizace 3 nebo novější** | 40/60 MB/s pro vrstveny svazky<br><br>60/80 MB/s pro vrstveny svazky s vybranou při vytváření hlasitost archivace možností | Čtení výkon závisí na klientů generování a údržbu dostatečné hloubka fronty vstupu a výstupu. <br><br>Rychlost dosáhnout závisí na rychlosti základní úložiště účet použili. | 

& #42; Maximální výkon typ vstupu a výstupu byl měří se ze 100 % pro čtení a zápis ze 100 % scénáři. Skutečné výkon pravděpodobně nižší a závisí na vstupu a výstupu kombinovat a síťových podmínky.

& #42; & #42; Může být nižší výkon čísla před aktualizace 3.

## <a name="next-steps"></a>Další kroky

Zkontrolujte [požadavky na systém pro StorSimple](storsimple-system-requirements.md). 
