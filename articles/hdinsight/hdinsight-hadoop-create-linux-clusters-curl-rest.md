<properties
    pageTitle="Vytvoření Hadoop, HBase nebo bouře clusterů na Linux v pomocí otočení a rozhraní REST API Azure HDInsight | Microsoft Azure"
    description="Naučte se vytvářet clusterů na základě Linux HDInsight pomocí otočení, správce prostředků Azure šablon a rozhraní REST API Azure. Můžete určit typ obrázku (Hadoop HBase a bouře) nebo pomocí skriptů nainstalovat vlastní součásti."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Vytvoření na základě Linux clusterů v pomocí otočení a rozhraní REST API Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Rozhraní REST API Azure umožňuje provádět správu operace službám v Azure informacemi o platformě, včetně vytvoření nového zdroje, jako jsou založené na Linux HDInsight clusterů. V tomto dokumentu se naučíte se vytvářet správce prostředků Azure šablony konfigurace HDInsight obrázku a související úložiště a pak pomocí otočení nasadit šabloně rozhraní REST API Azure k vytvoření nového clusteru HDInsight.

> [AZURE.IMPORTANT] Kroky v tomto dokumentu pomocí výchozího počtu uzlů kolegy (4) pro HDInsight obrázku. Pokud máte v plánu na víc než 32 pracovníka se při vytvoření obrázku nebo roztažením clusteru po vytvoření, je nutné vybrat velikostí hlavy uzel s 14GB paměti ram a alespoň na úrovni 8 jádra.
>
> Další informace o velikosti uzel a související náklady najdete v článku [ceny HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Zjistit předpoklady pro

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure rozhraní příkazového řádku__. Rozhraní příkazového řádku Azure slouží k vytvoření služby základní, které pak bude použito k vygenerování tokeny ověřování pro požadavky rozhraní REST API Azure.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __otáčení__. Tento nástroj je k dispozici prostřednictvím systému správy balíčku nebo si můžete stáhnout z [http://curl.haxx.se/](http://curl.haxx.se/).

    > [AZURE.NOTE] Pokud používáte prostředí PowerShell spusťte příkazy v tomto dokumentu, musíte nejdřív odebrat `curl` alias, který předtím vytvořil ve výchozím nastavení. Alias používá vyvolat-WebRequest, rutiny prostředí PowerShell místo otočení při použití `curl` příkaz z příkazovém řádku prostředí PowerShell a vrátí chyby pro mnoho příkazů v tomto dokumentu.
    > 
    > Odebrání tohoto aliasu, použijte následující z příkazového řádku prostředí PowerShell:
    >
    > `Remove-item alias:curl`
    >
    > Po odebrání alias by měl nebudou moct používat verzi otočení nainstalované v počítači.

### <a name="access-control-requirements"></a>Požadavky na řízení přístupu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Vytvoření šablony

Azure šablony řízení zdrojů jsou JSON dokumenty, které popisují __pole Skupina zdroje__ a všechny zdroje v něm (například HDInsight.) Tento přístup na základě šablony umožňuje definovat všechny zdroje, které potřebujete pro HDInsight do jedné šablony a správa změn do skupiny jako celek prostřednictvím __nasazení__ použít změny u skupiny.

Šablony jsou k dispozici obvykle ze dvou částí; samotné šablony a parametry souboru, který naplní hodnot konkrétní konfigurace. Pro exmaple, název clusteru, jménem a heslem správce. Použijete-li přímo rozhraní REST API, musíte zkombinovat do jednoho souboru. Formát tento dokument JSON je:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Toto je například spojení soubory šablony a parametrech [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), která vytvoří na základě Linux clusteru zabezpečené SSH uživatelský účet pomocí hesla.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

V tomto příkladě se použije v kroků uvedených v tomto dokumentu. Zástupný symbol _hodnoty_ v části __Parametry__ na konci dokumentu třeba nahradit hodnoty, které chcete použít pro svůj cluster.

##<a name="login-to-your-azure-subscription"></a>Přihlášení k předplatnému Azure

Postupujte podle kroků uvedených v části [připojit ke předplatné Azure z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku)](../xplat-cli-connect.md) a připojovat se k odběru pomocí `azure login` příkaz.

##<a name="create-a-service-principal"></a>Vytvořte hlavní název služby

> [AZURE.NOTE] Tyto kroky jsou zkrácený verzi informace uvedené v části _Ověřit služby základní heslem - Azure rozhraní příkazového řádku_ dokumentu [ověřování služby jistinu pomocí Správce prostředků Azure](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) . Tímto postupem vytvořte novou službu zabezpečení, které mohou sloužit k ověření rozhraní REST API žádosti o použitých k vytvoření Azure zdroje, jako jsou HDInsight obrázku.

1. Z příkazového řádku terminálové relace nebo prostředí, použijte následující příkaz seznam předplatné Azure.

        azure account list
        
    V rozevíracím seznamu vyberte předplatné, které chcete použít a Všimněte si, ve sloupci __Id__ . To je __ID předplatného__ a se použije ve většině kroků v tomto dokumentu.

2. Vytvoření nové aplikace v Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Nahrazení hodnot `--name`, `--home-page`, a `--identifier-uris` s vlastními hodnotami. Zadejte heslo pro nový záznam služby Active Directory.
    
    > [AZURE.NOTE] Protože vytváříte tuto aplikaci k ověření prostřednictvím služeb jistinu `--home-page` a `--identifier-uris` hodnot není potřeba odkazovat skutečné webovou stránku hostovaných na Internetu. jenom musí být jedinečný URI.
    
    Z data vrácená uložte hodnotu __ID aplikace__ .
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Vytvořte hlavní použít hodnotu __ID aplikace__ vráceny dříve službu.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Z data vrácená uložte hodnotu __Id objektu__ .
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. Přiřadíte roli __vlastník__ služby základní použít hodnotu __ID objektu__ vráceny dříve. Také je nutné použít __ID předplatného__ , které jste získali dříve.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Po dokončení tohoto příkazu služby základní má nyní vlastník přístup k ID zadané předplatného.

##<a name="get-an-authentication-token"></a>Získání token ověřování

1. Pomocí následujících vyhledání __ID klienta__ pro vaše předplatné.

        azure account show -s <subscription ID>
        
    Z data vrácená vyhledání __ID klienta__.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Vytvoření nového tokenu pomocí rozhraní REST API Azure.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Nahraďte hodnoty získané nebo nepoužívali __TenantID__, __ID aplikace__a __heslo__ .

    Pokud je tento žádost o úspěšné, obdržíte odpověď 200 řady a obsah odpovědí bude obsahovat JSON dokumentu.

    Dokument JSON vrácené tohoto požadavku bude obsahovat prvku s názvem __access_token__; hodnota: Tento element je přístupový token musíte pomocí ověřování žádosti o použité v dalších částech tohoto dokumentu.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Následující slouží k vytvoření nové skupiny prostředků. Než budete moct vytvářet zdroje, jako jsou clusteru HDInsight nutné nejprve vytvořit skupinu. 

* Nahraďte __SubscriptionID__ ID předplatného dostali při vytváření jistinu služby.
* Nahraďte __AccessToken__ přístupový token dostali v předchozím kroku.
* Nahraďte __DataCenterLocation__ datovém centru, které chcete vytvořit pole Skupina zdroje a materiály, v. Například "Jih centrální nám". 
* Nahraďte název, který chcete použít pro tuto skupinu __ResourceGroupName__ :

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Pokud je tento žádost o úspěšné, se dostanete odpověď 200 řady a těle odpovědi bude obsahovat JSON dokumentu, který obsahuje informace o skupině. `"provisioningState"` Elementu bude obsahovat hodnotu `"Succeeded"`.

##<a name="create-a-deployment"></a>Vytvoření nasazení

Pomocí následující nasazení konfigurace clusteru (šablony a parametr hodnoty) do skupiny zdrojů.

* Nahraďte __SubscriptionID__ a __AccessToken__ hodnotami nepoužívali. 
* Nahraďte __ResourceGroupName__ zdroje název skupiny, kterou jste vytvořili v předchozí části.
* Nahraďte __DeploymentName__ název, který chcete použít pro toto nasazení.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Pokud jste si uložili JSON dokument obsahující šablonu a parametrech do souboru, můžete použít následující místo `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Pokud je tento žádost o úspěšné, obdržíte odpověď 200 řady a těle odpovědi bude obsahovat JSON dokumentu, který obsahuje informace o nasazení operaci.

> [AZURE.IMPORTANT] Všimněte si, že byl odeslán nasazení, ale nebyla dokončena v současné době. Může trvat několik minut, obvykle asi 15; nasazení dokončete.

##<a name="check-the-status-of-a-deployment"></a>Kontrola stavu nasazení

Kontrola stavu nasazení, následujícím způsobem:

* Nahraďte __SubscriptionID__ a __AccessToken__ hodnotami nepoužívali. 
* Nahraďte __ResourceGroupName__ zdroje název skupiny, kterou jste vytvořili v předchozí části.

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Vrátí to informace JSON dokumentu, který obsahuje informace o nasazení operaci. `"provisioningState"` Prvek bude obsahovat stav nasazení; Pokud tato stránka obsahuje hodnotu `"Succeeded"`, pak nasazení byla úspěšně dokončena. V tomto okamžiku svůj cluster by měl být k dispozici pro použití.

##<a name="next-steps"></a>Další kroky

Teď úspěšně jste vytvořili HDInsight clusteru, použijte následující se dozvíte, jak pracovat s svůj cluster. 

###<a name="hadoop-clusters"></a>Hadoop clusterů

* [Použití podregistru s HDInsight](hdinsight-use-hive.md)
* [Použití Prasátko s HDInsight](hdinsight-use-pig.md)
* [Použití MapReduce s HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase clusterů

* [Začínáme s HBase na HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Můžete vyvíjet aplikace Java pro HBase na HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Bouře clusterů

* [Můžete vyvíjet Java topologie pro bouře na HDInsight](hdinsight-storm-develop-java-topology.md)
* [Použití Python součástí bouře na HDInsight](hdinsight-storm-develop-python-topology.md)
* [Nasazení a sledovat topologií s bouře na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
