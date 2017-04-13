<properties
   pageTitle="Azure SQL databáze Azure Případová studie - Umbraco | Microsoft Azure"
   description="Další informace o použití databáze SQL Umbraco rychle zřízení a měřítko služby pro tisíce tenantům v cloudu"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="carlrab"/>

# <a name="umbraco-uses-azure-sql-database-to-quickly-provision-and-scale-services-for-thousands-of-tenants-in-the-cloud"></a>Umbraco používá databáze SQL Azure rychle měřítko a poskytování služeb pro tisíce tenantům v cloudu

![Umbraco Logo](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco je Oblíbené otevřít zdroj správu obsahu systém (cm), která by umožnit spuštění nic z malé kampaně nebo brožury webů do složitých aplikací pro Fortune 500 společnosti a weby globální média. 

> "Máme velmi velké komunity vývojáře, kteří používají systému s víc než 100 000 vývojáři na naše fóra a víc než 350,000 weby, které jsou živé, systém Umbraco."

> – Umbraco Mortena Christensen technické potenciální zákazník

> [AZURE.VIDEO azure-sql-database-case-study-umbraco]

Pro zjednodušení implementace zákazníků, Umbraco přidali Umbraco jako-Service (UaaS): software jako-service (SaaS) nabídky, která nemusí v místním nasazení nabízí předdefinované proměnné velikosti a odstraní správy nároky povolením vývojáři zaměření na Správa kódů product inovace spíše než řešení. Umbraco je mohli dát tyto výhody pomocí flexibilní modelu (PaaS) platformy jako služba nabízených Microsoft Azure.

UaaS umožňuje SaaS zákazníkům používání funkcí Umbraco cm, která byla dříve mimo jejich dosažení. Tyto zákazníci jsou zřízení s prostředím cm pracovní obsahující produkční databáze. Zákazníci můžete přidat až dva další databáze pro pracovní prostředí, v závislosti na jejich požadavky a vývoj. Při požadavku na nové prostředí, automatického procesu přiřadí zákazníka databáze automaticky. Nové databáze je připraven v sekundách, protože databáze již byla předem vytvořena tak, že Umbraco z Azure pružná fondu dostupných databází (viz obrázek 1).


![Zřizovací životního cyklu Umbraco](./media/sql-database-implementation-umbraco/figure1.png)

Obrázek 1. Zřizovací cyklu Umbraco jako služba (UaaS)
 
##<a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Azure pružná fondů a automatizace zjednodušit nasazení

Databáze SQL Azure a další služby Azure Umbraco Zákazníci můžete poskytování vlastními silami prostředí a Umbraco můžete snadno sledovat a spravovat databáze jako součást intuitivní pracovního postupu:

1.  Poskytování

    Umbraco udržuje objemu 200 k dispozici předem zřizování databází ze pružná fondů. Když pro UaaS zaregistruje nový zákazník, Umbraco poskytuje zákazníkovi nové prostředí cm v reálném čase blízké přiřazením databáze z fondu dostupnosti.

    Fond dostupnost dosažení prahové hodnoty, se vytvoří nový pružná fond a nové databáze jsou předem zřizování přidělit zákazníkům podle potřeby.

    Implementace je plně automatické použití knihovny správy C# a fronty Bus služby Azure.

2.  Využití

    Zákazníci používají jedno až tři prostředí (pro výrobní, pracovní nebo vývoj) každý s vlastní databázi. Zákazník databáze jsou dostupné ve fondů pružná databáze, která umožňuje Umbraco poskytovat účinné měřítka aniž byste museli povolená zřízení.

    ![Přehled projektu Umbraco](./media/sql-database-implementation-umbraco/figure2.png)

    ![Podrobnosti projektu Umbraco](./media/sql-database-implementation-umbraco/figure3.png)

    Obrázek 2. Umbraco jako-Service (UaaS) zákazníkům web s příkazem Přehled projektu a podrobností

    Databáze SQL Azure pomocí jednotek databázové transakce (DTUs) představují relativní power potřebných pro reálný databázové transakce. Pro zákazníky UaaS databáze obvykle pracují na asi 10 DTUs, ale každý z nich má pružnost zobrazit jako služba. To znamená, že UaaS můžete zajistit zákazníci vždy potřebné zdroje i špičce. Například během posledních události sports neděle píků jednu databázi zkušenosti zákazníků UaaS až 100 DTUs po celou dobu hry. Azure pružná fondů umožnily Umbraco podporuje vyžadující vysoké bez snížení výkonu.

3.  Sledování

    Umbraco monitory databáze aktivity používání řídicích panelů v portálu Azure spolu s vlastní e-mailová upozornění.

4.  Obnovení havárie

    Azure nabízí dvě možnosti havárie obnovení (DR): aktivní Geo replikace a Geo obnovit. Možnost DR vyberte společnost, závisí na svých [cílů provozní kontinuitu](sql-database-business-continuity.md).

    Aktivní Geo replikace poskytuje nejrychlejší úroveň odpovědi v případě výpadek služeb. Použití aktivní Geo replikace, až se čtyřmi čitelné druhotné můžete vytvořit na serverech v různých oblastech a pak můžete zahájit přepnutí do jednotlivých druhotné v případě selhání.

    Umbraco nevyžaduje Geo replikace, ale jej využít Azure Geo-obnovení zajistit minimální prostoje v případě výpadku. Obnovení GEO závisí na zálohy databáze v geo nadbytečné Azure úložiště. Který umožňuje uživatelům obnovit ze záložní kopie při výpadku v oblasti primární.

5.  Odinstalaci poskytování

    Při odstranění prostředí projektu jsou během vymazání fronty Bus služby Azure odebrány všechny přidružené databáze (vývoj, pracovní nebo live). Tento automatického procesu obnoví nepoužitý databáze do fondu pružná dostupnosti databáze na Umbraco je zpřístupňují pro budoucí zřizování a zachovat maximální využití.

##<a name="elastic-pools-allow-uaas-to-scale-with-ease"></a>Pružná fondů povolit UaaS rozšířit usnadnění

Využijete Azure pružná databáze fondů Umbraco můžete optimalizovat pro jeho Zákazníci bez nutnosti nad nebo pod poskytování. Umbraco aktuálně má téměř 3000 databáze přes 19 fondů pružná databáze, umožňuje snadno zobrazit podle potřeby tak, aby zahrnoval jejich stávajících 325,000 zákazníků nebo nové zákazníkům, kteří jsou připravené k nasazení cm v cloudu.

Ve skutečnosti podle Mortena Christensen technické vést na Umbraco, "UaaS teď dochází růst asi 30 noví zákazníci po dnech. Zákazníci jsou nadšený pohodlí moci zřízení nových projektů v sekundách, okamžitě publikovat aktualizace své živou weby vývojové prostředí pomocí jedním kliknutím nasazení a měnit tak, jak rychle-li najít chyby."

Pokud zákazník není nutné zadávat prostředí druhé a třetí dál, můžete jednoduše odebrat tyto prostředí. Které uvolní prostředky, které mohou sloužit jako součást fondu Umbraco pružná dostupnosti databáze pro ostatní zákazníky.

![Architektura Umbraco nasazení](./media/sql-database-implementation-umbraco/figure4.png)

Obrázek 3. Architektura nasazení UaaS na Microsoft Azure

##<a name="the-path-from-datacenter-to-cloud"></a>Cesta z datacentra do cloudu

Při vývojáři Umbraco původně rozhodnutí o přesunutí do modelu SaaS budou věděli, že by potřebují efektivní a scalable způsob vytvoření službu.

> "Fondů pružná databáze jsou ideální přizpůsobit pro naše SaaS podporovat, protože jsme nahoru nebo dolů v případě potřeby vytočit kapacity. Zřízení je to snadné a s naše nastavením, abychom využití nejvýše."

> – Umbraco Mortena Christensen technické potenciální zákazník

"Chtěli jsme naše přemýšlet o řešení potíží s jejích zákazníků, není správu infrastruktury. Chtěli jsme Usnadněte zákazníci abyste získali hodnotu většina"říká Niels Hartvig zakládajícími Umbraco. "Původně považovány hostingu servery sebe, ale plánování kapacity by byly při důvodů." Coincidentally Umbraco není využívat správci žádné databáze, která zdůrazňuje nabízená klíčové hodnota pro používání UaaS.

Jeden důležité cílem pro vývojáře Umbraco bylo umožňují UaaS zákazníci zřídit prostředí rychle a bez kapacitu. Ale poskytující vyhrazené hostovanou službu v Umbraco datacentrech by vyžadovaly spousty nadbytečné kapacitu pro zpracování roztržení při zpracování. Který by chtěl přidání značné výpočetním infrastrukturu, která by byly pravidelně underutilized.

Kromě toho týmu vývoje Umbraco chtěli řešení, které byste mohli znovu použít co nejvíc jejich existující kód nejvíce podporovat. Vývojář Umbraco Mikkel Madsen větu "nebylo spokojení s tím vývojářské nástroje společnosti Microsoft, které se již znáte, jako je Microsoft SQL Server, databáze Microsoft Azure SQL, ASP.net a Internetové informační služby (IIS). Před investovat do IaaS nebo PaaS cloudové řešení, jsme potřebovali, abyste měli jistotu, že ho by podporují naše nástroje systému Microsoft a platformy, takže máme by ke změnám rozsáhlé naše kód základní. "

Která splňuje všechna kritéria Umbraco hledáte partner cloudu pomocí následující kvalifikaci:

-   Dostatečnou kapacitu a spolehlivosti
-   Podpora vývojového nástroje systému Microsoft tak, aby Umbraco inženýrů nebudete muset úplně reinvent jejich vývojové prostředí:
-   Informace o stavu v celém geografické trzích, ve kterých UaaS soutěží (firmy je nutné ověřit, že se můžou dostat k svá data rychle a jejich data se ukládají v umístění, které splňují jejich místní zákonných požadavků)

##<a name="why-umbraco-chose-azure-for-uaas"></a>Proč Umbraco rozhodli, že Azure pro UaaS

Podle Mortena Christensen "po zvážení naše možnosti jsme vybrali Azure protože splněné všechny naše kritérií, z správy a škálovatelnost znalosti a efektivitu nákladů. Nastavení prostředí na Azure VMs a každé prostředí obsahuje vlastní instanci databáze SQL Azure se všechny instance fondů pružná databáze. Tím, že odděluje databází mezi vývoj, pracovní a živou prostředí, nabízíme zákazníci robustní výkonu izolace odpovídají měřítko – velké win. "

Mortena budou dál problémy, "dříve, jsme dosáhl ručně zřízení servery webových databázích. Teď můžeme nemusíte myslet. Všechno, co je automatické – z zřizování vyčištění. "

Mortena je také spokojení s možnostmi změny měřítka poskytovanou Azure. "Fondů pružná databáze jsou ideální přizpůsobit pro naše SaaS podporovat, protože jsme nahoru nebo dolů v případě potřeby vytočit kapacity. Zřízení je to snadné a s naše nastavením, abychom využití nejvýše." Mortena států, "zjednodušení pružná fondů spolu s assurance osy – založené na službě DTUs dostaneme power zřízení nového fondů zdrojů jako služba. Nedávno, jednu z jejích větší zákazníků Špičatá 100 DTUs v reálném prostředí. Použití Azure, náš pružná fondů podle zákazníka databáze se zdroji, které jsou potřebné v reálném čase aniž byste museli předpovídání DTU požadavky. Jednoduše řečeno, zákazníci získat zapnout kolem okamžiku očekávají a jsme zahájit naše výkonu úrovni služeb smlouvami."

Mikkel Madsen sečte: "Jsme jste kterých je založena výkonné Azure algoritmus, která bude propojena běžné situace SaaS (nových zákazníků pro rychlého připojení v reálném čase ve velkém měřítku) naše aplikace vzoru (předem zřizování databází, obě vývoj a live) nad technologii (pomocí Azure Bus služby fronty ve spojení s Azure SQL databáze)."

##<a name="with-azure-uaas-is-exceeding-customer-expectations"></a>S Azure, což je UaaS překročení očekávání zákazníka

Od výběr Azure jako jeho partner cloudu, byla Umbraco mohli dát UaaS zákazníci s optimalizaci výkonu správu obsahu bez potřebné z vlastního hostovanou řešení investice IT zdroje. Jak Mortena s textem, ", které máme rádi developer pohodlí a škálovatelnost, která Azure dostaneme a zákazníci jsou thrilled s funkcemi a spolehlivosti. Celkově bylo skvělé win nám!"
 
## <a name="more-information"></a>Další informace

- Další informace o fondů Azure pružná databáze, najdete v článku [fondů pružná databáze](sql-database-elastic-pool.md).

- Další informace o Bus služby Azure najdete v tématu [Bus služby Azure](https://azure.microsoft.com/services/service-bus/).

- Další informace o Web rolí a kolegy, najdete v článku [pracovní rolí](../fundamentals-introduction-to-azure.md#compute). 

- Další informace o virtuální sítě najdete v tématu [virtuální síť](https://azure.microsoft.com/documentation/services/virtual-network/).    

- Další informace o zálohování a obnovení najdete v tématu [nepřerušený](sql-database-business-continuity.md).  

- Další informace o sledování ppols najdete v tématu [sledování fondů](sql-database-elastic-pool-manage-portal.md). 

- Další informace o Umbraco jako služba najdete v tématu [Umbraco](https://umbraco.com/cloud).

