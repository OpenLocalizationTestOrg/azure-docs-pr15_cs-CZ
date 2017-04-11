<properties
    pageTitle="Kurz: Azure Active Directory integrace Cezanne HR softwarem | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Cezanne HR Software."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Kurz: Azure Active Directory integrace Cezanne HR softwarem

Cílem tohoto kurzu je vidíte, jak integrovat Cezanne HR Software s Azure Active Directory (Azure AD).

Integrace s Azure AD Cezanne HR Software poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Cezanne HR Software
- Povolení uživatelům, aby automaticky získat přihlášení z Cezanne HR software (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD Cezanne HR softwarem, musíte následující položky:

- Předplatné Azure AD
- Cezanne HR Software jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Cezanne HR Software z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Přidání Cezanne HR Software z Galerie
Pokud chcete nakonfigurovat integraci Cezanne HR Software do Azure AD, potřebujete přidat Cezanne HR Software z Galerie do seznamu spravované SaaS aplikace.

**Přidání Cezanne HR softwaru z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .
    
    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.
    
    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Cezanne HR Software**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. V panelu Výsledky vyberte **Cezanne HR Software**a klikněte na **Dokončit** přidat aplikaci.

    ![Výběr aplikace v galerii](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Cezanne HR softwaru na základě zkušební uživatele s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému Cezanne HR Software pro uživatele v Azure AD. Odkaz relace mezi Azure Active Directory a související uživatelem softwaru HR Cezanne jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Cezanne HR Software.

Testování a konfigurace Azure AD jednotného přihlašování Cezanne HR softwarem, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření softwaru HR Cezanne testování uživatele](#creating-a-cezanne-hr-software-test-user)** – a protějšek Britta Simon Cezanne HR Software, který je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci Cezanne HR Software.

**Abyste mohli nakonfigurovat Azure AD jednotného přihlašování Cezanne HR softwarem, proveďte následující kroky:**

1. Na portálu klasické na stránce **Cezanne HR Software** aplikace integrace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k softwaru HR Cezanne** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL pomocí následujícího vzorce: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. V textovém poli **Odpovědět URL** zadejte adresu URL pomocí následujícího vzorce: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

    c. Klikněte na tlačítko **Další**

    > [AZURE.NOTE] Všimněte si, že budete muset aktualizovat tyto hodnoty skutečné znaménko na adresy URL a adresu URL odpověď. Získat tyto hodnoty, obraťte se na tým podpory Cezanne HR softwaru přes <mailto:info@cezannehr.com>.

4. Na stránce **konfigurovat jednotné přihlašování v článku Cezanne HR Software** proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.

5. V okně různých webového prohlížeče přihlašování ke tenantovi Cezanne HR Software jako správce.

6. V levém navigačním podokně klikněte na **Nastavení systému**. Přejděte na **Nastavení zabezpečení**. Přejděte na **Jednotné přihlašování**.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. V okně **Povolit uživatelům se přihlaste pomocí následující služby jednoho přihlašování (SSO)** panely zaškrtnutí políčka **SAML 2.0** a vyberte možnost **Upřesnit konfiguraci** vedle ní.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Klikněte na tlačítko **Přidat nový** .

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. Proveďte následující kroky v části **Poskytovatelů IDENTIT SAML 2.0** .

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    na. Zadejte název poskytovatele Identity jako **Zobrazované jméno**.

    b. **Identifikátor Entity** textové pole vložit hodnotu **Entity ID** z Azure AD Průvodce konfigurací aplikace.

    c. Změňte **vazbu SAML** na "PŘÍSPĚVKEM".

    d. V **Koncový bod služby tokenu zabezpečení** textové pole vložit hodnotu **Jedné přihlašování adresy URL služby** z Azure AD Průvodce konfigurací aplikace.

    e. "Http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" zadejte do pole **název atributu ID uživatele**.

    f. Klepněte na ikonu **Nahrát** nahrát stažený certifikát z Azure AD.

    g. Klikněte na tlačítko **Ok** . 

10. Klikněte na tlačítko **Uložit** .

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

12. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** do textového pole zadejte **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-cezanne-hr-software-test-user"></a>Vytvoření uživatele test Cezanne HR Software

Pokud chcete povolit uživatelům Azure AD přihlásit Cezanne HR Software, musí být zřízení do Cezanne HR softwaru. V případě Cezanne HR Software zřizování se ručně naplánovaný úkol.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Zřízení uživatelský účet, proveďte následující kroky:

1.  Přihlaste se k webu Cezanne HR softwaru společnosti jako správce.

2.  V levém navigačním podokně klikněte na **Nastavení systému**. Přejděte na **Správa uživatelů**. Přejděte na **Přidat nového uživatele**.

    ![Nového uživatele] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Nového uživatele")

3.  V části **Podrobnosti o osobě** proveďte pod kroky:

    ![Nového uživatele] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Nového uživatele")

    na. Nastavení **interního uživatele** jako vypnuto.

    b. Do textového pole **jméno** zadejte **Britta**.  

    c. **Příjmení** do textového pole zadejte **Simon**.

    d. **E-mailu** do textového pole zadejte e-mailovou adresu účtu Britta Simon.

4.  V části **Informace o účtu** proveďte pod takto:

    ![Nového uživatele] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Nového uživatele")

    na. Do textového pole **uživatelské jméno** zadejte e-mailovou adresu Britta Simon.

    b. Do textového pole **heslo** zadejte heslo účtu Britta Simon.

    c. Vyberte **HR Professional** roli **zabezpečení**.

    d. Klikněte na **OK**.

5. Přejděte na kartu **Jednotného přihlašování** a v oblasti **SAML 2.0 identifikátory** vyberte **Přidat nový** .

    ![Uživatel] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Uživatel")

6. Vyberte svého poskytovatele Identity pro **Zprostředkovatele identit** a do textového pole **Identifikátor uživatele**, zadejte e-mailovou adresu účtu Britta Simon.

    ![Uživatel] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Uživatel")
    
7. Klikněte na tlačítko **Uložit** .

    ![Uživatel] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Uživatel")


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup Cezanne HR Software.
    
![Přiřazení uživatele][200]

**Pokud chcete přiřadit Britta Simon Cezanne HR Software, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
    
    ![Přiřazení uživatele][201]

2. V seznamu aplikací vyberte **Cezanne HR Software**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. V nabídce na horní klikněte na **uživatele**.
    
    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.
    
    ![Přiřazení uživatele][205]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.
 
Po kliknutí na dlaždici Cezanne HR Software na panelu aplikace Access můžete by měla získat automaticky přihlášení na aplikaci Cezanne HR Software.

## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png
