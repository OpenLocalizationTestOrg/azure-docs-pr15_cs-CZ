<properties
    pageTitle="Kurz: Azure Active Directory integrace s Tangoe příkaz Premium Mobile | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Tangoe příkaz Premium Mobile."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Kurz: Azure Active Directory integrace s Tangoe příkaz Premium Mobile

V tomto kurzu se naučíte, jak integrovat Tangoe příkaz Premium Mobile s Azure Active Directory (Azure AD).

Integrace Tangoe příkaz Premium Mobile s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Tangoe příkaz Premium Mobile
- Povolení uživatelům, aby automaticky získat přihlášení z Tangoe příkaz Premium Mobile (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat Azure AD integrace s Tangoe příkaz Premium Mobile, musíte následující položky:

- Předplatné Azure
- Mobilní Premium příkaz Tangoe jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí. 

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Mobile Premium příkaz Tangoe z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-tangoe-command-premium-mobile-from-the-gallery"></a>Přidání Tangoe příkaz Premium Mobile z Galerie
Pokud chcete nakonfigurovat integraci Tangoe příkaz Premium Mobile do Azure AD, potřebujete přidat Tangoe příkaz Premium Mobile z Galerie do seznamu spravované SaaS aplikace.

**Přidání Mobile Premium příkaz Tangoe z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Tangoe příkaz Premium Mobile**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_01.png)

7. V podokně výsledků vyberte **Tangoe příkaz Premium Mobile**a pak klikněte na **Dokončit** přidat aplikaci.

![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_02.png)




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Tangoe příkaz Premium Mobile založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v Tangoe příkaz Premium Mobile je pro uživatele v Azure AD. Jinými slovy odkaz vztah mezi Azure Active Directory a související uživatele v Tangoe příkaz Premium Mobile musí vytvořit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Tangoe příkaz Premium Mobile.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Tangoe příkaz Premium Mobile, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření Tangoe příkaz Premium Mobile testování uživatele](#creating-an-tangoe-test-user)** – mít protějšek Britta Simon v Tangoe příkaz Premium Mobile, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci Tangoe příkaz Premium Mobile.


**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s Tangoe příkaz Premium Mobile, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **Tangoe příkaz Premium mobilní** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6]


2. Na stránce, **jakým způsobem uživatelé přihlásit k Tangoe příkaz Premium Mobile** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_03.png) 


3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_04.png) 


    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování Tangoe příkaz Premium mobilní aplikaci pomocí následujícího vzorce: **"https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=\<klienta Vystavitel\>& cílový =\<cílovou adresu URL stránky\>"**.

    b. V textovém poli **Odpovědět URL** zadejte adresu URL do následujícího vzorce: **"https://sso.tangoe.com/sp/ACS.saml2"**

    > [AZURE.NOTE]  Pokud neznáte správné hodnoty pro adresy URL, můžete hodnoty nad jako zástupné symboly a žádost o správné hodnoty z Zákaznická podpora Tangoe přidružit.


4. Na stránce **Konfigurace jednotného přihlašování na mobilní telefon Premium příkaz Tangoe** proveďte následující kroky:
 
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_05.png) 

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.


5.  Jednotné přihlašování nakonfigurován pro aplikaci získáte kontaktujte vaše Tangoe Zákaznická podpora přidružení a zadejte tyto údaje:


    - Soubor stažený metadat
    - **Adresa URL vydavatel**
    - Adresu **URL SAML jednotné přihlašování**
    - **Adresa URL jednoho odhlašovací služby**


  
6. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
  
    ![Azure AD jednotné přihlášení][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.

V seznamu uživatelů zvolte **Britta Simon**.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png)


5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-tangoe-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-tangoe-command-premium-mobile-test-user"></a>Vytvoření uživatele test Tangoe příkaz Premium Mobile

V této části vytvoříte uživatele s názvem Britta Simon v Tangoe příkaz Premium Mobile. Tangoe příkaz Premium mobilní aplikace potřebovat všem uživatelům v aplikaci předtím jednotné přihlašování zřízení. Takže prosím práce s přidružit podpora zákazníků Tangoe zřízení tyto uživatele do aplikace. 


> [AZURE.NOTE] Pokud potřebujete k vytvoření uživatele ručně nebo dávkové uživatelů, budete muset kontaktovat tým podpory Tangoe příkaz Premium Mobile.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělení Tangoe příkaz Premium mobilní přístup.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Tangoe příkaz Premium Mobile, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Tangoe příkaz Premium Mobile**.

![Konfigurace jednotného přihlašování](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Tangoe příkaz Premium Mobile na panelu aplikace Access můžete by měla získat automaticky přihlášení z Tangoe příkaz Premium mobilní aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_205.png
