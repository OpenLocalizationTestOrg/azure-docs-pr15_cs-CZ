## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Připravit k ověřování požadavků správce prostředků Azure

Je třeba ověřit všechny operace, které provedete na prostředky pomocí [Správce prostředků Azure] [ lnk-authenticate-arm] s Azure Active Directory (AD). Nejjednodušší způsob, jak konfigurovat toto je použití prostředí PowerShell nebo rozhraní příkazového řádku Azure.

Měli byste nainstalovat [Azure PowerShell 1.0] [ lnk-powershell-install] nebo vyšší, než budete pokračovat.

Následující kroky ukazují, jak nastavit ověřování pomocí hesla pro aplikaci AD pomocí prostředí PowerShell. Tyto příkazy lze spouštět ve standardní relaci prostředí PowerShell.

1. Přihlaste se k předplatnému Azure pomocí následujícího příkazu:

    ```
    Login-AzureRmAccount
    ```

2. Poznamenejte si **TenantId** a **SubscriptionId**. Budete je potřebovat později.

3. Vytvořte novou aplikaci služby Active Directory Azure pomocí následujícího příkazu, držitelé místo nahrazení:

    - **{Zobrazovaný název}:** zobrazovaný název vaší aplikace, například **MySampleApp**
    - **{Adresa URL domovské stránky}:** adresu URL domovské stránky vaší aplikace, například **http://mysampleapp/home**. Není nutné tuto adresu URL přejděte v reálné aplikaci.
    - **{Identifikátor aplikace}:** Jedinečný identifikátor, například **http://mysampleapp**. Není nutné tuto adresu URL přejděte v reálné aplikaci.
    - **{Heslo}:** Heslo, které budete používat k ověření s vaší aplikace.

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. Poznamenejte si **identifikátor ApplicationId** aplikace, kterou jste vytvořili. Budete potřebovat později.

5. Vytvořte novou službu hlavní pomocí následujícího příkazu, nahraďte **identifikátor ApplicationId** z předchozího kroku **{MyApplicationId}** :

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. Nastavení přiřazení rolí pomocí následujícího příkazu, nahraďte váš **identifikátor ApplicationId** **{MyApplicationId}** .

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
Nyní jste dokončili vytváření aplikace Azure AD, která vám umožní ověřit z vlastní aplikace C#. Později v tomto návodu budete potřebovat následující hodnoty:

- TenantId
- SubscriptionId
- ApplicationId
- Heslo

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md
