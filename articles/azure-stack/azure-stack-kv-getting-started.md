<properties
    pageTitle="Začínáme s Azure zásobníku klíčové trezoru | Microsoft Azure"
    description="Začínáme s používáním trezoru klíč zásobníku Azure"
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
    ms.date="10/18/2016"
    ms.author="ricardom"/>


# <a name="getting-started-with-key-vault"></a>Začínáme s trezoru klíč

Tato část popisuje postup vytvoření trezoru, spravovat klíče a tajemství i povolit uživatelům nebo aplikací k vyvolat operace v trezoru ve vrstvě Azure. Následující kroky předpokládají existuje předplatné klienta a KeyVault služby je zaregistrovaná v rámci tohoto předplatného. Příklad příkazy jsou založené na KeyVault rutin k dispozici v rámci SDK prostředí PowerShell Azure.

## <a name="enabling-the-tenant-subscription-for-vault-operations"></a>Povolení předplatné klienta pro operace trezoru 

Před operace u jakékoli trezoru můžete problém, je třeba zajistit, že vaše předplatné aktivované řešení operace trezoru. Je možné potvrdit, že zadáním následujícího příkazu Powershellu:

    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -AutoSize

Výstup výše uvedený příkaz by měl zprávu "Registrované" pro stav "Registrace" každý řádek.

    ProviderNamespace RegistrationState ResourceTypes Locations
    Microsoft.KeyVault Registered {operations} {local}
    Microsoft.KeyVault Registered {vaults} {local}
    Microsoft.KeyVault Registered {vaults/secrets} {local}
    

 Pokud to není velikost písmen, byste měli vyvolat tento příkaz zaregistrovat KeyVault služby v rámci předplatného:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault

A následující výstup příkazu:
    
    ProviderNamespace : Microsoft.KeyVault
    RegistrationState : Registered
    ResourceTypes : {operations, vaults, vaults/secrets}
    Locations : {local}


>[AZURE.NOTE] Pokud se zobrazí chyba: "*předplatné není registrovaný u Azure klíč trezoru*" při vyvolání KeyVault rutiny potvrďte jste zapnuli automatický přístup zprostředkovatele prostředků KeyVault za výše uvedených pokynů.


## <a name="creating-a-hardened-container-a-vault-in-azure-stack-to-store-and-manage-cryptographic-keys-and-secrets"></a>Vytvoření kontejneru zesílený (trezoru) ve vrstvě Azure k ukládání a správě cryptographic klíče a tajemství

K vytvoření trezoru, by měl ke klientovi nejdřív vytvořit skupinu zdrojů. Následující příkazy Powershellu vytvářet skupina zdroje a potom trezoru v dané skupině zdroje. V příkladu obsahuje taky typické výstup tuto rutinu.

### <a name="creating-a-resource-group"></a>Vytvoření skupiny zdrojů:

    New-AzureRmResourceGroup -Name vaultrg010 -Location local -Verbose -Force

Výstup:

    VERBOSE: Performing the operation "Replacing resource group ..." on target "".
    VERBOSE: 12:52:51 PM - Created resource group 'vaultrg010' in location 'local'
    ResourceGroupName : vaultrg010
    Location : local
    ProvisioningState : Succeeded
    Tags :
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010
    

### <a name="creating-a-vault"></a>Vytvoření trezoru.


    New-AzureRmKeyVault -VaultName vault010 -ResourceGroupName vaultrg010 -Location local -Verbose

Výstup:

    VaultUri : https://vault010.vault.azurestack.local
    TenantId : 5454420b-2e38-4b9e-8b56-1712d321cf33
    TenantName : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Sku : Standard
    EnabledForDeployment : False
    EnabledForTemplateDeployment : False
    EnabledForDiskEncryption : False
    AccessPolicies : {5454420b-2e38-4b9e-8b56-1712d321cf33}
    AccessPoliciesText :
    Tenant ID : 5454420b-2e38-4b9e-8b56-1712d321cf33
    Object ID : ca342e90-f6aa-435b-a11c-dfe5ef0bfeeb
    Application ID :
    Display Name : Tenant Admin (tenantadmin1@msazurestack.onmicrosoft.com)
    Permissions to Keys : get, create, delete, list, update, import, backup, restore
    Permissions to Secrets : all
    OriginalVault : Microsoft.Azure.Management.KeyVault.Vault
    ResourceId : /subscriptions/fa881715-3802-42cc-a54e-a06adf61584d/resourceGroups/vaultrg010/providers/Microsoft.KeyVault/vaults/vault010
    VaultName : vault010
    ResourceGroupName : vaultrg010
    Location : local
    Tags : {}
    TagsTable :
    
Výstup tuto rutinu zobrazí vlastnosti klíčové trezoru, který jste právě vytvořili. Jsou dva nejdůležitější vlastnosti:

-   **Název trezoru**: V tomto příkladu je to **vault010**. Použijete tento název pro jiné rutiny klíč trezoru.

-   **Identifikátor URI trezoru**: V tomto příkladu je to https://vault010.vault.azurestack.local. Aplikace, které využívají trezoru prostřednictvím jeho rozhraní REST API musíte použít tento identifikátor URI.

Váš účet Azure je teď oprávnění provádět všechny operace s trezoru klíče. Co ještě nikdo jiný je.


## <a name="operating-on-keys-and-secrets"></a>Pracují na klíče a tajemství

Po vytvoření trezoru, postupujte pod postup vytvoření spravovat klíče a tajemství:

### <a name="creating-a-key"></a>Vytvoření klíče

Chcete-li vytvořit klávesovou, použijte **Přidat AzureKeyVaultKey** na níže uvedeném příkladu. Po vytvoření úspěšné klíče bude výstupní rutinu nově vytvořený klíčové údaje.

    Add-AzureKeyVaultKey -VaultName \$vaultName -Name\$keyVaultKeyName -Verbose -Destination Software

Následující obrázek je výstup rutiny *AzureKeyVaultKey přidat* :

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff
    
Teď můžete odkázat tento klíč, který jste vytvořili nebo nahrát trezoru klíč Azure pomocí jeho URI. Získáte pomocí **https://vault010.vault.azurestack.local:443/klíče/keyVaultKeyName001** vždy aktuální verzi; a pomocí **https://vault010.vault.azurestack.local:443/klíče/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff** této konkrétní verzi.

### <a name="retrieving-a-key"></a>Načítání klíč

**Get-AzureKeyVaultKey** použijte k načítání klávesy a její podrobnosti na následujícím příkladu:

    Get-AzureKeyVaultKey -VaultName vault010 -Name keyVaultKeyName001

Následující obrázek je výstup Get-AzureKeyVaultKey

    Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes
    Key : {"kid":"https://vault010.vault.azurestack.local/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff","kty":"RSA","key\_ops":\["encrypt"
    ,"decrypt","sign","verify","wrapKey","unwrapKey"\],"n":"nDkcBQCyyLnXtbwFMnXOWzPDqWKiyjf0G3QTxvuN\_MansEn9X-91q4\_WFmRBCd5zWBqz671iuZO\_D4r0P25
    Fe2lAq\_3T1gATVNGR7LTEU9W5h8AoY10bmt4e0y66Jn2vUV-UTCz4\_vtKSKoiuNXHFR\_tGZ-6YX-frqKIiC8pbE4Qvz1x-c7E-eM\_Cpu87koL95n-Hl3wQRQRPXEPRR6gcHR5E74D1
    gLEFCWKySTo4nXtLoeBMNK5QYEBZIAS61ACbR4czjHn6ty-tZeVTc7hyK\_UO2EbJovQIAhyayfq018uNtCBzjjkqJKnY34kviVCPoTQqOdpHa0FHrloe5FeIw","e":"AQAB"}
    VaultName : vault010
    Name : keyVaultKeyName001
    Version : 86062b02b10342688f3b0b3713e343ff
    Id : https://vault010.vault.azurestack.local:443/keys/keyVaultKeyName001/86062b02b10342688f3b0b3713e343ff

### <a name="setting-a-secret"></a>Nastavení tajná

    $secretvalue = ConvertTo-SecureString 'User@123' -AsPlainText -Force
    Set-AzureKeyVaultSecret -Name MySecret-VaultName vault010 -SecretValue $secretvalue
    
Výstup

    Vault Name : vault010
    Name : MySecret
    Version : 65a387f2ed4a416180e852b970846f5b
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret/65a387f2ed4a416180e852b970846f5b
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags : 

### <a name="retrieving-a-secret"></a>Načtení tajná

    Get-AzureKeyVaultSecret -VaultName vault010

Výstup

    Vault Name : vault010
    Name : MySecret
    Version :
    Id : https://vault010.vault.azurestack.local:443/secrets/MySecret
    Enabled : True
    Expires :
    Not Before :
    Created : 8/17/2016 10:49:52 PM
    Updated : 8/17/2016 10:49:52 PM
    Content Type :
    Tags :

Teď klíčové trezoru a klíč nebo tajná je připraven k aplikacím používat.
Musí povolit aplikace jejich použití.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Povolit aplikaci pro použití klávesy nebo tajná 

Povolit aplikaci pro přístup k klíč nebo tajná v trezoru použijte rutinu Set -**AzureRmKeyVaultAccessPolicy** .

Například pokud vaše jméno trezoru je *ContosoKeyVault* a aplikaci, kterou chcete povolit má *ID klienta* *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*a chcete-li povolit aplikaci dešifrování a přihlaste se pomocí klávesy v trezoru, spusťte:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Pokud chcete povolit stejné aplikace číst tajemství v trezoru, spusťte následující:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get


## <a name="next-steps"></a>Další kroky
[Nasazení OM heslem trezoru klíč](azure-stack-kv-deploy-vm-with-secret.md)

[Nasazení OM certifikátem trezoru klíč](azure-stack-kv-push-secret-into-vm.md)