<properties
    pageTitle="Nejčastější dotazy týkající se databáze ClearDB MySql aplikace službou Azure | Microsoft Azure"
    description="Odpovědi na běžné dotazy týkající se používání databáze ClearDB MySQL službou Azure aplikace."
    documentationCenter="php"
    services=""
    authors="sunbuild"
    manager="yochayk"
    editor=""
    tags="mysql"/>

<tags
    ms.service="multiple"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="sumuth"/>

# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Nejčastější dotazy týkající se databáze ClearDB MySql aplikace službou Azure

Tento článek zodpovídá běžné otázky týkající se používání a nákup databáze ClearDB MySQL Azure webových aplikací.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Jaké mám možnosti pro MySQL na Azure?

Máte několik možností:

* [Databáze MySQL sdílené ClearDB](/marketplace/partners/cleardb/databases/)

* [ClearDB MySQL Premium clusterů](/marketplace/partners/cleardb-clusters/cluster/)

* [Shluk MySQL spuštěna OM Azure](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)

* [Jedna instance spuštěna Azure OM MySQL](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md)

ClearDB je MySQL hostitelské službě a spravoval infrastruktury MySQL za vás. Při spuštění vlastní MySQL obrázku nebo databáze na Azure virtuálního počítače, budete muset nastavit MySQL server a v jednoduchosti je aktualizován opravy.

## <a name="do-i-need-a-credit-card-for-the-web-app--mysql-template-in-the-azure-marketplace"></a>Je potřeba platební kartou pro Web app + MySQL šablony v Azure Marketplace?

To záleží na typu předplatné, které používáte. Tady jsou některé často používané předplatného:

* [Platba podle příjmů](/offers/ms-azr-0003p/): vyžaduje platební kartou, a při nákupu placené databáze MySQL Účtovaná vaší kreditní karty.

* [Bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/): obsahuje přeplatky pomocí Microsoft Azure services, ale neumožňuje nákupní prostředků třetích stran. Koupit služby třetích stran nebo placené databáze MySQL musíte použít platební kartou povolený předplatného. Pro Web Apps můžete vytvořit databáze MySQL bezplatné ClearDB.

* [Web MSDN pro předplatné](/pricing/member-offers/msdn-benefits/) a **Web MSDN pro vývojáře Test zaplatit při přechodu**: podobně jako bezplatnou zkušební verzi předplatného MSDN vyžaduje kreditní karty k nákupu placené řešení MySQL ClearDB.

* [Enterprise Agreement (EA)](/pricing/enterprise-agreement/): EA zákazníci se vám účtovat poplatky před jejich EA jednotlivých čtvrtletí pro všechny nákupy Azure Marketplace (jiných výrobců) na samostatném, konsolidované faktuře neuvádí. Je faktura mimo peněžní potvrzení pro žádný marketplace nákup. Dejte pozor, aby v současné době Azure úložiště není dostupný zákazníkům zaregistrované v Ázerbájdžán, Chorvatsko, Norsko a Portorika. 

*  [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): můžete vytvořit jenom bezplatné ClearDB databáze pro Web Apps. Existuje bez omezení počtu bezplatné ClearDB MySQL databází, můžete vytvořit. Všimněte si, že bezplatné databáze se nemusí používat výrobní webových aplikacích pro tuto službu určené pouze pro zkušební verzi.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-the-azure-marketplace"></a>Proč účtována 3.50 pro Web app + MySQL z Azure Marketplace

Volba výchozí databáze je Titan, což je $3.50. Doporučujeme nezobrazovat náklady při vytváření databáze a můžete omylem zakoupit databázi, kterou jste neměli v úmyslu. Jsme jsou hledáte způsob, jak zlepšuje ale do té doby je třeba zkontrolovat všechny vaše vybrané ceny úrovní pro web app a databáze před kliknutím na **vytvořit** a od nasazení zdroje.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-to-my-database"></a>Používám MySQL na vlastní Azure virtuálního počítače. Se dá připojit aplikace Moje Azure webové databáze aplikace?

Ano. Web appu můžete připojit k databázi, dokud Azure OM Dal vzdálený přístup do webové aplikace. Další informace najdete v tématu [MySQL nainstalovat na počítač virtuální](virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Které země způsoby ClearDB Premium MySQL clusterů podporované?

[ClearDB Premium MySQL clusterů](/marketplace/partners/cleardb-clusters/cluster/) jsou k dispozici ve všech oblastech Azure Celosvětová dostupnost s výjimkou Indie, Austrálie, Brazílie jih a Čína.

## <a name="can-i-create-a-new-cluster-prior-to-creating-a-database-with-cleardb-premium-cluster-solution"></a>Můžete vytvořit nový cluster před vytváření databáze pomocí řešení clusteru ClearDB premium?

Ne, vytvoření prázdného clusterů ClearDB nepodporuje. Portál Azure umožňuje vytvářet databáze v obrázku, které můžete vytvořit nový cluster ve stejnou dobu.

## <a name="will-i-be-warned-if-i-try-to-delete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Zobrazí se upozornění, pokud se pokusíte odstranit databáze ClearDB MySQL, který používá pomocí jednoho z aplikace?

Ne, Azure nebude odpovíte Pokud odstraníte nákup marketplace, závisí na tom aplikace.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Oblasti, které můžu vytvořit ClearDB databází v?

Není k dispozici pro zákazníky zaregistrované v Ázerbájdžán, Chorvatsko, Norsko nebo Portoriko Azure Marketplace. ClearDB není dostupná v těchto oblastech.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>K čemu ceny osy zvolte pro databáze a výrobních web appu?

Použijte základní nebo vyšší úroveň ceny pro Web Apps. Pro ClearDB doporučujeme Saturn nebo Jupiter plánu. Prohlédněte si funkce a omezení při každé ceny osy [Web Apps](/pricing/details/app-service/) a [databáze ClearDB MySQL](/marketplace/partners/cleardb/databases/) zvolte ten, který vyhovuje vašim potřebám.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-to-another"></a>Jak můžu upgradovat svůj ClearDB databáze z jednoho plánu na druhý?

[Azure portál](https://portal.azure.com)můžete změnit sdílené databázi ClearDB hostingu. V tomto [článku](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) se dozvíte víc. Aktuálně Nepodporujeme upgrade ClearDB Premium clusterů Azure portálu.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Nevidím databáze aplikace ClearDB Azure portálu?

Pokud vytvoříme ClearDB databáze pomocí účtu správce prostředků Azure nebo [Novém portálu Azure](https://portal.azure.com), nebude viditelný na [Původní portál Azure](https://manage.windowsazure.com). Řešení to znamená ručně odkaz databáze na web app. Podobně-li vytvořit databázi ClearDB [staré portál](https://manage.windowsazure.com) nebude možné zobrazíte databáze v [Novém portálu Azure](https://portal.azure.com). Neexistuje žádné řešení pro tento scénář.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Kdo se dá spojit podporu, když Moje databáze dolů?

Kontaktní [ClearDB podporu](https://www.cleardb.com/developers/help/support) pro všechny databáze související s jejich stavem. Připravit poskytnout informacím Azure předplatného.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Můžete vytvořit další uživatele pro moje řešení clusteru databáze ClearDB MySQL? 

Ne. Nemůžete vytvořit další uživatelé můžete však vytvářet dalších databází na svůj cluster ClearDB databáze.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-to-planetary-plans-today-on-cleardb-portal"></a>Můžete Basic a Pro řadu databáze se upgradovat místních podobný planetární plány dnes na portálu ClearDB?

Ano, základní řadu, kterou může být databází upgradovat na místě (základní 60 prostřednictvím základní 500). Může být pro řadu upgradovat na místě (Pro 125 prostřednictvím Pro 1000) s výjimkou Pro 60. Upgrade databáze Pro 60 aktuálně nepodporujeme. 

## <a name="when-i-migrate-my-resources-from-one-subscription-to-another-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Pokud budu migrovat zdrojů z předplatných na jiný, databáze ClearDB MySQL získat poštovním i? 

Po provedení migrace zdrojů u předplatných platí určitá [omezení](./app-service-web/app-service-move-resources.md) . Databáze ClearDB MySQL je služba třetích stran a tedy získat nemigruje se v průběhu migrace Azure předplatného. Pokud spravujete není migrace databáze MySQL před migrace Azure prostředky, můžete zakázat databáze ClearDB MySQL. Ruční migrace databáze nejdřív a pak proveďte migraci Azure předplatné pro webovou aplikaci. 

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-to-an-ea-subscription"></a>Můžu převést databázi ClearDB z předplatného platební kartou k předplatnému EA?

Existující databáze ClearDB pomocí platební kartou přidružený k stávající předplatné. Pokud chcete používat předplatné EA budete muset migrovat data do nové databáze:

* Koupit novou databázi ClearDB k vašemu předplatnému EA.
* Přenést data na nové databáze.
* Aktualizace aplikace pomocí nové databáze.
* Odstraňte staré ClearDB databázi.

Při vytvoření nové webové aplikace pomocí MySQL (ClearDB) nebo vytvoření databáze MySQL (ClearDB) předplatné, které zvolíte Určuje, jak bude platit pro službu. S předplatným EA nebude blokujeme zásobovací služeb třetí strany, jako je ClearDB Azure portálu. Předplatná EA je faktura mimo peněžní potvrzení a je faktura čtvrtletní a zpětně. Zákazník EA musel nastavit způsob platby, například platební kartu platit pro všechny služby marketplace třetích stran.

## <a name="where-can-i-see-the-charges-for-cleardb-resources-in-an-ea-subscription"></a>Kde lze zobrazit poplatky ClearDB zdrojů v předplatné EA?

Přímé EA zákazníci jsou zobrazeny na portálu Enterprise poplatky Azure Marketplace. Poznámka: všechny nákupy marketplace a spotřeby je faktura mimo peněžní potvrzení je faktura čtvrtletní a zpětně. Zákazníci EA muset zaplatit přímo na poskytovatele služeb třetích stran a můžete to udělat povolením způsob platby, například platební kartu s účtem EA.

Nepřímý.odkaz zákazníci EA najdete na stránce **Správa předplatného** portálu předplatné Azure Marketplace, ale ceny skrytý. Zákazníci kontaktování jejich VRSTVY informace o marketplace poplatky.

Správci zápisu EA Azure dá ovládat přístup k webu Azure Marketplace služeb třetí strany. Budou zakázat nebo znovu povolit přístup 3 nákup stran z obchodu v předplatná a Správa účtů v podnikovém portálu v části účty.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Kdo se dá spojit pro otázky o faktuře služby ClearDB v EA předplatného?

Kontaktujte [Zákaznickou podporu organizace](http://aka.ms/AzureEntSupport) s ohledem na fakturace v části jejich EA zápisu. Tým podpory portál EA zodpovědět nebo váš problém vyřešit.

 



## <a name="more-information"></a>Další informace

[Nejčastější dotazy týkající se Azure Marketplace](/marketplace/faq/)
