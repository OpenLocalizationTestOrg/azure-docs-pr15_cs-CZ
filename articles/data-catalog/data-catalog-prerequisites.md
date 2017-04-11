<properties
   pageTitle="Požadavky na Azure katalog dat | Microsoft Azure"
   description="Azure katalog dat požadavky – co je potřeba začít pracovat s katalogem dat Azure."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/21/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-prerequisites"></a>Požadavky na Azure katalogu dat

## <a name="what-do-i-need-to-get-started-with-azure-data-catalog"></a>Co je potřeba začít pracovat s katalogem dat Azure?

Budete potřebovat starat o před můžete nastavit **Katalog dat Azure**několik věcí. Nedělejte si starosti – nebude trvat dlouho!

## <a name="azure-subscription"></a>Azure předplatného
Pokud chcete nastavit katalog dat Azure, musíte být vlastníkem nebo spoluvlastníka Azure předplatného.

Azure předplatná lépe tak uspořádat přístup ke cloudové služby zdrojů, jako jsou katalog dat Azure. Jsou taky nápovědy můžete řídit, jak je vykázaného používání zdrojů, vám účtovat poplatky a zaplacené. Každého předplatného může obsahovat různé platba a fakturace vzhled tak, abyste měli různých předplatných a různých plánech oddělení, project, místní office a tak dál. Každý cloudové služby součástí předplatného a musíte mít předplatné před nastavením katalog dat Azure. Další informace najdete v tématu [Správa účtů, předplatná a správní role](../active-directory/active-directory-assign-admin-roles.md).

## <a name="azure-active-directory"></a>Azure Active Directory
Pokud chcete nastavit katalog dat Azure, musíte být přihlášeni pomocí uživatelský účet Azure Active Directory.

Azure Active Directory (Azure AD) obsahuje snadný způsob, jak pro svoji firmu správy identit a přístup, jak v cloudu a místní. Uživatele můžete použít jeden pracovní nebo školní účet pro jednotné přihlašování všechny cloudu a místní webová aplikace. Azure katalog dat používá Azure AD ověření přihlašování. Další informace najdete v tématu [Co je Azure Active Directory](../active-directory/active-directory-whatis.md).

> [AZURE.NOTE] [Azure portál](http://portal.azure.com/) umožňuje uživatelům a přihlaste se pomocí osobního Account Microsoft nebo Azure Active Directory pracovní nebo školní účet. Nastavení katalogu dat Azure pomocí portálu Azure nebo pomocí [portálu katalog dat](http://www.azuredatacatalog.com) musíte být přihlášeni pomocí účtu služby Azure Active Directory, ne osobním účtem.

## <a name="active-directory-policy-configuration"></a>Konfigurace zásad Active Directory

Uživatelé v některých případech může nastat situace, místo, kam se můžete přihlásit k portálu katalog dat Azure, ale při pokusu o přihlášení k nástroji registrace zdroje dat se kterými se setkávají chybovou zprávu, které brání přihlášení. Toto chování problému může dojít pouze v případě, že uživatel je v podnikové síti nebo může dojít, pouze pokud uživatel volající z mimo podnikové síti.

Registrační nástroj zdroj dat používá ověřování pomocí formulářů pro ověřování přihlášení proti služby Active Directory. Pro úspěšné přihlášení ověřování pomocí formulářů v musí být povolený globální zásadu ověření správcem služby Active Directory.

Globální ověřování zásady umožňují metody ověřování pro intranetové nebo extranetové připojení povolit nezávisle na sobě, jak je znázorněno níže. Chyby při přihlášení k může dojít, pokud není povolený ověřování pomocí formulářů pro síť, ze kterého se uživatel připojuje.

 ![Zásady globální ověřování služby Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Další informace najdete v tématu [Konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).
