<properties
    pageTitle="Práce s spolehlivé kolekce | Microsoft Azure"
    description="Přečtěte si doporučené postupy pro práci s spolehlivé kolekcí."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Práce s spolehlivé kolekce

Služba struktury nabízí stavové programovací model k dispozici pro vývojáře .NET prostřednictvím spolehlivé kolekcí. Konkrétně služby struktury poskytuje spolehlivé slovník a spolehlivé fronty třídy. Pokud použijete tyto třídy, váš stav je rozdělen (pro škálovatelnost) replikovat (dostupnosti) a zpracováván jako transakce v oddílu (pro kyseliny sémantiku). Pojďme podívejte se na typické použití objektu spolehlivé slovník a zjistit, jaké jeho skutečně vedou.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Všechny operace spolehlivé slovníku objektů (s výjimkou ClearAsync, který není vrátit), je nutné ITransaction objekt. Tento objekt obsahuje přidružená některou a všechny změny, které se pokoušíte vytvořit spolehlivé slovníku a/nebo spolehlivé fronta objektů v rámci jeden oddíl. Získání ITransaction objekt tak, že zavoláte oddíl je na StateManager CreateTransaction metody.
 
Ve výše uvedeného kódu ITransaction objekt je spolehlivé slovníku AddAsync metodě předán. Interně slovníku metody akceptované klíč trvat zámek pro čtečky/zápis spojené s klávesou. Pokud metodu upraví hodnoty klíče, metodu převezme klávesu zámek zapsat a pokud metodu bude číst jenom z hodnoty klíče, přečtěte si zámek je bere na klávesu. Protože AddAsync mění na klávesu hodnotu k hodnotě nové, předaný, se považuje uzamčení pro zápis klíče. Ano Pokud 2 (nebo více) podprocesů pokusí k přidání hodnot se stejným klíčem ve stejnou dobu, jeden podproces získá uzamčení pro zápis a dalších podprocesů zablokuje. Ve výchozím nastavení metod blok pro získání zámku; až 4 sekund Po 4 sekundy vyvolat metody TimeoutException. Přetížení metody, které umožňuje předat explicitní limitu, pokud chcete raději.
 
Obvykle zadejte kód reagovat TimeoutException tak, že ho zachycení a opakování celou operaci (viz výše uvedeného kódu). V mém jednoduchý kód jenom telefonické Task.Delay předávání 100 milisekund pokaždé, když. Ale ve skutečnosti, může být lepší místo toho použít určitý druh exponenciální zpoždění zpět vypnout.
 
Jakmile zámek získat, AddAsync přidá klíče a hodnoty odkazů na objekty do interního dočasné slovníku přidružená k objektu ITransaction. Důvodem je k poskytování čtení e vlastní zápisy sémantiku. To znamená po telefonickém hovoru s AddAsync, pozdější volání TryGetValueAsync (pomocí stejného objektu ITransaction) vrátí hodnotu i v případě nebyly dosud potvrzení transakce. Potom AddAsync jsou kód a hodnota pole bajtů pro objekty a připojí těchto polí bajt do souboru protokolu v místním uzlu. Nakonec AddAsync pošle pole bajtů vedlejší ostatními tak mají stejné informace klíč/hodnota. Přestože do souboru protokolu byla napsal klíč/hodnota údaje, informace není považovány za část slovník, dokud transakce, která jsou přidružena byla. 

Ve výše uvedeného kódu potvrdí volání CommitAsync všechny operace transakce. Konkrétně připojí Potvrdit informace do souboru protokolu v místním uzlu a také odešle potvrdit záznamu vedlejší ostatními. Jakmile hlasování (většinou) replice odpověděl, všechny změny dat jsou považovány trvalý a všechny zámků webů přidružených zkratky, které byly pracovat pomocí objektu ITransaction uvolnění tak, aby ostatní podprocesy/transakce můžete pracovat s stejné klíče a jejich hodnoty.

Pokud CommitAsync není volali už někdy (obvykle kvůli výjimce vyvolání), dostane uvolnit ITransaction objektu. Při odstraňování objektu nepotvrzené ITransaction, struktury služby připojí přerušení informace do souboru protokolu místní uzel a nic musí žádné sekundární replice posílat. A pak všechny zámků webů přidružených zkratky, které byly pracovat prostřednictvím transakce jsou.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Běžné nástrahy a jak se vyhnout je
Teď chápete fungování spolehlivé kolekce interně Podívejme se na některé běžné nevhodnému použití je. Viz následující kód:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Když pracujete s pravidelné .NET slovník, můžete přidat/hodnotu klíče do slovníku a změňte hodnotu vlastnosti (například LastLogin). Tento kód však nebude fungovat správně spolehlivé slovníku. Mějte na paměti z předchozích diskuse volání AddAsync jsou klíč/hodnota objekty, které chcete pole bajtů a pak uloží polích místní soubor a taky jim poslala sekundární replikám. Pokud později změnit vlastnosti, tím se změní vlastnosti na hodnotu v paměti. To nemá vliv na místní soubor nebo data odeslaná replikám. Pokud dojde k chybě procesu, co je v paměti vyvolá se pryč. Při spuštění nové obrázku nebo jiného otevřené změní primární, bude původní vlastnost hodnota co je k dispozici. 

Můžu nelze zdůrazňují dostatečně jak snadno se dají používat druh chybu nahoře. A pouze naučíte chybu případě, že proces havaruje. Správný způsob zadejte kód, který je jednoduše Chcete-li převrátit dva řádky:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Tady máme jiný příklad zobrazující běžné chybě:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Znovu slovníků .NET běžná kódu výše funguje a se řídí určitým vzorem běžné: vývojář používá klíč pro vyhledání hodnoty. Pokud existuje hodnota vývojář změní hodnotu na vlastnosti. Však s spolehlivé kolekce tento kód projevuje stejné problém co už popisované: __objektu nesmí změnit po tom, co udělíte spolehlivé kolekce.__
 
Aktualizovat hodnoty v kolekci spolehlivé správný způsob je získat odkaz ke stávající hodnotě a zvažte objekt uvedený tak, že tento odkaz neměnný. Vytvořte nový objekt, který je přesnou kopii původního objektu. Teď můžete změnit stav tento nový objekt a napsat nový objekt do kolekce tak, aby se serializovat do pole bajtů připojen k místnímu souboru a odeslané replikám. Po potvrzení změny, objekty v paměti, místní soubor a ostatními mít stejnou přesný stav. Stačí dobré!

Následující kód ukazuje správný způsob aktualizovat hodnoty v kolekci spolehlivé:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Definování neměnný datových typů nechcete, aby programmer chyby

Rádi ideálním případě kompilátoru zprávy o chybách při omylem plodiny kód, který mutuje stav objekt, který máte k zamyšlení neměnný. Ale kompilátoru C# nemá tuto možnost. Ano, abyste se vyhnuli možné programmer chyby, důrazně doporučujeme definovat typy pomocí spolehlivé kolekce je neměnný typů. Konkrétně znamená to, jestli chcete na základní typy hodnot (například čísla [Int32, UInt64 atd.] data a času, Guid, časový interval a podobně). A samozřejmě můžete použít také řetězec. Doporučujeme vyhnout vlastnosti kolekce jako serializaci a rekonstrukci je možné často můžete vyhledávání nepříznivě ovlivnit výkon. Ale pokud chcete použít vlastnosti kolekce, důrazně doporučujeme použití. Na čistého neměnný kolekce knihovna (System.Collections.Immutable). Tato knihovna není k dispozici ke stažení http://nuget.org. Doporučujeme také uzavírání vašich tříd a provádění pole jen pro čtení, kdykoli je to možné.

Typ informací o uživateli ukazuje, jak definovat neměnný typ využijete výše uvedené doporučení.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

Typ ItemId je také neměnný typ znázorněná na obrázku:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Správa schématu verzí (při upgradu)

Spolehlivé kolekce interně serializovat objekty pomocí. DataContractSerializer čistého společnosti. Sériové objekty zachován primární otevřené místní disk a přenášejí sekundární replikám. Během služby existence, je pravděpodobné, že budete chtít změnit typ dat (schéma) vyžaduje službu. Správa verzí dat velmi pečlivě musí přístup. Především musíte vždy mít rekonstruovat stará data. Konkrétně, znamená to, že kód deserializace musí být nekonečně zpětně kompatibilní: verze 333 kód služby, musíte mít k ovládání dat umístěný v kolekci spolehlivé verze 1 kód služby 5 lety.

Kód služby je navíc upgradovaném jednu doménu upgrade po druhém. Ano během upgradu, máte dvě verze služby kódu spuštění současně. Můžete třeba nemuseli nová verze služby kód použít nové schéma starší verze kód služby nebudete moci zpracovat nové schéma. Pokud je to možné, měli byste navrhnout jednotlivých verzích služby je kompatibilní směrem vpřed 1 verzí. Konkrétně znamená to, že V1 kód služby by měla jednoduše ignorovat schématu prvků, které není výslovně zpracovat. Musí být však možné uložit všechna data, která ji poslal, neví explicitně o a jednoduše zápisu zpátky se při aktualizaci slovníku klíče nebo hodnoty. 

>[AZURE.WARNING] Při změně schématu klíče, musíte se ujistit, váš klíč hash kód a algoritmy rovná se stabilní. Pokud změníte způsob jednu z těchto algoritmů pracovat, nebude moct vyhledat klávesu ve slovníku spolehlivé někdy znovu.
  
Můžete taky můžete provádět, co se obvykle nazývá upgrade fázi 2. 2 – fáze upgradu upgradujete služby z V1 V2: V2 obsahuje kód, který věděli, jak zacházet s nové změny schématu, ale není spustit tento kód. Až kód V2 načte V1 data, pracuje na něm a zapisuje V1 data. Potom po upgradu dokončení přes všechny upgradu domény se můžete nějak signál spuštěné instance V2 dokončení upgradu. (Jedním ze způsobů signálu je-li přejít na upgrade konfigurace; to je co dělá toto upgrade fázi 2.) Teď instance V2 můžete číst V1 data převést na V2 dat, pracovat a zapsání jako V2 data. Přečtení další instance V2 dat, není nutné převádět ho, budou jenom pracovat a zapsání V2 data.

## <a name="next-steps"></a>Další kroky
Další informace o vytváření smluv předávání kompatibilní dat, najdete v článku [Smlouvy dat kompatibilní s prohlížečem předat dál](https://msdn.microsoft.com/library/ms731083.aspx).

Informace o doporučených postupech týkajících se správy verzí dat smlouvy, najdete v článku [Správa verzí smlouvy Data](https://msdn.microsoft.com/library/ms731138.aspx). 

Zjistěte, jak implementovat verze chybám dat smlouvy, najdete v článku [Verze chybám serializace zpětná](https://msdn.microsoft.com/library/ms733734.aspx). 

Zjistěte, jak k zajištění struktury dat, který můžete spolupracovat mezi různými verzemi, najdete v článku [objekt IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
