<properties
    pageTitle="Připojení k Azure zásobníku s rozhraní příkazového řádku | Microsoft Azure"
    description="Naučte se používat rozhraní příkazového řádku různé platformy (rozhraní příkazového řádku) a Správa nasazení zdroje na zásobníku Azure"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Nainstalujte a nakonfigurujte Azure zásobníku rozhraní příkazového řádku

V tomto dokumentu doporučujeme vám pomůže proces přidávání a používání zdrojů Azure zásobníku na platformách Mac a Linux klienta pomocí rozhraní příkazového (rozhraní příkazového řádku Azure řádku).  

## <a name="install-azure-stack-cli"></a>Instalace Azure zásobníku rozhraní příkazového řádku

Pokud jste na Mac a Linux, můžete získat rozhraní příkazového řádku pomocí tento příkaz:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Připojení k Azure zásobníku
V následujících krocích je nakonfigurovat Azure rozhraní příkazového řádku pro připojení k vrstvě Azure. Potom přihlásit a získat informace o předplatném.

1.  Získání hodnoty pro active directory zdroje id spuštěním tohoto Powershellu:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  Zadejte následující příkaz rozhraní příkazového řádku přidat Azure zásobníku prostředí, jak zajistit aktualizovat *– active directory zdroje id* s daty získat adresu URL v předchozím kroku:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Přihlaste se pomocí následujícího příkazu (nahradit *uživatelské jméno* s své uživatelské jméno):

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Pokud dostáváte problémů ověření certifikátů, zakázat ověření certifikátu pomocí příkazu `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Režim Azure konfigurace správci zdrojů Azure můžete nastavte pomocí tento příkaz:

        azure config mode arm

5.  Načtení seznamu předplatných.

        azure account list     

## <a name="next-steps"></a>Další kroky

[Nasazení šablony s Azure rozhraní příkazového řádku](azure-stack-deploy-template-command-line.md)

[Připojení pomocí prostředí PowerShell](azure-stack-connect-powershell.md)

[Správa uživatelských oprávnění](azure-stack-manage-permissions.md)
