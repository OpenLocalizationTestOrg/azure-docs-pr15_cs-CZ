Zdroje|Výchozí Limit
---|---
Počet úložiště účtů na jedno předplatné|200<sup>1</sup>
TB na účtu úložiště|500 TB
Maximální počet objektů blob kontejnery, objekty BLOB, sdílené složky, tabulek, fronty, entity nebo zpráv v rámci účtu úložiště|Pouze limit je 500 TB kapacitou účtu
Maximální velikost kontejneru jednoho objektů blob, tabulku nebo fronty|500 TB
Maximální počet bloků v bloku objektů blob nebo připojit objektů blob|50 000
Maximální velikost bloku v bloku objektů blob nebo připojit objektů blob|4 MB
Maximální velikost objektů blob bloku nebo připojit objektů blob|50000 x 4 MB (poli 195 GB) 
Maximální velikost objektů blob stránky |1 TB
Maximální velikost tabulky entita|1 MB
Maximální počet vlastností v tabulce entita|252
Maximální velikost zprávy ve frontě|64 KB
Maximální velikost sdílené složky|5 TB
Maximální velikost souboru ve sdílené složce|1 TB
Maximální počet souborů ve sdílené složce|Pouze limit je 5 TB celkovou kapacitu sdílené složky
Maximální počet procesorů znalostní bázi Knowledge Base 8 za sdílet|1 000
Maximální počet souborů ve sdílené složce|Pouze limit je 5 TB celkovou kapacitu sdílené složky
Maximální počet objektů blob kontejnerů, objekty BLOB, sdílené složky, tabulek, fronty, entity nebo zpráv v rámci účtu úložiště|Pouze limit je 500 TB kapacitou účtu
Maximální počet zásady přístupu uložené na kontejner, sdílení souborů, tabulku nebo fronty|5
Celková požádat o sazbou (za předpokladu, že velikost objektů 1KB) účtu úložiště|Až 20 000 procesorů entit sekundu nebo zpráv za sekundu
Cíl výkon pro jeden objektů blob|Až 60 MB na druhou nebo až 500 požadavky za sekundu
Cíl výkon jednoho fronty (1 KB zprávy)|Až 2000 zpráv za sekundu
Výkon cílový oddíl jednou tabulkou (1 KB entity)|Až 2000 entity za sekundu
Cíl výkon pro sdílení souborů|Až 60 MB za sekundu
Max průniku<sup>2</sup> na účtu úložiště (nám oblasti)|10 GB/s Pokud GRS/ZRS<sup>3</sup> aktivní, 20 GB/s pro LRS
Max výstupní<sup>2</sup> na účtu úložiště (nám oblasti)|20 GB/s Pokud Vzdálená pomoc-GRS/GRS/ZRS<sup>3</sup> aktivní, 30 s technologií pro LRS
Max průniku<sup>2</sup> na účtu úložiště (Evropské and Asie oblastí)|Pokud je povoleno GRS/ZRS<sup>3</sup> , 10 GB/s pro LRS 5 s technologií
Max výstupní<sup>2</sup> na účtu úložiště (Evropské and Asie regiony)|10 GB/s Pokud Vzdálená pomoc-GRS/GRS/ZRS<sup>3</sup> aktivní, 15 GB/s pro LRS

<sup>1</sup> Jedná se o Standard a Premium úložiště účty. Pokud požadujete více než 200 účty úložiště, zkontrolujte žádost o podporu [Azure](https://azure.microsoft.com/support/faq/). Týmu Azure úložiště bude zkontrolujte váš případ firmy a může schválit až 250 úložiště účty. 

<sup>2</sup> *Průniku* odkazuje na všechna data (požadavky) odesílaného k účtu úložiště. *Výstupní* odkazuje na všechna data (odpovědi) přijímané z účtu úložiště.  

<sup>3</sup> Možnosti replikace v Azure úložiště, patří:

- **Vzdálená pomoc GRS**: geo nadbytečné úložiště přístup pro čtení. Pokud je povoleno Vzdálená pomoc GRS výstupní cílů sekundární umístění jsou stejné jako primární umístění.
- **GRS**: Geo nadbytečné úložiště. 
- **ZRS**: zóny nadbytečné úložiště. K dispozici pouze pro objekty BLOB blok. 
- **LRS**: místně nadbytečné úložiště. 

