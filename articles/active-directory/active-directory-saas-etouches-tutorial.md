<properties
    pageTitle="Kurz: Azure Active Directory integrace s eTouches | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a eTouches."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Kurz: Azure Active Directory integrace s eTouches

V tomto kurzu se naučíte, jak integrovat eTouches s Azure Active Directory (Azure AD).

Integrace eTouches s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k eTouches
- Povolení uživatelům, aby automaticky získat přihlášení z k eTouches (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí eTouches, musíte následující položky:

- Předplatné Azure AD
- ETouches jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání eTouches z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-etouches-from-the-gallery"></a>Přidání eTouches z Galerie
Pokud chcete nakonfigurovat integraci eTouches do Azure AD, potřebujete přidat eTouches z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat eTouches z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]
2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **eTouches**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_01.png)

7. V podokně výsledků vyberte **eTouches**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení

V této části Konfigurace a otestujte Azure AD jednotné přihlašování s eTouches založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v eTouches je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v eTouches jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v eTouches.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s eTouches, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření eTouches otestujte uživatele](#creating-a-predictix-price-reporting-test-user)** – mít protějšek Britta Simon v eTouches, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci eTouches.

aplikace eTouches očekává výrazy SAML v určitém formátu. Nakonfigurujte následující deklarací k této aplikaci. Na kartě **"Atrribute"** aplikace můžete spravovat hodnoty tyto atributy. Následující obrázek ukazuje příklad pro tuto. 

![Konfigurace jednotného přihlašování](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_07.png) 

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s eTocuhes, proveďte následující kroky:**


1. Na portálu na stránce integrace aplikace **eTouches** v nabídce v horní části Azure klasické klepněte na položku **atributy**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-etouches-tutorial/tutorial_general_80.png) 


2. V dialogovém okně **SAML tokenu atributy** pro každý řádek v tabulce níže, proveďte následující kroky:

  	| Název atributu | Hodnota atributu |
  	| --- | --- |    
  	| E-mailu | User.Mail |

    na. Klikněte na **Přidat uživatele atribut** otevřete dialogové okno **Přidat Attribure uživatele** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-etouches-tutorial/tutorial_general_81.png) 


    b. Do textového pole **Attrubute název** napište název atribut zobrazí pro tento řádek.

    c. Ze seznamu **Hodnota atributu** selsect hodnota atributu zobrazí pro tento řádek.

    d. Klikněte na **dokončení**.  
    

3. Na portálu klasické na stránce integrace **eTouches** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

4. Na stránce **jakým způsobem uživatelé přihlásit k eTouches** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_03.png) 

5. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace eTouches pomocí následujícího vzorce: **https://www.eiseverywhere.com/saml/accounts/?sso&accountid=\<accountid\>**.
    
    b. Klikněte na tlačítko **Další**
 
6. Na stránce **konfigurovat jednotné přihlašování v eTouches** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_05.png)

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.


7. Získat SSO nakonfigurován pro aplikaci, proveďte následující kroky v aplikaci eTouches:

    na. Přihlaste se k aplikaci **eTouches** pomocí administrátorská práva.
    
    b. Přejděte na konfigurace **SAML**

    c. V části **Obecné nastavení** vkládání obsahu Azure AD federace metadat do textového pole.

    d. Klikněte na tlačítko **Uložit a zachovat**

    e. Klikněte na tlačítko **Metadata aktualizace** v části SAML Metadata. 

    f. Otevřete stránku a provede jednotné přihlašování. Jakmile pracuje jednotné přihlašování pak můžete nastavit uživatelské jméno

    g. Do pole **uživatelské jméno** vyberte **email** , jak je znázorněno na následujícím obrázku. 

    h. Kopírovat **jednotného přihlašování k URL nebo ACS** hodnotu a vložit ho do Azure AD aplikace konfigurace Průvodce znaménko na adresu URL do textového pole.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png)

8. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

9. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.


![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-etouches-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:  ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-etouches-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-etouches-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-etouches-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-etouches-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-etouches-test-user"></a>Vytvoření uživatele test eTouches

V této části vytvoříte uživatele s názvem Britta Simon v eTouches. Zkontrolujte pracujte se na tým podpory eTouches přidáte uživatele v platformu eTouches.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k eTouches.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon eTouches, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **eTouches**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_50.png) 

3. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici eTouches na panelu aplikace Access můžete by měla získat automaticky přihlášení z eTouches aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_205.png
