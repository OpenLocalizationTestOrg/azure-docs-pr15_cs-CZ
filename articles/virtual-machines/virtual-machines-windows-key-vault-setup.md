<properties
    pageTitle="Nastavení trezoru klíč pro virtuálních počítačích v správce prostředků Azure | Microsoft Azure"
    description="Jak nastavit trezoru klíč pro použití s virtuálního počítače správce prostředků Azure."
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
    ms.date="05/31/2016"
    ms.author="singhkay"/>

# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Nastavení trezoru klíč pro virtuálních počítačích v správce prostředků Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu

Ve vrstvě správce prostředků Azure tajemství/certifikáty modelovat jako zdroje informací, které jsou k dispozici zdroje poskytovatel klíč trezoru. Další informace o klíč trezoru najdete v tématu [Co je Azure klíč trezoru?](../key-vault/key-vault-whatis.md)

>[AZURE.NOTE] 
>
>1. Aby trezoru klíč pro použití se správce prostředků Azure virtuálních počítačích, musí být vlastnost **EnabledForDeployment** na klíč trezoru nastavená na hodnotu true. Můžete provedete v různých klientech.
>
>2. Klíč trezoru je potřeba vytvořit ve stejném předplatné a jako Virtual Machine místo konání.

## <a name="use-powershell-to-set-up-key-vault"></a>Nastavit klíč trezoru pomocí prostředí PowerShell
Vytvoření klíčových trezoru pomocí prostředí PowerShell najdete v tématu [Začínáme s Azure klíč trezoru](../key-vault/key-vault-get-started.md#vault).

Pro nové klíčové trezorů můžete použít tuto rutinu Powershellu:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Pro existující klíčové trezorů můžete použít tuto rutinu Powershellu:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a>US rozhraní příkazového řádku nastavit klíč trezoru
Vytvoření klíčových trezoru pomocí rozhraní příkazového řádku (rozhraní příkazového řádku) najdete v tématu [Správa trezoru klíč pomocí rozhraní příkazového řádku](../key-vault/key-vault-manage-with-cli.md#create-a-key-vault).

Pro rozhraní příkazového řádku je nutné před přiřadit zásadu nasazení vytvořit klíčové trezoru. Pomocí následujícího příkazu můžete udělat toto:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a>Pomocí šablon můžete nastavit klíč trezoru
Když použijete šablonu, je potřeba nastavit `enabledForDeployment` vlastnost `true` zdroje klíč trezoru.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Další možnosti, které je možné konfigurovat při vytváření klíčové trezoru pomocí šablon najdete v tématu [vytvoření klíčových trezoru](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
