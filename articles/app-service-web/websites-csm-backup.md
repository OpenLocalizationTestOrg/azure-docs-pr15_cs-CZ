<properties
    pageTitle="ZBÝVAJÍCÍ umožňuje zálohování a obnovení aplikace služeb aplikací"
    description="Naučte se používat rozhraní API RESTful volání pro zálohování a obnovení aplikace v aplikaci služby Azure"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>ZBÝVAJÍCÍ umožňuje zálohování a obnovení aplikace služeb aplikací

> [AZURE.SELECTOR]
- [Prostředí PowerShell](../app-service/app-service-powershell-backup.md)
- [ROZHRANÍ REST API](websites-csm-backup.md)

[Aplikace služby aplikace](https://azure.microsoft.com/services/app-service/web/) zálohovat lze jako objekty BLOB v Azure úložiště. Zálohování mohou také obsahovat databáze v aplikaci. Pokud aplikaci někdy omylem odstranili, nebo pokud potřebujete-li se vrátit na předchozí verzi aplikace, může ho obnovit z předchozí zálohy. Zálohování se teď dá kdykoliv na vyžádání nebo zálohy můžou být naplánované vhodných intervalech.

Tento článek vysvětluje, jak zálohování a obnovení aplikace s žádostí o rozhraní API RESTful. Pokud chcete vytvořit a spravovat aplikace zálohy graficky pomocí portálu Azure, přečtěte si článek [obecnějším údajům web app v aplikaci služby Azure](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Začínáme
Odeslání žádosti o ZBÝVAJÍCÍ, je potřeba vědět vaší aplikaci **název** **pole Skupina zdroje**a **id předplatného**. Tyto informace najdete po kliknutí na vaši aplikaci distribuovali zásuvné **Aplikaci služby** [Azure portálu](https://portal.azure.com). Příklady v tomto článku jsme konfigurujete webu **backuprestoreapiexamples.azurewebsites.net**. Je uložený ve skupinovém rámečku výchozí Web WestUS zdroje a běží předplatného s ID 00001111-2222-3333-4444-555566667777.

![Informace o ukázkových webu][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Zálohování a obnovení rozhraní REST API
Teď můžeme převezme několik příkladů použití rozhraní REST API zálohování a obnovení aplikace. Každý příklad obsahuje adresy URL a HTTP žádost o text. Ukázková adresa URL zahrnuje zástupné symboly omotané do složených závorek, například {id předplatného}. Nahraďte zástupný text odpovídající informace o aplikaci. Odkaz tady je popis jednotlivých zástupný symbol, který se zobrazí v příklady adres URL.

* id předplatného – ID Azure předplatného obsahující aplikace
* zdroje skupiny – název – Skupina prostředků obsahující aplikace
* název – název aplikace
* zálohování – id – ID aplikace Zálohování

Kompletní dokumentaci rozhraní API, včetně několik volitelné parametrů, které mohou být součástí žádost HTTP najdete v článku [Explorer Azure zdroje](https://resources.azure.com/).

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Zálohování aplikace na vyžádání
K obecnějším údajům aplikace okamžitě, pošlete žádost **příspěvek** **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Tady je adresa URL vypadá jako při použití náš příklad Web. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/Backup/**

Zadejte JSON objektu v těle vaši žádost o zadání účtu, který úložiště používat k ukládání zálohování. Vlastnost s názvem **storageAccountUrl**, který obsahuje [Adresa URL přidružení zabezpečení](../storage/storage-dotnet-shared-access-signature-part-1.md) udělení přístupu k zápisu do kontejneru úložišti Azure obsahující záložní objektů blob musí být JSON objektu. Pokud chcete zálohovat databáze, je nutné zadat seznam obsahující názvy, typů a řetězce připojení databáze k zálohování.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Ihned po přijetí žádosti, nebude zahájen zálohy aplikace. Záložní může trvat delší dobu dokončení. Odpověď na HTTP obsahuje číslo ID, které můžete použít v jiné žádosti o zobrazení stavu zálohování. Tady je příklad textu HTTP odpověď na žádost o naše zálohu.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Chybové zprávy najdete v vlastnosti souboru protokolu HTTP odpovědi.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Naplánování automatické zálohování
Kromě zálohování aplikace na službu, můžete taky plánujete záložní stát automaticky.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Nastavte si nový plán automatické zálohování
Pokud chcete nastavit plánu zálohování, pošlete žádost o **umístění** na **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Takto vypadá adresu URL webu náš příklad. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/config/Backup**

Žádost o subjekt musí mít JSON objekt, který určuje konfigurace zálohování. Tady je příklad se všechny požadované parametry.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

V tomto příkladu nakonfiguruje aplikaci automaticky zálohovat všechny sedmi dnů. Parametry **frequencyInterval** a **frequencyUnit** společně zjistit, jak často zálohy dojít. Platné hodnoty **frequencyUnit** se **hodinu** **dne**. Například obecnějším údajům aplikace každých 12 hodin, nastavte frequencyInterval 12 a frequencyUnit na hodinu.

Staré zálohy se automaticky odeberou z účtu úložiště. Můžete určit, jak staré zálohy lze nastavením **retentionPeriodInDays** parametru. Chcete-li vždy používáte-li alespoň jeden zálohování uložili, bez ohledu na stáří se se nastavují **keepAtLeastOneBackup** true (pravda).

### <a name="get-the-automatic-backup-schedule"></a>Získání automatického zálohování naplánovat
Získání aplikace pro záložní konfigurace, pošlete žádost o **příspěvek** na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

Adresu URL webu náš příklad je **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>Stav zálohy
V závislosti na tom, jak velké aplikace zálohy může chvíli k dokončení. Zálohování mohou také nezdařilo časový limit, nebo částečně úspěšné. Zobrazení stavu všech aplikace na zálohování, pošlete žádost **získat** adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Zobrazení stavu konkrétní zálohy, pošlete žádost získat adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Takto vypadá adresu URL webu náš příklad. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

Objekt JSON podobně jako tento příklad obsahuje text odpovědi.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Stav zálohy je typ výčtu. Tady je každý možný stav.

* 0 – probíhající: zálohování byl spuštěn, ale nebyla dosud provedeny.
* 1 – nezdařilo: Zálohování neproběhla.
* 2 – úspěšné: Zálohování byla úspěšně dokončena.
* 3 – TimedOut: Zálohování se nedokončí v čase a bylo zrušeno.
* 4 – vytvořili: Žádost o zálohu je ve frontě, ale ještě nezačala.
* 5 – přeskočilo: Zálohování není pokračovat, protože plán aktivaci příliš mnoho zálohy.
* 6 – PartiallySucceeded: Zálohování bylo úspěšné, ale některé soubory nebyly zálohovat, protože nemůže přečíst. To obvykle způsobeno výhradní zámek je umístěn na soubory.
* 7 – DeleteInProgress: Zálohování požádal chcete odstranit, ale nebyla ještě odstraněna.
* 8 – DeleteFailed: Zálohování nelze odstranit. Tomu může dojít, protože skončila přidružení zabezpečení adresy URL, která byla použita k vytvoření zálohy.
* 9 – odstranit: Zálohování bylo odstraněna.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Obnovení aplikace ze zálohy
Pokud byl odstraněn aplikace nebo pokud se chcete vrátit na předchozí verzi aplikace, můžete aplikaci obnovení ze zálohy. Vyvolat obnovení, pošlete žádost o **příspěvek** na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Takto vypadá adresu URL webu náš příklad. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/Restore**

V hlavním textu žádosti o odeslání JSON objekt, který obsahuje vlastností pro obnovení. Tady je příklad obsahující všechny požadované vlastnosti:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Obnovte si nové aplikace
Někdy může chcete vytvořit novou aplikaci při obnovení záložní, místo přepisování již existující aplikace. K tomuto účelu změňte požadavku na adresu URL tak, aby ukazovaly na novou aplikaci, kterou chcete vytvořit a **změnit vlastnosti **Přepsat** v ve formátu JSON**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Odstranění aplikace Zálohování
Pokud chcete odstranit zálohy, odešlete žádost o **Odstranění** URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Takto vypadá adresu URL webu náš příklad. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>Správa URL přidružení zabezpečení zálohování
Služba Azure aplikací se pokusí zálohování zabránění úložišti Azure pomocí adresy URL přidružení zabezpečení, které při vytvoření zálohy. Pokud je tato adresa URL přidružení zabezpečení už není platný, zálohování nelze odstranit prostřednictvím rozhraní REST API. Však můžete aktualizovat přidružení zabezpečení URL přidružené záložní tak, že žádost o **příspěvek** na adresu URL **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Takto vypadá adresu URL webu náš příklad. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/Sites/backuprestoreapiexamples/backups/1/list**

V hlavním textu žádosti o odeslání JSON objekt obsahující nová adresa URL přidružení zabezpečení. Tady je příklad.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] Z bezpečnostních důvodů přidružení zabezpečení URL přidružené zálohy nevrátí při odesílání požadavek GET pro konkrétní zálohy. Pokud chcete zobrazit adresu URL přidružení zabezpečení přidruženou zálohy, pošlete žádost o příspěvek na stejnou adresu URL výše. V hlavním textu žádosti o zahrňte prázdného JSON objektu. Odpovědi ze serveru obsahuje všechny informace o této zálohy, včetně jeho adresu URL přidružení zabezpečení.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
