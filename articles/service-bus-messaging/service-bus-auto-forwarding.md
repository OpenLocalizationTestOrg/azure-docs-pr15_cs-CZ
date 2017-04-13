<properties 
    pageTitle="Automatické přeposílání zpráv entity Bus služby | Microsoft Azure"
    description="Jak řetězu fronty nebo předplatné jiného fronty nebo tématu."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Zřetězení služby Bus jednotek automatické přeposílání

Funkci *automatické přeposílání* umožňuje zřetězení fronty nebo předplatné jiného fronty nebo tématu, které je součástí stejný obor názvů. Při zapnuté funkci automatické přeposílání, služby Bus automaticky odstraní zprávy, které jsou umístěná v prvním fronty nebo předplatného (zdrojový) a umístí do druhého fronty nebo tématu (cíl). Všimněte si, že je možné odeslat zprávu o Cílová entita přímo. Navíc není možné zřetězení podfronty, jako je do fronty nedoručených zpráv na jiný fronty nebo tématu.

## <a name="using-auto-forwarding"></a>Použití automatické přeposílání

Povolit automatické přeposílání nastavením vlastnosti [SubscriptionDescription.ForwardTo][] nebo [QueueDescription.ForwardTo][] objektům [QueueDescription][] nebo [SubscriptionDescription][] zdroje, jako v následujícím příkladu.

```
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Cílová entita musí existovat v době, kdy se vytvoří zdrojové entity. Pokud Cílová entita neexistuje, vrátí služby Bus výjimku po zobrazení výzvy k vytvoření zdrojové entity.

Automatické přeposílání umožňuje rozšiřování jednotlivá témata. Služba Bus omezuje [počet předplatná dané témat](service-bus-quotas.md) 2 000. Další předplatná vejdou vytvořením témata druhé úrovně. Všimněte si, že i když nejsou vázaná tak, že služba Bus omezení počtu předplatná, přidání druhé úrovně témata můžete zlepšit celkový výkon tématu.

![Automatické přeposílání scénářů][0]

Automatické přeposílání můžete taky oddělit od příjemce zprávy odesílatelů. Zvažte například ERP systému, který se skládá ze tří modulů: zpracování objednávek řízení zásob a řízení vztahů se zákazníky. Každý z těchto modulů vygeneruje zpráv, které nejsou byla zařazena do fronty do odpovídající téma. Alice a Jan jsou obchodní zástupci, kteří se chtějí všech zpráv, které se týkají svým zákazníkům. Aby se tyto zprávy Alice a Jan vytvořit osobní fronty a přihlášení k odběru jednotlivých hodnot ERP témata, která automaticky přeposlat všechny zprávy na jejich fronty.

![Automatické přeposílání scénářů][1]

Pokud Alice přejde na dovolenou, své osobní fronty, spíše než téma ERP, zaplní. V tomto scénáři protože prodejní zástupce neobdržel všechny zprávy žádná témata ERP někdy dosáhla kvóty.

## <a name="auto-forwarding-considerations"></a>Automatické přeposílání co byste měli zvážit

Pokud Cílová entita nahromaděné velké množství zpráv a větší než kvóty nebo je zakázaný Cílová entita, entit zdroj přidává zprávy do jeho [doručena](service-bus-dead-letter-queues.md) , dokud není místa v cílovém (nebo entita je znovu povolit). Tyto zprávy zůstanou live ve frontě doručena tak explicitně musí přijímání a jejich zpracování ve frontě doručena.

Při zřetězení společně jednotlivých témat a získat složené téma s mnoho předplatných, je vhodné mít střední počet předplatných na téma první úrovně a mnoho předplatných na témata druhé úrovně. Například první úrovně téma s 20 předplatná, každý z nich zřetězené druhé úrovně téma u 200 předplatných umožňuje vyšší výkon než první úrovně téma s 200 předplatná každý zřetězené druhé úrovně téma s 20 předplatná.

Služba Bus bills najednou pro každou předávané zprávy. Například odesláním každé zprávy na téma s 20 předplatná, každý z nich nakonfigurované tak, aby automatické předávání zpráv na jiný fronty nebo tématu se vám účtovat poplatky jako 21 operace Pokud všechna předplatná první úrovně zobrazí kopii zprávy.

Vytvořit předplatné, které je zřetězené do jiného fronty nebo téma, poznámkové bloky pro školy předplatného vyžaduje oprávnění **Spravovat** na zdroj i Cílová entita. Posílání zpráv do tématu zdroj vyžaduje oprávnění **Odeslat** na téma zdroje.

## <a name="next-steps"></a>Další kroky

Podrobné informace o automatické přeposílání najdete v těchto tématech:

- [SubscriptionDescription.ForwardTo][]
- [QueueDescription][]
- [SubscriptionDescription][]

Další informace o zvýšení výkonu služby Bus, najdete v článku [Partitioned zpráv entity][].

  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [SubscriptionDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.forwardto.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [SubscriptionDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptiondescription.aspx
  [0]: ./media/service-bus-auto-forwarding/IC628631.gif
  [1]: ./media/service-bus-auto-forwarding/IC628632.gif
  [Rozdělený entity zasílání zpráv]: service-bus-partitioning.md