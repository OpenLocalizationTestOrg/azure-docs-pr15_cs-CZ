<properties
    pageTitle="Kurz: Azure Active Directory integrace se službou Facebook v práci | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Facebooku v práci."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Kurz: Azure Active Directory integrace se službou Facebook v praxi

Cílem tohoto kurzu je vidíte, jak integrovat Facebook při práci s Azure Active Directory (Azure AD).

Integrace služby Facebook při práci s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo bude mít přístup k Facebooku v praxi 
- Můžete automaticky zřízení účtu pro uživatele, kteří obdrželi přístup k Facebooku v praxi
- Povolení uživatelům, aby automaticky získat přihlášení z k Facebooku v praxi (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě 

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat integraci Azure AD pomocí CS hvězdiček, musíte následující položky:

- Předplatné Azure AD
- Povolené Facebook při práci s jednotné přihlašování

Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Přidání Facebook při práci v galerii
Abyste mohli nakonfigurovat integraci Facebook při práci v Azure AD, potřebujete přidat Facebook při práci v galerii do seznamu spravované SaaS aplikace.

**Přidat Facebook při práci v galerii, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.
    
    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Facebook v práci**.

7. V podokně výsledků vyberte **Facebook v práci**a potom klikněte na **Dokončit** přidat aplikaci.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Tato část popisuje, jak povolit uživatelům ověření k Facebooku při práci s účtem v Azure Active Directory federation podle protokolu SAML pomocí.

**Abyste mohli nakonfigurovat Azure AD jednotného přihlašování k Facebooku v práci, proveďte následující kroky:**

1.  Po přidání Facebook v práci na portálu Azure klasické, klikněte na **Konfigurovat jednotného přihlašování**.

2.  V okně **Konfigurovat adresa URL App** zadejte adresu URL místo, kam se uživatelé Přihlaste se k Facebooku na aplikace. Toto je Facebook na adrese URL práce klienta (Příklad: https://example.facebook.com/). Až budete hotovi, klikněte na tlačítko **Další**.

3.  V okně prohlížeče jiný web Přihlaste se jako správce k Facebooku na webu společnosti práce.

4. Postupujte podle pokynů v následující adresu URL a konfigurace Facebook v práci Azure AD jako zprostředkovatelem identit: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Po dokončení, vraťte se do okna prohlížeče s portálu Azure klasické, zaškrtnutím políčka potvrďte, že jste dokončili postup a potom klikněte na **Další** a **Dokončit**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Automaticky zřizování uživatelů k Facebooku v praxi

Azure AD podporuje možnost automaticky synchronizuje podrobnosti o účtu přiřazená uživatelů Facebooku v práci. Tento automatické synchronizace klienta umožňuje Facebook v práci přístup k datům, je třeba povolit uživatelům připojovat před jejich pokusu o přihlášení poprvé. Ho taky deaktivovat ustanovení uživatelů z Facebooku v práci při přístup byl odvolán v Azure AD.

Automatické vytváření můžete nastavit tak, že kliknete na **konfigurovat účet zřizování** v okně Azure klasický portálu.

Další podrobnosti o tom, jak konfigurovat automatické vytváření najdete v tématu [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Přiřazení uživatelů Facebooku v praxi

Zřizování AAD uživatelům neuvidíte Facebook v práci na jejich Panel přístup musí být přiřazena přístup do portálu Azure klasické.

**Přiřazení uživatelů k Facebooku v práci:**

1.  Na úvodní stránce pro Facebook v práci na portálu Azure klasické klikněte na **přiřadit účty**.

2.  V nabídce **Zobrazit** vyberte, zda chcete přiřadit uživatele nebo skupiny na Facebook v práci a klikněte na tlačítko se zaškrtnutím.

3.  Ve výsledném seznamu vyberte uživatele nebo skupiny, kterému chcete přiřadit Facebook v práci.

4.  Zápatí stránky klikněte na tlačítko **přiřadit** .


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png




