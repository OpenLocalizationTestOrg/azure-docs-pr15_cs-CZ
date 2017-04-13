<properties
   pageTitle="Spolehlivé kolekce | Microsoft Azure"
   description="Stavová služby služby struktury poskytují spolehlivé kolekce, které vám umožní psát vysoce dostupné scalable a nízkou latencí cloudu aplikace."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Úvod k spolehlivé kolekce stavové služby struktury služby Azure

Spolehlivé kolekce umožňují psát důrazně k dispozici, scalable a nízkou latencí cloudu aplikace, jako kdyby se vytvářejí jeden počítač aplikace. Třídy v oboru **Microsoft.ServiceFabric.Data.Collections** poskytují sadu kolekce mimo pole, které automaticky zpřístupnit váš stav vysoce. Vývojáři muset program jenom pro spolehlivé rozhraní API kolekce a zpřístupněte spolehlivé kolekce Správa replikovanou a místní stavu.

Klíčové rozdíl mezi spolehlivé kolekce a ostatní vysoká dostupnost technologiích (například Redis služby Azure tabulka a služba Azure fronty) je, že stav bude k dispozici místně v instanci služby při taky poskytovány vysoce. To znamená, že:

- Všechny operace čtení jsou místní, jehož výsledkem zhoršeným latence a vysoký výkon přečte.
- Všechny zápisy vzniknou minimální počet sítě IOs, jehož výsledkem je zhoršeným latence a vysoký výkon zapíše.

![Obrázek vývoj kolekcí.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Spolehlivé kolekce můžete považovat za natural vývoj třídy **System.Collections** : s novou sadou kolekce, které jsou určeny pro aplikace cloudu a více počítače bez zvyšuje složitost pro vývojáře. Spolehlivé kolekce jako takové jsou:

- Replikovat: Jsou vysoké dostupnosti replikovat změny stavu.
- Trvalé: Data uložena na disk odolnost proti rozsáhlé výpadků (například výpadku power datacentra).
- Asynchronní: Rozhraní API jsou asynchronní zajistit, že podprocesy se nebudou blokovat při by vstupů/výstupů.
- Transakční: Rozhraní API využít odběru transakce, můžete snadno spravovat více spolehlivé kolekce v rámci služby.

Spolehlivé kolekce poskytují silných konzistence záruky mimo pole před uskutečněním důvody o stavu aplikace snadnější.
Směrový konzistence zadáváním zajištění transakce potvrzení dokončení až po celé transakce po přihlášení hlasování většinou replik, včetně primární.
Dosáhnout méně bezpečné konzistence aplikace můžete potvrdit, že jsem zpět žadateli klienta/před vrátí asynchronní potvrzení.

Spolehlivé API kolekce jsou vývojem kolekcí souběžné rozhraní API (nachází v oboru **System.Collections.Concurrent** ):

- Asynchronní: Vrátí hodnotu úkolu, protože na rozdíl od souběžné kolekce operace replikovat a zachován.
- Ne, parametry: používá `ConditionalValue<T>` se vrátíte logická hodnota a hodnotu Ne výstupní parametry. `ConditionalValue<T>`je třeba `Nullable<T>` ale nevyžaduje T být struktura.
- Transakce: Pomocí objektu transakce umožňují uživatelům skupině Akce na víc spolehlivé kolekcí v transakci.

V současné době **Microsoft.ServiceFabric.Data.Collections** obsahuje dvě kolekce:

- [Spolehlivé slovníku](https://msdn.microsoft.com/library/azure/dn971511.aspx): představuje kolekci replikovanou transakční a asynchronní párů klíč/hodnota. Podobně jako **ConcurrentDictionary**klávesu a hodnotu může být libovolného typu.
- [Spolehlivé fronty](https://msdn.microsoft.com/library/azure/dn971527.aspx): představuje replikovanou transakční a asynchronní striktně první se změnami, použity fronty. Podobně jako **ConcurrentQueue**hodnota může být libovolného typu.

## <a name="isolation-levels"></a>Úrovně izolace
Úroveň izolace definující míru, do které musí být transakce izolovaný od změny, které udělali ostatní transakce.
Existují dvě úrovně izolace, které podporuje spolehlivé kolekce:

- **Opakující pro čtení**: Určuje, že příkazy nedokáže číst data, která byla změnili, ale ještě nejste potvrzeného tak, že ostatní transakce a že žádné transakce můžete změnit data, která četl aktuální transakce až do dokončení aktuální transakce. Další informace najdete v tématu [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
- **Snímek**: Určuje, že data jen pro čtení všech následný výběr v transakce budou využití transakce konzistentní verzi data, která byla na začátku transakce.
Transakce lze rozpoznat pouze změny dat, které byly potvrzeného před začátkem transakce.
Úpravy dat, které udělali ostatní transakce po začátku aktuální transakce nejsou viditelné pro příkazy spouštění v aktuální transakci.
Výsledek je, jako kdybyste příkazy v transakce získat snímek potvrzené dat tak, jak byly na začátku transakce.
Snímky jsou konzistentní v kolekcích spolehlivé.
Další informace najdete v tématu [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Spolehlivé kolekce automaticky zvolte požadovanou úroveň izolace můžete u dané operace čtení v závislosti na operaci a roli otevřené při vytváření transakce.
Následuje tabulka zobrazující výchozí hodnoty úrovně izolace pro spolehlivé slovník a fronty operace.

| Operace \ rolí      | Primární          | Sekundární        |
| --------------------- | :--------------- | :--------------- |
| Jedinou osobou pro čtení    | Opakující pro čtení  | Snímek         |
| Výčet \ počet   | Snímek         | Snímek         |

>[AZURE.NOTE] Společná pro operace typu Single Entity příklady `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.

Spolehlivé slovník a spolehlivé fronty domovské stránce podpory pro čtení vaše data zapisuje.
Jinými slovy všechny zápisu v rámci transakce uvidí následující pro čtení, které patří stejné transakci.

## <a name="locking"></a>Uzamčení
V části spolehlivé kolekce jsou všechny transakce uspořádáním dvou: transakce neuvolní zámky získal dokud transakce končí přerušení nebo potvrzení.

Spolehlivé slovníku používá úroveň řádku zamknutí pro všechny operace jedna entita.
Spolehlivé fronta ztrátách souběžné striktně transakční FIFO vlastnosti.
Spolehlivé fronty používá operace úrovně uzamčení povolení jedna transakce se `TryPeekAsync` a/nebo `TryDequeueAsync` a jedna transakce se `EnqueueAsync` najednou.
Poznámka: Chcete-li zachovat FIFO, pokud `TryPeekAsync` nebo `TryDequeueAsync` někdy sleduje spolehlivé fronta je prázdná, bude i uzamknout `EnqueueAsync`.

Zápis operace jsou vždy získána výhradním zámků webů.
Pro operace čtení uzamčení závisí na několika faktorech.
Všechny operace čtení udělat pomocí izolace snímku je zámek bezplatné.
Všechny operace opakující pro čtení ve výchozím nastavení, která bude uzamčení sdílené.
Který podporuje opakující číst další operace, však uživatele můžete požádat o zámkem aktualizace místo sdílené zámek.
Aktualizace lock je asymetrické lock lze zabránit běžnou zablokování, který bude proveden poté více transakce uzamknout zdroje potenciální aktualizace později.

Matice lock kompatibility najdete níže:

| Žádost o \ udělena | Žádná         | Sdílené       | Aktualizace      | Exkluzivní    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| Sdílené            | Ke konfliktu  | Ke konfliktu  | Konflikt    | Konflikt     |
| Aktualizace            | Ke konfliktu  | Ke konfliktu  | Konflikt    | Konflikt     |
| Exkluzivní         | Ke konfliktu  | Konflikt     | Konflikt    | Konflikt     |

Všimněte si, že argument časového limitu v spolehlivé rozhraní API kolekce se používá pro zjišťování zablokování.
Například se snažíte číst a aktualizovat K1 dvě transakce (T1 a T2).
Je možné, je zablokování, protože obě skončily s sdílené zámek.
V tomto případě jednu nebo obě operace vyprší platnost.

Všimněte si, že výše scénář zablokování skvělé příklady jak můžete zablokování zámkem aktualizace.

## <a name="persistence-model"></a>Trvalé modelu
Spolehlivé správci stavu a spolehlivé kolekcí podle trvalé modelu, který se nazývá protokolu a kontrolní bod.
Toto je modelu, kde je každé změně stavu přihlášení k lyncu na disku a použít pouze v paměti.
Dokončení sám stát je zachován jen zřídka (také Kontrolní bod).
Výhody je, že rozdílů jsou povolené do sekvenční vlastností pouze přidat zápisy na disku pro zvýšení výkonu.

Pochopíte lépe modelu protokolu a kontrolní bod, nejdřív Podívejme se na scénář nekonečné disku.
Spolehlivé správci stavu protokoly všechny operace předtím, než je replikovat.
Díky spolehlivé kolekce použít pouze operaci v paměti.
Od trvalý protokoly, i když otevřené selhává a potom se musí restartovat, spolehlivé správce stavu obsahuje dost informací v jeho protokoly k přehrání všechny operace, které se ztratí otevřené.
Je nekonečné disku záznamů protokolu nikdy nebudete potřebovat má být odebrán a spolehlivé kolekce musí ke správě pouze stavu nedostatku paměti.

Teď Pojďme se podívat na scénář konečných disku.
Jak záznamů protokolu nahromadit, bude spolehlivé správci stavu docházet místa na disku.
Předtím, než k tomu dojde, spolehlivé správci stavu potřebuje zkrátit jeho protokolu uvolníte místo pro nová záznamy.
Bude vyžadovat spolehlivé kolekce pro kontrolu stavu nedostatku paměti na disk.
Je odpovědností spolehlivé kolekce uchovávat stavu až tomuto bodu.
Po spolehlivé kolekce dokončit jejich kontroly, spolehlivé správci stavu oříznout protokol abyste uvolnili místo na disku.
Tímto způsobem, pokud otevřené se musí restartovat, spolehlivé kolekce obnoví stavu kontrolní a spolehlivé správce stavu obnovit a přehrávání všechny změny stavu, ke kterým došlo od kontrolu.

>[AZURE.NOTE] Přidání jinou hodnotu z kontrola je, že zlepšuje výkon obnovení běžné případy.
Je to proto kontroly obsahovat jenom nejnovější verze.

## <a name="recommendations"></a>Doporučení

- Neměňte objektu vlastní typ vrácené operace čtení (například `TryPeekAsync` nebo `TryGetValueAsync`). Spolehlivé kolekce, stejně jako souběžné kolekce vrátit odkaz na objekty a ne kopie.
- Proveďte hloubkové kopírovat vráceného objektu vlastní typ před jeho úpravou. Protože struktury a typy předdefinované průchodu podle hodnoty, není potřeba udělat kopii hloubkové na nich.
- Nepoužívejte `TimeSpan.MaxValue` pro časové limity relací. Zjišťování zablokování bude použito časové limity relací.
- Nepoužívejte transakce po byla potvrzeného, přerušena nebo odstraněny.
- Nepoužívejte výčet mimo, který byl vytvořený v rozsahu transakce.
- Nevytvářejte transakce v rámci jiné transakce `using` údajů vzhledem k tomu může způsobit zablokování.
- Zkontrolujte, že je vaše `IComparable<TKey>` implementaci je správně. Systém převezme závislost to pro sloučení kontrolních bodů.
- Používejte zámkem aktualizace čtení položky s záměr ji nechcete, aby určitou druhu zablokování aktualizovat.
- Zvažte použití zálohování a obnovení funkce mít havárie obnovení.
- Vyhněte se společným operace jedna entita a více entity operace (například `GetCountAsync`, `CreateEnumerableAsync`) v jedné transakce kvůli úrovně izolace různých.

Tady jsou některé věci, které mějte na paměti:

- Výchozí časový limit je 4 sekundy pro všechny spolehlivé kolekce rozhraní API. Většina uživatelů neměli přepsat.
- Zrušení token výchozí `CancellationToken.None` ve všech spolehlivé rozhraní API kolekcí.
- Typ klíče parametr (*TKey*) spolehlivé slovníku musí správně implementovat `GetHashCode()` a `Equals()`. Klávesy musí být neměnný.
- Dosáhnout dostupnost pro spolehlivé kolekce by měl mít každé služby alespoň cílové a minimální otevřené nastavte velikost 3.
- Operace čtení na sekundární může číst, které nejsou kvora potvrzeného verze.
To znamená, že verze data, která čte z jednoho sekundární může NEPRAVDA zvýšily.
Čtení primární jsou samozřejmě vždy stabilní: můžete nikdy být NEPRAVDA zvýšily.

## <a name="next-steps"></a>Další kroky

- [Spolehlivé Představení aplikace služby](service-fabric-reliable-services-quick-start.md)
- [Práce s spolehlivé kolekce](service-fabric-work-with-reliable-collections.md)
- [Spolehlivé oznámení služby](service-fabric-reliable-services-notifications.md)
- [Spolehlivé služby zálohování a obnovení (havárie obnovení)](service-fabric-reliable-services-backup-restore.md)
- [Správce konfigurace spolehlivý stavu](service-fabric-reliable-services-configuration.md)
- [Začínáme se službou rozhraní API webových služeb struktury](service-fabric-reliable-services-communication-webapi.md)
- [Rozšířené možnosti použití spolehlivé služby programování modelu](service-fabric-reliable-services-advanced-usage.md)
- [Referenční informace pro vývojáře spolehlivé Collections](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
