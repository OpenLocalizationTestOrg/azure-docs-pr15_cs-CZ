<properties
    pageTitle="Kurz: Azure Active Directory integrace s SanSan | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SanSan."
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


# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Kurz: Azure Active Directory integrace s SanSan

V tomto kurzu se naučíte, jak integrovat SanSan s Azure Active Directory (Azure AD).

Integrace SanSan s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k SanSan
- Povolení uživatelům, aby automaticky získat přihlášení na k SanSan (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí SanSan, musíte následující položky:

- Předplatné Azure AD
- SanSan jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu otestujte Microsoft Microsoft Azure AD jednotné přihlašování v testovacím prostředí. Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SanSan z Galerie
2. Konfigurace a testování Microsoft Azure AD jednotné přihlašování


## <a name="adding-sansan-from-the-gallery"></a>Přidání SanSan z Galerie
Pokud chcete nakonfigurovat integraci SanSan do Azure AD, potřebujete přidat SanSan z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat SanSan z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **SanSan**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_01.png)

7. V podokně výsledků vyberte **SanSan**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_06.png)

##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Konfigurace a testování Microsoft Azure AD jednotné přihlašování
V této části Konfigurace a otestujte Microsoft Azure AD jednotné přihlašování s SanSan založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v SanSan je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v SanSan jinými slovy, je nutné stanovit.
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v SanSan.

Testování a konfigurace aplikace Microsoft Azure AD jednotné přihlašování s SanSan, je potřeba provést následující stavební bloky:

1. **[Konfigurace aplikace Microsoft Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Microsoft Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření SanSan testování uživatele](#creating-an-sansan-test-user)** – a protějšek Britta Simon SanSan, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Microsoft Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Konfigurace Microsoft Azure AD jednotné přihlášení

V tomto oddílu povolení Microsoft Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci SanSan.


**Abyste mohli nakonfigurovat Microsoft Azure AD jednotné přihlašování s SanSan, proveďte následující kroky:**


1. Na portálu Azure klasické na stránce integrace **SanSan** aplikace klikněte na konfigurovat jednotné přihlašování otevřete dialogové okno Konfigurace jednotného přihlašování.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png) 

2. Na stránce **jakým způsobem uživatelé přihlásit k SanSan** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole zadejte adresu URL do následujícího vzorce:

  	| Prostředí             | ADRESA URL |
  	| :--                     | :-- |
  	| PC web                  | `https://ap.sansan.com/v/saml2/<company name>/acs`|
  	| Nativní mobilní aplikaci       | `https://internal.api.sansan.com/saml2/<company name>/acs` |
  	| Nastavení mobilního prohlížeče | `https://ap.sansan.com/s/saml2/<company name>/acs` |


    b. Do textového pole **identifikátor** zadejte adresu URL do následujícího vzorce: 

  	| Prostředí             | ADRESA URL |
  	| :--                     | :-- |
  	| PC web                  | `https://ap.sansan.com/v/saml2/<company name>`|
  	| Nativní mobilní aplikaci       | `https://internal.api.sansan.com/saml2/<company name>` |
  	| Nastavení mobilního prohlížeče | `https://ap.sansan.com/s/saml2/<company name>` |


    c. Klikněte na tlačítko **Další**.

4. Na stránce **konfigurovat jednotné přihlašování v SanSan** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5.  Jednotné přihlašování nakonfigurován pro aplikaci získáte kontaktů tým podpory SanSan a vám pomůžou Konfigurace jednotného přihlašování. Zkontrolujte poskytněte mu Tyhle informace:

    • Stažený **certifikátu**

    • **ID zprostředkovatele identit**

    • Adresu **URL SAML jednotné přihlašování**

    • **Jednoho odhlásit adresy URL služby**

> [AZURE.NOTE] Nastavení také pracovní zátěž: mobilní aplikace a mobilní prohlížeče spolu s web počítače PC prohlížeče. 


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
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sansan-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sansan-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sansan-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasného hesla** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sansan-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sansan-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-sansan-test-user"></a>Vytvoření uživatele test SanSan

V této části vytvoříte uživatele s názvem Britta Simon v SanSan. Aplikace SanSan potřebovat uživateli zřízení v aplikaci předtím jednotné přihlašování. 


> [AZURE.NOTE] Pokud potřebujete k vytvoření uživatele ručně nebo dávkové uživatelů, budete muset kontaktovat tým podpory SanSan.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k SanSan.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon SanSan, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **SanSan**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Microsoft Azure AD jednotného přihlašování konfiguraci pomocí panelu aplikace Access.
Po kliknutí na dlaždici SanSan na panelu aplikace Access můžete by měla získat automaticky přihlášení z SanSan aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_205.png
