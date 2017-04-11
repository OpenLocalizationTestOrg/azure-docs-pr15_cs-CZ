<properties 
    pageTitle="Kurz: Azure Active Directory integrace OfficeSpace softwarem | Microsoft Azure" 
    description="Naučte se používat OfficeSpace Software s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Kurz: Azure Active Directory integrace OfficeSpace softwarem
  
Cílem tohoto kurzu je zobrazit integrace Azure a OfficeSpace softwaru.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   OfficeSpace Software jednotného přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti OfficeSpace Software (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD přiřazené OfficeSpace Software.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací OfficeSpace softwaru
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Scénář")
##<a name="enabling-the-application-integration-for-officespace-software"></a>Povolení integrace aplikací OfficeSpace softwaru
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace OfficeSpace softwaru.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace OfficeSpace softwaru, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **OfficeSpace Software**.

    ![Galerie aplikace] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Galerie aplikace")

7.  V podokně výsledků vyberte **OfficeSpace Software**a potom klikněte na **Dokončit** přidat aplikaci.

    ![OfficeSpace Software] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace Software")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit OfficeSpace softwaru s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro OfficeSpace Software vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **OfficeSpace Software** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlášení = na] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Konfigurace jednotného přihlášení = na")

2.  Na stránce **jakým způsobem uživatelé přihlásit k softwaru OfficeSpace** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **OfficeSpace Software znaménko na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci OfficeSpace Software (například: "*https://company.officespacesoftware.com*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v OfficeSpace softwaru** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se jako správce k webu OfficeSpace softwaru společnosti.

6.  Přejděte na **správce \> spojnic**.

    ![Správce] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Správce")

7.  Klikněte na **Povolení SAML**.

    ![Spojnice] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Spojnice")

8.  V části **SAML se tak mohli ověřovat** proveďte následující kroky:

    ![Konfigurace SAML] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "Konfigurace SAML")

    1.  Na portálu na **konfigurovat jednotné přihlašování v článku OfficeSpace Software** dialog stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa url poskytovatele odhlásit** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v článku OfficeSpace Software** dialog stránce Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **klienta idp cílovou adresu url** .
    3.  Zkopírujte hodnotu **miniaturu** z exportovaný certifikát a potom je vložte do textového pole **otisku certifikátu idp klienta** .  

        >[AZURE.TIP]
        Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    4.  Klikněte na **Uložit nastavení**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit OfficeSpace Software, musí být zřízení do OfficeSpace softwaru. V případě OfficeSpace Software zřizování je automatické úkolu.  
Existuje žádné úkol za vás.  
Automatické vytvoření uživatelů v případě potřeby při prvním jednoho přihlašování pokusu o.

>[AZURE.NOTE]Můžete použít jakékoli další OfficeSpace Software uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou OfficeSpace Software k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>Přiřazení uživatelů k OfficeSpace Software, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce **OfficeSpace Software **aplikace integrace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).