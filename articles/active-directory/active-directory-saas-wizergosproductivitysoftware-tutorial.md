<properties
    pageTitle="Kurz: Azure Active Directory integrace Wizergos produktivity softwarem | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Software pro zvýšení produktivity Wizergos."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Kurz: Azure Active Directory integrace Wizergos produktivity softwarem 

Cílem tohoto kurzu je vidíte, jak integrovat Wizergos Productivity Software s Azure Active Directory (Azure AD).

Integrace s Azure AD Wizergos produktivitu Software poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Wizergos produktivity Software
- Povolení uživatelům, aby automaticky získat přihlášení z Wizergos produktivity software (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD Wizergos produktivity softwarem, musíte následující položky:

- Předplatné Azure AD
- Software pro zvýšení produktivity Wizergos jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Wizergos produktivity Software z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Přidání Wizergos produktivity Software z Galerie
Pokud chcete nakonfigurovat integraci Wizergos Productivity Software do Azure AD, potřebujete přidat Wizergos Productivity Software z Galerie do seznamu spravované SaaS aplikace.

**Přidání Wizergos produktivity softwaru z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .
    
    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.
    
    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Wizergos produktivitu Software**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)

7. V panelu Výsledky vyberte **Wizergos produktivity Software**a klikněte na **Dokončit** přidat aplikaci.

    ![Výběr aplikace v galerii](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Wizergos produktivity softwaru na základě zkušební uživatele s názvem "Britta Simon".

Pro jednotné přihlašování pracovat musí Azure AD ví, co je databáze odpovídajících funkcí pro uživatele v Wizergos produktivity Software pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Wizergos produktivity Software jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Wizergos produktivity Software.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s BynWizergos produktivitu Softwareder, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření softwaru Productivity Wizergos otestujte uživatele](#creating-a-wizergos-productivity-software-test-user)** – a protějšek Britta Simon Wizergos Productivity Software, který je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci Wizergos produktivity Software.

**Abyste mohli nakonfigurovat Azure AD jednotného přihlašování Wizergos Productivity softwarem, proveďte následující kroky:**

1. Na portálu klasické na stránce **Software produktivitu Wizergos** aplikace integrace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k softwaru produktivity Wizergos** vyberte **Azure AD jednotného přihlašování**a klikněte na **Další**:
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)

3. Na stránce **Konfigurovat nastavení aplikace** dialogového okna klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)

4. Na stránce **konfigurovat jednotné přihlašování v článku Wizergos produktivitu Software** klikněte na tlačítko **Stáhnout certifikát**a uložte jej ve vašem počítači:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)

5. V okně různých webového prohlížeče přihlašování ke tenantovi Wizergos produktivity Software jako správce.

6. V nabídce hamburger vyberte **Správce**.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)

7. Na stránce pro správu v nabídce vlevo vyberte **ověřování** a klepněte na **Azure AD**.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)

8. Proveďte následující kroky v části **ověřování** .

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)

    na. Klikněte na tlačítko **NAHRÁT** nahrát stažený certifikát z Azure AD. 

    b. V **Adrese URL Vystavitel** textové pole vložit hodnotu **Vystavitel URL** z Azure AD Průvodce konfigurací aplikace.

    c. V adresu **URL přihlašování** textové pole vložit hodnotu **Jedné přihlašování adresy URL služby** z Azure AD Průvodce konfigurací aplikace.

    d. V **Jedné adresy URL Sign-Out** textové pole vložit hodnotu **Jedné adresy URL služby Sign-out** z Azure AD Průvodce konfigurací aplikace.

    e. Klikněte na tlačítko **Uložit** .

9. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

10. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png)

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-wizergos-productivity-software-test-user"></a>Vytvoření uživatele test Wizergos Productivity Software

V této části vytvoříte uživatele s názvem Britta Simon Wizergos produktivity softwaru. Zkontrolujte pracovat se na tým podpory Wizergos produktivity softwaru přes [support@wizergos.com](emailTo:support@wizergos.com) přidáte uživatele v platformu Wizergos produktivity Software.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup Wizergos produktivitu softwaru.
    
   ![Přiřazení uživatele][200]

**Pokud chcete přiřadit Britta Simon Wizergos produktivity Software, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
    
    ![Přiřazení uživatele][201]

2. V seznamu aplikací vyberte **Wizergos produktivity Software**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)

1. V nabídce na horní klikněte na **uživatele**.
    
    ![Přiřazení uživatele][203]

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.
    
    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.
 
Po kliknutí na dlaždici Wizergos Productivity Software na panelu aplikace Access můžete by měla získat automaticky přihlášení na aplikaci Wizergos Productivity Software.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
