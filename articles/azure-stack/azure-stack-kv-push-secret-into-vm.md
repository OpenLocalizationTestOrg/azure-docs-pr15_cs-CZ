<properties
    pageTitle="Nasazení virtuálního počítače pomocí certifikátu pomocí Azure zásobníku klíč trezoru | Microsoft Azure"
    description="Zjistěte, jak nasazení virtuálního počítače a vloží certifikát z trezoru klíč zásobníku Azure"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="create-vms-and-include-certificates-retrieved-from-key-vault"></a>Vytvoření VMs a zahrnout certifikáty načtené z trezoru klíč

Ve vrstvě Azure VMs nasazení prostřednictvím Správce prostředků Azure a certifikáty můžete uložit v Azure zásobníku klíč trezoru. Pak Azure zásobníku (Microsoft.Compute zdroje poskytovatele specifickou) předá je do vaší VMs při zavedení VMs. Certifikáty lze použít v mnoha případech, včetně SSL, šifrování a ověřování na základě certifikátů.

Tímto způsobem můžete nechat certifikát bezpečné. Je teď v OM obrázek nebo v souborech konfigurace aplikace nebo někde jinde nebezpečné. Nastavením příslušné zásady přístupu pro klíčové trezoru můžete určit, kdo získá přístup k certifikátu. Další výhodou je, že zvládne nápor vlastních certifikátů na jednom místě v Azure zásobníku klíč trezoru.

Tady je rychlý přehled o celém postupu:

-   Potřebujete certifikát ve formátu .pfx.

-   Vytvoření klíčových trezoru (pomocí šablony nebo následující ukázkový skript).

-   Zkontrolujte, že jste zapnuli přepínačem EnabledForDeployment.

-   Nahrajte certifikát jako tajná.

## <a name="deploying-vms"></a>Nasazení VMs

Tento ukázkový skript vytvoří klíčové trezoru a potom ukládá certifikáty uložené v soubor .pfx místního adresáře do klíčové trezoru jako tajná.

    $vaultName = "contosovault"
    $resourceGroup = "contosovaultrg"
    $location = "local"
    $secretName = "servicecert"
    $fileName = "keyvault.pfx"
    $certPassword = "abcd1234"

    # =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

    $fileContentBytes = get-content \$fileName -Encoding Byte

    $fileContentEncoded =
    [System.Convert\]::ToBase64String(\$fileContentBytes)
    $jsonObject = @"
    {
    "data": "\$filecontentencoded",
    "dataType" :"pfx",
    "password": "\$certPassword"
    }
    @$jsonObjectBytes = [System.Text.Encoding\]::UTF8.GetBytes(\$jsonObject)
    $jsonEncoded = \[System.Convert\]::ToBase64String(\$jsonObjectBytes)
    Switch-AzureMode -Name AzureResourceManager
    New-AzureResourceGroup -Name \$resourceGroup -Location \$location
    New-AzureKeyVault -VaultName \$vaultName -ResourceGroupName
    $resourceGroup -Location \$location -sku standard -EnabledForDeployment
    $secret = ConvertTo-SecureString -String \$jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName \$vaultName -Name \$secretName -SecretValue \$secret

První část skript přečte soubor .pfx a potom ukládá ho ve formátu JSON objekt s na soubor obsahu ve formátu Base 64 kódování. Objekt JSON je také ve formátu Base 64 kódování.

Potom vytvoří nové skupiny prostředků a potom vytvoří klíčové trezoru. Poznámka: Parametr posledního příkazu Nový AzureKeyVault "-EnabledForDeployment", která uděluje přístup k Azure (konkrétně zprostředkovatele prostředků Microsoft.Compute) číst tajemství z klíče trezoru nasazení.

Posledního příkazu jednoduše ukládá JSON objekt ve formátu Base 64 zakódovaný klíčové trezoru jako tajná.

Tady je ukázka výstupu z předchozího skriptu:

    VERBOSE:  – Created resource group 'contosovaultrg' in
    location
    'eastus'
    ResourceGroupName : contosovaultrg
    Location          : eastus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions NotActions
                        ======= ==========
                        \*
    ResourceId        :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56/re
    sourceGroups/contosovaultrg
    VaultUri             : https://contosovault.vault.azure.net
    TenantId             : xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
    TenantName           : xxxxxxxx
    Sku                  : standard
    EnabledForDeployment : True
    AccessPolicies       : {xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47}
    AccessPoliciesText   :
                           Tenant ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-2d7cd011db47
                           Object ID              :
                           xxxxxxxx-xxxx-xxxx-xxxx-b092cebf0c80
                           Application ID         :
                           Display Name           : Derick Developer  (derick@contoso.com)
                           Permissions to Keys    : get, create, delete,
                           list, update, import, backup, restore
                           Permissions to Secrets : all
    OriginalVault        : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId           :
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-dd149b4aeb56                 
    /resourceGroups/contosovaultrg/providers/Microsoft.KeyV
                           ault/vaults/contosovault
    VaultName            : contosovault
    ResourceGroupName    : contosovaultrg
    Location             : eastus
    Tags                 : {}
    TagsTable            :
    SecretValue     : System.Security.SecureString
    SecretValueText :
    ew0KImRhdGEiOiAiTUlJSkN3SUJBekNDQ01jR0NTcUdTSWIzRFFFSEFh
    Q0NDTGdFZ2dpME1JSUlzRENDQmdnR0NTcUdTSWIzRFFFSEFhQ0NCZmtF           
    Z2dYMU1JSUY4VENDQmUwR0N5cUdTSWIzRFFFTUNnRUNvSUlFL2pDQ0JQ
    &lt;&lt;&lt; Output truncated… &gt;&gt;&gt;
    Attributes      :
    Microsoft.Azure.Commands.KeyVault.Models.SecretAttributes
    VaultName       : contosovault
    Name            : servicecert
    Version         : e3391a126b65414f93f6f9806743a1f7
    Id              :
    https://contosovault.vault.azure.net:443/secrets/servicecert
                      /e3391a126b65414f93f6f9806743a1f7

Teď můžeme připraveni nasazení šablony OM. Poznámka: URI tajná z výstupu (jako zvýrazněný v předchozím výstup zelenou barvou).

Budete potřebovat šablony umístěné v tomto poli. Parametry systému zajímají (kromě obvykle OM parametry) jsou názvu trezoru, skupina zdroje trezoru a tajné URI. Samozřejmě můžete stáhnout z GitHub a změňte podle potřeby.

Při nasazení tento OM Azure vloží certifikátu do OM.
Ve Windows se přidají certifikáty v soubor .pfx soukromým klíčem není možné exportovat. Certifikát bude přidán do umístění certifikátů LocalMachine, se obchodu certifikát, který uživatel zadal. Linux, soubor certifikátu je uložena v adresáři /var/lib/waagent s názvem souboru &lt;UppercaseThumbprint&gt;CRT X509 soubor certifikátu a &lt;UppercaseThumbprint&gt;.prv privátním klíčem.
Jsou obě tyto soubory .pem formátu.

Aplikace obvykle najde certifikátu pomocí Miniatura a nevyžaduje změny.

## <a name="retiring-certificates"></a>Do důchodu certifikátů


V předchozí části jsme ukázal, jak použít nový certifikát pro existující VMs. Ale starý certifikát je pořád v OM a nejde odebrat. Zvýšit zabezpečení můžete změnit atribut staré tajná "Zakázáno", takže i v případě staré šablony se pokusí vytvořit virtuálního počítače s touto verzí staré certifikátu, bude. Zde je, jak nastavit konkrétní tajné verzi zakázat:

    Set-AzureKeyVaultSecretAttribute -VaultName contosovault -Name servicecert -Version e3391a126b65414f93f6f9806743a1f7 -Enable 0

## <a name="conclusion"></a>Uzavření


Tímto způsobem nový certifikát můžete uchovávat nezávislá na obrázek OM nebo datové aplikace. Tak, aby byla odebrána jeden bod zobrazení.

Certifikát můžete taky obnovit a nahrát do klíč trezoru aniž by bylo nutné znovu vytvořit obrázek OM nebo balíček pro nasazení aplikace. Aplikace je ještě potřeba dodaného s novou URI pro nové verze certifikát přes.

Oddělením certifikát od OM nebo datové aplikace jsme teď mít omezenou číslo osoby, které se mají přímý přístup k certifikátu.

Výhodou Teď máte jeden praktické místo klávesy trezoru ke správě všechny certifikáty, včetně všechny verze, která byla nasazená určitou dobu.

## <a name="next-steps"></a>Další kroky

[Nasazení OM heslem trezoru klíč](azure-stack-kv-deploy-vm-with-secret.md)

[Povolení aplikace pro přístup k trezoru klíč](azure-stack-kv-sample-app.md)