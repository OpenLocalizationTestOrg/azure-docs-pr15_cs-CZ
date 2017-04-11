<properties 
    pageTitle="Kurz: Azure Active Directory integrace s izolovaného prostoru služby Salesforce | Microsoft Azure"
    description="Zjistěte, jak pomocí služby Salesforce izolovaného prostoru Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!." 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/28/2016" 
    ms.author="jeedes" />


#<a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Kurz: Azure Active Directory integrace s izolovaného prostoru služby Salesforce
>[AZURE.TIP]O názor, klikněte [sem](http://go.microsoft.com/fwlink/?LinkId=521878).
  
Cílem tohoto kurzu je zobrazit integrace Azure a služby Salesforce izolovaného prostoru.  
Karanténu umožnit můžete vytvořit více kopií vaší organizace, v samostatném prostředí pro jiné účely, například vývoj, testování a školení bez omezení dat a aplikací ve vaší organizaci výrobní služby Salesforce.  
Další informace najdete v tématu [Přehled izolovaného prostoru](https://help.salesforce.com/HTViewHelpDoc?id=create_test_instance.htm&language=en_US)
  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Izolovaného prostoru v Salesforce.com
  
Pokud nemáte platné izolovaného prostoru v Salesforce.com ještě, budete muset kontaktovat služby Salesforce.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro izolovaného prostoru služby Salesforce
2.  Konfigurace jednotného přihlašování
3.  Povolení vaší domény
4.  Konfigurace zřizování uživatelů
5.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769571.png "Scénář")
##<a name="enabling-the-application-integration-for-salesforce-sandbox"></a>Povolení integrace aplikací pro izolovaného prostoru služby Salesforce
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace služby Salesforce izolovaného prostoru.

###<a name="to-enable-the-application-integration-for-salesforce-sandbox-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace služby Salesforce izolovaného prostoru, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700994.png "Aplikace")

4.  Otevřete **Aplikaci Galerie**, klikněte na **Přidat aplikaci**a potom klikněte na **Přidat aplikaci pro svoji organizaci používat**.

    ![Co chcete udělat?] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC700995.png "Co chcete udělat?")

5.  Do **vyhledávacího pole**zadejte **Izolovaného prostoru služby Salesforce**.

    ![Galerie aplikace] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC710978.png "Galerie aplikace")

6.  V podokně výsledků vyberte **Izolovaného prostoru služby Salesforce**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Izolovaného prostoru služby Salesforce] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746474.png "Izolovaného prostoru služby Salesforce")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřování služby Salesforce s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Služby Salesforce izolovaného prostoru** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC749323.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k izolovaného prostoru služby Salesforce** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Izolovaného prostoru služby Salesforce] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746479.png "Izolovaného prostoru služby Salesforce")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit na adresu URL** do textového pole, zadejte adresu URL pomocí následujícího vzorce `http://company.my.salesforce.com`a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781022.png "Konfigurace aplikace URL")

4. Pokud jste už nakonfigurovali jednotného přihlašování pro další instanci služby Salesforce izolovaného prostoru v adresáři, musíte taky nakonfigurovat **identifikátor** mít stejnou hodnotu jako **přihlásit na adresu URL**. Pole **identifikátor** najdete zaškrtnutím políčka **Zobrazit rozšířená nastavení** na stránce **Konfigurace adresa URL aplikace** dialogového okna.

4.  Na stránce **konfigurovat jednotné přihlašování v izolovaném prostoru služby Salesforce** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781023.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se jako správce k izolovaného prostoru služby Salesforce.

6.  V nabídce na horní klepněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781024.png "Nastavení")

7.  V navigačním podokně na levé straně klikněte na **Ovládací prvky zabezpečení**a potom klikněte na **Nastavení jednotného přihlašování**.

    ![Nastavení jednotného přihlašování] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781025.png "Nastavení jednotného přihlašování")

8.  V části Nastavení jednotného přihlašování proveďte následující kroky:

    ![Nastavení jednotného přihlašování] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781026.png "Nastavení jednotného přihlašování")

    na.  Vyberte **SAML povolené**.
    
    b.  Klikněte na **Nový**.

9.  V části SAML přihlašování nastavení jednotného proveďte následující kroky:

    ![SAML přihlašování nastavení jednotného] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781027.png "SAML přihlašování nastavení jednotného")

    na.  Do textového pole Název zadejte název konfigurace (například: *SPSSOWAAD\_Test*).
    
    b.  Na portálu na **konfigurovat jednotné přihlašování v izolovaném prostoru služby Salesforce** dialog stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vystavitel** .
    
    c.  Do textového pole **Entity Id** zadejte **https://test.salesforce.com** , pokud se jedná jeho první instanci služby Salesforce izolovaného prostoru, který přidáváte do adresáře vaší. Pokud už jste přidali instanci služby Salesforce izolovaného prostoru a pak pro typ **Entity ID** **Znaménko na adresu URL**, která by měla být v tomto formátu:`http://company.my.salesforce.com`
    
    d.  Klikněte na tlačítko **Procházet** k nahrání stažený certifikát.
    
    e.  Jako **Typ identit SAML**zaškrtněte políčko **výraz obsahuje Identifikátor federace z objektu uživatele**.
    
    f.  Jako **Místo identit SAML**vyberte položku **Identity v elementu NameIdentifier příkazu předmět**.
    
    g.  Na portálu na **konfigurovat jednotné přihlašování v izolovaném prostoru služby Salesforce** dialog stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL přihlašovací poskytovatele Identity** .
    
    h.  SFDC nepodporuje SAML odhlásit.  Jako alternativu vložte "https://login.windows.net/common/wsfederation?wa=wsignout1.0" do textového pole **Adresa URL odhlášení poskytovatele Identity** .
    
    Můžu.  Jako **Služby, které iniciuje požádat o vazbu zprostředkovatele**vyberte **Publikovat HTTP**.
    
    j. Klikněte na **Uložit**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781028.png "Konfigurace jednotného přihlašování")

##<a name="enabling-your-domain"></a>Povolení vaší domény
  
V této části předpokládá, že jste již vytvořili doménu.  
Další informace najdete v tématu [Definování svůj název domény](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

###<a name="to-enable-your-domain-perform-the-following-steps"></a>Povolení vaší domény, proveďte následující kroky:

1.  V levém navigačním podokně klikněte na **Správa domén**a potom klikněte na **My Domain.**

    ![Mé domény] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781029.png "Mé domény")

    >[AZURE.NOTE]Zkontrolujte, jestli správně nakonfigurované vaší domény.

2.  V části **Přihlašovací stránky nastavení** klikněte na tlačítko **Upravit**jako **Služba ověřování**, potom vyberte název SAML jednoho přihlašování nastavení s předchozím oddílem a nakonec klikněte na **Uložit**.

    ![Mé domény] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC781030.png "Mé domény")
  
Jakmile máte doménu nakonfigurovali, vaši uživatelé používejte adresu URL domény na přihlášení k izolovaného prostoru služby Salesforce.  
Abyste získali hodnotu adresy URL, klikněte na jednotné přihlašování profil, který jste vytvořili v předchozí části.
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Cílem tento oddíl je přehledu povolení uživatele zřizování adresářové služby Active Directory do služby Salesforce izolovaného prostoru.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Na portálu služby Salesforce v horním navigačním panelu vyberte svoje jméno uživatele nabídku rozbalit:

    ![Vlastní nastavení] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698773.png "Vlastní nastavení")

2.  Z nabídky uživatele vyberte **Nastavení osobního** otevřete stránku **Nastavení** .

3.  V levém podokně klikněte na položku **osobní** rozbalte oddíl osobní a klikněte na **Obnovit moje zabezpečení tokenu**:

    ![Vlastní nastavení] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698774.png "Vlastní nastavení")

4.  Na stránce **Obnovit moje zabezpečení tokenu** klikněte na **Obnovit tokenu zabezpečení** požádat o e-mailu, který obsahuje tokenu zabezpečení Salesforce.com.

    ![Nový Token] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC698776.png "Nový Token")

5.  Kontrola složky Doručená pošta e-mailu pro e-mailu z Salesforce.com s "**Salesforce.com zabezpečení potvrzení**" jako předmět.

6.  Zkontrolujte e-mailu a zkopírujte hodnotu tokenu zabezpečení.

7.  Na portálu na stránce **služby salesforce izolovaného prostoru** aplikace integrace Azure klasické klikněte na **Konfigurovat zřizování uživatelů** otevřete dialogové okno **Konfigurovat zřizování uživatelů** .

    ![Konfigurace zřizování uživatelů] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769573.png "Konfigurace zřizování uživatelů")

8.  Na stránce **Zadejte svoje přihlašovací údaje izolovaného prostoru služby Salesforce povolit zřizování automatické uživatele** zadejte následující nastavení:

    ![Izolovaného prostoru služby Salesforce] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC746476.png "Izolovaného prostoru služby Salesforce")

    na.  **Služby Salesforce izolovaného prostoru správce uživatelské jméno** do textového pole zadejte název účtu izolovaného prostoru služby Salesforce, který má profil **Správce systému** v Salesforce.com přiřazené.

    b.  Do textového pole **Heslo správce služby Salesforce izolovaného prostoru** zadejte heslo k tomuto účtu.

    c.  Do textového pole **Tokenu zabezpečení uživatele** vložte hodnotu tokenu zabezpečení.

    d.  Klikněte na **Ověřit** Zkontrolujte konfiguraci.

    e.  Kliknutím na tlačítko **Další** a otevře se stránka **potvrzení** .

9.  Na stránce **potvrzení** klikněte na **Hotovo** uložte konfiguraci.
##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-salesforce-sandbox-perform-the-following-steps"></a>Přiřazení uživatelů do služby Salesforce izolovaného prostoru, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace aplikace **Služby Salesforce izolovaného prostoru **klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC769574.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-salesforce-sandbox-tutorial/IC767830.png "Ano")
  
Teď by měla až 10 minut a ověřte, že účet synchronizování do služby Salesforce karanténě.
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](https://msdn.microsoft.com/library/dn308586).
