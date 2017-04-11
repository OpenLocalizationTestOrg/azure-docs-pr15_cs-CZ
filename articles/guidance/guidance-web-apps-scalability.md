<properties
   pageTitle="Scalable webové aplikace | Architektura Azure odkaz | Microsoft Azure"
   description="Vylepšení škálovatelnost ve webové aplikaci spuštěné v Microsoft Azure."
   services="app-service,app-service\web,sql-database"
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/12/2016"
   ms.author="mwasson"/>


# <a name="improving-scalability-in-a-web-application"></a>Vylepšení škálovatelnost ve webové aplikaci 

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tento článek popisuje doporučené architektura pro vylepšení škálovatelnost a výkonu ve webové aplikaci na Microsoft Azure. Architektura je založena na [Architektura Azure odkaz: základní webové aplikace][basic-web-app]. Doporučení a co byste měli zvážit od tohoto článku vyrovnat tato architektura stejně.

>[AZURE.NOTE] Azure obsahuje dva různé nasazení modely: Správce zdrojů a klasické. Tento článek používá správce zdroje, který Microsoft doporučuje nové nasazení.

## <a name="architecture-diagram"></a>Diagram architektury

![[0]][0]

Architektura má tyto prvky:

- **Pole Skupina zdroje**. [Pole Skupina zdroje] [ resource-group] je kontejner logické Azure zdrojů. 

- ** [V prohlížeči] [ app-service-web-app] ** a ** [rozhraní API aplikace][app-service-api-app]**. Typická moderní aplikace mohou obsahovat web a jeden nebo více RESTful webového rozhraní API. Webového rozhraní API může být spotřebované množství prohlížeče klientů prostřednictvím AJAX, nativní klientské aplikace nebo aplikací na straně serveru. Důležité informace o navrhování webu rozhraní API, najdete v článku [pokyny pro návrh rozhraní API][api-guidance].    

- **WebJob**. Použití [Azure WebJobs] [ webjobs] spuštění úkoly dlouho probíhajících na pozadí. WebJobs je možné spouštět na plán, průběžně, nebo v odpovědi na aktivační událost, například přepnutím zprávu do fronty. WebJob běží jako pozadí obrázku v kontextu aplikace pro aplikaci služby. 

- **Fronta**. V architektura ukazuje tato část fronty aplikací úlohy na pozadí vložíte zprávu do [Úložiště fronty Azure] [ queue-storage] fronty. Zpráva aktivace fungují v WebJob. 

    Můžete taky můžete použít službu Bus fronty. Porovnání, najdete v článku [Azure fronty služby Bus fronty – porovnání a v porovnání s][queues-compared].

- **Mezipaměti**. Uložení částečně statická data v [Mezipaměti Redis Azure][azure-redis].  

- **CDN**. Použití [Síť pro doručování obsahu Azure] [ azure-cdn] (CDN) obsah do mezipaměti veřejně přístupná, malá latence a rychlejší doručení obsahu.

- **Ukládání dat.** Použití [Databáze SQL Azure] [ sql-db] pro relačních dat. Relačních dat, zvažte NoSQL úložiště, jako je úložiště tabulek Azure nebo [DocumentDB][documentdb].

- **Hledání azure**. Pomocí pole [Hledat Azure] [ azure-search] přidat funkci hledání, včetně návrhy hledání, rozmazaný hledání a hledání specifické pro určitý jazyk. Azure hledání se obvykle používá ve spojení s jinou úložiště dat, zejména pokud úložiště primární dat vyžaduje striktních konzistence. V tomto přístupu byste autoritativní data v jiném zdroji dat a umístění indexu vyhledávání do hledání Azure. Azure hledání lze také sloučit indexu jedním hledání z více ukládají data.  

- **E-mailu nebo SMS**. Pokud potřebujete-li odeslat e-mailu nebo zprávy SMS aplikace, použijte třetích stran služby, třeba SendGrid nebo Twilio, nikoli vytváření prvek pro tuhle funkci přímo do aplikace.

## <a name="recommendations"></a>Doporučení

### <a name="app-service-apps"></a>Aplikace služby aplikace 

Doporučujeme vytváření webové aplikace a rozhraní API webových jako samostatné aplikace aplikaci služby. Tento návrh můžete spustit v samostatných plánech aplikaci služby umožňující zase měřítko nezávisle na sobě. Pokud nepotřebujete úroveň škálovatelnost na nejdřív, můžete nasazení aplikace do plánu stejného a je přesuňte na samostatné plány později v případě potřeby. (Pro plány Basic, Standard a Premium je faktura instancí OM v plánu, nikoli aplikace. V tématu [aplikaci služby ceny][app-service-pricing])

Pokud budete chtít použít *Snadno tabulek* nebo funkce *Snadno rozhraní API* aplikace služby mobilních aplikací, vytvořte samostatné aplikace aplikaci služby k tomuto účelu.  Tyto funkce závisí na určitou aplikaci framework, aby mohli.

### <a name="webjobs"></a>WebJobs

Pokud WebJob náročný, zvažte nasazení aplikace prázdné aplikaci služby v samostatných plán služeb aplikací, stanovit vyhrazené instance WebJob. V tématu [pokyny úlohy pozadí][webjobs-guidance].  

### <a name="cache"></a>Mezipaměti

Zvýšit výkon a škálovatelnost: pomocí [Azure Redis mezipaměti] [ azure-redis] do mezipaměti některá data. Zvažte použití Redis mezipaměti pro:

- Částečně statické transakce data.

- Stav relace.

- Výstup ve formátu HTML. To může být užitečné v aplikacích, které vykreslení složité výstup ve formátu HTML. 

Podrobné pokyny pro návrh strategie pro ukládání do mezipaměti, najdete v článku [pokyny pro ukládání do mezipaměti][caching-guidance].

### <a name="cdn"></a>CDN 

Použití [Azure CDN] [ azure-cdn] na statický obsah mezipaměti. Hlavních výhod CDN je snížit latence pro uživatele, protože obsah cached na *edge serveru* , který je tomu vašemu nejbližší uživatel geograficky. CDN můžete také změnit zatížením aplikací, protože tento přenos není právě uskutečněných jednotlivými aplikace.

- Pokud aplikace je tvořen převážně statické stránky, zvažte použití CDN do mezipaměti celou aplikaci. V tématu [použití Azure CDN služby Azure aplikace][cdn-app-service].

- V opačném vložit statický obsah, jako jsou například obrázky, CSS a HTML soubory, do úložiště Azure a pomocí CDN tyto soubory v mezipaměti. V tématu [Integrace úložiště účet u CDN][cdn-storage-account].

> [AZURE.NOTE] Azure CDN nelze vytisknout obsah, který vyžaduje ověření.

Podrobnější pokyny naleznete v tématu [pokyny obsahu Delivery Network (CDN)][cdn-guidance]. 

### <a name="storage"></a>Úložiště

Moderní aplikace často zpracovat velké objemy dat. Pokud chcete zobrazit v cloudu, je důležité zvolte typ správné úložiště. Tady je pár doporučení podle směrného plánu.  Podrobné pokyny, najdete v článku [Hodnocení možnosti úložiště dat Polyglot trvalé řešení][polyglot-storage].

Co chcete uložit | Příklad | Doporučené úložiště
--- | --- | ---
Soubory | Obrázky, dokumenty, soubory PDF | Úložiště objektů Blob Azure
Klíč/dvojice | Vyhledat podle ID uživatele dat profilů uživatelů | Úložiště tabulek Azure
Krátké zprávy určené pro aktivaci další zpracování | Požadavky na pořadí | Azure fronty úložiště, Bus frontě nebo tématu Bus služby
Bez relačních dat pomocí flexibilní schématu vyžadující základní dotazování | Katalogu produktů | Databáze dokumentu, například Azure DocumentDB, MongoDB nebo Apache CouchDB
Relačních dat, které vyžadují bohatší dotazu podpory, vždy schématu nebo silných konzistence | Stavu skladových zásob produktu | Databáze Azure SQL

## <a name="scalability-considerations"></a>Škálovatelnost: co byste měli zvážit

### <a name="app-service-app"></a>Aplikace služby aplikace

Pokud řešení obsahuje několik aplikaci služby aplikace, zvažte nasazení je k oddělení plány aplikaci služby. Tento přístup umožňují měřítko nezávisle na sobě, protože se spouštějí v samostatných instancích. Podrobnosti o rozšiřování, najdete v článku [Co byste měli zvážit škálovatelnost] [ basic-web-app-scalability] v tématu [základní webové aplikace architektura][basic-web-app].

Podobně zvažte uvedení WebJob do vlastní plán, takže úlohy na pozadí nespouštět na stejné instance, kteří zpracovávat požadavky HTTP.  

### <a name="sql-database"></a>Databáze SQL

Zvýšit škálovatelnost databázi SQL *sharding* databázi &mdash; tedy ve vodorovném směru rozdělení databáze. Sharding umožňuje rozšiřování databáze ve vodorovném směru, použitím [pružná databázové nástroje][sql-elastic]. Potenciální výhody sharding patří:

- Lepší výkon transakce.

- Dotazy můžete pracovat rychleji přes podmnožinu dat. 

### <a name="azure-search"></a>Azure hledání

Azure hledání odebere režijních probíhá vyhledávání komplexních dat z úložiště primární dat a můžete změnit velikost zpracovávání načíst. Zobrazit [úrovně zdroje měřítko pro dotaz a indexování úloh hledání Azure][azure-search-scaling].

## <a name="security-considerations"></a>Otázky bezpečnosti pro

### <a name="cross-origin-resource-sharing-cors"></a>Sdílení (CORS) Origin více zdrojů

Pokud vytváříte web a webového rozhraní API jako samostatné aplikace na webu nemůže volat klientských AJAX k rozhraní API Pokud povolíte CORS. 

> [AZURE.NOTE] Zabezpečení prohlížeče zabrání vytvoření AJAX požadavky na jinou doménu na webovou stránku. Toto omezení se nazývá zásad stejný původ a zabrání škodlivým webu čtení sentitive dat z jiného webu. CORS je standard W3C, který umožňuje serveru zmírnit zásad stejné origin a povolit některé požadavky křížově origin při odmítnutí ostatním.

Aplikace služby podporuje předdefinované pro CORS, aniž by musel vytvářeli kód aplikace. V tématu [spotřebě aplikace rozhraní API ze JavaScript pomocí CORS][cors]. Přidání webu do seznamu povolených typů pro rozhraní API. 

### <a name="sql-database-encryption"></a>Šifrování databáze SQL

Použití [Průhledných šifrování dat] [ sql-encryption] v případě potřeby šifrování data na ostatních v databázi. Tato funkce provede v reálném čase šifrování a dešifrování celou databázi (včetně zálohování a souborů protokolu transakce), aniž by bylo změn do aplikace. Šifrování přidáte některé latence, je vhodné k oddělení data, která musí být zabezpečený do vlastní databáze a povolit šifrování jenom pro tuto databázi.  

## <a name="next-steps"></a>Další kroky

- Vyšší dostupnost nasazení aplikace v víc než jedné oblasti a použití [Azure přenosy správce] [ tm] pro překlopení. Další informace najdete v tématu [Architektura Azure odkaz: webové aplikace s vysokou dostupnost][web-app-multi-region].    

<!-- links -->

[api-guidance]: ../best-practices-api-design.md
[app-service-web-app]: ../app-service-web/app-service-web-overview.md
[app-service-api-app]: ../app-service-api/app-service-api-apps-why-best-platform.md
[app-service-pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[azure-cdn]: https://azure.microsoft.com/en-us/services/cdn/
[azure-redis]: https://azure.microsoft.com/en-us/services/cache/
[azure-search]: https://azure.microsoft.com/en-us/documentation/services/search/
[azure-search-scaling]: ../search/search-capacity-planning.md
[background-jobs]: ../best-practices-background-jobs.md
[basic-web-app]: guidance-web-apps-basic.md
[basic-web-app-scalability]: guidance-web-apps-basic.md#scalability-considerations 
[caching-guidance]: ../best-practices-caching.md
[cdn-app-service]: ../app-service-web/cdn-websites-with-cdn.md
[cdn-storage-account]: ../cdn/cdn-create-a-storage-account-with-cdn.md
[cdn-guidance]: ../best-practices-cdn.md
[cors]: ../app-service-api/app-service-api-cors-consume-javascript.md
[documentdb]: https://azure.microsoft.com/en-us/documentation/services/documentdb/
[polyglot-storage]: https://github.com/mspnp/azure-guidance/blob/master/Polyglot-Solutions.md
[queue-storage]: ../storage/storage-dotnet-how-to-use-queues.md
[queues-compared]: ../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md
[resource-group]: ../resource-group-overview.md
[sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[sql-elastic]: ../sql-database/sql-database-elastic-scale-introduction.md
[sql-encryption]: https://msdn.microsoft.com/en-us/library/dn948096.aspx
[tm]: https://azure.microsoft.com/en-us/services/traffic-manager/
[web-app-multi-region]: ./guidance-web-apps-multi-region.md
[webjobs-guidance]: ../best-practices-background-jobs.md
[webjobs]: ../app-service/app-service-webjobs-readme.md
[0]: ./media/blueprints/paas-web-scalability.png "Webová aplikace v Azure s rozšiřitelnost"