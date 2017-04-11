<properties
   pageTitle="Vytvoření Vyrovnávání zatížení interní pomocí šablony v správce prostředků | Microsoft Azure"
   description="Naučte se vytvářet Vyrovnávání zatížení interní pomocí šablony ve Správci zdrojů"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-a-template"></a>Vytvoření Vyrovnávání zatížení interní použít šablonu?

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klasický nasazení modelu](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Nasazení šablony pomocí klikněte na nasazení

Ukázková šablona k dispozici v úložišti veřejné používá parametr soubor, který obsahuje výchozí hodnoty použito k vygenerování scénář jsme je popsali výše. Nasazení tuto šablonu používat klikněte na nasazení, přejděte na [Tento odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer), klikněte na **Deploy Azure**, nahraďte výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů na portálu.

## <a name="deploy-the-template-by-using-powershell"></a>Nasazení šablony pomocí prostředí PowerShell

Abyste mohli nasadit na šablonu, kterou jste si stáhli pomocí prostředí PowerShell, postupujte následujícím způsobem.

1. Pokud máte Azure Powershellu, přečtěte si, [Jak nainstalovat a konfigurace prostředí PowerShell Azure](../../articles/powershell-install-configure.md) a postupujte podle pokynů úplně za účelem Přihlaste se k Azure a potom vyberte předplatné.
2. Stáhněte si soubor parametry na místní disk.
3. Upravit tento soubor a uložit, abyste ji.
4. Spusťte rutinu **New-AzureRmResourceGroupDeployment** vytvořit skupinu zdrojů pomocí šablony.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Nasazení šablony pomocí rozhraní příkazového řádku Azure

Nasazení šablony pomocí rozhraní příkazového řádku Azure, postupujte následujícím způsobem.

1. Pokud máte Azure rozhraní příkazového řádku, přečtěte si téma [instalace a konfigurace Azure rozhraní příkazového řádku](../../articles/xplat-cli-install.md) a postupujte podle pokynů až do okamžiku, kdy vyberte svůj účet Azure a předplatné.
2. Příkaz **azure konfigurace režim** přepněte do režimu správce prostředků, jak je ukázáno v následujícím příkladu.

        azure config mode arm

    Tady je očekávané výstup pro výše uvedené příkaz:

        info:    New mode is arm

3. Otevřete soubor parametr, vyberte obsah a uložte do souboru ve vašem počítači. V tomto příkladu jsme parametry soubor uložili *parameters.json*.

4. Příkaz **vytvořit azure skupiny nasazení** nasazení nové Vyrovnávání zatížení interní pomocí šablony a parametr soubory můžete stáhnout a kterou nad. Seznam zobrazený po výstup vysvětluje parametry použité.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Další kroky

[Konfigurace režimu rozdělení Vyrovnávání zatížení pomocí spřažení IP zdroje](load-balancer-distribution-mode.md)

[Nastavení nečinnosti TCP vypršení časového limitu Vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)



