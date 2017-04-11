<properties
    pageTitle="Kurz: Azure Active Directory integrace s ServiceNow a ServiceNow Express | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ServiceNow a ServiceNow Express."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>Kurz: Integrace Azure Active Directory s ServiceNow a ServiceNow Express.


V tomto kurzu se naučíte, jak integrovat ServiceNow a ServiceNow Express s Azure Active Directory (Azure AD).

Integrace ServiceNow a ServiceNow Express s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k ServiceNow a ServiceNow Express
- Povolení uživatelům, aby automaticky získat přihlášení z ServiceNow a ServiceNow Express (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí ServiceNow a ServiceNow Express, musíte následující položky:

- Předplatné Azure AD
- Pro ServiceNow, instance nebo klienta ServiceNow, verze Calgary nebo vyšší
- Pro ServiceNow Express instanci ServiceNow Express Helsinkách verze nebo vyšší
- ServiceNow klienta musí mít [Víc poskytovatele jeden znak na modul plug-in](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) povolené. Můžete to provést odeslání žádosti o služby na <https://hi.service-now.com/> 


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí. Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ServiceNow z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení ServiceNow nebo ServiceNow Express


## <a name="adding-servicenow-from-the-gallery"></a>Přidání ServiceNow z Galerie
Pokud chcete nakonfigurovat integraci ServiceNow nebo ServiceNow Express do Azure AD, potřebujete přidat ServiceNow z Galerie do seznamu spravované SaaS aplikace. 

**Pokud chcete přidat ServiceNow z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **ServiceNow**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. V podokně výsledků vyberte **ServiceNow**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části můžete nakonfigurovat a testovací Azure AD jednotné přihlašování s ServiceNow nebo ServiceNow Express založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v ServiceNow je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v ServiceNow jinými slovy, je nutné stanovit.
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v ServiceNow. Ke konfiguraci a otestujte Azure AD jednotné přihlašování s ServiceNow, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotného přihlašování pro ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - uživatelům tuto funkci lze použít.
2. **[Konfigurace Azure AD jednotného přihlašování pro ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - uživatelům tuto funkci lze použít.
3. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření ServiceNow testování uživatele](#creating-a-servicenow-test-user)** – a protějšek Britta Simon ServiceNow, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
6. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

> [AZURE.NOTE] Pokud chcete konfigurovat ServiceNow vynechat kroku 2. Podobně platí Pokud chcete konfigurovat ServiceNow Express vynechat krok 1.

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Konfigurace Azure AD jednotné přihlašování pro ServiceNow

1.  V klasickém portálu Azure AD na stránce **ServiceNow** aplikace integrace klikněte **Konfigurace jednotného přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k ServiceNow** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurovat nastavení aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Konfigurace aplikace URL")

    na. **Znaménko ServiceNow na adresu URL** do textového pole, zadejte adresu URL používané uživatelům přihlašování do aplikace ServiceNow následujícího vzorce: `https://<instance-name>.service-now.com`.

    b. **Identifikátor URI** do textového pole, zadejte svoji adresu URL používá uživatelů k přihlašování do aplikace ServiceNow následujícího vzorce: `https://<instance-name>.service-now.com`.

    c. Klikněte na tlačítko **Další**

4.  Pokud chcete, aby Azure AD automatickou konfiguraci ServiceNow pro ověřování na základě SAML, zadejte název instance ServiceNow, správce uživatelského jména a hesla správce ve formuláři **Automatická konfigurace jednotného přihlašování** a klikněte na *Konfigurovat*. Všimněte si, že podle jména správce může mít **security_admin** role přiřazené v ServiceNow pro tuto pracovat. V opačném ruční konfigurace ServiceNow používání Azure AD jako poskytovatele identit SAML, klikněte na **Ruční konfigurace aplikací pro jednotné přihlašování**, a pak klikněte na tlačítko **Další** a proveďte následující kroky.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Konfigurace aplikace URL")



5.  Na stránce **konfigurovat jednotné přihlašování v ServiceNow** klikněte na **Stáhnout certifikát**uložit soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigurace jednotného přihlašování")

1. Přihlašování do aplikace ServiceNow jako správce.
2. Aktivace _Integrace - více poskytovatele jeden přihlašování instalační program_ modul plug-in pomocí následujících další kroky:

    na. V navigačním podokně na levé straně naleznete v části **Systém definice** a potom klikněte na **moduly plug-in**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Modul plug-in aktivovat")
    
    b. Hledání _Integrace - více poskytovatele jeden přihlašování instalační program_.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Modul plug-in aktivovat")

    c. Vyberte modulu plug-in. Rigth klikněte na a vyberte **Aktivovat/Upgrade**.
    
    d. Klikněte na tlačítko **Aktivovat** .

2. V navigačním podokně na levé straně klikněte na **Vlastnosti**.  

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Konfigurace aplikace URL")


3. V dialogovém okně **Vlastnosti jednotného přihlašování k více zprostředkovatele** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Konfigurace aplikace URL")

    na. Jako **Povolit více poskytovatele jednotné přihlašování**vyberte **Ano**.

    b. Jako **Povolit protokolování ladění máte víc poskytovatele integrace jednotného přihlašování**vyberte **Ano**.

    c. Do **pole na uživatele tabulku, která...** textového pole zadejte **uživatelské_jméno**.

    d. Klikněte na **Uložit**.



1. V navigačním podokně na levé straně klikněte na **x509 certifikáty**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Konfigurace jednotného přihlašování")


1. V dialogovém okně **Certifikáty X.509** klikněte na **Nový**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Konfigurace jednotného přihlašování")


1. V dialogovém okně **Certifikáty X.509** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigurace jednotného přihlašování")

    na. Klikněte na **Nový**.

    b. Do textového pole **název** zadejte název pro konfiguraci vašeho (například: **TestSAML2.0**).

    c. Vyberte **aktivní**.

    d. Jako **Formát**vyberte **PEM**.

    e. Jako **Typ**vyberte **Důvěryhodnosti certifikátu úložiště přihlašovacích údajů**.
    
    f. Otevřete ve formátu Base 64 zakódovaný certifikát v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **PEM certifikát** .

    g. Klikněte na **Aktualizovat**.


1. V navigačním podokně na levé straně klikněte na **Zprostředkovatelé identit jiní**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Konfigurace jednotného přihlašování")

1. V dialogovém okně **Zprostředkovatelé identit jiní** klikněte na **Nový**:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Konfigurace jednotného přihlašování")


1. V dialogovém **Zprostředkovatelé identit jiní** klikněte na **SAML2 Update1?**:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Konfigurace jednotného přihlašování")


1. V dialogovém okně Vlastnosti Update1 SAML2 proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Konfigurace jednotného přihlašování")


    na. do textového pole **název** zadejte název pro konfiguraci vašeho (například: **SAML 2.0**).

    b. Do pole **Uživatelské** textové pole, zadejte **e-mailu** nebo **user_id**, podle toho, které pole se používá pro uživatele v ServiceNow nasazení jednoznačně identifikovat. 
    
    > [AZURE.NOTE] Je možné configue Azure AD pro posílat ID uživatele Azure AD (hlavní uživatelské jméno) nebo e-mailovou adresu jako jedinečný identifikátor v SAML token tak, že přejdete do **ServiceNow > atributy > jednotného přihlašování** část Azure klasické portálem a mapování požadovaného pole atribut **nameidentifier** . Hodnota atributu vybrané uložená v Azure AD (například hlavní uživatelské jméno) musí odpovídat hodnoty uložené ve ServiceNow pro zadané pole (například user_id)

    c. Na portálu klasické Azure AD zkopírujte hodnotu **ID zprostředkovatele identit** a potom je vložte do textového pole **Adresa URL poskytovatele Identity** .

    d. Na portálu klasické Azure AD zkopírujte její hodnotu **Adresa URL požadovat ověření** a potom je vložte do textového pole **zprostředkovatele identit AuthnRequest** .

    e. Na portálu klasické Azure AD zkopírujte hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **zprostředkovatele identit SingleLogoutRequest** .

    f. **Domovská stránka ServiceNow** do textového pole zadejte adresu URL domovskou stránku instance ServiceNow.

    > [AZURE.NOTE] Na domovské stránce instance ServiceNow je tvořen **ServieNow klienta URL** a **/navpage.do** (například:`https://fabrikam.service-now.com/navpage.do`).
 

    g. V **Entity ID / Vystavitel** textové pole, zadejte adresu URL vašeho klienta ServiceNow.

    h. V textovém poli **URL cílovou skupinu** zadejte adresu URL vašeho klienta ServiceNow. 

    Můžu. **Protokol vazba pro IDP SingleLogoutRequest** do textového pole, zadejte **urn: oasis: názvy: tc: SAML:2.0:bindings:HTTP-přesměrovat**.

    j. Zásady NameID do textového pole, zadejte **urn: oasis: názvy: tc: SAML:1.1:nameid-formát: neurčeným**.

    k. Zrušte zaškrtnutí políčka **vytvořit AuthnContextClass**.

    l. **Metoda AuthnContextClassRef**zadejte `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. To je potřeba jenom Pokud jsou pouze organizace cloudu. Pokud používáte místního nasazení služby AD FS nebo MFA pro ověřování neměli konfigurovat tato hodnota. 

    m. Do textového pole **Zkosení hodiny** zadejte **60**.

    n. Jako **Jeden znak na skript**vyberte **MultiSSO_SAML2_Update1**.

    o. Jako **x509 certifikát**, vyberte certifikát, který jste vytvořili v předchozím kroku.

    p. Klikněte na **Odeslat**. 



6. Na portálu klasické Azure AD vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigurace jednotného přihlašování")

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.
 
    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigurace jednotného přihlašování")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Konfigurace Azure AD jednotné přihlašování pro ServiceNow Express

1.  V klasickém portálu Azure AD na stránce **ServiceNow** aplikace integrace klikněte **Konfigurace jednotného přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k ServiceNow** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurovat nastavení aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Konfigurace aplikace URL")

    na. **Znaménko ServiceNow na adresu URL** do textového pole, zadejte adresu URL používané uživatelům přihlašování do aplikace ServiceNow následujícího vzorce: `https://<instance-name>.service-now.com`.

    b. V textovém poli **URL Vystavitel** zadejte adresu URL používá uživatelů k přihlašování do aplikace ServiceNow sledovat vzorek `https://<instance-name>.service-now.com`.

    c. Klikněte na tlačítko **Další**

4.  Klikněte na **Ruční konfigurace aplikací pro jednotné přihlašování**, a pak klikněte na tlačítko **Další** a postupujte podle následujících pokynů.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Konfigurace aplikace URL")

5.  Na stránce **konfigurovat jednotné přihlašování v ServiceNow** klikněte na tlačítko **Stáhnout certifikát**, uložte soubor certifikátu místně na vašem počítači a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigurace jednotného přihlašování")

6. Přihlašování do aplikace ServiceNow Express jako správce.

7. V navigačním podokně na levé straně klikněte na **Jednotné přihlašování**.  

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Konfigurace aplikace URL")


8. V dialogovém okně **Jednotného přihlašování** klikněte na ikonu konfigurace v pravém horním rohu a nastavit následující vlastnosti:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Konfigurace aplikace URL")

    na. Zapíná a vypíná **Povolit více poskytovatele jednotné přihlašování** vpravo.

    b. Zapíná a vypíná **Povolit protokolování pro více integrace jednotného přihlašování poskytovatele ladění** vpravo.

    c. Do **pole na uživatele tabulku, která...** textového pole zadejte **uživatelské_jméno**.


9. V dialogovém **Jednotného přihlašování** klikněte na **Přidat nový certifikát**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Konfigurace jednotného přihlašování")



10. V dialogovém okně **Certifikáty X.509** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigurace jednotného přihlašování")

    na. Do textového pole **název** zadejte název pro konfiguraci vašeho (například: **TestSAML2.0**).

    b. Vyberte **aktivní**.

    c. Jako **Formát**vyberte **PEM**.

    d. Jako **Typ**vyberte **Důvěryhodnosti certifikátu úložiště přihlašovacích údajů**.

    e. Vytvoření souboru ve formátu Base 64 zakódovaný z stažený certifikát.
   
    > [AZURE.NOTE] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).
    
    f. Otevřete ve formátu Base 64 zakódovaný certifikát v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **PEM certifikát** .

    g. Klikněte na **Aktualizovat**.


11. V dialogovém **Jednotného přihlašování** klikněte na **Přidat nové IdP**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Konfigurace jednotného přihlašování")


12. V dialogovém okně **Přidat nový zprostředkovatele identit** v části **Konfigurace zprostředkovatele identit**proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Konfigurace jednotného přihlašování")


    na. Do textového pole **název** zadejte název pro konfiguraci vašeho (například: **SAML 2.0**).

    b. Na portálu klasické Azure AD zkopírujte hodnotu **ID zprostředkovatele identit** a potom je vložte do textového pole **Adresa URL poskytovatele Identity** .

    c. Na portálu klasické Azure AD zkopírujte její hodnotu **Adresa URL požadovat ověření** a potom je vložte do textového pole **zprostředkovatele identit AuthnRequest** .

    d. Na portálu klasické Azure AD zkopírujte hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **zprostředkovatele identit SingleLogoutRequest** .

    e. Jako **Certifikát zprostředkovatele identit**vyberte certifikát, který jste vytvořili v předchozím kroku.


13. Klikněte na **Upřesnit**a v části **Další vlastnosti zprostředkovatele identit**, proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Konfigurace jednotného přihlašování")

    na. **Protokol vazba pro IDP SingleLogoutRequest** do textového pole, zadejte **urn: oasis: názvy: tc: SAML:2.0:bindings:HTTP-přesměrovat**.

    b. **Zásady NameID** do textového pole, zadejte **urn: oasis: názvy: tc: SAML:1.1:nameid-formát: neurčeným**.    

    c. **Metoda AuthnContextClassRef**zadejte **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
    
    d. Zrušte zaškrtnutí políčka **vytvořit AuthnContextClass**.

14. V části **Další vlastnosti poskytovatele služeb**proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Konfigurace jednotného přihlašování")

    na. **Domovská stránka ServiceNow** do textového pole zadejte adresu URL domovskou stránku instance ServiceNow.

    > [AZURE.NOTE] Na domovské stránce instance ServiceNow je tvořen **ServieNow klienta URL** a **/navpage.do** (například: `https://fabrikam.service-now.com/navpage.do`).

    b. V **Entity ID / Vystavitel** textové pole, zadejte adresu URL vašeho klienta ServiceNow.

    c. **Identifikátor URI cílové skupiny** do textového pole zadejte adresu URL vašeho klienta ServiceNow. 

    d. Do textového pole **Zkosení hodiny** zadejte **60**.

    e. Do pole **Uživatelské** textové pole, zadejte **e-mailu** nebo **user_id**, podle toho, které pole se používá pro uživatele v ServiceNow nasazení jednoznačně identifikovat.
    
    > [AZURE.NOTE] Je možné configue Azure AD pro posílat ID uživatele Azure AD (hlavní uživatelské jméno) nebo e-mailovou adresu jako jedinečný identifikátor v SAML token tak, že přejdete do **ServiceNow > atributy > jednotného přihlašování** část Azure klasické portálem a mapování požadovaného pole atribut **nameidentifier** . Hodnota atributu vybrané uložená v Azure AD (například hlavní uživatelské jméno) musí odpovídat hodnoty uložené ve ServiceNow pro zadané pole (například user_id)

    f. Klikněte na **Uložit**. 


15. Na portálu klasické Azure AD vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigurace jednotného přihlašování")

16. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.
 
    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Cílem tento oddíl je přehledu povolení uživatele zřizování adresářové služby Active Directory pro ServiceNow.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1. V Azure klasické portálu pro správu, na stránce integrace **ServiceNow** aplikace klikněte na **Konfigurovat zřizování uživatelů**. 

    ![Zřizování uživatelů] (./media/active-directory-saas-servicenow-tutorial/IC769498.png "Zřizování uživatelů")


2. Na stránce **Zadejte svoje přihlašovací údaje ServiceNow povolit zřizování automatické uživatele** zadejte následující nastavení konfigurace: Konfigurace zřizování uživatelů 

     na. Do textového pole **Název Instance ServiceNow** napište název instance ServiceNow.

     b. Do textového pole **ServiceNow správce uživatelské jméno** zadejte název účtu správce ServiceNow.

     c. Do textového pole **Heslo správce ServiceNow** zadejte heslo k tomuto účtu.

     d. Klikněte na **Ověřit** pro ověření vaší konfigurace.

     e. Kliknutím na tlačítko **Další** a otevře se stránka **Další kroky** .

     f. Pokud chcete zřízení všem uživatelům této aplikace, vyberte "**automaticky zřízení všechny uživatelské účty v adresáři k této aplikaci**". 

    ![Další kroky] (./media/active-directory-saas-servicenow-tutorial/IC698804.png "Další kroky")

     g. Na stránce **Další kroky** klikněte na **Hotovo** uložte konfiguraci.

### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   


### <a name="creating-a-servicenow-test-user"></a>Vytvoření uživatele ServiceNow test

V této části vytvoříte uživatele s názvem Britta Simon v ServiceNow. V této části vytvoříte uživatele s názvem Britta Simon v ServiceNow. Pokud nevíte, jak přidávat uživatele ve vašem účtu ServiceNow nebo ServiceNow Express, obraťte se na tým podpory ServiceNow.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup k ServiceNow.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon ServiceNow, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **ServiceNow**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu všichni uživatelé vyberte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici ServiceNow na panelu aplikace Access můžete by měla získat automaticky přihlášení z ServiceNow aplikaci.

## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
