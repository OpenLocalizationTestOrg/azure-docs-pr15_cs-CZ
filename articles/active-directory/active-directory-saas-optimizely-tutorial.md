<properties
    pageTitle="Kurz: Azure Active Directory integrace s Optimizely | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Optimizely."
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
    ms.date="09/11/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Kurz: Azure Active Directory integrace s Optimizely

V tomto kurzu se naučíte, jak integrovat Optimizely s Azure Active Directory (Azure AD).

Integrace Optimizely s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Optimizely
- Povolení uživatelům, aby automaticky získat přihlášení z k Optimizely (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Optimizely, musíte následující položky:

- Předplatné Azure AD
- **Optimizely** jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí. Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Optimizely z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-optimizely-from-the-gallery"></a>Přidání Optimizely z Galerie
Pokud chcete nakonfigurovat integraci Optimizely do Azure AD, potřebujete přidat Optimizely z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Optimizely z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Optimizely**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_01.png)

7. V podokně výsledků vyberte **Optimizely**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Optimizely založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v Optimizely je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Optimizely jinými slovy, je nutné stanovit.
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Optimizely.

Testování a konfigurace Azure AD jednotné přihlašování s Optimizely, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření Optimizely testování uživatele](#creating-an-optimizely-test-user)** – a protějšek Britta Simon Optimizely, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Optimizely.

Aplikace Optimizely očekává výrazy SAML má být atribut s názvem "e-mailu". Hodnota "e-mailu" by měla být e-mailu Optimizely rozpoznala, které můžete získat ověřeny Azure AD. Nakonfigurujte deklaraci "e-mailu" pro tuto aplikaci. Na kartě **"Atributy"** aplikace můžete spravovat hodnoty tyto atributy. Následující obrázek ukazuje příklad pro tuto. 


![Konfigurace jednotného přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_03.png) 


**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Optimizely, proveďte následující kroky:**

1. Na portálu na stránce integrace aplikace **Optimizely** v nabídce v horní části Azure klasické klepněte na položku **atributy**.
     
    ![Konfigurace jednotného přihlašování][5]

2. V dialogu tokenu atributy SAML přidejte atribut "e-mailu".

    na. Klikněte na **Přidat uživatele atribut** otevřete dialogové okno **Přidat atribut uživatele** . 
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_05.png)

    b. **Název atributu** do textového pole zadejte název atributu "e-mail".

    c. **Hodnota atributu** seznamu vyberte hodnotu atributu "userprincipalname" nebo libovolnou hodnotu, která obsahuje e-mailu rozpozná Azure AD a Optimizely.

    d. Klikněte na **dokončení**.
3. V nabídce nahoře klikněte na **Rychlý Start**.

    ![Konfigurace jednotného přihlašování][6]
4. Na portálu klasické na stránce integrace **Optimizely** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][7] 

5. Na stránce **jakým způsobem uživatelé přihlásit k Optimizely** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_06.png)

6. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)


    na. **Přihlaste se na adresu URL** do textového pole zadejte:`https://app.optimizely.net/contoso`

    b. **Identifikátor URI** do textového pole zadejte:`urn:auth0:optimizely:contoso`

    c. Klikněte na tlačítko **Další**. 


    > [AZURE.NOTE] Hodnoty pro **Znaménko na adresu URL** a **identifikátor URI** jsou pouze zástupné symboly správných hodnot. Najdete pokyny k aquiring skutečné hodnoty z Optimizely dál v tomto kurzu.

7. Na stránce **konfigurovat jednotné přihlašování v Optimizely** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_08.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Zkopírujte adresu **URL služby jednotného přihlašování**.

8. Pokud chcete dostat SSO nakonfigurován pro aplikaci, kontaktujte svého správce účtu Optimizely a zadejte následující informace:

    - Pokud chcete stažený certifikátu 
    - Adresa URL služby jednotného přihlašování
 
    Odpověď na e-mailu Optimizely umožňuje přihlásit na adresu URL (SP inicializovaný SSO) a identifikátor URI (ID Entity poskytovatele služby) hodnoty.

9. Přejděte zpátky na stránku dialogové okno **Konfigurovat nastavení aplikace** a pak proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)

    na. **Přihlaste se na adresu URL** do textového pole zadejte **Inicializovaný SP URL jednotné přihlašování** poskytovanou Optimizely.

    b. Do textového pole **identifikátor** zadejte **ID Entity poskytovatele služeb** poskytovanou Optimizely.

    c. Klikněte na tlačítko **Další**.

10. Na stránce **konfigurovat jednotné přihlašování v Optimizely** proveďte následující kroky:
    
    ![Azure AD jednotné přihlášení][10]

    na. Vyberte potvrzení jednotné přihlašování.

    b. Klikněte na tlačítko **Další**.

11. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
    ![Azure AD jednotné přihlášení][11]

12. V jiném okně prohlížeče přihlašování do aplikace Optimizely.
13. Klikněte na název v pravém horním rohu a pak **Nastavení účtu**klienta.

    ![Azure AD jednotné přihlášení](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

14. Na kartě Účet zaškrtněte políčko **Povolit jednotné přihlašování** v části jednotné přihlašování v části **Přehled** .

    ![Azure AD jednotné přihlášení](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)

### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.
V seznamu uživatelů zvolte **Britta Simon**.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-optimizely-test-user"></a>Vytvoření uživatele test Optimizely

V této části vytvoříte uživatele s názvem Britta Simon v Optimizely.

1. Na domovské stránce vyberte kartu **spolupracovníci**
2. Klikněte na **Nový Collaborator** přidáte nové collaborator do projektu.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3.  Zadejte e-mailovou adresu a jejich přiřazení role. Klikněte na **pozvánku**.


    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. Tato osoba obdrží pozvat e-mailu. Používání e-mailovou adresu. budou muset přihlásit Optimizely.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Optimizely.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Optimizely, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Optimizely**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu všichni uživatelé vyberte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Optimizely na panelu aplikace Access můžete by měla získat automaticky přihlášení z Optimizely aplikaci.

## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-optimizely-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_205.png
