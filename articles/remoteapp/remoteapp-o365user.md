
<properties 
    pageTitle="Jak používat Azure RemoteApp s uživatelské účty služeb Office 365 | Microsoft Azure"
    description="Naučte se používat Azure RemoteApp s uživatelských účtů Office 365"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Jak Azure RemoteApp pomocí služby Office 365 uživatelských účtů

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Pokud máte předplatné Office 365 máte Azure Active Directory ukládá uživatelská jména a hesla použitá pro přístup ke službám Office 365. Třeba když uživatelé aktivace Office 365 ProPlus se ověřování Azure AD chcete zkontrolovat licence. Většina zákazníků chcete použít stejné adresář s Azure RemoteApp.

Pokud nasazujete Azure RemoteApp používáte nejspíš Azure předplatného, které souvisí s jinou Azure AD. Abyste mohli používat adresáře služeb Office 365, musíte se přesuňte Azure předplatného do adresáře.

Informace o nasazení klientské aplikace Office 365 najdete v tématu [použití vaše předplatné Office 365 s Azure RemoteApp](remoteapp-officesubscription.md).
 
## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Fáze 1: Registrace bezplatného předplatného Office 365 Azure Active Directory
Pokud používáte portálu Azure classic, postupem [zaregistrovat bezplatnou předplatné Azure Active Directory](https://technet.microsoft.com/library/dn832618.aspx) získat přístup pro správu k Azure AD pomocí portálu pro správu Azure. Jako výsledek tento proces by měla Přihlaste se k portálu Azure a najdete v článku adresáři nekoná – v tomto okamžiku neuvidíte mnohem víc od úplného Azure předplatné, které používáte s Azure RemoteApp je v jiném adresáři.

Mějte na paměti jméno a heslo účtu správce, který jste vytvořili v tomto kroku – potřebovali ve fázi 2.

Pokud používáte portálu Azure, podívejte se na [tom, jak zaregistrovat a aktivace bezplatného Azure Active Directory pomocí portálu Office 365](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Fáze 2: Změna Azure AD přidružený k předplatnému Azure.
Nyní klikněte na tlačítko změnit svoje předplatné Azure z aktuálního adresáře do adresáře služeb Office 365, který jsme spolupracovali ve fázi 1.

Postupujte podle pokynů v tématu [Změna klienta služby Azure Active Directory v Azure RemoteApp](remoteapp-changetenant.md). Věnujte zvláštní pozornost podle těchto kroků:

- Kroku #1: Pokud jste v toto předplatné nasadili Azure RemoteApp (ARA), zkontrolujte, že odeberete všechny Azure AD uživatelských účtů z některé kolekce ARA nejdřív před pokusem o něco jiného. Můžete taky můžete odstranit všechny existující kolekce.
- Krok #2: Toto je důležitým krokem. Je potřeba použít s účtem Microsoft (například @outlook.com) jako správce služby předplatného; je to proto jsme nemůže obsahovat žádné uživatelských účtů z existující Azure AD připojena k předplatnému – v takovém případě jsme nebude možné přesunout do jiné Azure AD.
- Krok #4: Při přidávání existující adresář, systému vás vyzve k tohoto adresáře se přihlaste pomocí účtu správce. Ujistěte se pomocí účtu správce z fázi 1.
- Krok #5: Změňte nadřazený adresář předplatné Office 365 adresář. V konečném důsledku by měl být, klikněte v části Nastavení -> předplatná předplatného seznamy adresáři Office 365. 
![Změna adresáře nadřazené předplatné](./media/remoteapp-o365user/settings.png)
 

V tomto okamžiku předplatného Azure RemoteApp souvisí s Office 365 Azure AD; můžete použít existující uživatelské účty Office 365 s Azure RemoteApp!




