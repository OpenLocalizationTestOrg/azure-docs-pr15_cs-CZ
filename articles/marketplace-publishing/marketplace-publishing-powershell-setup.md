<properties
   pageTitle="Nastavení prostředí PowerShell vytvořit virtuálního počítače pro Tržiště | Microsoft Azure"
   description="Pokyny k nastavení prostředí PowerShell Azure a jeho použití jako volitelné proces tok vytvořit OM obrázky do kterých se nasadí a prodávat na Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/04/2016"
   ms.author="hascipio"/>

# <a name="set-up-azure-powershell-to-create-an-offer-for-the-azure-marketplace"></a>Nastavení prostředí PowerShell Azure k vytvoření nabídky pro Azure Marketplace
Podrobné informace o tom, jak nastavit Powershellu v Azure najdete v tématu [jak instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md). Jednoduchý přístup je použijte metodu certifikát, který importuje certifikát potřebný pro ověřování a soubory ke stažení. Jak získat potřebné certifikát, získáte pomocí rutiny **Get-AzurePublishSettingsFile** . Po zobrazení výzvy, uložte soubor. Import certifikátu do relaci Powershellu, získáte pomocí rutiny **AzurePublishSettingsFile importovat** .

Ke konfiguraci a uložení nastavení předplatného Microsoft Azure pro relaci Powershellu, pomocí rutiny **Set-AzureSubscription** a **Vyberte AzureSubscription** :

        Set-AzureSubscription -SubscriptionName “mySubName” -CurrentStorageAccountName “mystorageaccount”
        Select-AzureSubscription -SubscriptionName "mySubName" –Current

Prvního příkazu přiřadí výchozí účet úložiště k předplatnému (třeba pro některé OM zřizovací operace).  Druhý zajišťuje předplatné stávající (rozpozná jiných rutiny).

## <a name="see-also"></a>Viz taky
- [Začínáme: jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md)
- [Vytvoření obrázku virtuálního počítače pro Marketplace](marketplace-publishing-vm-image-creation.md)
