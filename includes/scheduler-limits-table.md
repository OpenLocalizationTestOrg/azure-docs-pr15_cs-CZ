Následující tabulka popisuje jednotlivé hlavní kvót, omezení, výchozí hodnoty a omezením v Azure plánovači.

|Zdroje|Popis limit|
|---|---|
|**Velikost úlohy**|Maximální velikost je 16 kB. Pokud umístění nebo opravy výsledkem větší než tato omezení projektu, bude vrácena 400 Chybný požádat o stavový kód.|
|**Žádost o velikosti adresy URL**|Maximální velikosti doručovaných požadavku na adresu URL je 2048 znaků.|
|**Velikost agregační záhlaví**|Velikost maximální agregační záhlaví je 4096 znaků.|
|**Počet záhlaví**|Počet maximální záhlaví je 50 záhlaví.|
|**Velikost textu**|Velikost maximální textu je 8192 znaků.|
|**Období opakování**|Maximální opakování období je 18 měsíců.|
|**Čas spuštění**|Maximální počet "čas spuštění" je 18 měsíců.|
|**Historie úlohy**|Maximální odpověď textu uložené v historie úlohy je 2048 bajtů.|
|**Četnosti**|Výchozí maximální četnost kvóta je 1 hodina v kolekci bezplatné úlohy a 1 minuta v kolekci standardní projektu. Maximální frekvence je, která dokáže nahradit v kolekci úlohy nižší než maximální hodnota. Všechny úlohy v kolekci úlohy jsou omezené hodnotu nastavenou na kolekci projektu. Pokud budete chtít vytvořit úlohu s frekvenci vyšší než maximální četnosti v kolekci úlohy žádost selhat s 409 stavový kód konflikt.|
|**Úlohy**|Výchozí maximální úlohy kvóta je 5 úlohy v kolekci bezplatné úlohy a 50 úlohy v kolekci standardní projektu. Maximální počet úloh lze konfigurovat pro kolekci projektu. Všechny úlohy v kolekci úlohy jsou omezené hodnotu nastavenou na kolekci projektu. Pokud se pokusíte vytvořit více projektů než maximální úlohy kvóta, požadavek selže s 409 stavový kód konfliktu.|
|**Uchování historie úlohy**|Historie úlohy se zachovají až 2 měsíce nebo až poslední 1000 spuštění.|
|**Uchování dokončený a chybovém úlohy**|Dokončení a chybovém úlohy se zachovají 60 dní.|
|**Časový limit**|Není statický (ne, která dokáže nahradit) žádost časový limit 60 sekund pro HTTP akce. Delší spuštění operací postupujte podle protokolu HTTP asynchronní protokoly například vrátit 202 okamžitě ale pokračovat v práci na pozadí.|
