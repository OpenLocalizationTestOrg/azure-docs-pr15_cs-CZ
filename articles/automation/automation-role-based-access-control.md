<properties 
   pageTitle="Řízení přístupu na základě rolí v Azure automatizaci | Microsoft Azure"
   description="Řízení přístupu na základě rolí (RBAC) umožňuje správu přístupu pro Azure zdroje. Tento článek popisuje, jak nastavit RBAC v Azure automatizaci."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="automatizace rbac azure rbac řízení přístupu na základě rolí" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/12/2016"
   ms.author="magoedte;sngun"/>

# <a name="role-based-access-control-in-azure-automation"></a>Řízení přístupu na základě rolí v Azure automatizaci

## <a name="role-based-access-control"></a>Řízení přístupu na základě rolí

Řízení přístupu na základě rolí (RBAC) umožňuje správu přístupu pro Azure zdroje. [RBAC](../active-directory/role-based-access-control-configure.md)můžete oddělit úkolů v rámci týmu a udělit jenom množství přístup uživatelů, skupin a aplikace, které potřebují k provedení své práce. Na základě rolí access lze udělit uživatelů pomocí portálu Azure, Azure nástroje příkazového řádku nebo API správy Azure.

## <a name="rbac-in-automation-accounts"></a>RBAC v automatizaci účty

V Azure automatizaci přístup, která přiřazením odpovídající roli RBAC uživatelé, skupiny a aplikací v oboru automatizaci účtu. Tady jsou integrované role automatizaci účet podporuje:  

|**Role** | **Popis** |
|:--- |:---|
| Vlastník | Role vlastníka umožňuje přístup ke všem zdroje a akce v rámci účet automatizaci včetně poskytnutí přístupu jiných uživatelů, skupin a aplikací na Spravovat účet automatizaci. |
| Skupiny přispěvatelů | Role přispěvatele umožňuje spravovat všechno kromě úpravami jiný uživatel oprávnění k automatizaci účtu. |
| Čtečky | Roli Čtenář umožňuje zobrazit všechny zdroje na automatizaci účtu, ale nemůžete proveďte požadované změny.|
| Automatizace operátor | Automatizace operátor roli umožňuje provozní úkoly třeba spustit, zastavit, pozastavení, obnovení a naplánovat úlohy. Tato role je užitečné, pokud chcete zamknout vašeho účtu automatizaci zdrojů, jako jsou pověření majetek i runbooks prohlédnutí nebo změnit, ale přesto povolit členy vaší organizace, provést tyto runbooks. |
| Správce přístup uživatelů | Role správce přístup uživatelů umožňuje spravovat přístup uživatelů k automatizaci Azure účtům. |

>[AZURE.NOTE] Nelze udělit oprávnění na konkrétní postupu runbook nebo runbooks, jenom pro zdroje a akce v rámci automatizaci účtu.  

V tomto článku Nemůžeme vás provede jednotlivými nastavíte RBAC v Azure automatizaci. Ale nejdřív Podívejme bližší pohled na vaše jednotlivých oprávnění Přispěvatel, Reader, automatizaci operátor a správce přístup uživatelů tak, aby jsme získat dobře porozumět před udělením kdokoliv práva k automatizaci účtu.  V opačném případě může vést k před nechtěným nebo nežádoucí důsledky.     

## <a name="contributor-role-permissions"></a>Oprávnění role přispěvatele

V následující tabulce jsou uvedeny konkrétní akce, které lze vykonat role přispěvatele v automatizaci.

| **Pole Typ zdroje** | **Pro čtení** | **Zápis** | **Odstranění** | **Jiné akce** |
|:--- |:---|:--- |:---|:--- |
| Účet Azure automatizaci | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | 
| Automatizace certifikát majetku | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | |
| Automatické připojení materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | 
| Automatické připojení typ materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | 
| Automatizace pověření materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | |
| Automatizace plánu materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | |
| Automatizace proměnné materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | |
| Automatické vyplňování stavu konfigurace | | | | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) |
| Typ zdroje pracovního postupu Runbook hybridní | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | 
| Azure automatizace úloh | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | 
| Automatizace úloh toku | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Automatizace plánu projektu | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | |
| Automatizace modul | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | |
| Postupu Runbook Azure automatizaci | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) |
| Automatizace postupu Runbook koncept | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) |
| Automatizace postupu Runbook koncept testovací úlohy | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | 
| Automatizace Webhook | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) |

## <a name="reader-role-permissions"></a>Oprávnění role Readeru

V následující tabulce jsou uvedeny konkrétní akce, které lze vykonat roli Čtenář v automatizaci.

| **Pole Typ zdroje** | **Pro čtení** | **Zápis** | **Odstranění** | **Jiné akce** |
|:--- |:---|:--- |:---|:--- |
| Správce klasický předplatného | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Správa zámek | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Oprávnění | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | |
| Operace poskytovatele | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Přiřazování rolí | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Definice role | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | 

## <a name="automation-operator-role-permissions"></a>Oprávnění role automatizaci operátor

V následující tabulce jsou uvedeny konkrétní akce, které lze vykonat roli automatizaci operátor v automatizaci.

| **Pole Typ zdroje** | **Pro čtení** | **Zápis** | **Odstranění** | **Jiné akce** |
|:--- |:---|:--- |:---|:--- |
| Účet Azure automatizaci | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | 
| Automatizace certifikát majetku | | | |
| Automatické připojení materiálů | | | |
| Automatické připojení typ materiálů | | | |
| Automatizace pověření materiálů | | | |
| Automatizace plánu materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automatizace proměnné materiálů | | | |
| Automatické vyplňování stavu konfigurace | | | | |
| Typ zdroje pracovního postupu Runbook hybridní | | | | | 
| Azure automatizace úloh | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | 
| Automatizace úloh toku | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | |  
| Automatizace plánu projektu | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automatizace modul | | | |
| Postupu Runbook Azure automatizaci | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatizace postupu Runbook koncept | | | |
| Automatizace postupu Runbook koncept testovací úlohy | | | |  
| Automatizace Webhook | | | |

Další podrobnosti [automatizaci operátor akce](../active-directory/role-based-access-built-in-roles.md#automation-operator) jsou uvedeny akce nepodporuje roli automatizaci operátor na automatizaci účtu a jeho zdroje.

## <a name="user-access-administrator-role-permissions"></a>Oprávnění role správce přístup uživatelů

V následující tabulce jsou uvedeny konkrétní akce, které lze vykonat roli správce přístup uživatelů v automatizaci.

| **Pole Typ zdroje** | **Pro čtení** | **Zápis** | **Odstranění** | **Jiné akce** |
|:--- |:---|:--- |:---|:--- |
| Účet Azure automatizaci | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatizace certifikát majetku | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatické připojení materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatické připojení typ materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatizace pověření materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatizace plánu materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatizace proměnné materiálů | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatické vyplňování stavu konfigurace | | | | |
| Typ zdroje pracovního postupu Runbook hybridní | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Azure automatizace úloh | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Automatizace úloh toku | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Automatizace plánu projektu | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatizace modul | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Postupu Runbook Azure automatizaci | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatizace postupu Runbook koncept | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automatizace postupu Runbook koncept testovací úlohy | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | | | 
| Automatizace Webhook | ![Zeleným stavovým](media/automation-role-based-access-control/green-checkmark.png) | | |

## <a name="configure-rbac-for-your-automation-account-using-azure-portal"></a>Konfigurace RBAC účtu automatizaci portálu Azure

1.  Přihlaste se k [Portálu Azure](https://portal.azure.com/) a otevřete svůj účet automatizaci z zásuvné automatizaci účty.  

2.  Klikněte na ovládací prvek **přístup** v pravém horním rohu. Otevře se **Uživatelé** zásuvné, kde můžete přidat nové uživatele, skupiny a aplikací spravovat svůj účet automatizaci a zobrazit role stávajících nakonfigurované pro automatizaci účet.  

    ![Tlačítko přístupu](media/automation-role-based-access-control/automation-01-access-button.png)  

>[AZURE.NOTE] **Správci předplatné** již existuje jako výchozího uživatele. Skupiny správců služby active directory předplatné zahrnuje správci služby a co-administrator(s) předplatného Azure. Správce služeb je vlastník předplatného Azure a jeho zdrojů a bude role vlastníka převzít pro automatizaci účty příliš. To znamená, že přístup **Inherited** pro **Správce služby a dalších správců** předplatné a jeho **přiřazené** pro všechny ostatní uživatele. Klikněte na **předplatné správci** zobrazíte další informace o jeho oprávnění.  

### <a name="add-a-new-user-and-assign-a-role"></a>Přidání nového uživatele a přiřadit roli

1.  V zásuvné uživatelé klikněte na **Přidat** otevřete **zásuvné přístup přidat** místo, kam můžete přidat uživatele, skupinu nebo aplikace a přiřadit roli na ně.  

    ![Přidání uživatele](media/automation-role-based-access-control/automation-02-add-user.png)  

2.  Vyberte roli ze seznamu dostupných rolí. Jsme zvolí roli **Čtenář** , ale můžete vybrat některé z dostupných předdefinovaných role, které podporuje automatizaci účet nebo vlastní roli, kterou může definovali.  

    ![Vyberte roli](media/automation-role-based-access-control/automation-03-select-role.png)  

3.  Klikněte na **Přidat uživatele** otevřete zásuvné **Přidat uživatele** . Pokud jste přidali uživatelů, skupin nebo aplikací pro správu předplatného a pak jsou uvedeny tyto uživatele a můžete je přidat přístup vyberte. Pokud nejsou k dispozici všech uživatelů, kteří jsou, nebo pokud uživatel vás zajímají přidání nenajdete pak klikněte na **Pozvat** otevřete **pozvání Host** zásuvné, kde můžete pozvat uživatele s platnou adresu e-mailu účet Microsoft jako je Outlook.com, OneDrive nebo Xbox Live ID. Jakmile jste zadali e-mailovou adresu uživatele, klikněte na **Vyberte** přidat uživatele, a pak klikněte na **OK**. 

    ![Přidání uživatelů](media/automation-role-based-access-control/automation-04-add-users.png)  
 
    Teď byste měli vidět uživatel přidaný zásuvné **Uživatelé** s přiřazenou roli **Readeru** .  

    ![Seznam uživatelů](media/automation-role-based-access-control/automation-05-list-users.png)  

    Můžete také přiřadit role uživatele z **role** zásuvné. 

1. Klikněte na položku **role** uživatele zásuvné otevřete **zásuvné role**. Z tohoto zásuvné můžete zobrazit název roli, počet uživatelů a skupin k tuto roli přiřadit.

    ![Přiřazení role z zásuvné uživatelů](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
    >[AZURE.NOTE] Řízení přístupu na základě rolí můžete nastavit jenom na úrovni automatizaci účtu a ne všechny zdroje pod účtem automatizaci.

    Přiřadit víc než jednu roli uživateli, skupině nebo aplikace. Například pokud roli **Automatizaci operátor** spolu s **role Čtenář** přidat uživatele, pak je můžete zobrazit všechny zdroje automatizaci, jakož i spusťte úloh postupu runbook. Můžete rozbalit rozevírací seznam všech role přiřazené uživateli.  

    ![Zobrazení více rolí](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  
 
### <a name="remove-a-user"></a>Odebrání uživatele

Můžete odebrat oprávnění k přístupu pro uživatele, který není Správa účtu automatizaci nebo kdo nepracuje pro organizaci. Následující najdete postup, jak odebrat uživatele: 

1.  Z zásuvné **uživatele** vyberte přiřazování rolí, kterou chcete odebrat.

2.  Kliknutím na tlačítko **Odebrat** v zásuvné podrobnosti o přiřazení.

3.  Klikněte na **Ano** potvrďte odebrání. 

    ![Odebrání uživatelů](media/automation-role-based-access-control/automation-08-remove-users.png)  

## <a name="role-assigned-user"></a>Role přiřazené uživateli

Když se uživateli přiřadí role přihlášení k účtu automatizaci, teď vidí vlastníka účet v seznamu **Výchozí adresářů**. Abyste mohli zobrazit automatizaci účet, který jste přidali do jejich musí přejděte v adresáři výchozí vlastníka výchozí adresář.  

![Výchozí adresář](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### <a name="user-experience-for-automation-operator-role"></a>Uživatelské prostředí pro automatizaci operátor rolí

Pokud uživatel, kterému je přiřazena zobrazení roli automatizaci operátor automatizaci účet, který kterému jsou přiřazené, můžete zobrazit pouze seznam runbooks, postupu runbook úlohy a plánů vytvořili v účtu automatizaci ale nedá se zobrazit jejich definice. Budou začít, ukončit, pozastavení, obnovení nebo plánování úlohy postupu runbook. Uživatel nebude mít přístup k dalším automatizaci materiálům například konfigurací hybridního pracovní skupiny nebo DSC uzlů.  

![Přístup k resourcres](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

Poté, co uživatel klikne na postupu runbook, příkazy pro zobrazení zdroje nebo upravit postupu runbook nejsou podle role operátor automatizaci neumožňuje k nim přístup.  

![Přístup k upravit postupu runbook](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

Uživatel bude mít přístup k zobrazení a k vytváření plánů, ale nebudete mít přístup k jiný typ materiálů.  

![Přístup k prostředky](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Tento uživatel taky nemá přístup k zobrazení webhooks přidružené postupu runbook

![Přístup k webhooks](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## <a name="configure-rbac-for-your-automation-account-using-azure-powershell"></a>Konfigurace RBAC účtu automatizaci pomocí prostředí PowerShell Azure

Také je možné konfigurovat přístupu na základě rolí k automatizaci účtu pomocí následující [rutiny prostředí PowerShell Azure](../active-directory/role-based-access-control-manage-access-powershell.md).

• [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) obsahuje seznam všech RBAC role, které jsou k dispozici v Azure Active Directory. Chcete-li zobrazit všechny akce, které můžete provádět konkrétní rolí můžete tento příkaz spolu s vlastnost **název** .  
    **Příklad:**  
    ![Získání definice role](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) seznamy Azure AD RBAC přiřazování rolí v zadaném oboru. Bez parametrů tento příkaz vrátí všechna přiřazení rolí podle předplatného. Použít parametr **ExpandPrincipalGroups** přiřazení seznamu přístupu pro zadaný uživatel i skupiny uživatel členem.  
    **Příklad:** Pomocí následujícího příkazu seznam všech uživatelů a jejich role obchodního vztahu automatizaci.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Získání přiřazování rolí](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [Nový AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) přiřadit přístup uživatelů, skupin a aplikací pro určitý obor.  
    **Příklad:** Pomocí následujícího příkazu přiřadit roli "Automatizaci operátor" pro uživatele v rozsahu automatizaci účtu.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Nové přiřazování rolí](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• Umožňuje [Odebrat AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) odeberte přístup zadaný uživatel, skupiny nebo aplikace určitý obor.  
    **Příklad:** Pomocí následujícího příkazu odebrat uživatele z role "Automatizaci operátor" rozsahu automatizaci účtu.

    Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

V předchozích příkladech nahraďte **Id**, **předplatné Id**, **název skupiny prostředků** a **název účtu automatizaci** podrobnosti o účtu. Zvolte **Ano** potvrďte před pokračováním odebrat přiřazování rolí uživatele.   


## <a name="next-steps"></a>Další kroky
-  Informace o různých způsobech konfigurace RBAC pro automatizaci Azure najdete v příručce [Správa RBAC s Azure Powershellu](../active-directory/role-based-access-control-manage-access-powershell.md).
- Podrobnosti o různé způsoby, jak začít postupu runbook najdete v tématu [spuštění postupu runbook](automation-starting-a-runbook.md)
- Informace o různých postupu runbook typech naleznete v příručce [Azure automatizaci postupu runbook typy](automation-runbook-types.md)

