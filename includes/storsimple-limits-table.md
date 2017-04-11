<!--author=alkohli last changed: 12/15/15-->

| Identifikátor URI limit | Limit | Komentáře |
|----------------- | ------|--------- |
| Maximální počet přihlašovací údaje účtu úložiště | 64 | |
| Maximální počet hlasitost kontejnery | 64 | |
| Maximální počet svazky | 255 | |
| Maximální počet plány na šířku pásma šablony | 168 | Plán pro každou hodinu každý den v týdnu (24 * 7). |
| Maximální velikosti doručovaných vrstvené hlasitosti na fyzické zařízeních | 64 TB pro 8100 a 8600 | 8100 a 8600 jsou fyzických zařízení. |
| Maximální velikosti doručovaných vrstvené hlasitosti na virtuální zařízeních v Azure | 30 TB pro 8010 <br></br> 64 TB pro 8020 | 8010 a 8020 jsou virtuální zařízení v Azure, které používají standardní úložiště a Premium úložiště. |
| Maximální velikosti doručovaných místně připnuté hlasitosti na fyzické zařízeních | 9 TB pro 8100 <br></br> 24 TB pro 8600 | 8100 a 8600 jsou fyzických zařízení. |
| Maximální počet připojení iSCSI | 512 | |
| Maximální počet připojení iSCSI z iniciátory | 512 | |
| Maximální počet záznamů řízení přístupu na zařízení | 64 | |
| Maximální počet svazky za zásady zálohování | 24 | |
| Maximální počet zálohy zachovají za zásady zálohování | 64 | |
| Maximální počet plány za zásady zálohování | 10 | |
| Maximální počet snímků libovolný typ, který můžete uchovávat za hlasitosti | 256 | Jedná se o místní snímky a snímky v cloudu. |
| Maximální počet snímky, které mohou být prezentovat v libovolném zařízení | 10 000 | |
| Maximální počet svazky, které můžete provádí souběžně pro zálohování, obnovení nebo klonovat | 16 |<ul><li>Pokud existuje více než 16 svazky, se budou zpracovány postupně při zpracování sloty k dispozici.</li><li>Nové zálohy klonovaný obnovená hlasitost vrstvené nelze projevit až do dokončení operace. Však svazku místní zálohy mohou po hlasitost je online.</li></ul>|
| Obnovení a klonovat obnovit dobu svazky vrstvené | < 2 minuty | <ul><li>Hlasitost je k dispozici v rámci 2 minuty obnovení nebo klonovat operace, bez ohledu na velikost hlasitost.</li><li>Výkon svazku původně může být nižší než normální jako většina data a metadata stále nachází v cloudu. Jako toky dat z cloudu v zařízení StorSimple může zvýšit výkon.</li><li>Celkový čas ke stažení metadat závisí na velikosti přidělené hlasitost. Metadata není automaticky přenesený do zařízení na pozadí sazbou 5 minut za TB dat přidělené hlasitost. Tento kurz může mít vliv na tak, že šířka internetového pásma do cloudu.</li><li>Obnovení nebo operace klonovat dokončení po všechna metadata v zařízení.</li><li>Zálohování nelze provést, dokud nebude na obnovit nebo klonovat dokončení operace plně.|
| Obnovení obnovit dobu místně připnuté svazky | < 2 minuty | <ul><li>Hlasitost je k dispozici ve dvou minut po obnovení, bez ohledu na velikost hlasitost.</li><li>Výkon svazku původně může být nižší než normální jako většina data a metadata stále nachází v cloudu. Jako toky dat z cloudu v zařízení StorSimple může zvýšit výkon.</li><li>Celkový čas ke stažení metadat závisí na velikosti přidělené hlasitost. Metadata není automaticky přenesený do zařízení na pozadí sazbou 5 minut za TB dat přidělené hlasitost. Tento kurz může mít vliv na tak, že šířka internetového pásma do cloudu.</li><li>Na rozdíl od vrstvené svazky, v případě místně připnuté objemů dat hlasitost taky stažení místně na zařízení. Obnovení je dokončeno při všechna data Hlasitost zařízení.</li><li>Obnovení operace může být dlouhé a celkový čas na dokončení obnovení závisí na velikosti zřizování místní objem, šířka internetového pásma a existujících dat v zařízení. Zálohování místně připnuté svazku jsou povoleny v průběhu operace obnovení.|
| Dostupnost tenké obnovení | Poslední překlopení | |
| Maximální klienta pro čtení i zápis výkon (je-li podávané množství od osy SSD) * | 920/720 MB/s jednoho 10GbE sítě rozhraní | Až 2 x s MPIO dvě rozhraní a sítě. |
| Maximální klienta pro čtení i zápis výkon (je-li podávané množství od osy pevný disk) * | 120/250 MB/s |
| Maximální klienta pro čtení i zápis výkon (je-li podávané množství od osy cloudu) * | 11/41 MB/s | Čtení výkon závisí na klientů generování a údržbu dostatečné hloubka fronty vstupu a výstupu. |

& #42; Maximální výkon typ vstupu a výstupu byl měří se ze 100 % pro čtení a zápis ze 100 % scénáři. Skutečné výkon pravděpodobně nižší a závisí na vstupu a výstupu kombinovat a síťových podmínky.
