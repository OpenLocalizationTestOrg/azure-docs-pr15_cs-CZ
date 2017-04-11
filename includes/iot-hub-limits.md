V následující tabulce jsou uvedeny limity přidružený k jiné službě úrovní (S1, S2, S3, F1). Informace o nákladech každé *jednotku* v každé vrstvy najdete v článku [IoT centrální ceny](https://azure.microsoft.com/pricing/details/iot-hub/).

| Zdroje | Standardní S1 | Standardní s2 | Standardní S3 | Bezplatné F1 |
| -------- | ----------- | ----------- | ----------- | ------- |
| Zprávy/den | 400,000 | 6,000,000   | 300,000,000 | 8 000   |
| Maximální počet jednotek | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Pokud očekáváte více než 200 jednotky pomocí S1 nebo S2 nebo S3 rozbočovače osy, kontaktujte podporu společnosti Microsoft.

V následující tabulce jsou uvedeny limity, které se vztahují k IoT Centrum zdrojů:

| Zdroje | Limit |
| -------- | ----- |
| Maximální zaplacený IoT rozbočovače jedno předplatné Azure | 10 |
| Maximální bezplatného rozbočovače IoT jedno předplatné Azure | 1 |
| Maximální počet zařízení identity<br/>  Vrátí jednu telefonuje | 1 000 |
| Centrální IoT zprávy maximální uchovávání informací u zpráv zařízení do cloudu | 7 dnů |
| Maximální velikost zprávy zařízení do cloudu | 256 ZNALOSTNÍ BÁZI KNOWLEDGE BASE |
| Maximální velikosti doručovaných dávku zařízení do cloudu | 256 ZNALOSTNÍ BÁZI KNOWLEDGE BASE |
| Maximální počet zpráv v listu zařízení do cloudu | 500 |
| Maximální velikost zprávy cloudu zařízení | 64 KB |
| Maximální hodnota TTL cloudu zařízení zpráv | dva dny |
| Doručení maximální počet různých hodnot cloudu zařízení <br/> zprávy | 100 |
| Počet maximální doručování zpráv zpětné vazby <br/> odpověď na zprávu cloudu zařízení | 100 |
| Maximální hodnota TTL názory zpráv v aplikaci <br/> odpověď na zprávu cloudu zařízení | dva dny |

> [AZURE.NOTE] Pokud budete potřebovat víc než 10 placené rozbočovače IoT v Azure předplatného, kontaktujte prosím podporu společnosti Microsoft.

Služba IoT centrální omezení žádosti o překročení kvóty takto:

| Omezení | Za centrální hodnota |
| -------- | ------------- |
| Operace registru identity <br/> (vytvoření, načíst, seznam, aktualizovat a odstranit), <br/> jednotlivé nebo hromadných import nebo export | 5000/min/jednotky (S3) <br/> 100/min/jednotky (S1 a S2). |
| Připojení zařízení | 6000/sec/jednotky (S3) 120/sec/jednotky (S2), 12/sec/jednotky (S1). <br/>Minimální 100/sec. |
| Odešle zařízení do cloudu | 6000/sec/jednotky (S3) 120/sec/jednotky (S2), 12/sec/jednotky (S1). <br/>Minimální 100/sec. |
| Odešle cloudu zařízení | 5000/min/jednotky (S3), 100/min/jednotky (S1 a S2). |
| Shluk zařízení obdrží | 50000/min/jednotky (S3) 1000/min/jednotky (S1 a S2). |
| Nahrávání souborů | 5000 soubor nahrát oznámení/min/jednotky (S3), 100 soubor odeslat oznámení/min/jednotky (S1 a S2). <br/> Identifikátory URI 10000 přidružení zabezpečení může být se pro účet Azure úložiště najednou.<br/> 10 přidružení zabezpečení URI/je možné zařízení se najednou. |
