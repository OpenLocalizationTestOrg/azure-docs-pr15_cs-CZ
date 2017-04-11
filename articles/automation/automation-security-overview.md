<properties
   pageTitle="Zabezpečení Azure automatizaci | Microsoft Azure"
   description="Tento článek obsahuje přehled automatizaci zabezpečení a různé metody ověřování k dispozici pro automatizaci účty v Azure automatizaci."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="automatizace zabezpečení, zabezpečené automatizaci" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/29/2016"
   ms.author="magoedte" />

# <a name="azure-automation-security"></a>Azure automatizaci zabezpečení
Azure automatizaci umožňuje automatizace úkolů proti zdrojů v Azure, místní a s jinými poskytovateli cloudu například Amazon webové služby AWS.  Aby postupu runbook k provedení požadované akce musí mít oprávnění zabezpečený přístup k zdroji s minimálními práva požadované v rámci předplatného.  
Tento článek se zabývat těmito oblastmi různých situacích ověřování podporuje automatizaci Azure a vám ukáže, jak začít pracovat na základě prostředí nebo prostředí, které budete potřebovat pro správu.  

## <a name="automation-account-overview"></a>Automatizace účtu přehled
Automatizace Azure poprvé, pokud je třeba vytvořit alespoň jeden účet automatizaci. Automatizace účty umožňují izolovat automatizaci zdroje (runbooks, prostředky, konfigurace) od zdrojů obsažené v dalších účtů automatizaci. Automatizace účtů můžete použít k oddělení zdrojů do samostatných logické prostředí. Příklad je možné použít jeden účet pro vývoj, jiné pro výrobní a jiné pro místního prostředí.  Účet Azure automatizaci se liší od vašeho účtu Microsoft nebo účty vytvořené v Azure předplatné.

Automatizace materiály pro každý účet automatizaci souvisí s jedinou Azure oblast, ale automatizaci účtů můžete přidávání a používání zdrojů v jakékoli oblasti. Hlavní důvod, proč vytvořit automatizaci účtů v různých oblastech by, pokud máte zásad, které vyžadují dat a materiály pro odpojí do konkrétní oblasti.

>[AZURE.NOTE]Automatizace účty a prostředky, které obsahují vytvořené v portálu Azure, nemají přístup k Azure klasické portálu. Pokud si chcete spravovat tyto účty nebo jejich zdrojů pomocí prostředí Windows PowerShell, je nutné použít Správce prostředků Azure moduly.

Všechny úkoly, které provádíte proti zdrojů pomocí Správce prostředků Azure a Azure rutin v Azure automatizaci musí ověřit Azure pomocí ověřování na základě přihlašovacích údajů organizace identity Azure Active Directory.  Ověřování na základě certifikát původní metody ověřování s režimem Správa služby Azure, ale byl složité nastavení.  Ověřování Azure s uživatelem Azure AD byla zavedená zpět v 2014 nejen zjednodušit proces konfigurace ověření účtu, ale také podporují možnost interaktivně ověřování Azure pomocí jednoho uživatelského účtu, který spolupracovali správce prostředků Azure i klasické zdroje.   

Aktuálně při vytváření nového účtu automatizaci Azure portálu se automaticky vytvoří:

-  Spustit jako účet, který vytvoří nový objekt zabezpečení služby Azure Active Directory, certifikát a přiřadí řízení přístupu na základě rolí přispěvatelů (RBAC), které bude sloužící ke správě zdrojů správce prostředků prostřednictvím runbooks.
-  Klasický spustit jako účet uložením správy certifikát, který bude sloužící ke správě Správa služby Azure nebo klasická zdrojů prostřednictvím runbooks.  

Řízení přístupu na základě rolí neexistuje pomocí Správce prostředků k poskytnutí povolené akce na Azure AD uživatelský účet a spustit jako účet a ověření jistinu této služby Azure.  Přečtěte si [řízení přístupu na základě rolí v článku automatické Azure](../automation/automation-role-based-access-control.md) pro získání dalších informací vám pomohli vytvořit modelu pro správu automatizaci oprávnění.  

Runbooks spuštěna pracovního postupu Runbook hybridní ve vaší datacentru nebo proti výpočetních služby v AWS nelze použít stejným způsobem, který se obvykle používá k runbooks ověřování Azure zdroje.  Je to proto tyto materiály používáte mimo Azure a proto bude vyžadují vlastní zabezpečovací přihlašovací údaje podle automatizaci ověření na zdroje, které bude přistupovat místně.  

## <a name="authentication-methods"></a>Metody ověřování

Následující tabulka shrnuje různé metody ověřování pro každé prostředí nepodporuje Azure automatizace a článek popisující, jak nastavit ověřování pro váš runbooks.

Metoda  |  Prostředí  | Článek
----------|----------|----------
Azure AD uživatelského účtu | Azure správce zdrojů a Správa služby Azure | [Ověření Runbooks s Azure AD uživatelského účtu](../automation/automation-sec-configure-aduser-account.md)
Azure spustit jako účet | Azure správce prostředků | [Ověření Runbooks s Azure spustit jako účet](../automation/automation-sec-configure-azure-runas-account.md)
Azure klasické spustit jako účet | Správa služby Azure | [Ověření Runbooks s Azure spustit jako účet](../automation/automation-sec-configure-azure-runas-account.md)
Ověřování systému Windows | Místní datacentru | [Ověření Runbooks hybridní postupu Runbook pracovníků](../automation/automation-hybrid-runbook-worker.md)
Přihlašovací údaje AWS | Amazon webové služby | [Ověření Runbooks s webovým službám Amazon (AWS)](../automation/automation-sec-configure-aws-account.md)



