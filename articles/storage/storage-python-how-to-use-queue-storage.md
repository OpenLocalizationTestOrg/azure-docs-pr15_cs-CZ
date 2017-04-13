<properties
    pageTitle="Použití úložiště fronty z Python | Microsoft Azure"
    description="Naučte se používat službu Azure fronty z Python můžete vytvářet a odstraňovat fronty, vložení, získat a odstraňování zpráv."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>Použití úložiště fronty z Python

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Základní informace

Tato příručka, uvidíte, jak provádět běžné scénáře použití úložiště služby Azure fronty. Vzorky psaných Python a používat [Microsoft Azure úložiště SDK Python]. Scénáře nichž se uplatní zahrnují **Vložení**, **prohlížení**, **začíná**a **odstraňování** zpráv, jakož i **vytváření a odstraňování fronty**. Další informace o frontách naleznete v části [Další kroky].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Postup: Vytvoření fronty

Objekt **QueueService** umožňuje pracovat s fronty. Následující kód vytvoří objekt **QueueService** . Přidejte následující v horní části jakéhokoli Python souboru, ve kterém chcete programově přístup k úložišti Azure:

    from azure.storage.queue import QueueService

Následující kód vytvoří objekt **QueueService** pomocí klíč účtu a název účtu úložiště. Nahraďte "Můj účet" a "mykey" název účtu a klíče.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Jak: Vložte do zprávy do fronty

Vložit zprávu do fronty, používat **umístění\_zprávy** způsob, jak vytvořit novou zprávu a přiřadit ji někomu fronty.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Způsob: Náhled na další zprávu

Můžete prohlížet zprávy začátku fronty bez odebrání ve frontě tak, že zavoláte **Náhled\_zprávy** metody. Ve výchozím nastavení **Náhled\_zprávy** prohlédne podstatu sdělení.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Jak: Dequeue zprávy

Kód odebere zprávu z fronty ve dvou krocích. Když voláte **získat\_zprávy**, se zobrazí následující zpráva ve frontě ve výchozím nastavení. Zpráva vrácených **získat\_zprávy** vidět jiný kód čtení zpráv z této fronty. Ve výchozím nastavení tato zpráva zobrazena neviditelné 30 sekund. Dokončete zprávu odebráním fronty musíte také zavolat **Odstranit\_zprávy**. Krocích popsaných odebrání zprávy zajišťuje, že když kódu nepodaří zpracování zprávy kvůli chybě hardware a software, jiné instanci aplikace kódu můžete zobrazí stejná zpráva a zkuste to znovu. Volání kód **Odstranit\_zprávy** doprava po zpracování zprávy.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

Je možné upravit načítání zpráv z fronty dvěma způsoby:
Nejdřív můžete získat dávku zprávy (až 32). Za druhé můžete nastavit časový limit prodloužit nebo zkrátit invisibility umožňující kódu více nebo méně času plně zpracuje každou zprávu. Následující příklad kódu používá **získat\_zprávy** způsob, jak se 16 zprávy v jednom volání. Zpracuje každou zprávu pomocí a pak pro smyčky. Také nastaví časový limit invisibility pět minut pro každou zprávu.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Postup: Změna obsah ve frontě zprávy

Můžete změnit obsah zprávy v místě ve frontě. Pokud se zpráva představuje úkolu práce, může pomocí této funkce můžete aktualizovat stav pracovního úkolu. Kód pod používá **Aktualizovat\_zprávy** způsob aktualizace zprávy. Časový limit viditelnost se nastaví na 0, což znamená, tato zpráva se okamžitě zobrazí aktualizaci obsahu.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Jak: Získání délka fronty

Odhad počet zpráv může vstoupit do fronty. **Získat\_fronty\_metadat** metoda dotazem služba fronty vrátit metadata o fronty a **approximate_message_count**. Výsledek totiž pouze přibližnou zprávy můžete přidat i odebrat po služba fronty odpovídá požadavku.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Jak: Odstranění fronty

Pokud chcete odstranit fronty a všech zpráv, na které se nacházejí v ní, kontaktujte **Odstranit\_fronty** metody.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy fronty úložiště, tyto odkazy vedou na další informace.

- [Středisko pro vývojáře Python](/develop/python/)
- [Služby Azure úložiště rozhraní REST API](http://msdn.microsoft.com/library/azure/dd179355)
- [Blog týmu Azure úložiště]
- [Microsoft Azure úložiště SDK Python]

[Blog týmu Azure úložiště]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure úložiště SDK Python]: https://github.com/Azure/azure-storage-python
