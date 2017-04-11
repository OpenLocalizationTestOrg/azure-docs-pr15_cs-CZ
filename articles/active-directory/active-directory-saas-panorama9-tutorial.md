<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Panorama9 | Microsoft Azure" 
    description="Naučte se používat Panorama9 s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Kurz: Azure Active Directory integrace s Panorama9
  
Cílem tohoto kurzu je zobrazit integrace Azure a Panorama9.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Panorama9 jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Panorama9 (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Panorama9.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Panorama9
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-panorama9-tutorial/IC790016.png "Scénář")
##<a name="enabling-the-application-integration-for-panorama9"></a>Povolení integrace aplikací pro Panorama9
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Panorama9.

###<a name="to-enable-the-application-integration-for-panorama9-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Panorama9, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-panorama9-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-panorama9-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-panorama9-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-panorama9-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Panorama9**.

    ![Galerie aplikace] (./media/active-directory-saas-panorama9-tutorial/IC790017.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Panorama9**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Panorama9] (./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Panorama9 s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Panorama9 vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Panorama9** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-panorama9-tutorial/IC790019.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Panorama9** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-panorama9-tutorial/IC790020.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Panorama9 na adresu URL** do textového pole, zadejte adresu URL používané uživatelů se přihlásit k Panorama9 (například: "*https://dashboard.panorama9.com/saml/access/3262*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-panorama9-tutorial/IC790021.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Panorama9** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte ji místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-panorama9-tutorial/IC790022.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Panorama9 jako správce.

6.  Na panelu nástrojů na horní klikněte na **Spravovat**a klepněte na položku **rozšíření**.

    ![Rozšíření] (./media/active-directory-saas-panorama9-tutorial/IC790023.png "Rozšíření")

7.  V dialogovém **rozšíření** klikněte na **Jednotné přihlašování**.

    ![Jednotné přihlašování] (./media/active-directory-saas-panorama9-tutorial/IC790024.png "Jednotné přihlašování")

8.  V části **Nastavení** proveďte následující kroky:

    ![Nastavení] (./media/active-directory-saas-panorama9-tutorial/IC790025.png "Nastavení")

    1.  Na portálu na **konfigurovat jednotné přihlašování v Panorama9** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Adresa URL poskytovatele Identity** .
    2.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **otisku certifikát** .  

        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    3.  Klikněte na **Uložit**.

9.  Na portálu klasické Azure AD vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-panorama9-tutorial/IC790026.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Panorama9, musí být zřízení do Panorama9.  
V případě Panorama9 zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Panorama9** jako správce.

2.  V nabídce na horní klikněte na **Spravovat**a klikněte na **uživatele**.

    ![Uživatelé] (./media/active-directory-saas-panorama9-tutorial/IC790027.png "Uživatelé")

3.  Klikněte na **+**.

4.  V oddílu uživatelských dat proveďte následující kroky:

    ![Uživatelé] (./media/active-directory-saas-panorama9-tutorial/IC790028.png "Uživatelé")

    1.  **E-mailu** do textového pole zadejte e-mailovou adresu platné Azure Active Directory uživatele, kterého chcete poskytování.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE]Můžete použít jakékoli další Panorama9 uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Panorama9 k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-panorama9-perform-the-following-steps"></a>Přiřazení uživatelů k Panorama9, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Panorama9** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-panorama9-tutorial/IC790029.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-panorama9-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).