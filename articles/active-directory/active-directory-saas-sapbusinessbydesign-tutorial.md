<properties
    pageTitle="Kurz: Azure Active Directory integrace s SAP firmy ByDesign | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAP firmy ByDesign."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Kurz: Azure Active Directory integrace s ByDesign firmy SAP

V tomto kurzu se naučíte, jak integrovat SAP firmy ByDesign s Azure Active Directory (Azure AD).

Integrace SAP firmy ByDesign s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k ByDesign firmy SAP
- Povolení uživatelům, aby automaticky získat přihlášení z k modelům SAP ByDesign Business (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí ByDesign firmy SAP, musíte následující položky:

- Předplatné Azure AD
- SAP firmy ByDesign jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SAP firmy ByDesign z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>Přidání SAP firmy ByDesign z Galerie
Abyste mohli nakonfigurovat integraci SAP firmy ByDesign do Azure AD, potřebujete přidat SAP firmy ByDesign z Galerie do seznamu spravované SaaS aplikace.

**SAP firmy ByDesign přidat z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.


3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **SAP firmy ByDesign**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. V podokně výsledků vyberte **SAP firmy ByDesign**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Služby Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s ByDesign firmy SAP založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co databáze odpovídajících funkcí pro uživatele systému SAP firmy ByDesign je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v SAP firmy ByDesign jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v SAP firmy ByDesign.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s SAP firmy ByDesign, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření obchodního ByDesign SAP testování uživatele](#creating-an-sap-business-bydesign-test-user)** – a protějšek Britta Simon ByDesign firmy SAP, který je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci SAP Business ByDesign.

SAP firmy ByDesign aplikace očekává výrazy SAML v určitém formátu. Nakonfigurujte následující deklarací k této aplikaci. Na kartě **"Atrribute"** aplikace můžete spravovat hodnoty tyto atributy. Následující obrázek ukazuje příklad pro tuto. 


**Nakonfigurovat SAP firmy ByDesign Azure AD jednotné přihlašování, proveďte následující kroky:**


1. Na portálu na stránce integrace aplikace **SAP firmy ByDesign** v nabídce v horní části Azure klasické klepněte na položku **atributy**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. V seznamu atributy SAML tokenu atributy vyberte atribut nameidentifier a potom klikněte na **Upravit**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. V dialogovém okně Upravit atribut uživatele proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    na. Hodnota atributu seznamu vyberte **ExtractMailPrefix()** fuction

    b. V seznamu pošta vyberte atribut uživatele, který chcete použít pro implementaci. 
    Například pokud chcete použít číslo zaměstnance jako jedinečný identifikátor uživatele a které máte uložené v ExtensionAttribute2 hodnota atributu, vyberte **user.extensionattribute2**. 

    c. Klikněte na **dokončení**. 
    

4. Na portálu klasické na stránce integrace aplikace **SAP firmy ByDesign** klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

5. Na stránce **jakým způsobem uživatelé přihlásit k SAP firmy ByDesign** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole zadejte adresu URL používané vaší uživatelům přihlašování do aplikace SAP firmy ByDesign pomocí následujícího vzorce:`https://<servername>.sapbydesign.com`
    
    b. Klikněte na tlačítko **Další**
 
7. Na stránce **konfigurovat jednotné přihlašování v SAP firmy ByDesign** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.


8. Získat SSO nakonfigurován pro aplikaci, proveďte následující kroky:

    na. Přihlaste se k portálu SAP firmy ByDesign s oprávněními správce.

    b. Přejděte na **aplikace a běžné úlohy správy uživatele** a klikněte na kartu **Zprostředkovatele identit** .

    c. Klikněte na **Nový zprostředkovatele identit** a vyberte soubor XML metadat, který jste stáhli z portálu Microsoft Azure klasické. Importováním metadata systému automaticky uloží požadovaný podpis a šifrování osvědčení.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. Zahrnout **Adresy URL služby spotř výraz** do žádosti SAML, zaškrtněte **Zahrnout výraz spotř adresy URL služby**.

    e. Klikněte na tlačítko **Aktivovat jednotného přihlašování**.

    f. Uložte provedené změny.

    g. Klikněte na kartu **Moje systému** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. Zkopírujte adresu **URL jednotného přihlašování** a vložit ho do textového pole **Azure AD přihlašovací adresa URL** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    Můžu. Určete, zda zaměstnance můžete ručně zvolit přihlášení pomocí ID uživatele a heslo nebo jednotné přihlašování tak, že vyberete **Výběru zprostředkovatele identit ručně**.

    j. V části **Adresa URL jednotné přihlašování** zadejte adresu URL použitý pro zaměstnance k přihlášení do systému. 
    V adresu URL odeslání zaměstnance rozevírací seznam můžete vybrat z následujících možností:
    
    **Adresa URL jednotné přihlašování**
 
    Systém odešle pouze adresu URL normální systém zaměstnance. Zaměstnance nelze přihlášení pomocí jednotného přihlašování a musí používat heslo nebo certifikát místo.

    **ADRESA URL JEDNOTNÉ PŘIHLAŠOVÁNÍ** 

    Systém odešle pouze adresu URL jednotné přihlašování zaměstnance. Zaměstnanec se přihlásit pomocí jednotného přihlašování. Ověření se přesměruje prostřednictvím IdP.

    **Automatické výběru**
 
    Pokud jednotné přihlašování není aktivní, odešle systému adresu URL normální systém zaměstnance. Pokud je aktivní jednotné přihlašování, systému zkontroluje, zda zaměstnance hesla. Pokud heslo k dispozici, jednotné přihlašování URL a bez přihlašování URL jsou odeslány zaměstnance. Ale pokud zaměstnanec žádné heslo, pouze adresu URL jednotné přihlašování se pošle zaměstnance.

    k. Uložte provedené změny.

9. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

10. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.


![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>Vytvoření uživatele test ByDesign firmy SAP

V této části vytvoříte uživatele s názvem Britta Simon v SAP firmy ByDesign. Zkontrolujte pracujte se na tým podpory SAP firmy ByDesign přidáte uživatele v platformu SAP firmy ByDesign. 

> [AZURE.NOTE] Ujistěte se, že má hodnotu NameID shodovat s pole uživatelské jméno v platformu SAP firmy ByDesign.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělení přístup SAP firmy ByDesign.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon SAP firmy ByDesign, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **SAP firmy ByDesign**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici SAP firmy ByDesign na panelu aplikace Access můžete by měla získat automaticky přihlášení z SAP firmy ByDesign aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
