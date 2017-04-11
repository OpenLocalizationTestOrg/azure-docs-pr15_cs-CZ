<properties 
    pageTitle="Kurz: Azure Active Directory integrace s ŽÁDNÝM | Microsoft Azure" 
    description="Naučte se používat ŽÁDNÝM s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Kurz: Azure Active Directory integrace s ŽÁDNÝM
  
Cílem tohoto kurzu je předvedení ŽÁDNÝM integrace se službou Azure Active Directory (Azure AD).

Integrace ŽÁDNÝM s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k ŽÁDNÝM
- Povolení uživatelům, aby automaticky získat přihlášení z k ŽÁDNÝM (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s ŽÁDNÝM, musíte následující položky:

- Platné Azure předplatné
- ŽÁDNÝM jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ŽÁDNÝM z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení

## <a name="adding-mcm-from-the-gallery"></a>Přidání ŽÁDNÝM z Galerie
Pokud chcete nakonfigurovat integraci ŽÁDNÝM do Azure AD, potřebujete přidat ŽÁDNÝM z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat ŽÁDNÝM z galerie, proveďte následující kroky:**

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **ŽÁDNÝM**.

    ![Galerie aplikace] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Galerie aplikace")

7.  V podokně výsledků vyberte **ŽÁDNÝM**a potom klikněte na **Dokončit** přidat aplikaci.

    ![ŽÁDNÝM] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "ŽÁDNÝM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování s ŽÁDNÝM založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému ŽÁDNÝM uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v ŽÁDNÝM jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v ŽÁDNÝM.

Testování a konfigurace Azure AD jednotné přihlašování s ŽÁDNÝM, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření ŽÁDNÝM testování uživatele](#creating-a-mcm-test-user)** – a protějšek Britta Simon ŽÁDNÝM, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování
  
V této části Povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování ŽÁDNÝM aplikace.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s ŽÁDNÝM, proveďte následující kroky:**

1.  Na portálu na stránce **ŽÁDNÝM** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k ŽÁDNÝM** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Microsoft Azure AD jednotné přihlášení] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure AD jednotné přihlášení")

3.  Na stránce Konfigurovat nastavení aplikace dialogové okno proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "Konfigurace aplikace URL")

    na. **Přihlaste se na adresu URL** do textového pole, zadejte: `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    b. Klikněte na tlačítko **Další**

4.  Na stránce **konfigurovat jednotné přihlašování v ŽÁDNÝM** klikněte na tlačítko **Stáhnout metadat**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Konfigurace jednotného přihlašování")

5. Získat SSO nakonfigurován pro aplikaci, kontaktujte tým podpory ŽÁDNÝM. Připojit soubor stažený metadat a sdílejte ho s ŽÁDNÝM týmu k nastavení jednotné přihlašování ve své straně.

6.  Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Konfigurace jednotného přihlašování")

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Konfigurace jednotného přihlašování")


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test

Cílem tento oddíl je vytvoření uživatele test na portálu klasické s názvem Britta Simon.

![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   

###<a name="creating-a-mcm-test-user"></a>Vytvoření uživatele ŽÁDNÝM test
  
V této části vytvoříte uživatele s názvem Britta Simon v ŽÁDNÝM. Zkontrolujte pracujte se na tým podpory ŽÁDNÝM přidáte uživatele v platformu ŽÁDNÝM.

>[AZURE.NOTE]Můžete použít jakékoli další ŽÁDNÝM uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou ŽÁDNÝM k poskytování AAD uživatelským účtům.


###<a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD
  
Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k ŽÁDNÝM.
    
![Přiřazení uživatelů] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Přiřazení uživatelů")

**Pokud chcete přiřadit Britta Simon ŽÁDNÝM, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
    
    ![Přiřazení uživatelů] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Přiřazení uživatelů")

2. V seznamu aplikací vyberte **ŽÁDNÝM**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. V nabídce na horní klikněte na **uživatele**.
    
    ![Přiřazení uživatelů] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Přiřazení uživatelů")

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.
    
    ![Přiřazení uživatelů] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Přiřazení uživatelů")


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.
 
Po kliknutí na dlaždici ŽÁDNÝM na panelu aplikace Access můžete by měla získat automaticky přihlášení z ŽÁDNÝM aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)