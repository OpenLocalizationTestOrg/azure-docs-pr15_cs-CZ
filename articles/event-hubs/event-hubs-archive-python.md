<properties
    pageTitle="Azure návodu události rozbočovače archivu | Microsoft Azure"
    description="Příklad, který používá Azure Python SDK k demonstrují použití funkce archivu rozbočovače události."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>Událost rozbočovače archivu návod: Python

Archiv rozbočovače událostí je nová funkce rozbočovače události, která umožňuje automaticky předávat data toku v centrální události k úložišti objektů Blob Azure účtu podle svého výběru. Usnadňuje provádět dávkové zpracování na data v reálném čase streamování. Tento článek popisuje, jak pomocí Python archivu rozbočovače události. Další informace o události rozbočovače archiv naleznete v [článku Přehled](event-hubs-archive-overview.md).

Tento příklad používá Azure Python SDK k demonstrují použití funkce archivu. Sender.py rozešle simulovaný klimatizace telemetrie rozbočovače události ve formátu JSON. Centrální událostí je nakonfigurovaný na použití funkce Archivovat zapsat tato data do objektů blob úložiště na listy. Archivereader.py pak bude číst tyto objekty BLOB vytvoří připojit soubor na zařízení a zapíše data do souborů CSV.

Co bude provést

1.  Vytvoření účtu úložiště objektů Blob Azure a objektů blob kontejneru v ní pomocí portálu Azure

2.  Vytvoření události centrální názvů, pomocí portálu Azure

3.  Vytvoření události centrální funkce archivu povolena, pomocí portálu Azure

4.  Odeslání dat do centra události se skriptem Python

5.  Čtení souborů z archivu a zpracování s jinou Python skriptu

Zjistit předpoklady pro

1.  Python 2.7.x

2.  Předplatné Azure

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Vytvořte účet Azure úložiště

1.  Přihlaste se k [portálu Azure][].

2.  V levém navigačním podokně na portálu klikněte na nový a pak klikněte na Data + úložiště a klepněte na účet úložiště.

3.  Vyplňte pole v zásuvné účtu úložiště a klikněte na **vytvořit**.

    ![][1]

4.  Až uvidíte zprávu **Úspěšně nasazení** , klikněte na nový účet úložiště a v zásuvné **Essentials** klikněte na **objekty BLOB**. Až se otevře zásuvné **objektů Blob služby** , klikněte na **+ kontejneru** nahoře. Jméno container **archivu**a potom na Zavřít zásuvné **objektů Blob služby** .

5.  Klikněte do levého zásuvné **přístupových kláves z verze** a zkopírujte název účtu úložiště a přínosu **klíč1**. Uložte tyto hodnoty do programu Poznámkový blok nebo někde jinde dočasné.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Vytvořit skript Python události odešlete rozbočovače události

1.  Otevřete editor Oblíbené Python například [Visual Studio kód][].

2.  Vytvořte skript s názvem **sender.py**. Tento skript odešle 200 události rozbočovače události. Jsou to jednoduché klimatizace čtení odeslané ve formátu JSON.

3.  Vložte tento kód do sender.py:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  Aktualizujte předchozí kód použít název oboru názvů a hodnoty klíče, které jste získali při vytváření názvů rozbočovače události.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Vytvořit skript Python ke čtení souborů archivu

1.  Vyplňte zásuvné a klikněte na **vytvořit**.

2.  Vytvořte skript s názvem **archivereader.py**. Tento skript přečte archivace souborů a vytvoření souboru na zařízení zapsat data jenom pro toto zařízení.

3.  Vložte tento kód do archivereader.py:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Nezapomeňte vložit odpovídající hodnoty název účtu úložiště a klíče při volání `startProcessing`.

## <a name="run-the-scripts"></a>Spustit skripty

1.  Otevřete okno příkazového řádku, který má Python v cestě a spusťte tyto příkazy k instalaci základní balíčků Python:

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Pokud používáte dřívější verzi buď úložišti azure nebo azure budete muset používat **– upgrade** možnost

    Je taky potřeba spustit následující (není potřeba ve většině počítačů):

    ```
    pip install cryptography
    ```

2.  Změna adresáře na místo, kam jste uložili sender.py a archivereader.py a spusťte tento příkaz:

    ```
    start python sender.py
    ```
    
    Spustí se nový Python proces spustit odesílatel.

3. Teď chvíli počkejte archivace spouštět. Do původní okno příkazového řádku zadejte tento příkaz:

    ```
    python archivereader.py
    ```

Tento archivu procesor používá místní adresář stahovat všechny objekty BLOB z účtu nebo kontejneru úložiště. Zpracuje ty, které nejsou prázdné a napište výsledky jako soubory CSV do místního adresáře.

## <a name="next-steps"></a>Další kroky

Další informace o události rozbočovače navštivte následující odkazy:

- [Základní informace o události rozbočovače archivace][]
- Kompletní [Ukázková aplikace, který využívá rozbočovače události][].
- Ukázka [měřítko, událostí zpracování s rozbočovače události][] .
- [Přehled rozbočovače událostí][]
 

[Azure portálu]: https://portal.azure.com/
[Základní informace o události rozbočovače archivace]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Kód Visual Studio]: https://code.visualstudio.com/
[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Rozšiřování události zpracování s rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
