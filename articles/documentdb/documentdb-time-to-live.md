<properties
   pageTitle="Platnost dat v DocumentDB s Hodnota time to live | Microsoft Azure"
   description="S TTL Microsoft Azure DocumentDB umožňuje mít dokumenty automaticky odstraněné ze systému po určité době."
   services="documentdb"
   documentationCenter=""
   keywords="Hodnota Time to live"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Platnost dat v kolekcích DocumentDB automaticky se hodnota time to live

Aplikace můžete vytvářet a ukládat velké objemy dat. Některé z těchto dat, jako je počítač generované události dat, protokoly a uživatele relace informace slouží jenom pro omezené časové období. Jakmile se data změní přebytečné požadavků aplikace není bezpečný vymazat data a zmenšení potřeb úložiště aplikace.

"Time to live" nebo TTL Microsoft Azure DocumentDB umožňuje mít dokumenty automaticky odstraněné z databáze po určité době. Výchozí time to live můžete nastavit na úrovni kolekce a v jednotlivých dokumentů přepsat. Když TTL je nastavená jako výchozí kolekce nebo na úrovni dokumentu DocumentDB automaticky odstraní dokumenty, které jsou po tuto dobu v sekundách, protože se naposledy změnily.

Hodnota Time to live v DocumentDB používá posun proti při dokument naposledy změnil. Můžete to udělat používá _ts pole, která existuje na každý dokument. Pole _ts je časové razítko epocha formátu UNIX představujícím datum a čas. Pole _ts se aktualizuje každé změně dokumentu. 

## <a name="ttl-behavior"></a>Hodnota TTL chování

Funkce TTL řídí TTL vlastnosti na dvou úrovních - úrovni kolekce a úrovni dokumentů. Hodnoty se nastavují v sekundách a je považováno za rozdíl od _ts, který byl dokument naposledy změněno.

 1.  DefaultTTL kolekce
  * Pokud chybí (nebo nastavit na hodnotu null), se automaticky neodstraní dokumenty.
  
  * Je-li prezentace a hodnotu "-1" = nekonečné – dokumenty nevyprší ve výchozím nastavení
  
  * Pokud prezentace a hodnota je nějaké číslo ("n") – dokumenty platnost "n" sekund za poslední změny

 2.  Hodnota TTL pro dokumenty: 
  * Vlastnost je platný jenom v případě, že je k dispozici pro kolekci nadřazené DefaultTTL.
  
  * Zruší DefaultTTL hodnotu nadřazené kolekci.

Hned po vypršení platnosti dokumentu (ttl + _ts > = aktuální čas serveru), že je dokument označený jako ", kterým vypršela platnost". Žádná operace bude povoleno na tyto dokumenty po tuto dobu a jejich vyloučit z výsledků všechny dotazy provádí. Dokumenty se odstraní fyzicky systému a budou odstraněny na pozadí napříklade později. Všechny [Žádosti o jednotky (RUs)](documentdb-request-units.md) z kolekce rozpočtu nebudou spotřebovávat.

Podle výše uvedeného postupu můžete vidět v následující tabulce:

|       | DefaultTTL chybí/není nastavena na kolekci | DefaultTTL = -1 na kolekce | DefaultTTL = "n" v kolekci|
| ------------- |:-------------|:-------------|:-------------|
| Hodnota TTL chybějící u dokumentu| Nic přepsat na úrovni dokumentu od dokumentu a kolekce mají žádné koncepci TTL. | V této kolekci nejsou žádné dokumenty vyprší. | U dokumentů v této kolekci vyprší po uplynutí interval n. |
| Hodnota TTL = -1 na dokumentu | Nic přepsat na úrovni dokumentů od kolekci nemá definovat DefaultTTL vlastnost, která mohou přepsat dokumentu. Hodnota TTL v dokumentu je rušení interpretovaný systém. | V této kolekci nejsou žádné dokumenty vyprší. | Dokument s TTL =-1 v této kolekci Neomezená platnost vyprší. Všechny dokumenty vyprší po intervalu "n". |
|  Hodnota TTL = n v dokumentu | Nic přepsat na úrovni dokumentů. Hodnota TTL dokumentu ve zrušením interpretovaný systém. | Dokument s TTL = n vyprší po n interval v sekundách. Další dokumenty dědí interval-1 a neomezené platnosti. | Dokument s TTL = n skončí za interval n v sekundách. Další dokumenty zdědí "n" interval z kolekce. |


## <a name="configuring-ttl"></a>Konfigurace TTL

Ve výchozím nastavení je ve výchozím nastavení ve všech kolekcích DocumentDB a u všech dokumentů vypnutá hodnota time to live.

## <a name="enabling-ttl"></a>Povolení TTL

Povolit TTL na kolekci nebo dokumenty v rámci kolekce, budete muset nastavit vlastnost DefaultTTL kolekce -1 nebo kladné číslo nenulového. Nastavení DefaultTTL-1 znamená, výchozí všechny dokumenty v kolekci bude live trvale, ale DocumentDB služby měli sledovat tuto kolekci pro dokumenty, které mají přepsat tuto výchozí.

## <a name="configuring-default-ttl-on-a-collection"></a>Konfigurace výchozí TTL kolekce

Je možné konfigurovat výchozí čas s dynamickými na úrovni kolekce. 

U kolekce nastavit hodnotu TTL, budete muset zadat nenulového kladné číslo určující období v sekundách platnost všechny dokumenty v dokumentu (_ts) kolekci po poslední úpravy.

Nebo můžete nastavit jako výchozí, aby -1, která předpokládá, že bude donekonečna udržovat live všechny dokumenty vložené v kolekci ve výchozím nastavení.

## <a name="setting-ttl-on-a-document"></a>Nastavení TTL v dokumentu

Kromě nastavení výchozí TTL kolekce můžete nastavit konkrétní TTL na úrovni dokumentu. Tím se přepsat výchozí nastavení kolekce.

Pokud chcete nastavit hodnotu TTL dokumentu, musíte zadat nenulového kladné číslo označující období v sekundách platnosti dokumentu za poslední úpravy dokumentu (_ts).

Pokud chcete nastavit tento posun vypršení platnosti, nastavení pole TTL v dokumentu.

Pokud má dokument bez pole TTL, se použije výchozí kolekci.

Pokud je hodnota TTL zakázané na úrovni, budou dokud hodnota TTL není povoleno znovu v kolekci ignorovat pole TTL v dokumentu.


## <a name="extending-ttl-on-an-existing-document"></a>Rozšíření TTL na existujícího dokumentu

Hodnota TTL dokumentu můžete obnovit provedením všechny operace zápisu na tomto dokumentu. Tím nastaví _ts aktuální čas a čas zbývající do dokumentu vypršení nastavená tak, že hodnota ttl, začne znovu.

Pokud se chcete změna ttl dokumentu, stejně jako můžete u jiných nastavitelné pole můžete aktualizovat pole.


## <a name="removing-ttl-from-a-document"></a>Odebrání TTL z dokumentu

Pokud byl nastaven TTL dokumentu a už nechcete, aby tento dokument platnosti, pak můžete získat dokument, odeberte pole TTL a nahradit dokument na serveru.

Pole TTL odebraný z dokumentu, použije se výchozí nastavení kolekce.

Zastavit dokumentu ze kdy vám platnost vyprší a není odvozeno z kolekce musíte nastavit hodnotu TTL -1.


## <a name="disabling-ttl"></a>Zakázání TTL

Zakažte TTL úplně v kolekci a ukončení procesu pozadí hledáte vypršela platnost dokumenty Vlastnost DefaultTTL v kolekci by měl odeberou.

Odstranění tato vlastnost se liší od nastavení na -1. Nastavení-1 znamená nové dokumenty přidané do kolekce bude live trvale, ale je možné přepsat na konkrétní dokumentům v kolekci.

Odebrání tato vlastnost úplně kolekci znamená, že, že nejsou žádné dokumenty vyprší, i když jsou dokumenty, které mají explicitně přepsat původní výchozí.


## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY

**Co bude TTL stát Já?**

Neexistuje žádná další náklady nastavení TTL dokumentu.

**Jak dlouho trvá odstranit dokument po hodnota TTL nahoru?**

Dokumenty označené jako nedostupná hned po vypršení platnosti dokumentu (ttl + _ts > = aktuální čas serveru). Žádná operace bude povoleno na tyto dokumenty po tuto dobu a jejich vyloučit z výsledků všechny dotazy provádí. Dokumenty odstraňují fyzicky systém na pozadí. To nebudete používat všechny RUs z rozpočtu v kolekci.

**Pokud trvá časový úsek odstranit dokumenty, budou spočítá směrem k Moje kvóty (a faktury), dokud jejich odstranění?**

Ne, po vypršení platnosti dokumentu se nebude vám nebudou účtovat poplatky pro ukládání dokumentů a velikosti dokumentů neprojeví směrem k kvóty úložiště pro kolekci.

**Bude mít TTL dokumentu žádný vliv na RU náklady?**

Ne, bude žádný vliv na RU poplatky za operací na všechny dokumenty v rámci DocumentDB.

**Bude odstranění dokumenty dopad na výkon, které můžu zřízení na můj kolekce?**

Ne, podávání žádosti o proti kolekci bude získat přednost před proces pozadí odstranit dokumentů. Přidání TTL pro všechny dokumenty nebudou mít vliv na to.

**Když skončí platnost dokumentu, jak dlouho ho zůstanou v mé kolekce dokud se neodstraní?**

Hned po vypršení platnosti dokumentu už bude přístupných osobám s postižením. Přesný čas dokumentu zůstane ve vaší kolekci skutečně odstraněné je determinizaci a budou založeny na při procesu pozadí je možné odstranit dokument.

**Vypršela platnost dokumenty odstraní se na všech uzlech, nebo je "postupně consistent?"**

Dokument nebude k dispozici ve stejnou dobu na všech uzlech a ve všech oblastech.

**Existuje RU náklady pro sledování TTL úlohy na pozadí?**

Ne, je zdarma RU při to.

**Jak často se TTL vypršení zaškrtnuté políčko?**

Kontrola TTL platnosti se nestane jako pozadí obrázku. Při odpovídání na žádost o službu back-end postupujte vložené kontroly a vyloučit všechny dokumenty, které vypršela platnost. Odstranění skutečný dokument je pouze proces, který asynchronní běží na pozadí. Četnost tento proces je určen k dispozici RUs v kolekci.

**Funkce TTL pouze platí pro všechny dokumenty, nebo můžete platnost jednotlivé dokumentu nemovitostí s hodnotou?**

Hodnota TTL platí pro celý dokument. Pokud chcete konec platnosti jenom část dokumentu, pak se doporučuje extrahování část z hlavního dokumentu v samostatný "propojené" dokument a potom použijte hodnotu TTL na extrahované dokumentu.

**Má funkce TTL zvláštní indexování požadavky?**

Ano. V kolekci může mít [indexování zásad nastavit](documentdb-indexing-policies.md) opožděné nebo konzistentní. Nastavování DefaultTTL na kolekce s indexování nastavit na stav žádná budou výsledkem je chyba, stejně jako pokusíte vypnout indexování v kolekci obsahující DefaultTTL už nastavení.


## <a name="next-steps"></a>Další kroky

Další informace o Azure DocumentDB, odkaz na stránku služby [*si přečtěte následující dokumentaci*](https://azure.microsoft.com/documentation/services/documentdb/) .




