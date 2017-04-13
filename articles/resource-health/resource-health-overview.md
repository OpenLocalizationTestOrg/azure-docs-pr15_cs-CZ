<properties
   pageTitle="Azure přehled stavu zdrojů | Microsoft Azure"
   description="Základní informace o stavu Azure zdroje"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# <a name="azure-resource-health-overview"></a>Azure stavu Přehled zdrojů

Azure stavu zdroje je služba, která poskytuje stav jednotlivých Azure zdrojů a poskytuje akce pokyny k řešení potíží. V prostředí cloudu, kde není možné přímo přístup k serverům nebo infrastruktury prvky cílem pro stav zdroje je snížit čas, který zákazníkům utratit za Poradce při potížích, zejména snížit čas strávený zjištění, pokud kořenovém problém obsahuje v aplikaci nebo pokud je způsobená události uvnitř platformu Azure.

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-the-resource-is-healthy-or-not"></a>Co je považován za zdroj a jak funguje zdroje stavu rozhoduje o zdroje při správný nebo ne? 
Kdy je zdroj uživatel vytvořil instanci typu zdroje poskytovanou do služby, třeba: virtuální počítač, do webových aplikací nebo v databázi SQL. 

Stav zdroje závisí na signály, které zdroje a/nebo službu zjistit, zda je zdroj správný nebo ne. Je důležité si všimněte si, že právě stavu pouze zdrojům pro stav jeden určitého zdroje zadejte a není zvažte další prvky, které přispívají k celkový stav. Třeba při vytváření sestav, že stav virtuálního počítače pouze požadovaná část výpočetním infrastruktury je považován za, tedy problémy v síti se nezobrazí ve zdravotnictví zdrojů, pokud není výpadku deklarované služby v takovém případě ho bude být vytažená pomocí panelu v horní části zásuvné. Další informace o výpadku služby nabízených dál v tomto článku. 

## <a name="how-is-resource-health-different-from-service-health-dashboard"></a>Čím se liší od řídicí panel stavu služeb zdrojů stavu?

Informace seřazené podle stavu zdroje je podrobnějších než co poskytuje společnost řídicí panel stavu služeb. Během SHD informuje uživatele o události, které mají vliv stavu služby v oblasti, stav zdroje poskytuje informace týkající se určitého zdroje, například ho zpřístupnit události, které mají vliv dostupnosti virtuálního počítače, webové aplikace nebo databáze SQL. Například pokud uzel neočekávaně restartování se zákazníky, jehož virtuálních počítačích spustili na uzel budou moct získat důvod, proč jejich OM byl k dispozici pro určitého časového období.   

## <a name="how-to-access-resource-health"></a>Jak získat přístup ke stavu zdrojů
Dostupné prostřednictvím zdroje stavu služeb způsoby 2 pro přístup k prostředku stavu.

### <a name="azure-portal"></a>Azure portálu
Stav zásuvné zdroje na portálu Azure obsahuje podrobné informace o stavu zdroje i doporučené kroky, kterými se liší v závislosti na aktuální stav zdroje. Tento zásuvné poskytuje nejlepší možnosti při dotazování zdroje stavu usnadňuje přístup k jiným zdrojům do portálu. Jak jsme zmínili před, se liší podle aktuálního stavu sadu doporučené akcí v zásuvné stavu zdrojů:

* Správný zdroje: protože zjistil žádný problém, které by mohly ovlivnit stavu zdroje akce se zaměřuje na pomáhá proces řešení potíží. Například poskytuje přímý přístup k zásuvné Poradce při potížích, který nabízí pokyny k řešení nejběžnější nominální zákazníci problémy.
* Nefunkční zdroje: K řešení problémů způsobená Azure, zobrazí zásuvné akce Microsoft trvá (nebo přijal) obnovit daný zdroj. Problémy s způsobené uživatele iniciovanému akce, bude zásuvné možných seznam zákazníků akce tak řešení tohoto problému a obnovit daný zdroj.  

Jakmile jste přihlášení k portálu Azure, přístup k zásuvné stavu zdrojů dvěma způsoby: 

###<a name="open-the-resource-blade"></a>Otevřete zásuvné zdroje
Otevřete zásuvné zdroje pro daný zdroj. Na zásuvné nastavení, která se otevře vedle zásuvné zdrojů klikněte na stav zdroje otevřete zásuvné stavu zdrojů. 

![Zásuvné stavu zdrojů](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### <a name="help-and-support-blade"></a>Nápověda a podpora zásuvné
Kliknutím na otazník v pravém horním rohu následným výběrem nápovědy + podpory otevřete zásuvné Nápověda a podpora. 

**Na horním navigačním panelu**

![Nápověda k + podpory](./media/resource-health-overview/HelpAndSupport.png)

Kliknutí na dlaždici otevře zásuvné předplatné stavu zdroje, které zobrazí seznam všech zdrojů v předplatného. Vedle každého zdroje je ikona označující jeho stavu. Kliknutím na jednotlivé zdroje se otevře zásuvné stavu zdrojů.

**Dlaždice stavu zdrojů**

![Dlaždice stavu zdrojů](./media/resource-health-overview/resourceHealthTile.png)

## <a name="what-does-my-resource-health-status-mean"></a>Co znamená stav zdroje stavu?
Existuje 4 různých stavu stavy, které se můžou objevit pro zdroj.

### <a name="available"></a>K dispozici
Služba nerozpoznal problémy platformě, který by mohl ovlivňovat dostupnost zdroje. To je označen ikonou zelenou značku zaškrtnutí. 

![Je zdroj dostupný](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Není k dispozici

V tomto případě službu zjistil probíhající problém v platformu ovlivňující ochranu dostupnost zdroje, například na uzel místo, kam OM spuštěná neočekávaně restartování. To je označen ikonou červené upozornění. Další informace o tomto problému je k dispozici v prostřední části zásuvné, včetně: 

1.  Jaké akce Microsoft trvá obnovit daný zdroj 
2.  Podrobné časovou osu s problému, včetně času očekávané řešení
3.  Seznam doporučené akce pro uživatele 

![Zdroj není k dispozici](./media/resource-health-overview/Unavailable.png)

### <a name="unavailable--customer-initiated"></a>Není k dispozici – zákazníka, které iniciuje
Zdroje není dostupný kvůli požadavek zákazníka ATP zastavením zdroj vyžaduje restartování počítače. To je označen ikonou modré informační. 

![Zdroj není k dispozici kvůli uživatele iniciovanému akce](./media/resource-health-overview/userInitiated.png)

### <a name="unknown"></a>Neznámý
Služba neobdržel informace o tyto materiály pro víc než 5 minut. To je označen ikonou otazník šedá. 

Je důležité mít na paměti, že se nejedná konečné označení, že není něco s zdroje, aby zákazníci měli těmito doporučeními řídit:

* Pokud zdroje běží podle očekávání, ale jeho stavu v nastavený na hodnotu Neznámý stav zdroje, existují bez problémů a očekáváte stav zdroje k aktualizaci správný po několika minutách.
* Pokud jsou problémy s přístupem ke zdroji a jeho stav v nastavený na hodnotu Neznámý stav zdroje, může být včasné může to být z problému a další vyšetřování by měl provést až po aktualizaci stavu správný nebo chybná

![Zdroje stav neznámý](./media/resource-health-overview/unknown.png)

## <a name="service-impacting-events"></a>Události ovlivňující ochranu služby
Pokud zdroje možná ovlivněny událost probíhající ovlivňující ochranu služby, zobrazí se v horní části zásuvné stavu zdrojů v pruhu. Kliknutím na hlavičky se otevře zásuvné auditovat události, která se zobrazí další informace o výpadku.

![Stav zdroje mohou mít vliv SIE](./media/resource-health-overview/serviceImpactingEvent.png)

## <a name="what-else-do-i-need-to-know-about-resource-health"></a>Co dalšího je potřeba vědět o stavu zdrojů?

### <a name="signal-latency"></a>Latence signál
Signály, které kanálu stavu zdrojů, může být až 15 minut zpoždění, které mohou způsobit nesrovnalosti mezi aktuálním stavu zdroje a jeho skutečné dostupnosti. Je důležité mějte na paměti při vám pomůže odstranit nepotřebné čas strávený vyšetřování možné problémy. 

### <a name="special-case-for-sql"></a>Zvláštní případ pro SQL 
Stav zdroje sestavy stavu databáze SQL, nikoli serveru SQL server. Během přechodu tento postup obsahuje běžného stav obrázku, vyžaduje, aby víc součástí a služeb vzít v úvahu stav databáze. Aktuální signálu závisí na přihlášení k databázi, což znamená, že u databází, které zobrazí běžná přihlášení (zahrnující mimo jiné, která přijímá žádosti o spuštění dotazu) stavu stav se pravidelně zobrazí. Pokud databázi nepoužíval po dobu zabere nejméně 10 minut, bude se přesune do Neznámý stav. Není to znamená, že databáze je k dispozici, pouze signál má byla vyzařovaného, protože byly provedeny bez přihlášení. Připojení k databázi a spuštění dotazu bude posílat signály potřebné k určení a aktualizovat stav databáze.

## <a name="feedback"></a>Zpětné vazby
Jsme vždy otevřít pro názory a návrhy! Pošlete nám prosím [návrhů](https://feedback.azure.com/forums/266794-support-feedback). Můžete navíc zapojit s abychom prostřednictvím [Twitter](https://twitter.com/azuresupport) nebo na [fóra MSDN](https://social.msdn.microsoft.com/Forums/azure).
