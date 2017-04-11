<properties
    pageTitle="Kurz: Azure Active Directory integrace s 8 × 8 virtuální Office | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active adresář a 8 x 8 virtuální Office."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Kurz: Azure Active Directory integrace se službou Office virtuální 8 x 8

Cílem tohoto kurzu je vidíte, jak integrovat Office virtuální 8 x 8 s Azure Active Directory (Azure AD).

Integrace 8 x 8 virtuální Office s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Office virtuální 8 x 8
- Povolení uživatelům, aby automaticky získat přihlášení z Office virtuální 8 x 8 (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s 8 x 8 virtuální Office, musíte následující položky:

- Předplatné Azure AD
- 8 × 8 virtuální Office jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Microsoft Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání 8 x 8 virtuální Office z Galerie
2. Konfigurace a testování Microsoft Azure AD jednotné přihlašování


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Přidání 8 x 8 virtuální Office z Galerie
Pokud chcete nakonfigurovat integraci Office virtuální 8 x 8 do Azure AD, potřebujete přidat 8 x 8 virtuální Office z Galerie do seznamu spravované SaaS aplikace.

**Přidání 8 × 8 virtuální Office z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horním na **aplikace** .
    
    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **8 x 8 virtuální Office**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. V podokně výsledků vyberte **8 x 8 virtuální Office**a pak klikněte na **Dokončit** přidat aplikaci.

    ![Výběr aplikace v galerii](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfigurace a testování Microsoft Azure AD jednotné přihlašování
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Microsoft Azure AD jednotné přihlašování s 8 × 8 virtuální Office podle testovací uživatelské jméno s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí vědět, jaké databáze odpovídajících funkcí pro uživatele systému 8 x 8 je virtuální Office pro uživatele v Azure AD. Jinými slovy odkaz vztah mezi Azure Active Directory a související uživatele v 8 × 8 virtuální Office je potřeba vytvořit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelského jména** v Office virtuální 8 x 8.

Testování a konfigurace aplikace Microsoft Azure AD jednotné přihlašování s 8 x 8 virtuální Office, je potřeba provést následující stavební bloky:

1. **[Konfigurace aplikace Microsoft Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD otestovat uživatele](#creating-an-azure-ad-test-user)** – otestovat Microsoft Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření uživatele test virtuální Office 8 x 8](#creating-a-8x8-virtual-office-test-user)** – mít protějšek Britta Simon v 8 x 8 virtuální Office, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Microsoft Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Konfigurace Microsoft Azure AD jednotné přihlášení

V tomto oddílu povolení Microsoft Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci Office virtuální 8 × 8.

**Abyste mohli nakonfigurovat Microsoft Azure AD jednotné přihlašování s 8 x 8 virtuální Office, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace aplikace **Office virtuální 8 x 8** klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jak chcete uživatelům Přihlaste se k Office virtuální 8 x 8** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    na. **Adresa URL odpověď** do textového pole zadejte:`https://sso.8x8.com/saml2`

    b. Klikněte na tlačítko **Další**

4. Na stránce **Konfigurace jednotného přihlašování v Office virtuální 8 × 8** proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.

5. Přihlášení k vašemu tenantovi 8 × 8 virtuální Office jako správce.
6. Vyberte **virtuální Office účtu správce** na panelu aplikace.

    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. Vyberte účet **firmy** a správa klikněte na tlačítko **Přihlásit** .

    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. Klikněte na kartu **Klienti** v seznamu v nabídce.

    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. Klikněte na **Jednotné přihlašování** v seznamu účtů.

    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. Zaškrtněte políčko **Signle přihlášení** v části metody ověřování a klikněte na **SAML**.

    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. Zkopírujte **adresu URL SAML SSO**, **jednoduchým zpívají se adresa URL služby** a **adresu URL Vystavitel** z Azure AD se **přihlásit**k adresy URL, **odhlášení adresy URL** a **Vystavitel URL** v 8 x 8 virtuální Office. 

    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![Konfigurace na straně aplikace](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. Pokud chcete odeslat certifikát, který jste si stáhli z Azure AD na tlačítko **prohlížeče** .

13. Klikněte na tlačítko **Uložit** .

14. Na portálu klasický vyberte potvrzovacím jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

15. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu klasické s názvem Britta Simon.
    
![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-8x8-virtual-office-test-user"></a>Vytvoření uživatele 8 x 8 virtuální Office test

Cílem tento oddíl je vytvoření uživatele v 8 x 8 virtuální Office s názvem Britta Simon. 8 x 8 virtuální Office podporuje zřizování za běhu, což je standardně povolený.

Neexistuje žádná akce položku, kterou v této části. Při pokusu o přístup k Office virtuální 8 x 8, pokud dosud neexistuje se vytvoří nový uživatel. 

> [AZURE.NOTE] Pokud je potřeba ručně vytvořit uživatelem, budete muset kontaktovat tým podpory 8 x 8 virtuální Office.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup k Office virtuální 8 x 8.
    
![Přiřazení uživatele][200]

**Pokud chcete přiřadit Britta Simon 8 x 8 virtuální Office, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201]

2. V seznamu aplikací vyberte **8 x 8 virtuální Office**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. V nabídce na horní klikněte na **uživatele**.
    
    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je testování Microsoft Azure AD jednotného přihlašování konfigurace pomocí panelu aplikace Access.

Po kliknutí na dlaždici 8 × 8 virtuální Office na panelu aplikace Access můžete by měla získat automaticky přihlášení na aplikaci Office virtuální 8 × 8.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
