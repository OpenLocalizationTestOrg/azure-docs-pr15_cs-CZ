<properties
    pageTitle="Kurz: Azure Active Directory integrace s RunMyProcess | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a RunMyProcess."
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
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Kurz: Azure Active Directory integrace s RunMyProcess

Cílem tohoto kurzu je vidíte, jak integrovat RunMyProcess s Azure Active Directory (Azure AD).

Integrace RunMyProcess s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k RunMyProcess
- Povolení uživatelům, aby automaticky získat přihlášení z k RunMyProcess (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí RunMyProcess, musíte následující položky:

- Předplatné Azure AD
- RunMyProcess jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání RunMyProcess z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-runmyprocess-from-the-gallery"></a>Přidání RunMyProcess z Galerie
Pokud chcete nakonfigurovat integraci RunMyProcess do Azure AD, potřebujete přidat RunMyProcess z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat RunMyProcess z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **RunMyProcess**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. V podokně výsledků vyberte **RunMyProcess**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí RunMyProcess založeného na uživateli testu s názvem "Britta Simon".

Pro jednotné přihlašování pro práci je potřeba Azure AD ví, co uživatel protějšek v RunMyProcess je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v RunMyProcess jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v RunMyProcess.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s RunMyProcess, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření RunMyProcess otestujte uživatele](#creating-a-runmyprocess-test-user)** – a protějšek Britta Simon RunMyProcess, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci RunMyProcess.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s RunMyProcess, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **RunMyProcess** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k RunMyProcess** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL pomocí následujícího vzorce: `https://live.runmyprocess.com/live/<tenant id>`.
        
    b. Klikněte na tlačítko **Další**

    > [AZURE.NOTE] Všimněte si, že budete muset aktualizovat hodnotu skutečné znaménko na adresu URL. Získat tuto hodnotu, obraťte se na tým podpory RunMyProcess prostřednictvím <mailto:support@runmyprocess.com>.
 
4. Na stránce **konfigurovat jednotné přihlašování v RunMyProcess** klikněte na **Stáhnout certifikát** a potom uložit soubor v počítači:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. V okně různých webového prohlížeče přihlašování ke tenantovi RunMyProcess jako správce.

6. V levém navigačním panelu klikněte na **účet** a vyberte **Konfigurace**.

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. Přejděte do části **metody ověřování** a proveďte pod kroky:

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    na. Jako **způsob**vyberte **jednotné přihlašování s Samlv2**.

    b. V **přesměrování jednotné přihlašování** textové pole vložit hodnotu **SAML jednotného přihlašování k URL** z Azure AD Průvodce konfigurací aplikace.

    c. V **přesměrování odhlášení** vložte textové pole hodnotu **Jedné adresy URL služby Sign-Out** ze Průvodce konfigurací aplikace Azure AD.

    d. V seznamu **Formát názvu Id** textové pole vložit hodnoty **Název identifikátor formát** z Průvodce konfigurací aplikace Azure AD.

    e. Zkopírujte obsah souboru stažený certifikát a potom je vložte do textového pole **certifikát** . 

    f. Klikněte na ikonu **Uložit** .
    
8. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

9. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:  ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   

### <a name="creating-a-runmyprocess-test-user"></a>Vytvoření uživatele RunMyProcess test
  
Pokud chcete povolit uživatelům Azure AD přihlásit RunMyProcess, musí být zřízení do RunMyProcess. V případě RunMyProcess zřizování se ručně naplánovaný úkol.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti RunMyProcess jako správce.

2.  Klikněte na **účet** a v levém navigačním panelu vyberte **Uživatelé** a potom klikněte na **Nový uživatel**.

    ![Nového uživatele] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "Nového uživatele")

3.  V části **Nastavení** proveďte následující kroky:

    ![Profil] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profil")

    na. Zadejte **jméno** a **e-mailu** platný AAD účet, který chcete poskytování do související textová pole.

    b. Vyberte **jazyk integrovaném vývojovém prostředí**, **jazyk** a **profilu**.

    c. Vyberte **Odeslat e-mail vytvoření účtu pro mě**.

    d. Klikněte na **Uložit**.

    >[AZURE.NOTE] Můžete použít jakékoli další RunMyProcess uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou RunMyProcess poskytování služby Azure Active Directory uživatelské účty.
    

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k RunMyProcess.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon RunMyProcess, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **RunMyProcess**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. řádek v dolní části klikněte na **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.
 
Po kliknutí na dlaždici RunMyProcess na panelu aplikace Access můžete by měla získat automaticky přihlášení z RunMyProcess aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
