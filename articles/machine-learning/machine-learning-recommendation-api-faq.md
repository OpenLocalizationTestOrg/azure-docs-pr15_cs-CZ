<properties 
    pageTitle="Nastavení a používání rozhraní API doporučení výukové počítači | Microsoft Azure" 
    description="Rozhraní API doporučení Microsoft vytvořené pomocí Azure počítače Learning – nejčastější dotazy" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/> 

#<a name="setting-up-and-using-machine-learning-recommendations-api-faq"></a>Nastavení a použití počítače výukové doporučení rozhraní API – nejčastější dotazy


**Co je doporučení?**

>[AZURE.NOTE]Měli byste začít používat službu kognitivní rozhraní API doporučení místo tato verze. Kognitivní službu doporučení se nahradí tuto službu a s novými funkcemi bude vyvinuté tam. Má nové funkce jako dávky podpory lepší rozhraní API aplikace Explorer, opět dostanete čistší prostředí povrchový, jednotnější registrace/fakturace rozhraní API, atd.
> Další informace o [migraci do nové kognitivní služby](http://aka.ms/recomigrate)

Organizace a firem, které jsou závislé na doporučení pro více zákazník a nahoru prodej produktů a služeb svým zákazníkům doporučení Azure počítač přečíst poskytuje modul Samoobslužné doporučení. Je implementaci pro spolupráci filtrování, který používá factorization matice jako algoritmus core. Vývojáři můžou získat přístup ke doporučení rozhraní REST API. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**Co mám dělat s doporučení?**

DOPORUČENÍ bere jako vstup položky nebo několika položek a vrátí seznam příslušná doporučení. Příklad: zákazníka online obchodě klikne produktu. Online prodejce, od odešle tento výrobek předávat na vstupu doporučení, naopak obdrží seznam produktů a rozhoduje o které z těchto produktů způsob jejich zobrazení zákazníkovi. Je vhodné Doporučení optimalizovat online úložiště a používat i informovat vaší vnitřní prodejů oddělení nebo volání centra.

**Je nějaké omezení použití?**

Doporučení platí následující omezení použití:
* Maximální počet modely jedno předplatné: 10
* Maximální počet položek, které můžou obsahovat katalogu: 100,000
* Maximální počet bodů použití, které jsou k dispozici je 5 000 000 ~. Nejstarší se odstraní Pokud nové bude nahráli nebo vykázaného.
* Maximální velikost dat, můžete odeslat v e-mailu (například import katalogu dat, import o využití) je 200 MB
* Počet transakce za sekundu (TPS) sestavení modelu doporučení, která není aktivní je TPS ~ 2. Sestavení modelu doporučení, která je aktivní může obsahovat maximálně 20 TPS.

##<a name="purchase-and-billing"></a>Nákup a fakturace 


**Kolik stojí doporučení období spuštění?**

Doporučení je služba na základě předplatného. Nabíjení vychází z hlasitosti transakce za měsíc. Můžete zkontrolovat [stránce nabídka] (https://datamarket.azure.com/dataset/amla/recommendations) na webu Microsoft Azure Marketplace ceny informace.

**Jsou všechny náklady spojené s s doporučení sledovat a uložit činnosti uživatelů pro mě?**

Není v okamžiku.

**Má doporučení bezplatnou zkušební verzi?**

Je bezplatná záznam, který je omezen na 10 000 transakce za měsíc.

**Kdy bude se vám nebudou účtovat poplatky doporučení?**

Na placené předplatné je jakéhokoli předplatného, u kterých se měsíční poplatek. Při nákupu na placené předplatné, vám bude účtovaná bezprostředně za použití za první měsíc. Bude účtovaná částku, která je přidružená k nabídky na stránce předplatné (plus příslušné daně). Tento měsíční poplatek je nastavená jako každý měsíc na stejný den kalendáře jako původní nákup až do zrušení předplatného. 

**Jak upgradovat na vyšší úroveň služby**

Kupte nebo aktualizovat předplatné z [stránce nabídka] (https://datamarket.azure.com/dataset/amla/recommendations) stránky na webu Microsoft Azure Marketplace.

Při upgradu předplatného:

* Nepřičítají se transakce, které jsou zbývající vašeho starého předplatného do nového předplatného. 
* Můžete zaplatit plnou cenu nové předplatné, i když máte nepoužitý transakce vašeho starého předplatného.

Proces upgradu předplatného:

* Nevigate [stránku nabídky] (https://datamarket.azure.com/dataset/amla/recommendations).
* Pokud ještě nejste přihlášení, přihlaste se na Tržiště.
* Všechny dostupné plány jsou uvedené v pravém podokně. Klikněte na přepínač pro plán, na který chcete upgradovat.
* Pokud chcete upgradovat, klikněte na **OK**. Pokud nechcete upgradovat, klikněte na **Zrušit**.

**Důležité** Důkladně si přečtěte dialogovým oknem před upgradem protože neexistují fakturace a použití důsledky.

**Když skončí Moje předplatné doporučení?**

Vaše předplatné skončí, když ji zrušit. Pokud chcete zrušit předplatné, postupujte podle následujících pokynů.

**Jak zruším předplatné doporučení?**

Zrušení předplatného, pomocí následujících kroků. Je-li aktuálního předplatného na placené předplatné, předplatného budou dál problémy v podstatě až do konce aktuální fakturační období. V případě potřeby zrušení okamžitě efektivní kontaktujte na [Podporu společnosti Microsoft](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

**Poznámka:** Náhrada je uveden, pokud předplatné zrušíte před skončením fakturační období nebo pro nepoužívané transakce v fakturační období.

* Přejděte na [stránce nabídka] (https://datamarket.azure.com/dataset/amla/recommendations).
* Pokud ještě nejste přihlášení, přihlaste se na Tržiště.
* Klepnutím na tlačítko **Zrušit** vpravo od pole Název sady dat a stav. Můžete použít toto předplatné až do konce aktuální fakturační období nebo vaše transakce je limitu (nastane dříve).

Pokud chcete zrušit předplatné okamžitě tak zakoupení nového předplatného, souborů lístek na [Podporu společnosti Microsoft](https://support.microsoft.com/oas/default.aspx?gprid=17024&st=1&wfxredirect=1&sd=gn).

##<a name="getting-started-with-recommendations"></a>Začínáme s doporučení

**Doporučení je pro mě?** 

Doporučení ve počítače výukové trvá organizace a firmy, které jsou závislé na doporučení pro více zákazník a nahoru prodej produktů nebo služeb svým zákazníkům. Pokud máte web vystavený zákazník, prodejců, vnitřní prodejců nebo Centrum odborné pomoci, a pokud nabízejí katalogu více než několik desítek produktů nebo služeb dolní řádek můžou využívat pomocí doporučení. 

Vyzkoušení doporučení je navržen velmi jednoduché. Aktuální verze na základě rozhraní API vyžaduje základní programovací dovedností. Pokud potřebujete pomoc, kontaktujte dodavatele, který vyvinuté váš web. Pokud máte interní IT oddělení nebo podnikových vývojář, třeba moct získat doporučení, aby pracoval za vás. 

**Jaké jsou požadavky pro nastavení doporučení?**

Doporučení vyžaduje protokol možností uživatele se týká katalogu. Pokud nepoužíváte t mít tyto protokol a máte vystaveného Web zákazníka, doporučení získáte činnosti uživatelů můžete. 

Doporučení také vyžaduje katalogu produktů nebo služeb. Pokud nepoužíváte t mít katalogu, doporučení pomocí skutečné zákazníka použití dat a generovat katalogu. Katalog předpokládanou nezahrnuje položky, které nebyly hlášené jako součást transakce uživatele.

**Jak nastavit doporučení pro první?**

Po změně [přihlášení k odběru] (https://datamarket.azure.com/dataset/amla/recommendations) doporučení, používejte dokumentaci k rozhraní API v [Azure počítače výukové doporučení – úvodní příručka](machine-learning-recommendation-api-quick-start-guide.md) k nastavení služby.

**Kde najdu své si přečtěte následující dokumentaci rozhraní API?** 

Dokumentace rozhraní API je [Azure počítače výukové doporučení – úvodní příručka](machine-learning-recommendation-api-quick-start-guide.md).

**Jaké možnosti máte k nahrání katalogu a použití dat na doporučení?**

Máte dvě možnosti pro odeslání katalogu a použití data: můžete exportovat data z systému CRM nebo jiných protokoly a ho nahrajte do doporučení nebo značky můžete přidat na web, který bude sledovat aktivity uživatelů. Pokud používáte druhá metoda, data uložena v Azure.

##<a name="maintenance-and-support"></a>Údržby a podpory.

**Jak velké Moje uvedenou množinu dat lze?**

Každou sadu dat může obsahovat maximálně 100 000 položek katalogu a až 2048 MB použití zásad správy informací.
Přihlášení k odběru navíc může obsahovat maximálně 10 datových sad (modely).

**Kde můžu získat technické podpory pro doporučení?**

Technická podpora je dostupná na webu [Microsoft Azure podpory](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) .

**Kde najdu své podmínky použití?**

[Výukové doporučení rozhraní API podmínky poskytování služby Microsoft Azure počítače](https://datamarket.azure.com/dataset/amla/recommendations#terms).



 
