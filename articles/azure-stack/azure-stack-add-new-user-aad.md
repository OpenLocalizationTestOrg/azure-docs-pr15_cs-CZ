<properties
    pageTitle="Přidání nového účtu klienta Azure zásobníku v Azure Active Directory | Microsoft Azure"
    description="Po zavedení Microsoft Azure zásobníku Koncepce, musíte vytvořit aspoň jednoho klienta uživatelský účet, můžete prozkoumat portálu klienta."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Přidání nového účtu klienta Azure zásobníku v Azure Active Directory

Po [nasazení balíčku Koncepce Azure](azure-stack-run-powershell-script.md)musíte mít uživatelský účet klienta, můžete prozkoumat portálu klienta a vyzkoušet nabídky a plány. Můžete vytvoření účtu klienta pomocí [portálu Azure](#create-an-azure-stack-tenant-account-using-the-azure-portal) nebo [pomocí Powershellu](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Vytvořte účet klienta zásobníku Azure pomocí portálu Azure

Musíte mít předplatné Azure používat portál Azure.

1. Přihlaste se k [Azure](http://manage.windowsazure.com).

2.  V levém navigačním panelu Microsoft Azure klikněte na **Služby Active Directory**.

3.  V seznamu adresářů klikněte na adresář, který chcete použít pro zásobníku Azure nebo vytvořte nový účet.

4.  Na této stránce adresář klikněte na **uživatele**.

5.  Klikněte na **Přidat uživatele**.

6.  V průvodci **Přidat uživatele** v seznamu **Typ uživatele** zvolte **nového uživatele ve vaší organizaci**.

7.  Do pole **uživatelské jméno** zadejte jméno uživatele.

8.  V **@** pole, vyberte příslušnou položku.

9.  Klikněte na Další šipky.

10.  Na stránce **profilu uživatele** v průvodci zadejte **jméno**, **Příjmení**a **zobrazované jméno**.

11. V seznamu **Role** vyberte **uživatele**.

12. Klikněte na Další šipky.

13. Na stránce **stažení dočasné heslo** klikněte na **vytvořit**.

14. Zkopírujte **nové heslo**.

15. Přihlaste se k Microsoft Azure pomocí nového účtu. Změna hesla po zobrazení výzvy.

16. Přihlaste se k `https://portal.azurestack.local` k novému účtu zobrazíte portal klienta.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Vytvoření účtu klienta zásobníku Azure pomocí prostředí PowerShell

Pokud nemáte předplatné Azure, nemůžete používat portál Azure k přidání uživatelského účtu klienta. V tomto případě místo toho můžete modul Azure Active Directory pro Windows PowerShell.

> [AZURE.NOTE] Pokud používáte Microsoft Account (Live ID) pro nasazení koncepce zásobníku Azure, nelze použít AAD Powershellu k vytvoření účtu klienta. 

1.  Nainstalujte [Microsoft Online Services Pomocníka pro přihlášení pro IT profesionály RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  Nainstalujte [Modul Azure Active Directory pro Windows PowerShell (64bitová verze)](http://go.microsoft.com/fwlink/p/?linkid=236297) a otevřete ho.

3.  Spusťte tyto rutiny:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Přihlaste se k Microsoft Azure pomocí nového účtu. Změna hesla po zobrazení výzvy.

5.  Přihlaste se k `https://portal.azurestack.local` k novému účtu zobrazíte portálu klienta.



