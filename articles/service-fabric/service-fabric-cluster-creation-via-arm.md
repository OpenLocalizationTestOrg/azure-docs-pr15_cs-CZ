
<properties
   pageTitle="Vytvoření zabezpečené služby struktury clusteru pomocí Správce prostředků Azure | Microsoft Azure"
   description="Tento článek popisuje, jak nastavit zabezpečeného clusteru struktury služby v Azure pomocí Správce prostředků Azure, Azure klíč trezoru a Azure Active Directory (AAD) pro ověření klienta."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Vytvoření struktury služby obrázku v Azure pomocí Správce prostředků Azure

> [AZURE.SELECTOR]
- [Azure správce prostředků](service-fabric-cluster-creation-via-arm.md)
- [Azure portálu](service-fabric-cluster-creation-via-portal.md)

Toto je podrobného průvodce, který vás provede jednotlivými kroky nastavení zabezpečeného clusteru struktury služby Azure v Azure pomocí Správce prostředků Azure. Tato příručka vás provede následujícími kroky:

 - Nastavte klíč trezoru ke správě klávesové zkratky pro zabezpečení obrázku a aplikace.
 - Vytvoření zabezpečené obrázku v Azure pomocí Správce prostředků Azure.
 - Ověřování uživatelů s Azure Active Directory (AAD) Správa clusteru.

Zabezpečené clusteru je clusteru zabraňuje neoprávněnému přístupu k operace správy zahrnující nasazení, upgradu a odstraňování aplikací služby a data, která obsahují. Nezabezpečené clusteru je clusteru, že kdokoli připojení kdykoli a provedení operace správy. Ačkoli je možné vytvořit nezabezpečené obrázku, je **vhodné k vytvoření zabezpečené clusteru**. Nezabezpečené clusteru **nelze později zabezpečit** – je nutné vytvořit nového obrázku.

Principy jsou stejné pro vytvoření zabezpečené clusterů, ať už jsou clusterů Linux clusterů nebo seskupení Windows. Další informace a pomocné skripty pro vytvoření zabezpečené Linux clusterů najdete v článku [vytvoření zabezpečené clusterů na Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Přihlaste se k Azure
Tato příručka používá [Azure PowerShell][azure-powershell]. Při spuštění novou relaci Powershellu, přihlaste se k účtu Azure a vyberte předplatné před spuštěním Azure příkazy.

Přihlaste se k účtu azure:

```powershell
Login-AzureRmAccount
```

Vyberte předplatné:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Nastavení trezoru klíč

Tato část se provede vytvoření trezoru klíč pro službu struktury cluster v Azure a struktury služeb aplikací. Dokončení Průvodce na klíč trezoru najdete v příručce [klíč trezoru příručku Začínáme s][key-vault-get-started].

Služba struktury používá certifikáty X.509 zabezpečené obrázku a poskytovat funkce zabezpečení aplikace. Azure trezoru klíče slouží ke správě certifikátů pro službu struktury clusterů Azure. Po nasazení cluster v Azure odpovědná za vytvoření clusterů struktury služby Azure zdroje poskytovatele použije certifikáty z trezoru klíče a nainstaluje clusteru VMs.

Následující diagram ukazuje vztah mezi klíč trezoru clusteru služby struktury a zprostředkovatele Azure zdroje, který používá certifikátů uložených v klíč trezoru při vytváření clusteru:

![Instalace certifikátu][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Cílem prvního kroku je vytvořit skupinu zdrojů speciálně pro klíč trezoru. Uvedení klíč trezoru do vlastního pole Skupina zdroje je vhodné. To umožňuje odebrat výpočetním a úložiště skupiny zdrojů, včetně skupina zdroje, který má svůj cluster služby struktury a nepřijít klíče a tajemství. Skupina zdroje, který má trezoru klíč musí být ve stejné oblasti jako obrázku, který jej používá.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Vytvoření klíčových trezoru 

Vytvoření trezoru klíč do nové skupiny prostředků. Klíč trezoru **musí být povolený nasazení** umožňuje zprostředkovatele zdroje služby struktury získání certifikátů z a nainstalovat na uzlech:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Pokud máte existující trezoru klíč, můžete ji povolit pro nasazení pomocí rozhraní Azure příkazového řádku:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Přidání certifikáty trezoru klíč

Používání certifikátů v struktury Služba ověřování a šifrování zajistit různé aspekty clusteru a její využití. Další informace o použití certifikátů v služby struktury, najdete v článku [scénáře zabezpečení služeb struktury clusteru][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Certifikát obrázku a server (povinné) 

Certifikát vyžaduje zabezpečené obrázku a zabránili neoprávněnému přístupu. Poskytuje zabezpečení clusteru několika způsoby:
 
 - **Clusteru ověření:** Ověří komunikace mezi uzly pro federaci obrázku. Pouze uzly, které můžete prokázali identitu s certifikát se můžete připojit clusteru.
 - **Ověřování serveru:** Ověří koncové body správy clusteru správy klientovi, aby klient management mohl ověřit, že je mluvení s reálným obrázku. Certifikát také poskytuje SSL pro rozhraní API pro správu HTTPS a služby struktury Explorer HTTPS.

Pokud chcete slouží k těmto účelům, certifikát musí být splněné tyto požadavky:

 - Certifikát musí obsahovat privátním klíčem.
 - Certifikát musí být vytvořena pro výměnu klíčů, exportovat do souboru osobních informací Exchange (.pfx).
 - Název subjektu certifikát musí odpovídat doménu použitou pro přístup ke clusteru struktury služby. Tento matchng je nutné zadat protokol SSL pro koncové body správy HTTPS a služby struktury Explorer clusteru. Certifikát SSL od certifikační autority (CA) nelze získat pro `.cloudapp.azure.com` domény. Musíte získat vlastního názvu domény pro svůj cluster. Pokud požadujete certifikát od certifikační Autority, název subjektu certifikátu musí odpovídat vlastní název domény použít pro svůj cluster.

### <a name="application-certificates-optional"></a>Certifikáty aplikace (volitelné)

Libovolný počet další certifikáty možné nainstalovat na obrázku z důvodů zabezpečení aplikace. Před vytvořením svůj cluster, zvažte scénáře zabezpečení aplikace, které vyžadují certifikát je třeba nainstalovat v jednotlivých uzlech, jako například:

 - Šifrování a dešifrování hodnot konfigurace aplikace
 - Šifrování dat mezi uzly během replikace 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formátování certifikáty pro použití poskytovatele Azure zdroje

Privátní klíče soubory (.pfx) můžete přidat a používá přímo prostřednictvím klíč trezoru. Však zprostředkovatele Azure prostředků vyžaduje klávesy se šipkami uložené ve formátu zvláštní JSON, který obsahuje .pfx jako base 64 zakódovaný řetězce a privátní klíče heslo. Tak, aby zahrnoval tyto požadavky, musí být klíče umístěná v řetězci JSON a potom uložených jako *tajemství* klíč trezoru.

Usnadnit tento postup je [k dispozici v GitHub]modulu prostředí PowerShell[service-fabric-rp-helpers]. Postupujte podle těchto pokynů prodloužíte pomocí modulu:

  1. Stáhněte celý obsah repo do místního adresáře. 
  2. Importujte modulu v okně Powershellu:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
`Invoke-AddCertToKeyVault` Příkaz v tomto prostředí PowerShell modulu automaticky naformátuje privátním klíčem certifikátu do řetězec formátu JSON a odešle ji do klíč trezoru. Můžete přidat certifikát obrázku a všechny další aplikace certifikáty do klíč trezoru. Opakujte tento krok pro všechny další certifikáty, které chcete nainstalovat v svůj cluster.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Předchozí řetězce jsou všechny předpoklady trezoru klíč pro konfiguraci služby struktury clusteru správce prostředků šablonu, která nainstaluje certifikáty uzel ověřování, Správa koncový bod zabezpečení a ověřování a jakékoli další zabezpečení aplikace, které používají certifikáty X.509. V tomto okamžiku nyní byste měli mít následující nastavení v Azure:

 - Pole Skupina zdroje trezoru klíč
   - Klíčové trezoru
     - Ověřovací certifikát serveru obrázku
     - Certifikáty aplikace

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Nastavení služby Azure Active Directory pro ověření klienta

AAD umožňuje organizacím (označované jako klienti) spravovat přístup uživatelů k aplikacím, které jsou rozdělit do aplikace založené na webu přihlášení uživatelského rozhraní a aplikací native client – možnosti. V tomto dokumentu předpokladu, že jste již vytvořili klienta. Pokud ne, začněte tím, [Jak získat tenanta služby Azure Active Directory]pro čtení[active-directory-howto-tenant].

Služba struktury clusteru nabízí několik vstupní body jeho funkce správy, včetně webových [Služeb struktury Průzkumníka] [ service-fabric-visualizing-your-cluster] a [Visual Studio][service-fabric-manage-application-in-visual-studio]. V důsledku toho vytvoříte dvě AAD žádosti o řízení přístupu k clusteru, jedné webové aplikace a jeden nativní aplikaci.

Zjednodušit některé kroky při konfiguraci AAD pomocí služby struktury clusteru, jsme vytvořili sadu skripty Windows Powershellu.

>[AZURE.NOTE] Je třeba provést tyto kroky *před* vytvořením clusteru tak v případech, kdy skripty očekávat clusteru jména a koncové body, musí být plánované hodnot, ne těch, které jste už vytvořili.

1. [Stáhněte si tyto skripty] [ sf-aad-ps-script-download] s vaším počítačem.

2. Klikněte pravým tlačítkem myši na soubor zip, vyberte **Vlastnosti**a pak zaškrtněte políčko **Odblokovat** a použít.

3. Extrahujte zip soubor.

4. Spuštění `SetupApplications.ps1`, poskytnutí TenantId, název_clusteru a WebApplicationReplyUrl jako parametry. Příklad:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Vyhledání **TenantId** spuštěním příkazu Powershellu "Get-AzureSubscription" ". Zobrazí se **TenantId** pro každý předplatné.

    **Název_clusteru** slouží k připojit znak před aplikací AAD vytvořené pomocí skriptu. Nemusí shodovat s názvem skutečné clusteru přesně tak, jak je určená jenom k usnadnit AAD artefakty namapovat clusteru struktury služby, který jste společně s.

    **WebApplicationReplyUrl** je výchozí koncový bod vracející AAD uživatelům po dokončení procesu přihlášení. Je vhodné nastavit to koncový bod služby struktury Explorer pro svůj cluster, která je ve výchozím nastavení:

    https://&lt;cluster_domain&gt;: 19080/Průzkumníka

    Zobrazí se výzva k přihlášení k účtu, který má oprávnění správce klienta AAD. Po odebrání skript vytvářejí vytvořit web a nativní aplikace představující svůj cluster struktury služby. Když se podíváte na klientovi aplikací [Azure klasické portál][azure-classic-portal], byste měli vidět dva nové položky:

    - *Název_clusteru*\_obrázku
    - *Název_clusteru*\_klienta

    Vytiskne skript Json požadované šablonou správce prostředků Azure při vytváření clusteru v následující části tak zachovat Powershellu okno.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Vytvoření šablony služby struktury clusteru správce prostředků

V této části výstup předchozí příkazy Powershellu slouží v šabloně služby struktury clusteru správce prostředků.

Správce prostředků ukázkové šablony jsou k dispozici v [galerii šablon Azure rychlého na GitHub][azure-quickstart-templates]. Tyto šablony mohou sloužit jako výchozí bod pro šablonu obrázku. 

### <a name="create-the-resource-manager-template"></a>Vytvoření šablony správce prostředků

Tato příručka používá [zabezpečeného clusteru 5 uzel] [ service-fabric-secure-cluster-5-node-1-nodetype-wad] příklad šablony a parametrů šablony. Stáhněte si `azuredeploy.json` a `azuredeploy.parameters.json` do počítače a otevřete oba soubory v oblíbených textovém editoru.

### <a name="add-certificates"></a>Přidání certifikátů

Certifikáty se přidají do šablony správce prostředků clusteru odkazem trezoru klávesy, která obsahuje klíče certifikátu. Doporučuje se, že tyto hodnoty klíče trezoru jsou umístěná v souboru Správce prostředků šablony parametry chcete zachovat správce prostředků opakovaně použitelný soubor šablony a uvolnit hodnot specifické pro nasazení.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>Přidání všech certifikátů VMSS osProfile

V části osProfile VMSS zdroje (Microsoft.Compute/virtualMachineScaleSets) musí být nakonfigurované každé certifikát, který je potřeba nainstalovat clusteru. Tím se nastaví zprostředkovatele prostředků nainstalovat certifikát na VMs. Jedná se o certifikátu obrázku a také všechny certifikáty zabezpečení aplikace, který budete chtít používat pro vaše aplikace:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Konfigurace služby struktury clusteru certifikátu

Ověřovací certifikát clusteru také v prostředku clusteru služby struktury (Microsoft.ServiceFabric/clusters) a rozšíření struktury služby pro nakonfigurovaný VMSS v VMSS zdroje. Díky poskytovatele služby struktury zdrojů ji nakonfigurovat pro použití u obrázku a ověřování serveru pro správu koncové body.

##### <a name="vmss-resource"></a>VMSS zdroje:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Služba struktury zdrojů:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>Vložení AAD konfigurace

Konfigurace AAD vytvořili lze vložit přímo do Správce prostředků šablony, ale doporučuje se extrahovat hodnoty do parametry nejdřív do souboru parametry k synchronizaci šabloně správce prostředků opakovaně použitelný a bezplatné hodnot konkrétní na nasazení.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < "Konfigurace arm" ></a>parametry šablony konfigurace správce prostředků

Nakonec použijte výstupní hodnoty z trezoru klíče a AAD Powershellu příkazy k naplnění parametry souboru:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
V tomto okamžiku nyní byste měli mít takto:

 - Pole Skupina zdroje trezoru klíč
    - Klíčové trezoru
    - Ověřovací certifikát serveru obrázku
    - Certifikát zašifrování dat
 - Azure Active Directory klienta 
    - AAD aplikace pro správu webu a služby struktury Explorer
    - AAD aplikaci pro správu native client –
    - Uživatelé, kteří mají přiřazené role 
 - Služba struktury clusteru správce prostředků šablony
    - Certifikáty nakonfigurované pomocí klávesy trezoru
    - Azure Active Directory nakonfigurovaná 

Následující diagram ukazuje, kde konfigurace klíč trezoru a AAD zapadají do celkového konceptu šablony správce prostředků.

![Správce prostředků závislost mapy][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Vytvoření clusteru

Teď jste připravení na vytvoření clusteru pomocí [ARM nasazení][resource-group-template-deploy].

#### <a name="test-it"></a>Vyzkoušejte

Pomocí následujícího příkazu Powershellu k testování šablony správce prostředků s parametry souboru:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Nasazení

Pokud test šablony správce prostředků projde, nasazení šablony správce prostředků se souborem parametrů pomocí následujícího příkazu Powershellu:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Přiřazení uživatelů k rolím

Po vytvoření aplikace pro svůj cluster potřebujete přiřadit uživatelům k rolím nepodporuje služby struktury: jen pro čtení a správce. Můžete dělat pomocí [Azure klasické portál][azure-classic-portal].

1. Přejděte do vašeho tenanta a zvolte aplikace.
2. Zvolte webové aplikace, který má název, jako je `myTestCluster_Cluster`.
3. Klikněte na kartu uživatelů.
4. Zvolte uživateli přiřadit a klikněte na tlačítko **přiřadit** v dolní části obrazovky.

    ![Přiřazení uživatelů k tlačítku role][assign-users-to-roles-button]

5. Vyberte roli přiřadit uživatelům.

    ![Přiřazení uživatelů k rolím][assign-users-to-roles-dialog]

>[AZURE.NOTE] Další informace o rolích v služby struktury najdete v článku [řízení přístupu na základě rolí pro klienty služby struktury](service-fabric-cluster-security-roles.md).

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Vytvoření zabezpečené clusterů na Linux

Usnadnit procesu skript helper získala [tady](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Pro používání tohoto skriptu Pomocník, předpokládá se, že už máte nainstalovaný rozhraní příkazového řádku Azure a nebude v path. Ujistěte se, že skript má oprávnění k provedení spuštěním `chmod +x cert_helper.py` po si ji stáhnete. Cílem prvního kroku je k přihlášení k účtu Azure pomocí rozhraní příkazového řádku s `azure login` příkaz. Po přihlášení k účtu Azure použijte Pomocníka s certifikátu podepsaného CA, jak ukazuje následující příkaz:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Tento příkaz vrátí následující tři řetězce jako výstup: 

1. SourceVaultID, což je ID pro nové KeyVault ResourceGroup vytvořen za vás. 

2. CertificateUrl pro přístup k certifikátu.

3. Miniatura certifikátu, která se použije pro ověření.


Následující příklad ukazuje, jak používat příkaz:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Spuštění příkazu předchozí nabízí tři řetězce následujícím způsobem:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 Název subjektu certifikát musí odpovídat doménu použitou pro přístup ke clusteru struktury služby. Toto je nutné zadat protokol SSL pro koncové body správy HTTPS a služby struktury Explorer clusteru. Certifikát SSL od certifikační autority (CA) nelze získat pro `.cloudapp.azure.com` domény. Musíte získat vlastního názvu domény pro svůj cluster. Pokud požadujete certifikát od certifikační Autority název subjektu certifikátu musí odpovídat vlastní název domény použít pro svůj cluster.

Jedná se o položky potřebných pro vytvoření clusteru struktury služba zabezpečené (bez AAD), jak je uvedeno na [Parametry šablony konfigurace správce prostředků](#configure-arm). Můžete se připojit ke zabezpečené clusteru pomocí pokynů v [ověřování klientského přístupu k clusteru](service-fabric-connect-to-secure-cluster.md). Náhled clusterů Linux nepodporují AAD ověřování. Můžete přiřadit role správců a klienta podle popisu v části [přiřazení role pro uživatele](#assign-roles). Při určování role správce a klienta pro náhled cluster Linux, je třeba zadat thumbprints certifikát pro ověřování (na rozdíl od název subjektu, protože žádné ověření řetězec nebo zrušení probíhá v této verzi preview).


Pokud chcete pomocí certifikátu podepsaného svým držitelem pro účely testování, můžete použít stejné skript k vytvoření certifikátu podepsaného svým držitelem a nahrajte ho do KeyVault, zadáním příznak `ss` nikoli adresu certifikát cestu a název certifikátu. Například najdete v článku pro vytváření a nahrávání certifikátu podepsaného svým držitelem tento příkaz:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Tento příkaz vrátí stejné tři SourceVault, CertificateUrl a Miniatura certifikátu, která slouží k vytvoření zabezpečené Linux clusteru, spolu s umístění umístění podepsaného svým držitelem řetězce. Budete potřebovat certifikátu podepsaného svým držitelem pro připojení k clusteru.  Můžete se připojit ke zabezpečené clusteru pomocí pokynů v [ověřování klientského přístupu k clusteru](service-fabric-connect-to-secure-cluster.md). Název subjektu certifikát musí odpovídat doménu použitou pro přístup ke clusteru struktury služby. Toto je nutné zadat protokol SSL pro koncové body správy HTTPS a služby struktury Explorer clusteru. Certifikát SSL od certifikační autority (CA) nelze získat pro `.cloudapp.azure.com` domény. Musíte získat vlastního názvu domény pro svůj cluster. Pokud požadujete certifikát od certifikační Autority název subjektu certifikátu musí odpovídat vlastní název domény použít pro svůj cluster.

Parametry uvedené skriptem helper můžete zadat portálu podle popisu v části [Vytvoření obrázku na portálu Azure](service-fabric-cluster-creation-via-portal.md#create-cluster-portal).

## <a name="next-steps"></a>Další kroky

Nyní máte zabezpečené obrázku s ověřováním správy poskytování služby Azure Active Directory. [Připojení k svůj cluster](service-fabric-connect-to-secure-cluster.md) další, a zjistěte, jak [Spravovat aplikace tajemství](service-fabric-application-secret-management.md).

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Odstranění problémů s nastavením služby Azure Active Directory pro ověření klienta

Pokud narazíte na chyby při nastavování služby Azure Active Directory pro ověření klienta, zkontrolujte následující suggestings možná řešení.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Služba struktury Explorer pokynů pro výběr certifikátu

#### <a name="problem"></a>Problém

Po přihlášení úspěšně na přihlašovací stránce AAD v Průzkumníku struktury služby prohlížeči vrátí na domovskou stránku ale zobrazí výzvu dialogové okno pro výběr certifikát.

![Dialogové okno Vybrat certifikát SFX][sfx-select-certificate-dialog]

#### <a name="reason"></a>Důvod, proč

Uživateli nebyl přiřadit roli v aplikaci AAD obrázku. Proto AAD ověření se nezdaří služby struktury clusteru. Služba struktury Explorer přejde zpět k ověření certifikátů.

#### <a name="solution"></a>Řešení

Postupujte podle pokynů k nastavení AAD a přiřazení role uživatelů. Navíc "uživatele PŘIŘAZENÍ povinné k přístupu" se doporučuje aplikace zapnout jako `SetupApplications.ps1` znamená.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Připojení pomocí prostředí PowerShell dojde k chybě se chyba: Zadaná pověření jsou neplatné

#### <a name="problem"></a>Problém

Při použití Powershellu ke připojit k obrázku použití režimu zabezpečení "AzureActiveDirectory", po přihlášení úspěšně na přihlašovací stránce AAD, připojení se nezdaří s chybou: Zadaná pověření jsou neplatné vidět.

#### <a name="solution"></a>Řešení

Stejně jako nahoře.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Průzkumník struktury služby podepisování naopak selhání: AADSTS50011

#### <a name="problem"></a>Problém

Po přihlášení na přihlašovací stránce v Průzkumníku struktury služby AAD stránky Vrátí znaménko selhání - AADSTS50011: na zpáteční adresu &lt;url&gt; neodpovídá odpovědět adresy nakonfigurované pro aplikace: &lt;guid&gt;. 

![Neodpovídají SFX zpáteční adresu][sfx-reply-address-not-match]

#### <a name="reason"></a>Důvod, proč

Aplikaci cluster(web) představující služby struktury Explorer pokusy o ověřování AAD jako součást žádost poskytuje přesměrování zpáteční adresu URL. Ale není uvedený v seznamu AAD aplikace odpovědět URL.

#### <a name="solution"></a>Řešení

Adresu url služby struktury Explorer na kartě Konfigurovat aplikace cluster(web) přidat odpověď URL nebo nahrazení jednu z položek v seznamu. Uložte.

![Adresa url webové aplikace odpověď][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Můžete znovu použít stejné AAD klienta více clusterů?

#### <a name="answer"></a>Answer (odpověď)

Ano. Ale nezapomeňte přidat adresu URL s služby struktury Průzkumníkem cluster(web) aplikace, který nebude fungovat jinak Explorer struktury služby.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Proč potřebuju ještě certifikát serveru povoleno AAD?

#### <a name="answer"></a>Answer (odpověď)

FabricClient FabricGateway konfigurovat vzájemné ověření. V případě AAD ověřování AAD integrace poskytuje identity klienta na server a certifikát serveru slouží k ověření identity serveru. Další informace o tom, jak funguje certifikát na služby struktury kontrola [certifikáty X.509 a struktury služby][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png