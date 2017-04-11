<properties
   pageTitle="Začínáme s Azure dávku rozhraní příkazového řádku | Microsoft Azure"
   description="Rychlé seznámení s příkazy dávku vstoupit Azure rozhraní příkazového řádku pro správu služby Azure dávku prostředků"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/30/2016"
   ms.author="marsma"/>

# <a name="get-started-with-azure-batch-cli"></a>Začínáme s Azure dávku rozhraní příkazového řádku

Rozhraní platformě Azure příkazového řádku (Azure rozhraní příkazového řádku) umožňuje spravovat dávku účty a zdroje, jako jsou skupiny, projektů a úkolů v prostředí příkazů Linux, Mac a Windows. S Azure rozhraní příkazového řádku můžete provádět a skriptů spoustu stejných úkolů provádět s rozhraní API dávku, Azure portálem a rutiny prostředí PowerShell dávku.

Tento článek vychází z Azure rozhraní příkazového řádku verze 0.10.5.

## <a name="prerequisites"></a>Zjistit předpoklady pro

* [Instalace Azure rozhraní příkazového řádku](../xplat-cli-install.md)

* [Připojení k předplatnému Azure rozhraní příkazového řádku Azure](../xplat-cli-connect.md)

* Přepněte do **režimu Správce zdrojů**:`azure config mode arm`

>[AZURE.TIP] Doporučujeme aktualizace instalace Azure rozhraní příkazového řádku nejčastější umožní využít výhod aktualizací služeb a vylepšení.

## <a name="command-help"></a>Nápovědu k příkazu

Text nápovědy pro každého příkazu můžete zobrazit v rozhraní příkazového řádku Azure přidáním `-h` jako jediná možnost za příkazem. Příklad:

* Získání nápovědy pro `azure` příkazu, zadejte:`azure -h`
* Seznam všechny příkazy dávku vstoupit rozhraní příkazového řádku, můžete:`azure batch -h`
* Pokud chcete získat nápovědu pro vytvoření účtu dávku, zadejte:`azure batch account create -h`

Pokud jste na pochybách, použít `-h` příkazového řádku možnost nápovědu k příkazům Azure rozhraní příkazového řádku.

## <a name="create-a-batch-account"></a>Vytvoření dávky účtu

Použití:

    azure batch account create [options] <name>

Příklad:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Vytvoří nový účet dávka se zadanými parametry. Třeba určit minimálně umístění pole Skupina zdroje a název účtu. Pokud ještě nemáte skupina zdroje, vytvořit spuštěním `azure group create`a zadejte jednu z oblasti Azure (například "západní NAS") `--location` možnost. Příklad:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] Název účtu listu musí být jedinečný v rámci oblasti Azure vytvořený účet. Může obsahovat pouze malá písmena alfanumerické znaky a musí být 3 24 znaků. Nelze použít speciální znaky, například `-` nebo `_` v účtu názvů listů.

### <a name="linked-storage-account-autostorage"></a>Propojené úložiště účtu (autostorage)

(Volitelně) propojíte **univerzální** úložiště účtu k vašemu účtu dávku po jeho vytvoření. Funkce [balíčků aplikací](batch-application-packages.md) dávku používá úložiště objektů blob v propojených obecné použití účtu úložiště, stejně jako knihovnu [Dávkový soubor konvence .NET](batch-task-output.md) . Tyto volitelné funkce vám pomohou při nasazení aplikace, které spustit dávku úkolů a uchování data, která vytvářejí.

Chcete-li propojit existující účet Azure úložiště do nového účtu dávku po jeho vytvoření, zadejte `--autostorage-account-id` možnost. Tato možnost vyžaduje plně kvalifikovaný zdroje ID účtu úložiště.

Nejdřív zobrazte podrobnosti účtu úložiště:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Použijte hodnotu z pole **adresa Url** pro `--autostorage-account-id` možnost. Hodnotu adresa Url začíná řetězcem "/ předplatná /" a obsahuje předplatné ID a zdroje cestu k tomuto účtu úložiště:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## <a name="delete-a-batch-account"></a>Odstranění dávky účtu

Použití:

    azure batch account delete [options] <name>

Příklad:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Odstraní zadaný dávku účet. Po zobrazení výzvy, potvrďte, že chcete odebrat účet (odebrání účtu může určitou dobu trvat dokončete).

## <a name="manage-account-access-keys"></a>Správa účtu přístupových kláves

Budete potřebovat kód Product key pro přístup k [Vytvoření a úpravě zdrojů](#create-and-modify-batch-resources) ve vašem účtu dávku.

### <a name="list-access-keys"></a>Seznam přístupových kláves

Použití:

    azure batch account keys list [options] <name>

Příklad:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Seznam účtu klíče daného účtu dávku.

### <a name="generate-a-new-access-key"></a>Generovat nové přístupová klávesa

Použití:

    azure batch account keys renew [options] --<primary|secondary> <name>

Příklad:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Obnoví klávesu zadaný účet u daného účtu dávku.

## <a name="create-and-modify-batch-resources"></a>Vytváření a změny dávky prostředky

Rozhraní příkazového řádku Azure umožňuje vytvořit, číst, aktualizovat a odstranění (CRUD) dávku zdrojů, jako jsou skupiny, výpočet uzly, projekty a úkoly. Tyto operace CRUD vyžadují účet název listu, přístupové klávesy a koncového bodu. Můžete zadat s `-a`, `-k`, a `-u` možnosti nebo sady [proměnné](#credential-environment-variables) využívající rozhraní příkazového řádku automaticky (je-li vyplněné).

### <a name="credential-environment-variables"></a>Proměnné přihlašovacích údajů

Můžete nastavit `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY`, a `AZURE_BATCH_ENDPOINT` proměnné místo určení `-a`, `-k`, a `-u` možnosti do příkazového řádku pro každý příkaz spustíte. Rozhraní příkazového řádku dávku používá tyto proměnné (Pokud nastavit) tak, že je možné vynechat `-a`, `-k`, a `-u` možnosti. Zbývající část tohoto článku se předpokládá použití těchto proměnné.

>[AZURE.TIP] Seznam klíče s `azure batch account keys list`a zobrazte koncový bod účtu se `azure batch account show`.

### <a name="json-files"></a>JSON souborů

Když vytvoříte dávku zdrojů, jako jsou fondů a úlohy, můžete určit JSON soubor obsahující konfigurace nového prostředku místo předávání parametrů jako parametry příkazového řádku. Příklad:

`azure batch pool create my_batch_pool.json`

Když můžete provést mnoho operace vytvoření zdroje použití jenom parametrů příkazového řádku, některé funkce vyžadují soubor ve formátu JSON obsahující údajů o zdroji. Pokud chcete zadat souborů prostředků pro zahájení úkolu například musí použít soubor JSON.

Vyhledání JSON potřebná k vytvoření prostředku, najdete pod odkazy [dávku REST API reference] [ rest_api] si přečtěte následující dokumentaci na webu MSDN. Každý "Přidat *pole Typ zdroje"*téma obsahuje příklad JSON pro vytváření zdroje, které můžete použít jako šablony pro soubory JSON. Například JSON pro vytvoření fondu najdete v [Přidat fond k účtu][rest_add_pool].

>[AZURE.NOTE] Pokud zadáte JSON souboru při vytváření zdroje, všechny ostatní parametry funkce, které zadáte do příkazového řádku pro daný zdroj jsou ignorovány.

## <a name="create-a-pool"></a>Vytvoření fondu

Použití:

    azure batch pool create [options] [json-file]

Příklad (virtuální počítač konfigurace):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Příklad (konfigurace služby cloudu):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Vytvoří fond výpočetním uzlů ve službě dávku.

Jak je uvedeno v [Přehled funkcí dávku](batch-api-basics.md#pool), máte dvě možnosti při výběru operační systém pro uzly ve vaší fondu: **virtuální počítač** a **Konfigurace služby cloudu**. Použití `--image-*` možnosti pro vytváření fondů konfiguraci virtuálního počítače a `--os-family` při vytváření fondů konfigurace služby cloudu. Nelze zadat obě `--os-family` a `--image-*` možnosti.

Můžete zadat fondu [balíčků aplikací](batch-application-packages.md) a příkazového řádku pro [zahájení úkolu](batch-api-basics.md#start-task). Chcete-li souborů prostředků pro zahájení úkolu, ale musí místo toho použijete [JSON soubor](#json-files).

Odstranění fond s:

    azure batch pool delete [pool-id]

>[AZURE.TIP] Vyhledání hodnot je to vhodné pro [seznam virtuálního počítače obrázků](batch-linux-nodes.md#list-of-virtual-machine-images) `--image-*` možnosti.

## <a name="create-a-job"></a>Vytvoření projektu

Použití:

    azure batch job create [options] [json-file]

Příklad:

    azure batch job create --id "job001" --pool-id "pool001"

Přidá úlohy k účtu dávku a určuje fondu dnem spustit svých úkolů.

Odstranění úlohy pomocí:

    azure batch job delete [job-id]

## <a name="list-pools-jobs-tasks-and-other-resources"></a>Seznam skupin, projekty, úkoly a další zdroje

Podporuje každý typ zdroje dávku `list` příkazu, který dotaz účtu dávku a zobrazí seznam zdrojů tohoto typu. Můžete například seznam fondů v váš účet a úkoly v projektu:

    azure batch pool list
    azure batch task list --job-id "job001"

### <a name="listing-resources-efficiently"></a>Efektivní seznam zdrojů

Pro rychlejší dotazu můžete určit **Vyberte** **Filtr**a **rozbalte** klauzule možnosti `list` operace. Chcete-li omezit množství dat vrácených službu dávku pomocí těchto možností. Protože všechny filtry výskytu serverovou ve výkresu protínají lince jen tu část dat, které vás zajímají. Tato klauzule umožňuje uložit šířky pásma (a tedy času) při provádění operací seznamu.

Například to vrátí jenom skupiny, jejichž ID začínat "renderTask":

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Rozhraní příkazového řádku dávku podporuje všechny tři klauzule nepodporuje dávku služby:

* `--select-clause [select-clause]`Vrácení podmnožiny vlastnosti pro každou entitu
* `--filter-clause [filter-clause]`Vrátí jenom entity, které splňují zadaný výraz OData
* `--expand-clause [expand-clause]`Získejte informace o entity telefonuje jednoho podkladového ZBÝVAJÍCÍ. Klauzule rozbalit podporuje pouze `stats` vlastnost v současné době.

Podrobnosti o tři klauzule a provádění dotazů seznamu s nimi najdete v tématu [efektivně zadat dotaz služby Azure dávku](batch-efficient-list-queries.md).

## <a name="application-package-management"></a>Správa aplikací balíčku

Balíčků aplikací lze zjednodušené nasazení aplikace do výpočetního uzlů v vaší fondů. S Azure rozhraní příkazového řádku můžete nahrát balíčků aplikací, Správa verzí balíčku a odstranění balíčků.

Vytvoření nové aplikace a přidání balíčku verze:

**Vytvoření** aplikace:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Přidat** balíčku aplikace:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktivace** balíčku:

    azure batch application package activate "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" zip

Nastavení **Výchozí verze** aplikace:

    azure batch application set "resgroup001" "batchaccount001" "MyTaskApplication" --default-version "1.10-beta3"

### <a name="deploy-an-application-package"></a>Nasazení balíčku aplikace

Při vytváření nového fondu můžete zadat jeden nebo více balíčků aplikací pro nasazení. Při zadávání balíčku času vytvoření fondu ho jako fondu spojení uzel používaný jednotlivých uzlech. Balíčky jsou také nasazeny po restartování nebo reimaged uzel.

Určení `--app-package-ref` při vytváření fond nasazení balíčku aplikace do fondu uzly při připojování fondu parametr. `--app-package-ref` Volbě seznam oddělený středníkem ID aplikace pro nasazení do výpočetního uzlů.

    azure batch pool create --pool-id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

Při vytvoření fondu pomocí parametry příkazového řádku nelze určíte aktuálně *jakou* verzi aplikace balíček pro nasazení na výpočetním uzly, třeba "1.10-beta3". Proto, je třeba nejprve určit výchozí verze aplikace s `azure batch application set [options] --default-version <version-id>` před vytvořením fondu (viz předcházejícího oddílu). Ale pokud můžete si nastavit verze balíčku fondu [JSON soubor](#json-files) místo používat parametrů příkazového řádku pro vytvoření fondu.

Další informace o balíčků aplikací najdete v [nasazení aplikace s Azure dávku balíčků aplikací](batch-application-packages.md).

>[AZURE.IMPORTANT] [Odkaz účet Azure úložiště](#linked-storage-account-autostorage) , musíte ke svému účtu dávku používat balíčků aplikací.

### <a name="update-a-pools-application-packages"></a>Aktualizace fondu a balíčků aplikací

Pokud chcete aktualizovat přiřazená existujícího fondu aplikací, zadejte `azure batch pool set` příkazu s `--app-package-ref` možnost:

    azure batch pool set --pool-id "pool001" --app-package-ref "MyTaskApplication2"

Abyste mohli nasadit nové balíčku aplikace pro výpočet uzlech již existujícího fondu, restartujte nebo třeba reimage tyto uzly:

    azure batch node reboot --pool-id "pool001" --node-id "tvm-3105992504_1-20160930t164509z"

>[AZURE.TIP] Seznam uzlů v fond spolu s jejich uzel ID, můžete získat s `azure batch node list`.

Mějte na paměti, že jste musí už nakonfigurovali aplikace s výchozí verze před nasazením (`azure batch application set [options] --default-version <version-id>`).

## <a name="troubleshooting-tips"></a>Tipy pro odstraňování potíží

Tato část je určená k poskytování prostředků pro použití při odstraňování potíží Azure rozhraní příkazového řádku. Nebudou řešení nutně všech problémů, ale můžou pomoct omezit příčinu a bod, který pomůže zdroje.

* Použití `-h` získat **text nápovědy** k libovolnému příkazu rozhraní příkazového řádku

* Použití `-v` a `-vv` zobrazíte **Podrobný** příkaz výstup; `-vv` je "extra" podrobného a zobrazí skutečné ZBÝVAJÍCÍ požadavky a odpovědi. Přepínače jsou užitečné pro zobrazení celé chyby výstupu.

* Můžete zobrazit **výstup příkazu jako JSON** s `--json` možnost. Například `azure batch pool show "pool001" --json` zobrazí vlastnosti na pool001 ve formátu JSON. Můžete zkopírovat a upravit tento výstup pro použití v `--json-file` (viz [JSON soubory](#json-files) dříve v tomto článku).

* [Fórum komunity dávku na webu MSDN] [ batch_forum] je zdroj skvělé nápovědy a úzce kontroloval členové týmu dávku. Ujistěte se, že pokud narazíte na problémy nebo libovolný text nápovědu k určité operaci tu pošlete svoje otázky.

* Některé zdroje dávku je aktuálně podporován Azure rozhraní příkazového řádku. Například nemůžete zadáte aktuálně balíčku aplikace *verze* pro fond pouze ID balíčku. V takovém případě budete muset zadat `--json-file` příkazu místo použití parametry příkazového řádku. Ujistěte se, že upozorňovat na nové zprávy v rámci nejnovější verze rozhraní příkazového řádku pro vystopovat budoucí vylepšení.

## <a name="next-steps"></a>Další kroky

*  V tématu [nasazení aplikace s balíčků aplikací Azure dávku](batch-application-packages.md) zjistěte, jak tuto funkci lze použít ke správě a nasazení aplikace, které spustit dávku výpočetním uzlů.

* Najdete v tématu [dotazu službu dávku efektivní](batch-efficient-list-queries.md) na další informace o omezení počtu položek a zadejte informace, které budou vráceny dotazů do listu.

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx