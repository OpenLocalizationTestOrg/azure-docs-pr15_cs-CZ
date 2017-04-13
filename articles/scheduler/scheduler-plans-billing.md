<properties
 pageTitle="Plány a fakturace v Azure Plánovač"
 description="Plány a fakturace v Azure Plánovač"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Plány a fakturace v Azure Plánovač

## <a name="job-collection-plans"></a>Plány kolekce projektu

Úlohy kolekce jsou fakturaci entitu v Azure plánovač. Úlohy kolekce obsahují několik úloh a použít tři plány – Free standardní a Premium – popsaných níže.

|**Kolekce plánu projektu**|**Maximální počet práce na projektu kolekci**|**Maximální počet opakování**|**Max úlohy kolekce jedno předplatné**|**Omezení**|
|:---|:---|:---|:---|:---|
|**Uvolnit**|5 práce na projektu kolekci|Jednou za hodinu. Nelze provést úlohy dochvilnější spoje než jednou za hodinu|Přihlášení k odběru smí až 1 kolekce volného projektu|Nelze použít [HTTP odchozí autorizaci objektu](scheduler-outbound-authentication.md)
|**Standardní**|50 práce na projektu kolekci|Jednou za minutu. Nelze provést úlohy dochvilnější spoje než jednou za minutu|Přihlášení k odběru smí až 100 kolekce standardní projektu|Přístup k úplnou sadu funkcí plánovače|
|**P10 Premium**|50 práce na projektu kolekci|Jednou za minutu. Nelze provést úlohy dochvilnější spoje než jednou za minutu|Přihlášení k odběru jsou povolené až 10 000 kolekcí úlohy P10 Premium. <a href="mailto:wapteams@microsoft.com">Kontaktujte nás</a> informace.|Přístup k úplnou sadu funkcí plánovače|
|**P20 Premium**|1 000 práce na projektu kolekci|Jednou za minutu. Nelze provést úlohy dochvilnější spoje než jednou za minutu|Přihlášení k odběru jsou povolené až 10 000 kolekcí úlohy P20 Premium. <a href="mailto:wapteams@microsoft.com">Kontaktujte nás</a> informace.|Přístup k úplnou sadu funkcí plánovače|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Aktualizace a dírám kolekce plánů projektu

Můžete upgradovat nebo omezit úlohy kolekce plán kdykoli mezi plány uvolnit, Standard a Premium. Však při přechodu do kolekce bezplatné úlohy přechodu se nemusí podařit, pro jednu z těchto důvodů:

- Bezplatné úlohy kolekce již existuje v předplatného
- Úlohy v kolekci úlohy má vyšší opakování než povoleno pro projekty v kolekcích volného projektu. Maximální opakování povolené v kolekci bezplatné úlohy se jednou za hodinu
- Existuje více než 5 úlohy v kolekci projektu
- Úlohy v kolekci úlohy má HTTP nebo HTTPS akci, která používá [HTTP odchozí autorizaci objektu](scheduler-outbound-authentication.md)

## <a name="billing-and-azure-plans"></a>Fakturace a Azure plány jednotného zasílání zpráv

Předplatná nejsou Účtovaná zdarma kolekce projektu. Pokud máte víc než 100 standardní úlohy kolekce (10 standardní fakturační jednotky), je lepší koupi mít všechny kolekce úlohy v plánu premium.

Pokud jste ještě jedna kolekce standardní úlohy a jednu kolekci úlohy premium, jsou fakturované jedné standardní fakturační jednotky _a_ jeden premium fakturační jednotky. Faktury Plánovač služeb na základě počtu kolekce aktivní úlohy, které se nastavují na standardní nebo premium; To je vysvětleno dále do dalších dvou oddílů.

## <a name="standard-billable-units"></a>Standardní fakturaci jednotky

Standardní fakturaci jednotka může obsahovat maximálně 10 kolekce standardní projektu. Protože kolekce standardní projektu můžete mít až 50 práce na projektu kolekci, jednu jednotku standardní fakturační umožňuje předplatné mít až 500 úlohy – až spuštění úlohy téměř 22 milionů za měsíc.

Pokud máte mezi 1 a 10 kolekce standardní úlohy, fakturovat pro 1 standardní fakturační jednotku. Pokud máte mezi 11 a 20 kolekcí standardní úlohy, fakturovat pro 2 standardní fakturační jednotky. Pokud máte mezi 21 a 30 standardní úlohy kolekcí fakturovat pro 3 standardní fakturační jednotky a tak dále.

## <a name="p10-premium-billable-units"></a>P10 Premium fakturaci jednotky

P10 fakturaci jednotku premium může obsahovat až 10 000 kolekcí úlohy premium P10. Protože kolekce P10 premium projektu můžete mít až 50 práce na projektu kolekci, jednu jednotku fakturační premium umožňuje předplatné mají maximálně 500 000 úlohy – až spuštění úlohy téměř 22 miliardy za měsíc.

Pokud máte mezi 1 až 10 000 kolekcí úlohy premium fakturovat pro 1 P10 premium fakturační jednotku. Pokud máte 10,001 až 20 000 premium úlohy kolekce fakturovat pro 2 P10 premium fakturační jednotky a tak dále.

P10 premium úlohy kolekce tak, mají stejné funkce jako standardní úlohy kolekce ale možnost Konec cena v případě, že aplikace vyžaduje mnoho kolekcí úlohy.

## <a name="p20-premium-billable-units"></a>P20 Premium fakturaci jednotky

P20 fakturaci jednotku premium může obsahovat až 5 000 kolekcí úlohy premium P20. Protože kolekce P20 premium projektu můžete mít až 1 000 práce na projektu kolekci, jednu jednotku fakturační premium umožňuje předplatné mít až 5 000 000 úlohy – až spuštění úlohy téměř 220 miliard za měsíc.

P20 premium úlohy kolekce poskytuje stejné možnosti jako P10 premium úlohy kolekce ale taky podporuje větší číslo úlohy na kolekci úlohy a větší celkového počtu úlohy celkové než P10 premium umožňuje mít víc škálovatelnost.

## <a name="billing-and-active-status"></a>Fakturace a aktivního stavu

Úlohy kolekce jsou vždy aktivní, pokud předplatné celý projevil iniciativu do některé dočasné zakázaném stavu kvůli problémům s fakturací. Jediný způsob, jak zajistit, že není fakturované kolekce projektu je buď nastavte ho _zdarma_ plánu nebo odstranění kolekce úlohy.

I když zakážete všechny úlohy v rámci kolekce úlohy v jedné operace, se nezmění fakturační stavu úlohy kolekce – kolekci úlohy se _pořád_ vám nebudou účtovat poplatky. Podobně kolekce prázdných úlohy jsou považované za aktivní a bude vám nebudou účtovat poplatky.

## <a name="pricing"></a>Ceny

Ceny podrobnosti, najdete v článku [Plánovač ceny](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Viz taky


 [Co je Plánovač?](scheduler-intro.md)

 [Azure Plánovač koncepty, terminologie a hierarchie entit](scheduler-concepts-terms.md)

 [Začínáme s používáním Plánovač na portálu Azure](scheduler-get-started-portal.md)

 [Azure odkaz Plánovač REST API](https://msdn.microsoft.com/library/mt629143)

 [Azure odkaz rutiny prostředí PowerShell Plánovač](scheduler-powershell-reference.md)

 [Azure vysoké dostupnosti Plánovač a spolehlivosti](scheduler-high-availability-reliability.md)

 [Azure Plánovač omezení, výchozí hodnoty a kódy chyb](scheduler-limits-defaults-errors.md)

 [Azure ověřování odchozích připojení Plánovač](scheduler-outbound-authentication.md)
