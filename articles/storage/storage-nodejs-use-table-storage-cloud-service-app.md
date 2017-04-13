<properties 
    pageTitle="Web app s úložiště tabulek (Node.js) | Microsoft Azure" 
    description="Kurz, který je založena na Web App s Express kurz přidáním služby Azure úložiště a modul Azure." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>Node.js webové aplikace pomocí úložiště

## <a name="overview"></a>Základní informace

V tomto kurzu rozšíříte aplikace vytvořený s využitím knihovny Microsoft Azure klienta pro Node.js pro práci s služby správy dat v kurzu [Node.js webové aplikace pomocí Express] . Rozšíříte aplikace k vytvoření aplikace založené na webu – seznam úkolů, která můžete nasadit Azure. Seznam úkolů umožňuje uživateli získání úkoly, přidávat nové úkoly a úkoly označte jako dokončený.

Položky úkolu jsou uložené v úložišti Azure. Azure úložiště poskytuje úložiště Nestrukturovaná data, která je chybám a snadno dostupné. Azure úložiště obsahuje několik datových struktur, kde můžete ukládat a přístup k datům a můžete využít služeb úložiště a rozhraní API součástí Azure SDK Node.js nebo prostřednictvím rozhraní REST API. Další informace najdete v tématu [ukládání a přístup k datům v Azure].

Tento kurz se předpokládá, že jste dokončili [Node.js webové aplikace] a [Node.js s Express][Node.js webové aplikace pomocí Express] kurzy.

Naučíte se:

-   Jak pracovat s modul Jade šablony
-   Jak lze pomocí služby správy dat Azure

Snímek obrazovky s dokončeným aplikace je níže:

![Dokončené webové stránky v internet Exploreru](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Nastavení úložiště pověření v Web.Config

Přístup k úložišti Azure, budete muset předat úložiště pověření. K tomuto účelu využít konfigurace aplikace.
Toto nastavení bude předávat jako proměnné uzel, které se pak jen pro čtení Azure SDK.

> [AZURE.NOTE] Úložiště pověření se použijí pouze při nasazení aplikace na Azure. Při spuštění v emulátor, použije aplikace emulátoru úložiště.

Proveďte následující kroky k načtení přihlašovací údaje účtu úložiště a přidejte je do konfigurace:

1.  Pokud ještě není otevřete spuštění prostředí PowerShell Azure z nabídky **Start** rozbalením **Všechny programy, Azure**, klikněte pravým tlačítkem myši **Azure PowerShell**a pak vyberte **Spustit jako správce**.

2.  Změna adresáře do složky obsahující aplikace. Například C:\\uzel\\tasklist\\WebRole1.

3.  V okně prostředí Powershell Azure zadejte následující rutinu k načtení informací o účtu úložiště:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    To načte seznam účtů úložiště a účet klíče přidružený hostovanou službu.

    > [AZURE.NOTE] Protože Azure SDK vytvoří účet úložiště při nasazujete službu, účtu úložiště by měl z nasazení aplikace v předchozí vodítka existovat.

4.  Otevřete soubor **ServiceDefinition.csdef** obsahující nastavení prostředí, které se používají při nasazení aplikace na Azure:

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Vložte následující blok pod **prostředí** prvek zaokrouhlením {úložiště účet} a {úložiště PŘÍSTUPOVÁ klávesa} s název účtu a primární klíč účtu úložiště, který chcete použít pro nasazení:

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![Obsah souboru web.cloud.config](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Uložit a zavřít Poznámkový blok.

### <a name="install-additional-modules"></a>Instalace další moduly

2. Zadejte následující příkaz a nainstalujte [azure], [uuid uzel], [nconf] a [asynchronní] moduly místně i tak, aby k souboru **package.json** uložení položky:

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    Výstup tohoto příkazu by měla vypadat podobně jako tento:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>Použití služby Tabulka v aplikaci uzel

V této části rozšíříte základní aplikace vytvořené pomocí příkazu **express** přidáním **task.js** soubor, který obsahuje modelu úkolů. Bude taky změnit existující **app.js** a nejradši **tasklist.js** používající modelu.

### <a name="create-the-model"></a>Vytvoření modelu

1. V adresáři **WebRole1** vytvořte nový adresář s názvem **modely**.

2. V adresáři **modely** vytvoření nového souboru s názvem **task.js**. Tento soubor bude obsahovat modelu doplňku pro úkoly vytvořené v aplikaci.

3. Na začátku tohoto souboru **task.js** přidáte následující kód neodkazuje požadované knihovny:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Pak přidáte kód definovat a export objektu úkolu. Tento objekt je zodpovědný za připojení k tabulce.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Dále přidejte následující kód pro definování další metody objektu úkolu umožňující interakce s daty uloženými v tabulce:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Uložte a zavřete soubor **task.js** .

### <a name="create-the-controller"></a>Vytvoření správce

1. V adresáři **WebRole1/směruje** nejradši s názvem **tasklist.js** a otevřete ho v textovém editoru.

2. Následující kód dodejte **tasklist.js**. Načte azure a asynchronní moduly, které využívají **tasklist.js**. Definuje také funkci **TaskList** předán instanci objektu **úkolu** , která byla definována dříve:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Přidávejte k souboru **tasklist.js** přidáním metodách **showTasks** **addTask**a **completeTasks**:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Uložte soubor **tasklist.js** .

### <a name="modify-appjs"></a>Úprava app.js

1. V adresáři **WebRole1** otevřete soubor **app.js** v textovém editoru. 

2. Na začátku tohoto souboru, přidejte následující načíst modul azure a nastavit klíč název a oddíl tabulky:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. V souboru app.js posuňte se dolů na místo, kam se zobrazí následující řádek:

        app.use('/', routes);
        app.use('/users', users);

    Nahraďte předchozí řádky s kódem ukázáno v následujícím příkladu. Spustí tato instance <strong>úkolu</strong> s připojením ke svému účtu úložiště. To je předán <strong>TaskList</strong>, který ho budou používat ke komunikaci s tabulku služba:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Uložte soubor **app.js** .

### <a name="modify-the-index-view"></a>Úprava zobrazení indexu

1. Změna adresáře k adresáři **zobrazení** a otevřete soubor **index.jade** v textovém editoru.

2. Nahraďte obsah souboru **index.jade** následující kód. Tato možnost definuje zobrazení pro existující úkoly, jakož i formuláře pro přidání nových úkolů a označování existující jako dokončený.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Uložte a zavřete soubor **index.jade** .

### <a name="modify-the-global-layout"></a>Úprava globální rozložení

Soubor **layout.jade** v adresáři **zobrazení** slouží jako globální šablony pro ostatní soubory **.jade** . V tomto kroku se budou ho upravit a použijte [Twitter zavádění](https://github.com/twbs/bootstrap), která je sada nástrojů, který usnadňuje hodní vypadající web navrhnout.

1. Stáhněte si a extrahujte soubory pro [Twitter zavádění](http://getbootstrap.com/). Zkopírujte soubor **bootstrap.min.css** z **zavádění\\dist\\css** složku, kterou chcete **veřejné\\šablony stylů** adresáře aplikace tasklist.

2. Ve složce **zobrazení** otevřete **layout.jade** v textovém editoru a nahraďte obsah takto:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Uložte soubor **layout.jade** .

### <a name="running-the-application-in-the-emulator"></a>Spuštění aplikace v emulátor

Pomocí následujícího příkazu ke spuštění aplikace v emulátor.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

V prohlížeči se otevře a zobrazí se na následující stránce:

![Web stránkování s názvem mého seznamu úkolů v tabulce obsahující úkoly a pole přidat nový úkol.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Formulář slouží k přidání položek nebo odstranění existující položky podle jejich označování jako dokončený.

## <a name="publishing-the-application-to-azure"></a>Publikování aplikace Azure


V okně prostředí Windows PowerShell volejte následující rutinu přeinstalujte hostovanou službu na Azure.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

Nahraďte **myuniquename** jedinečný název pro tuto aplikaci. **Datacentername** nahraďte názvem Azure datovém centru, například **Západní USA**.

Po dokončení nasazení byste měli vidět odpověď podobná této:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Stejně jako předtím vzhledem k tomu, že jste zadali **– spuštění** možnost, v prohlížeči se otevře a zobrazí aplikace spuštěné v Azure po dokončení publikování.

![Okno prohlížeče zobrazující na stránce Můj seznam úkolů. Adresa URL označuje, že na stránce je právě teď hostovaný ve Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Zastavení a odstranění části aplikace

Po nasazení aplikace, můžete ho zakázat, abyste mohli vyhnout náklady nebo vytvoření a nasazení jiných aplikací v rámci bezplatnou zkušební období.

Azure faktury webové role instancí za hodinu spotřebované množství času serveru.
Čas serveru je spotřebované množství po nasazení aplikace, i když instance se nepoužívá a jsou v přestal stavu.

Podle těchto kroků ukazují, jak zastavit a odstranění aplikace.

1.  V okně prostředí Windows PowerShell ukončení nasazení služby vytvořený v předchozí části s následující rutinu:

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    Zastavení služby může trvat několik minut. Zastavení služby, zobrazí se zpráva oznamující, že přestal.

3.  Pokud chcete odstranit službu, kontaktujte následující rutinu:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Po zobrazení výzvy zadejte **Y** k odstranění služby.

    Odstranění služby může trvat několik minut. Po odstranění službu zobrazí se zpráva oznamující, že došlo k odstranění službu.

  [Node.js webové aplikace pomocí Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Ukládání a přístup k datům v Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Node.js webovou aplikaci]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 
