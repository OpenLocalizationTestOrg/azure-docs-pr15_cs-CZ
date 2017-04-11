<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Panopto | Microsoft Azure" 
    description="Naučte se používat Panopto s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-panopto"></a>Kurz: Azure Active Directory integrace s Panopto
  
Cílem tohoto kurzu je zobrazit integrace Azure a Panopto.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Panopto klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Panopto (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Panopto.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Panopto
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-panopto-tutorial/IC777665.png "Scénář")
##<a name="enabling-the-application-integration-for-panopto"></a>Povolení integrace aplikací pro Panopto
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Panopto.

###<a name="to-enable-the-application-integration-for-panopto-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Panopto, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-panopto-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-panopto-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-panopto-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-panopto-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Panopto**.

    ![Galerie Appkication] (./media/active-directory-saas-panopto-tutorial/IC777666.png "Galerie Appkication")

7.  V podokně výsledků vyberte **Panopto**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Panopto] (./media/active-directory-saas-panopto-tutorial/IC782936.png "Panopto")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Panopto s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Panopto** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-panopto-tutorial/IC777667.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Panopto** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-panopto-tutorial/IC777668.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Panopto znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. Panopto.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-panopto-tutorial/IC777528.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Panopto** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-panopto-tutorial/IC777669.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Panopto jako správce.

6.  Na panelu nástrojů na levé straně klikněte na **systém**a potom klikněte na **Zprostředkovatelé identit jiní**.

    ![Systém] (./media/active-directory-saas-panopto-tutorial/IC777670.png "Systém")

7.  Klikněte na **Přidat poskytovatele**.

    ![Zprostředkovatelé identit jiní] (./media/active-directory-saas-panopto-tutorial/IC777671.png "Zprostředkovatelé identit jiní")

8.  V části poskytovatel SAML proveďte následující kroky:

    ![Konfigurace SaaS] (./media/active-directory-saas-panopto-tutorial/IC777672.png "Konfigurace SaaS")

    1.  V seznamu **Typ zprostředkovatele** vyberte **SAML20**
    2.  Do textového pole **Instance název** napište název instance.
    3.  Do textového pole **Popisný název** zadejte popisný název.
    4.  Na portálu na **konfigurovat jednotné přihlašování v Panopto** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vydavatel** .
    5.  Na portálu na **konfigurovat jednotné přihlašování v Panopto** dialogové okno stránce Azure klasické zkopírujte hodnotu **SAML jednotné přihlašování URL** a potom je vložte do textového pole **Odraz adresa Url stránky** .
    6.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    7.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **Veřejný klíč**
    8.  Klikněte na **Uložit**.
        ![Uložení] (./media/active-directory-saas-panopto-tutorial/IC777673.png "Uložení")

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-panopto-tutorial/IC777674.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Neexistuje žádné akce položky konfigurace uživatele zřizování Panopto.  
Při přiřazený uživatel se pokusí přihlásit Panopto pomocí panelu aplikace access, Panopto kontroluje, zda uživatel vyskytuje.  
Pokud tam ještě žádný uživatelský účet k dispozici, vytvoří se automaticky tak, že Panopto.

>[AZURE.NOTE]Můžete použít jakékoli další Panopto uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Panopto zřízení Azure AD uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-panopto-perform-the-following-steps"></a>Přiřazení uživatelů k Panopto, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Panopto **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-panopto-tutorial/IC777675.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-panopto-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).