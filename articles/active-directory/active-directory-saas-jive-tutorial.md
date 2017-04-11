<properties
    pageTitle="Kurz: Azure Active Directory integrace s Jive | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Jive."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Kurz: Azure Active Directory integrace s Jive

V tomto kurzu se naučíte, jak integrovat Jive s Azure Active Directory (Azure AD).

Integrace Jive s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Jive
- Povolení uživatelům, aby automaticky získat přihlášení na k Jive (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Jive, musíte následující položky:

- Předplatné Azure AD
- Jive jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Jive z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-jive-from-the-gallery"></a>Přidání Jive z Galerie
Pokud chcete nakonfigurovat integraci Jive do Azure AD, potřebujete přidat Jive z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Jive z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]
2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Jive**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. V podokně výsledků vyberte **Jive**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Jive založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v Jive je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Jive jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Jive.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Jive, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Jive testování uživatele](#creating-a-jive-test-user)** – a protějšek Britta Simon Jive, která je propojená s Azure AD znázornění jí.
4. **[Konfigurace zřizování uživatelů](#configuring-user-provisioning)** – přehledu povolení zřizování uživatele služby Active Directory uživatele do Jive účtů.
5. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
6. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Jive.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Jive, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **Jive** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jak chcete uživatelům přihlásit k Jive** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Jive pomocí následujícího vzorce: **https://\<jméno zákazníka\>. jivecustom.com**.
    
    b. Klikněte na tlačítko **Další**
 
4. Na stránce **konfigurovat jednotné přihlašování v Jive** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5. Přihlášení k vašemu tenantovi Jive jako správce.

6. V nabídce nahoře, klikněte na "**Saml**".

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    na. Na kartě **Obecné** vyberte **Povolit** .

    b. Klikněte na tlačítko "**Uložit všechna nastavení saml**".

7. Přejděte na kartu "**Idp metadat**".

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    na. Zkopírujte obsah souboru XML stažený metadat a potom je vložte do textového pole **metadat zprostředkovatele identit (IDP)** .

    b. Klikněte na tlačítko "**Uložit všechna nastavení saml**". 

8. Přejděte na kartu "**Mapování atributu uživatele**".

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    na. Do textového pole **e-mailu** zkopírujte a vložte název atributu **hromadné** hodnoty.

    b. **Křestní jméno** do textového pole zkopírujte a vložte název atributu **givenname** hodnoty.

    c. V textovém poli **Příjmení** zkopírujte a vložte název atributu **Příjmení** hodnoty.
    
9. Na portálu Azure AD vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
![Azure AD jednotné přihlášení][10]

10. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
  ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.


![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:  ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasného hesla** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



###<a name="creating-a-jive-test-user"></a>Vytvoření uživatele Jive test

V této části vytvoříte uživatele s názvem Britta Simon v Jive. Zkontrolujte pracujte se na tým podpory Jive přidáte uživatele v platformu Jive.


###<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Cílem tento oddíl je přehledu povolení uživatele zřizování adresářové služby Active Directory pro Jive.  
V rámci tohoto postupu je nutné zadat tokenu zabezpečení uživatele, které bude nutné požádat Jive.com.
  
Následující obrázek ukazuje příklad příslušné dialogové okno v Azure AD:

![Konfigurace zřizování uživatelů] (./media/active-directory-saas-jive-tutorial/IC698794.png "Konfigurace zřizování uživatelů")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Na portálu Správa Azure na stránce integrace **Jive** aplikace klikněte na **Konfigurovat zřizování uživatelů** otevřete dialogové okno **Konfigurovat zřizování uživatelů** .

2.  Na stránce **Zadejte svoje přihlašovací údaje Jive povolit zřizování automatické uživatele** zadejte následující nastavení:

    1.  V textovém poli **Jive správce uživatelské jméno** zadejte název účtu Jive obsahující profil **Správce systému** v Jive.com přiřazené.

    2.  Do textového pole **Heslo správce Jive** zadejte heslo k tomuto účtu.

    3.  **Adresa URL Jive klienta** do textového pole zadejte adresu URL Jive klienta.

        >[AZURE.NOTE]Adresa URL Jive klienta je adresa URL, která je vaše organizace používá k přihlášení do Jive.  
        Adresa URL má obvykle, v tomto formátu: **www.\< organizace\>. jive.com**.

    4.  Klikněte na **Ověřit** pro ověření vaší konfigurace.

    5.  Kliknutím na tlačítko **Další** a otevře se stránka **potvrzení** .

3.  Na stránce **potvrzení** klikněte na políčko Uložit konfiguraci.
  
Nyní můžete vytvořit testovací účet, až 10 minut a ověřte, že účet synchronizování k Jive.com.




### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Jive.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Jive, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Jive**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Jive na panelu aplikace Access můžete by měla získat automaticky přihlášení z Jive aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
