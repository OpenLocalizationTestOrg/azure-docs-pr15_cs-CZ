<properties
    pageTitle="Nastavení přístupu WinRM pro virtuálních počítačích v správce prostředků Azure | Microsoft Azure"
    description="Jak nastavit WinRM přístup pro použití s virtuálního počítače správce prostředků Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="singhkay"/>

# <a name="setting-up-winrm-access-for-virtual-machines-in-azure-resource-manager"></a>Nastavení přístupu WinRM pro virtuálních počítačích v správce prostředků Azure

## <a name="winrm-in-azure-service-management-vs-azure-resource-manager"></a>WinRM a Správa služby Azure správce prostředků Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu

* Základní informace o správce prostředků Azure najdete v tomto [článku](../azure-resource-manager/resource-group-overview.md)
* Rozdíly mezi Správa služby Azure a správce prostředků Azure najdete v tomto [článku](../resource-manager-deployment-model.md)

Klíčové rozdíl v nastavení konfigurace WinRM mezi dvěma štosech se vypočítá následujícím jak získá certifikát nainstalovaných OM. Certifikáty jsou ve vrstvě správce prostředků Azure modelovat jako zdroje spravuje poskytovatel klíč trezoru zdroje. Proto uživatel musí zadat vlastní certifikát a nahrát do trezoru klíč než ho začnete používat v virtuálního počítače.

Tady je postup, musíte provést k nastavení virtuálního počítače s připojením WinRM

1. Vytvoření klíčových trezoru
2. Vytvoření certifikátu podepsaného svým držitelem
3. Odeslat certifikát podepsaný svým držitelem trezoru klíč
4. Získání adresy URL pro váš certifikátu podepsaného svým držitelem trezoru klíč
5. Vytvořte odkaz svoji adresu URL podepsaného certifikátů při vytváření virtuálního počítače

## <a name="step-1-create-a-key-vault"></a>Krok 1: Vytvoření klíčových trezoru

Můžete použít pod příkazem Vytvořit trezoru klíč

```
New-AzureRmKeyVault -VaultName "<vault-name>" -ResourceGroupName "<rg-name>" -Location "<vault-location>" -EnabledForDeployment -EnabledForTemplateDeployment
```

## <a name="step-2-create-a-self-signed-certificate"></a>Krok 2: Vytvoření certifikátu podepsaného svým držitelem
Vytvoření certifikátu podepsaného svým držitelem pomocí tohoto skriptu prostředí PowerShell

```
$certificateName = "somename"

$thumbprint = (New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation Cert:\CurrentUser\My -KeySpec KeyExchange).Thumbprint

$cert = (Get-ChildItem -Path cert:\CurrentUser\My\$thumbprint)

$password = Read-Host -Prompt "Please enter the certificate password." -AsSecureString

Export-PfxCertificate -Cert $cert -FilePath ".\$certificateName.pfx" -Password $password
```

## <a name="step-3-upload-your-self-signed-certificate-to-the-key-vault"></a>Krok 3: Odeslání certifikátu podepsaného svým držitelem do trezoru klíč

Před nahráním certifikátu do trezoru klíč vytvořili v kroku 1, je nutné převést do formátu, který bude srozumitelný zprostředkovatele Microsoft.Compute prostředků. Pod prostředí PowerShell vám umožní skript, můžete to udělat

```
$fileName = "<Path to the .pfx file>"
$fileContentBytes = Get-Content $fileName -Encoding Byte
$fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

$jsonObject = @"
{
  "data": "$filecontentencoded",
  "dataType" :"pfx",
  "password": "<password>"
}
"@

$jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
$jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

$secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText –Force
Set-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>" -SecretValue $secret
```

## <a name="step-4-get-the-url-for-your-self-signed-certificate-in-the-key-vault"></a>Krok 4: Získání adresy URL pro váš certifikátu podepsaného svým držitelem trezoru klíč

Zprostředkovatel zdroje Microsoft.Compute potřebuje adresy URL tajná uvnitř trezoru klíče při zřizování OM. Umožňuje zprostředkovatele prostředků Microsoft.Compute ke stažení tajná a vytvořit odpovídající certifikát v OM.

>[AZURE.NOTE]Adresa URL tajná musí obsahovat také verzi. Ukázková adresa URL vypadá pod https://contosovault.vault.azure.net:443/tajemství/contososecret/01h9db0df2cd4300a20ence585a6s7ve


#### <a name="templates"></a>Šablony

Můžete získat odkaz na adresu URL do šablony pomocí pod kód

    "certificateUrl": "[reference(resourceId(resourceGroup().name, 'Microsoft.KeyVault/vaults/secrets', '<vault-name>', '<secret-name>'), '2015-06-01').secretUriWithVersion]"

#### <a name="powershell"></a>Prostředí PowerShell

Můžete získat adresu URL pomocí pod PowerShell command

    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id

## <a name="step-5-reference-your-self-signed-certificates-url-while-creating-a-vm"></a>Krok 5: Odkazovat svoji adresu URL podepsaného certifikátů při vytváření virtuálního počítače

#### <a name="azure-resource-manager-templates"></a>Azure správce prostředků šablony

Při vytváření OM pomocí šablony, certifikát získá které odkazují tajemství a oddílu winRM jako níže:

    "osProfile": {
          ...
          "secrets": [
            {
              "sourceVault": {
                "id": "<resource id of the Key Vault containing the secret>"
              },
              "vaultCertificates": [
                {
                  "certificateUrl": "<URL for the certificate you got in Step 4>",
                  "certificateStore": "<Name of the certificate store on the VM>"
                }
              ]
            }
          ],
          "windowsConfiguration": {
            ...
            "winRM": {
              "listeners": [
                {
                  "protocol": "http"
                },
                {
                  "protocol": "https",
                  "certificateUrl": "<URL for the certificate you got in Step 4>"
                }
              ]
            },
            ...
          }
        },

Ukázková šablona pro výše uvedené, najdete tady na [201 OM – winrm-keyvault-systému windows](https://azure.microsoft.com/documentation/templates/201-vm-winrm-keyvault-windows)

Zdrojový kód pro tuto šablonu se nachází na [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)

#### <a name="powershell"></a>Prostředí PowerShell

    $vm = New-AzureRmVMConfig -VMName "<VM name>" -VMSize "<VM Size>"
    $credential = Get-Credential
    $secretURL = (Get-AzureKeyVaultSecret -VaultName "<vault name>" -Name "<secret name>").Id
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName "<Computer Name>" -Credential $credential -WinRMHttp -WinRMHttps -WinRMCertificateUrl $secretURL
    $sourceVaultId = (Get-AzureRmKeyVault -ResourceGroupName "<Resource Group name>" -VaultName "<Vault Name>").ResourceId
    $CertificateStore = "My"
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore $CertificateStore -CertificateUrl $secretURL

## <a name="step-6-connecting-to-the-vm"></a>Krok 6: Připojení k OM
Abyste se mohli připojit k OM, budete muset zkontrolujte počítač nakonfigurovaný pro WinRM Vzdálená správa. Spuštění prostředí PowerShell jako správce a provedení pod příkaz a ujistěte se, že nastavíte.

    Enable-PSRemoting -Force

>[AZURE.NOTE] Možná budete muset Ujistěte se, že pokud výše uvedených možností nefunguje běží služba WinRM. Můžete dělat této pomocí`Get-Service WinRM`

Po dokončení instalace se můžete připojit k použití OM pod příkaz

    Enter-PSSession -ConnectionUri https://<public-ip-dns-of-the-vm>:5986 -Credential $cred -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck) -Authentication Negotiate