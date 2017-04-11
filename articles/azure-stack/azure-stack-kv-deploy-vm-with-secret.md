<properties
    pageTitle="Nasazení OM heslem uložené v Azure zásobníku klíč trezoru | Microsoft Azure"
    description="Naučte se nasadit OM heslem uložené v Azure zásobníku klíč trezoru"
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

# <a name="deploy-a-vm-by-retrieving-the-password-stored-in-key-vault"></a>Nasazení virtuálního počítače načtením heslo uložené v trezoru klíč

Když budete potřebovat k předání zabezpečené hodnoty (například hesla) jako parametr během nasazení, můžete uložit hodnotu jako tajná klíčové trezoru Azure zásobníku a odkazovat hodnotu v jiných šablon správce prostředků Azure. Můžete připsat pouze odkaz tajná šablony tajná nikdy vystaven. Není potřeba ručně zadat hodnotu pro tajná pokaždé, když nasadíte zdroje. Můžete určit, kteří uživatelé a objektů služby přístup k tajná.

## <a name="reference-a-secret-with-static-id"></a>Vytvořte odkaz tajná s statická ID

Odkaz tajná z parametry souboru, který splňuje hodnoty do šablony aplikace. Odkaz tajná předáním zdroje identifikátor klíčové trezoru a název tajná. V tomto příkladu musí existovat tajná klíčové trezoru. Použití statická hodnota pro své číslo ID zdroje.

    "parameters": {
    "adminPassword": {
    "reference": {
    "keyVault": {
    "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
    },
    "secretName": "sqlAdminPassword"


>[AZURE.NOTE]Parametr, který přijme tajná by měl být *securestring*.

## <a name="next-steps"></a>Další kroky
[Nasazení aplikace vzorku s trezoru klíč](azure-stack-kv-sample-app.md)

[Nasazení OM certifikátem trezoru klíč](azure-stack-kv-push-secret-into-vm.md)

