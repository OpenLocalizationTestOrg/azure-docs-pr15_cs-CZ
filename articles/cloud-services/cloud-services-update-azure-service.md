<properties
pageTitle="Jak aktualizovat do cloudové služby | Microsoft Azure"
description="Naučte se aktualizovat cloudovým službám v Azure. Zjistěte, jak pokračuje aktualizaci na do cloudové služby zajištění dostupnosti."
services="cloud-services"
documentationCenter=""
authors="Thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>Jak aktualizovat do cloudové služby

## <a name="overview"></a>Základní informace
Na 10 000 nohy aktualizace do cloudové služby, včetně jejího rolí a hosta OS, je zahrnuje tři kroky. Nejdřív musíte ho nahrát binární soubory a soubory konfigurace pro nové cloudové služby, nebo operační systém verze. Pak Azure si vyhrazuje výpočetním a síťové zdroje pro cloudové služby, na základě požadavků nové verze služby cloudu. Nakonec Azure provede inovace postupně aktualizovat klienta na novou verzi nebo hosta OS při zachování své dostupnosti. Tento článek popisuje podrobnosti Tento poslední krok – postupné inovace.

## <a name="update-an-azure-service"></a>Aktualizace služby Azure

Azure uspořádává role instance do logické seskupení s názvem upgradu domény (UD). Upgrade domény (UD) jsou logické sady instancí role, které se automaticky aktualizují, jako skupinu.  Azure aktualizaci cloudové služby jeden UD najednou, které umožňuje instance v jiných UDs pokračujte podávání přenosy.

Výchozí počet upgradu domény je 5. Rozdílný počet upgradu domény můžete určit zahrnutím atributu upgradeDomainCount v souboru definice služby (.csdef). Další informace o atribut upgradeDomainCount najdete v článku [WebRole schéma](https://msdn.microsoft.com/library/azure/gg557553.aspx) nebo [WorkerRole schéma](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Je-li provést aktualizaci na místě jeden či více rolí ve službě Azure aktualizovat sady instancí role podle upgradu domény, ke kterému patří. Azure aktualizace, které všechny instance danou upgradu doménu – zastavení, aktualizace, shromáždí zpět online – přejde na další domény. Tak, že zastavení pouze instance spuštěna v aktuální upgradu domény, Azure zajišťuje, že aktualizace se vyskytuje nejmenší působivé službu pracovního. Další informace najdete v tématu [jak aktualizace pokračuje](#howanupgradeproceeds) dál v tomto článku.

> [AZURE.NOTE] I když podmínky, **Aktualizovat** a **upgrade** významem mírně odlišnou v kontextu Azure, můžete být používají jako synonyma pro procesy a popisy funkcí v tomto dokumentu.

Služby musíte definovat nejméně dvě instance role pro tuto roli mají být aktualizovány v místě bez výpadek služeb. Pokud službu obsahuje pouze jednu instanci jednu roli, služby nebude k dispozici, dokud místních aktualizace proběhla.

Toto téma popisuje následující informace o Azure aktualizace:

-   [Povolené služby změn během aktualizace](#AllowedChanges)
-   [Jak upgrade probíhá](#howanupgradeproceeds)
-   [Návrat k aktualizaci](#RollbackofanUpdate)
-   [Zahájení více mutating operací na probíhající nasazení](#multiplemutatingoperations)
-   [Rozdělení rolí v upgradu doménách](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>Povolené služby změn během aktualizace
Následující tabulka zobrazuje povolené změny do služby při aktualizaci:

|Změny povoleny hostitelské služby a rolí|Aktualizace na místě|Fázovanou (VIP odkládací)|Odstranění a opětovném nasazení|
|---|---|---|---|
|Verze operačního systému|Ano|Ano|Ano
|Úroveň zabezpečení .NET|Ano|Ano|Ano|
|Virtuální počítač velikosti<sup>1</sup>|Ano,<sup>2</sup>|Ano|Ano|
|Místní uložení nastavení|Zvětšení jenom<sup>2</sup>|Ano|Ano|
|Přidání nebo odebrání role ve službě|Ano|Ano|Ano|
|Počet výskytů konkrétní rolí|Ano|Ano|Ano|
|Počet nebo typ koncové body služby|Ano,<sup>2</sup>|Ne|Ano|
|Názvy a hodnoty nastavení|Ano|Ano|Ano|
|Hodnoty (ale ne názvy) nastavení konfigurace|Ano|Ano|Ano|
|Přidání nových certifikátů|Ano|Ano|Ano|
|Změna existující certifikátů|Ano|Ano|Ano|
|Nasazení nového kódu|Ano|Ano|Ano|
<sup>1</sup> Změna velikosti omezený na podmnožinu velikosti k dispozici ke cloudové služby.

<sup>2</sup> Vyžaduje 1,5 SDK Azure nebo novější verze.

> [AZURE.WARNING] Změna velikosti virtuální počítač bude destroy místní data.


Při aktualizaci nejsou podporovány následující položky:

-   Změna názvu roli. Odebrat a potom přiřadit roli novým názvem.
-   Změna počtu Upgrade domény.
-   Zmenšení velikosti místních zdrojů.

Pokud vytváříte jiné aktualizace definice vaše služby, třeba zmenšení velikosti místních zdrojů, je nutné provést aktualizaci vyměňovat VIP. Další informace najdete v tématu [Zaměnit nasazení](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Jak upgrade probíhá
Můžete se rozhodnout, jestli chcete aktualizovat všechny role ve službě nebo jedné role ve službě. V obou případech všechny výskyty každou roli, která probíhá upgrade a patří do prvního upgradu domény jsou přerušili upgradu a pracovat v režimu online. Jakmile se dostanete do online režimu, instancí v druhém upgradu domény jsou přerušili upgradu a pracovat v režimu online. Cloudová služba může obsahovat maximálně jeden upgrade aktivní najednou. Upgrade probíhá vždy proti nejnovější verzi cloudovou službu.

Následující obrázek znázorňuje, jak upgrade probíhá Pokud upgradujete všechny role ve službě:

![Upgrade služeb] (media/cloud-services-update-azure-service/IC345879.png "Upgrade služeb")

Tento další diagram znázorňuje, jak aktualizovat vytvářejí Pokud upgradujete pouze jedné role:

![Upgrade rolí] (media/cloud-services-update-azure-service/IC345880.png "Upgrade rolí")  

> [AZURE.NOTE] Při upgradu službu z jedné instance na několika instancí služby bude uveden do během upgradu se provádí kvůli služby Azure inovace způsobem. Dostupnost termínu služby služby smlouva o úrovni platí jenom pro služby, které jsou nasazeny s více než jedna instance. Následující seznam popisuje, jak data na každé jednotce ovlivněny jednotlivé scénáře upgradu Azure služby:
>
>Restartujte počítač OM:
>
-   C: zachovají
-   D: zachovají
-   E: zachovají
>
>Restartujte počítač portálu:
>
-   C: zachovají
-   D: zachovají
-   E: odstraněna
>
>Portálu Reimage:
>
-   C: zachovají
-   D: odstraněna
-   E: odstraněna

>Místní Upgrade:
>
-   C: zachována
-   D: zachovají
-   E: odstraněna
>
>Migrace uzel:
>
-   C: odstraněna
-   D: odstraněna
-   E: odstraněna

>Poznámka:, že ve výše uvedeném seznamu jednotku E: představuje role kořenové jednotce a nesmí být pevně. Místo toho znázornit proměnná prostředí % RoleRoot % jednotku.

>Minimalizovat prostoje při upgradu jednou instancí služby, nasazení novou službu více instancí pracovní server a proveďte vyměňovat VIP.

Při automatické aktualizace řadiče struktury Azure pravidelně vyhodnotí stavu cloudovou službu zjistit, kdy je bezpečnější provede další UD. Tento stav hodnocení probíhá na základě podle rolí a byly použity pouze instance v rámci nejnovější verze) (tedy instance z UDs, které již byly šel). Ověří, že minimální počet instancí role pro každou roli získaly vyhovující terminálu stavu.

### <a name="role-instance-start-timeout"></a>Role Instance Start vypršení časového limitu
Správce struktury počká 30 minut pro každou roli instanci kontaktovat stavu spuštěno. Doba trvání vypršení časového limitu uplyne, struktury řadiče zůstanou procházení k nejbližšímu role.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>Návrat k aktualizaci
Azure poskytuje flexibilitu při řešení služby během aktualizace umožňují zahájit další operace službu, po přijetí žádosti o počáteční aktualizaci struktury správcem Azure. Vrácení zpět lze provádět pouze při aktualizaci (Změna konfigurace) nebo upgrade je ve stavu **probíhajícího** na nasazení. Aktualizace nebo upgrade se považuje v průběhu, dokud je aspoň jednou instancí služby, které ještě byl aktualizován nebyl na novou verzi. K ověření, zda je povolen vrácení zpět, zkontrolujte hodnoty příznaku RollbackAllowed vrácenou operace [Získat nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) a [Získat vlastnosti cloudové služby](https://msdn.microsoft.com/library/azure/ee460806.aspx) , je nastavený na hodnotu true.

> [AZURE.NOTE] Pouze smysl zavolat vrácení zpět na aktualizaci **místní** nebo upgradovat, protože VIP vyměňovat upgrady zahrnují nahrazení jednoho celý instanci služby s jiným.

Vrácení změn v průběhu aktualizace má následující důsledky na nasazení:

-   Všechny instance role, které vám to dosud nebyly aktualizováno nebo upgradu na novou verzi nejsou aktualizovat nebo upgradovat, protože tato instance už verzi cílového služby.
-   Všechny instance role, které vám to již byly aktualizovány nebo upgradu na novou verzi balíčku služby (\*.cspkg) souboru nebo konfiguraci služby (\*.cscfg) souboru (nebo oba soubory) budou vráceny před upgradem verzi tyto soubory.

To funkčně je k dispozici následující funkce:

-   [Vrácení aktualizovat Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) operaci, která lze volat na aktualizaci konfigurace (aktivovány volání [Konfigurace nasazení změnu](https://msdn.microsoft.com/library/azure/ee460809.aspx)) nebo upgrade (aktivovány, volající [Upgrade nasazení](https://msdn.microsoft.com/library/azure/ee460793.aspx)), dokud je aspoň jednou instancí služby, kterou nebyla aktualizována na novou verzi.
-   Element uzamčeno a RollbackAllowed element, které jsou vráceny jako součást obsah odpovědí operací [Získat nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) a [Získat vlastnosti cloudové služby](https://msdn.microsoft.com/library/azure/ee460806.aspx) :
    1.  Uzamknout element umožňuje zjistit, kdy operace mutating můžete uplatnit v dané nasazení.
    2.  RollbackAllowed element můžete zjistit, kdy operaci [Vrácení aktualizace nebo Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) můžete volat v dané nasazení.

    K provedení vrácení zpět není nutné kontrola uzamčeno a RollbackAllowed prvky. Stačí k potvrzení, že RollbackAllowed je nastavený na hodnotu true. Tyto prvky jsou vrácena pouze v případě těchto postupů jsou vyvolat pomocí hlavičky nastavena na "x-ms verze: 2011-10-01" nebo jeho novější verzí. Další informace o záhlaví Správa verzí najdete v článku [Správa služby správy verzí](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Pokud odvolání transakce aktualizace jsou někdy nebo není podporovaný upgrade, jedná se o následujícím způsobem:

-   Snížení místních zdrojů – Pokud aktualizace zvyšuje místních zdrojů pro roli Azure platformu neumožňuje vrácení zpět. 
-   Omezení kvóty – Pokud aktualizace proběhla měřítkem dolů operaci, kterou už můžete mít dostatečná kvóty pro využití dokončit vrácení zpět. Každý Azure předplatné má kvóty, která určuje maximální počet jádra, které můžete využívané všechny hostovanou služby, které patří k této předplatného. Pokud vrácení dané aktualizaci provádění přepnutím předplatného přes kvóty pak, nepovolí se vrácení zpět.
-   Podmínky závodní – pokud počáteční aktualizaci dokončí, vrácení zpět není možné.

Je například ze kdy vrácení aktualizace může být užitečné, pokud používáte operaci [Upgrade nasazení](https://msdn.microsoft.com/library/azure/ee460793.aspx) v režimu ručního na ovládací prvek, který je chránění sazba niž hlavní místní upgrade na vaše Azure hostované služby.

Při zavedení upgrade volání [Upgrade nasazení](https://msdn.microsoft.com/library/azure/ee460793.aspx) ruční režimu a začněte provede upgradu domény. Pokud nastane okamžik, při sledování upgradu, je Poznámka: některé instance role v první upgradu domény, které můžete prozkoumat mít přestane reagovat, můžete volat operaci [Vrácení aktualizace nebo upgradovat](https://msdn.microsoft.com/library/azure/hh403977.aspx) na nasazení ponechá beze změny instance, které nebyly dosud nebyly upgradované a instance vrácení zpět, které vám to nebyly upgradované na předchozí balíčku služby a konfiguraci.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Zahájení více mutating operací na probíhající nasazení
V některých případech je vhodné zahájení více souběžné mutating operací na probíhající nasazení. Například může provést aktualizaci service a během tuto aktualizaci je právě chránění přes služby, které mají být některé změny, například k natáčení aktualizace zpět, použít jinou aktualizaci nebo dokonce odstranit nasazení. Případ, ve kterém to může být potřeba je-li upgradu služeb obsahuje buggy kód, který způsobuje instanci upgradované rolí opakovaně zhroucení. V tomto případě řadiče struktury Azure nebude možné provést průběh při používání upgradovat, protože jsou správný nedostatečná počet instancí v upgradovaném domény. Tento stav se nazývá *zasekne nasazení*. Unstick nasazení tak, že vracení aktualizace nebo jej opatříte svěže aktualizace v horní části neúspěšný jednu.

Po přijetí původní žádost k aktualizaci nebo upgrade službu správcem struktury Azure vyjít další operace mutace. To znamená nemáte čekání na dokončení před zahájením jiné mutating operace počáteční operace.

Zahájit druhého operace aktualizace při prvním aktualizace probíhá provede podobný vrácení operace. Pokud druhá je v automatickém režimu, první upgradu doménu upgradují se hned, případně vést k instance z několika upgradu domén výpadku na stejném místě v čase.

Jsou mutating operace takto: [Konfigurace nasazení změnit](https://msdn.microsoft.com/library/azure/ee460809.aspx) [Upgrade nasazení](https://msdn.microsoft.com/library/azure/ee460793.aspx), [Stav nasazení aktualizace](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Odstranění nasazení](https://msdn.microsoft.com/library/azure/ee460815.aspx)a [Vrácení aktualizace nebo upgradovat](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Dvě operace, [Získat nasazení](https://msdn.microsoft.com/library/azure/ee460804.aspx) a [Získat vlastnosti cloudové služby](https://msdn.microsoft.com/library/azure/ee460806.aspx), vraťte se uzamčeno příznak, který můžete zkoumat a zjistit, zda operace mutating můžete uplatnit v dané nasazení.

Zavolat verzi z těchto postupů, která vrací příznak uzamknout, je nutné nastavit záhlaví požadavku na "x-ms verze: 2011-10-01" nebo pozdější. Další informace o záhlaví Správa verzí najdete v článku [Správa služby správy verzí](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Rozdělení rolí v upgradu doménách
Azure rozdělí výskyty role rovnoměrně řadou nastavení upgradu domény, které můžete konfigurovat jako součást soubor definice (.csdef) služby. Maximální počet upgradu domény je 20 a výchozí hodnota je 5. Další informace o tom, jak upravovat soubor definice služby najdete v článku [Definice schématu služby Azure (.csdef soubor)](cloud-services-model-and-package.md#csdef).

Pokud má vaše role deseti případy, ve výchozím nastavení každé upgradu domény jsou například dvě instance. Pokud má vaše role 14 instance, pak čtyři upgradu domén obsahovat tři instance a pátý doménu obsahuje dva.

Upgrade domény jsou označeny od nuly index: první upgradu doména má ID 0 a druhá upgradu doména má ID 1 a tak dál.

Následující obrázek znázorňuje rozdělení službu než obsahuje dva role při službu definuje dvě upgradu domény. Se službou osm výskyty roli web a devět instancí pracovního role.

![Rozdělení upgradu domén] (media/cloud-services-update-azure-service/IC345533.png "Rozdělení upgradu domén")

> [AZURE.NOTE] Všimněte si, že ovládací prvky Azure přidělování instance přes upgradu domény. Je možné určit, které instance přiřazených k které domény.

## <a name="next-steps"></a>Další kroky
[Jak spravovat Cloudovým službám](cloud-services-how-to-manage.md)  
[Jak sledovat Cloudovým službám](cloud-services-how-to-monitor.md)  
[Postup při konfiguraci Cloudovým službám](cloud-services-how-to-configure.md)  
