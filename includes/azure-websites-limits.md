Zdroje|Uvolnit|Sdílené (verze Preview)|Základní|Standardní|Premium (verze Preview)</th>
---|---|---|---|---|---
[Web, mobilní telefon nebo rozhraní API aplikace](https://azure.microsoft.com/services/app-service/) pro [aplikaci služby plán](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|Neomezený<sup>2</sup>|Neomezený<sup>2</sup>|Neomezený<sup>2</sup>
[Použití logických operátorů aplikace](https://azure.microsoft.com/services/app-service/logic/) na [plán služeb aplikací](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup>|10|10|10|20 na základní|20 na základní
[Plán služeb aplikací](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 na oblast|10 na pole Skupina zdroje|100 na pole Skupina zdroje|100 na pole Skupina zdroje|100 na pole Skupina zdroje
Výpočet instanci typu|Sdílené|Sdílené|Vyhrazené<sup>3</sup>|Vyhrazené<sup>3</sup>|Vyhrazené<sup>3</sup></p>
[Škálování](../articles/app-service-web/web-sites-scale.md) (max instance)|1 sdílí|1 sdílí|3 vyhrazené<sup>3</sup>|10 vyhrazené<sup>3</sup>|20 snaží (50 v pomocného mechanismu řízení)<sup>3, 4</sup>
Úložiště<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
Procesor Čas (krátký)<sup>6</sup>|3 minuty|3 minuty|Neomezeno platbu na standardní [sazby](https://azure.microsoft.com/pricing/details/app-service/)</a>|Neomezeno platbu na standardní sazba|Neomezeno platbu na standardní sazba
Čas (den) procesoru<sup>6</sup>|60 minut|240 minut|Neomezeno platbu na standardní [sazby](https://azure.microsoft.com/pricing/details/app-service/)</a>|Neomezeno platbu na standardní sazba|Neomezeno platbu na standardní sazba
Paměť (1 hodina)|1 024 MB na plán služeb aplikací|1 024 MB na aplikaci|NENÍ K DISPOZICI|NENÍ K DISPOZICI|NENÍ K DISPOZICI
Šířka pásma|165 MB|Neomezený, [rychlosti přenosu dat](https://azure.microsoft.com/pricing/details/data-transfers/) použít|Neomezeno přenos dat, kterou sazby|Neomezeno přenos dat, kterou sazby|Neomezeno přenos dat, kterou sazby
Architektura aplikací|32bitová verze|32bitová verze|32-bit nebo 64bitová verze|32-bit nebo 64bitová verze|32-bit nebo 64bitová verze
Web Sockets za instance<sup>7</sup>|5|35|350|Neomezený|Neomezený
Souběžné [ladění připojení](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) k jednomu aplikace|1|1|1|5|5
[subdoména azurewebsites.NET s FTP/S a SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
Podpora [vlastní domény](../articles/app-service-web/web-sites-custom-domain-name.md)||X|X|X|X
Vlastní doménu [podporují SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|||Neomezený|Neomezeno, 5 SNI SSL a 1 připojení IP SSL zahrnutá|Neomezeno, 5 SNI SSL a 1 připojení IP SSL zahrnutá
Vyrovnávání zatížení integrované||X|X|X|X
[Vždy zapnuto](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Plánované zálohy](../articles/app-service-web/web-sites-backup.md)||||Jednou za den|Jednou každých to 5 minut<sup>8</sup>
[Automatické měřítko](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
Podpora [Plánovač Azure](https://azure.microsoft.com/services/scheduler/)||X|X|X|X
[Sledování koncový bod](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Pracovní sloty (verze Preview)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
Vlastní domény za aplikace</a>||500|500|500|500
SLA||<p>|99,9 %|99.95 %<sup>10</sup>|99.95 %<sup>10</sup>

<sup>1</sup> Aplikace a kvóty úložiště jsou pro jednotlivé plán služeb aplikací Pokud není uvedeno jinak.  
<sup>2</sup> Skutečný počet aplikace, které budete moct hostovat u těchto počítačů závisí na aktivitu aplikace písma, velikosti instancí počítače a odpovídající využití prostředků.  
<sup>3</sup> Vyhrazené instance může být různých velikostí. Další informace najdete v článku [Aplikace služby ceny](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) .  
<sup>4</sup> Vrstvy Premium umožňuje až 50 vzorec vypočítá instance (vyměřené poplatky za jeho dostupnost) a 500 GB místa na disku při používání aplikace služby prostředí a 20 výpočet instance a 250 GB úložiště jinak.  
<sup>5</sup> Limit úložiště je celková velikost obsahu u všech aplikací v plánu stejného aplikaci služby. Další možnosti pro úložiště jsou dostupné v [Prostředí aplikace služby](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup> Tyto materiály jsou omezeny fyzické zdroje na vyhrazené instance (velikosti instance a počet instancí).  
<sup>7</sup> Pokud plánujete aplikace v základní osy na dvě instance, máte 350 souběžné připojení u každé dvě instance.  
<sup>8</sup> Vrstvy Premium umožňuje intervaly zálohování dolů až každých 5 minut při používání aplikace služby prostředí a 50 časy denně jinak.  
<sup>9</sup> Spusťte skripty nebo vlastní spustitelných na službu, podle plánu, případně nepřetržitě úlohy na pozadí v rámci instance aplikace služby. Vždy je potřebný pro provádění průběžných WebJobs. Azure Plánovač bezplatné nebo standardní je potřebný pro plánované WebJobs. Neexistuje žádný definovaná omezení počtu, mohlo by umožnit spuštění v aplikaci služby instance WebJobs, ale existují praktické datové limity, které jsou závislé na kód aplikace vyzkoušení dělat.   
<sup>10</sup> SLA přidělený nasazení, které správce několika instancích spuštěných s Azure přenosy % 99.95 nakonfigurován pro překlopení.  
