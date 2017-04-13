<properties
   pageTitle="Spolehlivé oznámení služby | Microsoft Azure"
   description="Koncepční si přečtěte následující dokumentaci pro oznámení služby spolehlivé struktury"
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
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Spolehlivé oznámení služby

Oznámení klientům umožnit, aby sledovat změny provedené na objekt, který se vás zajímá. Dva typy objektů podporuje oznámení: *Spolehlivé správci stavu* a *Spolehlivé slovníku*.

Jsou běžné důvody pro použití oznámení:

- Vytváření nastala zobrazení, například sekundární indexy nebo agregované filtrovaných zobrazení otevřené státu. Příklad je seřazené index všech klíčů v spolehlivé slovníku.
- Odesílání monitorování dat, jako je počet uživatelů, kteří přidali za poslední hodinu.

Oznámení při vyvolání jako součást použití operace. Z toho důvodu oznámení zacházet rychlé události možné a synchronní nezahrnujte drahé operace.

## <a name="reliable-state-manager-notifications"></a>Oznámení spolehlivý stavu Správce

Správci spolehlivý stavu, zobrazí oznámení pro následujících událostí:

- Transakce
    - Potvrzení
- Stav Správce
    - Opětovné sestavení
    - Přidání důvěryhodného stavu
    - Odebrání důvěryhodného stavu

Správce stavu spolehlivý sleduje aktuální transakce inflight. Pouze změny ve stavu transakce, který způsobuje oznámení na je transakce potvrzení.

Správce stavu spolehlivý udržuje kolekce spolehlivé států jako spolehlivé slovník a spolehlivé fronta. Správce stavu spolehlivý aktivuje oznámení, když změn v této kolekci: Přidání nebo odebrání důvěryhodného stavu nebo celá kolekce znovu.
Spolehlivé stavu správce kolekce je znovu ve třech případech:

- Obnovení: Při spuštění otevřené obnoví předchozího stavu z disku. Na konci obnovení používala **NotifyStateManagerChangedEventArgs** aktivováno událost, který obsahuje sadu obnoveného spolehlivé států.
- Úplné kopie: před otevřené můžete připojit se ke konfiguraci nastavení, musí být integrované. V některých případech to vyžaduje úplné kopie spolehlivé stavu Správce stavu z primární otevřené mohou být použity pro nedělá sekundární otevřené. Správce stavu spolehlivý na vedlejší otevřené používá **NotifyStateManagerChangedEventArgs** aktivováno události, který obsahuje sadu spolehlivé stavy, které získané od primární otevřené.
- Obnovení: V postupech obnovení havárie otevřené stavu lze obnovit ze zálohy prostřednictvím **RestoreAsync**. V takovém případě spolehlivé správce stavu na primární otevřené používá **NotifyStateManagerChangedEventArgs** aktivováno události, který obsahuje sadu spolehlivé stavy, které obnovení ze zálohy.

Registrace pro transakce upozornění nebo oznámení správce stavu, budete muset zaregistrovat události **TransactionChanged** nebo **StateManagerChanged** spolehlivé stav správce. Je běžné zaregistrovat tyto obslužné rutiny události se konstruktor stavová služba. Při registraci na konstruktoru zmeškáte nebude oznámení, která způsobila změnou po dobu platnosti **IReliableStateManager**.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

Obslužná rutina události **TransactionChanged** využívá **NotifyTransactionChangedEventArgs** podrobností o události. Vlastnost akce (například **NotifyTransactionChangedAction.Commit**), která určuje typ změny v ní. Obsahuje také transakce vlastnost, která obsahuje odkaz na transakce, která se změnila.

>[AZURE.NOTE] V současné době **TransactionChanged** události aktivovány jenom v případě, že transakce. Tato akce je pak rovno **NotifyTransactionChangedAction.Commit**. Ale události může v budoucnosti dosáhnout pro jiné typy změn stavu transakce. Doporučujeme kontrolu akce a zpracování události jenom v případě, že je takový, který očekáváte.

Následuje příklad **TransactionChanged** obslužné rutiny.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

Obslužná rutina události **StateManagerChanged** využívá **NotifyStateManagerChangedEventArgs** podrobností o události.
**NotifyStateManagerChangedEventArgs** má dvě podřízené: **NotifyStateManagerRebuildEventArgs** a **NotifyStateManagerSingleEntityChangedEventArgs**.
Použít vlastnost akce v **NotifyStateManagerChangedEventArgs** odevzdat **NotifyStateManagerChangedEventArgs** pro správné podtřídu:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** a **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Následuje obslužné příklad **StateManagerChanged** oznámení.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Spolehlivé oznámení slovníku

Spolehlivé slovník, zobrazí oznámení pro následujících událostí:

- Opětovného vytvoření: Když **ReliableDictionary** obnovila stavu obnoveného nebo zkopírované místní stavu nebo zálohování s názvem.
- Vymazání: S názvem při stavu **ReliableDictionary** je zrušeno prostřednictvím metody **ClearAsync** .
- Přidání: Volat, pokud položky přidala **ReliableDictionary**.
- Aktualizace: S názvem položky v **IReliableDictionary** po aktualizaci.
- Odebrat: S názvem po odstranění položky v **IReliableDictionary** .

Získat oznámení spolehlivé slovník, musíte zaregistrovat obslužná **DictionaryChanged** na **IReliableDictionary**. Je běžné zaregistrovat tyto obslužné rutiny události se v **ReliableStateManager.StateManagerChanged** přidat oznámení.
Registrace při přidání **IReliableDictionary** **IReliableStateManager** zaručuje, že zmeškáte nebudou žádná upozornění.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** je metodu vzorku, která volá v předchozím příkladu **OnStateManagerChangedHandler** .

Předchozí kód nastaví rozhraní **IReliableNotificationAsyncCallback** spolu s **DictionaryChanged**. Protože **NotifyDictionaryRebuildEventArgs** obsahuje **IAsyncEnumerable** rozhraní – které je potřeba vytvořit výčet asynchronní – znovu vytvořit oznámení při vyvolání prostřednictvím **RebuildNotificationAsyncCallback** místo **OnDictionaryChangedHandler**.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] V předchozím kódu jako součást zpracování oznámení opětovného vytvoření nejdřív udržované agregované stavu je zrušeno. Protože spolehlivé kolekce je právě opětovného vytvoření nové stavem nepatří všechny předchozí oznámení.

Obslužná rutina události **DictionaryChanged** využívá **NotifyDictionaryChangedEventArgs** podrobností o události.
**NotifyDictionaryChangedEventArgs** má pět dílčí. Vlastnost akce v **NotifyDictionaryChangedEventArgs** odevzdat **NotifyDictionaryChangedEventArgs** pro správné podtřídu:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** a **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Doporučení

- Dokončení *provést* oznámení událostí tak rychle.
- *Ne* všechny drahé provádět operace s externím (například operací) jako součást synchronní události.
- *Kontrolovat typ akce předtím, než zpracuje událost.* Nové typy akcí může v budoucnosti přidali.

Tady jsou některé věci, které mějte na paměti:

- Oznámení při vyvolání jako součást spuštění operaci. Příklad oznámení obnovení aktivoval jako poslední krok obnovení. Obnovení nebude dokončen, dokud zpracování události oznámení.
- Protože oznámení při vyvolání jako součást použití operace, klienti zobrazit pouze upozornění pro místně potvrzené operace. A protože operace je zaručena pouze místně potvrzené (tedy přihlášení k lyncu), může nebo nemusí být v budoucnosti vrátit zpět.
- Na cestě znovu jednoho oznámení aktivována pro každou použité operaci. To znamená, že pokud transakce T1 obsahuje Create(X), Delete(X) a Create(X), dostanete jedno oznámení pro vytvoření X, jeden pro odstranění a jeden pro vytváření znovu v tomto pořadí.
- Pro transakce, které obsahují více operací použijí se operace v pořadí, ve kterém jste dostali na primární otevřené od uživatele.
- V rámci zpracování false průběh může být některé operace vrátit zpět. Oznámení aktivovány pro tyto operace zpět stav otevřené návrat stabilní bodu. Jeden důležité rozdíl upozornění na zpět se vypočítá následujícím seskupená události, které mají duplicitní klíče. Například pokud transakce T1 je vrací zpět, zobrazí se jeden oznámení Delete(X).

## <a name="next-steps"></a>Další kroky

- [Spolehlivé kolekce](service-fabric-work-with-reliable-collections.md)
- [Spolehlivé Představení aplikace služby](service-fabric-reliable-services-quick-start.md)
- [Spolehlivé služby zálohování a obnovení (havárie obnovení)](service-fabric-reliable-services-backup-restore.md)
- [Referenční informace pro vývojáře spolehlivé Collections](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
