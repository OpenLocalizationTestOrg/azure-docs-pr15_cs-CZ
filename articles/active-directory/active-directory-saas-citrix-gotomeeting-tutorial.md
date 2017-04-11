<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Citrix GoToMeeting | Microsoft Azure" 
    description="Naučte se používat Citrix GoToMeeting s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!." 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-citrix-gotomeeting"></a>Kurz: Azure Active Directory integrace s Citrix GoToMeeting  
Platí pro: Azure

>[AZURE.TIP]O názor, klikněte [sem](http://go.microsoft.com/fwlink/?LinkId=522412).

Cílem tohoto kurzu je zobrazit integrace Azure a Citrix GoToMeeting. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Ke klientovi v Citrix GoToMeeting

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Citrix GoToMeeting
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Konfigurace] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768996.png "Konfigurace")



##<a name="enabling-the-application-integration-for-citrix-gotomeeting"></a>Povolení integrace aplikací pro Citrix GoToMeeting

Cílem tento oddíl je přehledu jak povolit integraci aplikace Citrix GoToMeeting.

###<a name="to-enable-the-application-integration-for-citrix-gotomeeting-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace Citrix GoToMeeting, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z Galerie] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC749322.png "Přidat aplikaci z Galerie")

6.  Do **vyhledávacího pole**zadejte **Citrix GoToMeeting**.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701006.png "Citrix GoToMeeting")

7.  V podokně výsledků vyberte **Citrix GoToMeeting**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Citrix GoToMeeting] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC701012.png "Citrix GoToMeeting")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Citrix GoToMeeting s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu jsou potřeba do vašeho tenanta Citrix GoToMeeting nahrát zakódovaný certifikát základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na stránce integrace **Citrix GoToMeeting** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **KONFIGUROVAT jeden znak zapnuto** .

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768997.png "Povolení jednotné přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Citrix GoToMeeting** vyberte **Microsoft Azure AD jednotného přihlašování**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768998.png "Konfigurace jednotného přihlašování")


3. Na stránce **Konfigurovat nastavení aplikace** klikněte na **Další**. 

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689981.png "Povolení jednotné přihlašování")

4.  Na stránce **konfigurovat jednotné přihlašování v Citrix GoToMeeting** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC768999.png "Konfigurace jednotného přihlašování")

5.  V jiném okně prohlížeče Přihlaste se k aplikaci Citrix organizace Center ([https://account.citrixonline.com/organization/administration/](https://account.citrixonline.com/organization/administration/)).

6. Klikněte na kartu **Zprostředkovatele identit** a pak proveďte následující kroky:  

    ![Nastavení SAML] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC6892321.png "Nastavení SAML")

    na. Vyberte **ručně**

    
    b. Na portálu na **konfigurovat jednotné přihlašování v Citrix GoToMeeting** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Přihlašovací adresa URL stránky** a potom je vložte do textového pole **Přihlašovací adresa URL stránky** . 

    
    c. Na portálu na **konfigurovat jednotné přihlašování v Citrix GoToMeeting** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL Sign-Out stránky** a potom je vložte do textového pole **Adresa URL odhlašovací stránky** .

    
    d. Na portálu na **konfigurovat jednotné přihlašování v Citrix GoToMeeting** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID entita** a potom je vložte do textového pole **ID Entity poskytovatele Identity** .

   
    e. Pokud chcete odeslat stažený certifikát, klikněte na **Odeslat certifikát**.

    
    f. Klikněte na **Uložit**.

6.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769000.png "Konfigurace jednotného přihlašování")


7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.

    ![Nastavení SAML] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC7689982.png "Nastavení SAML")





##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Cílem tento oddíl je přehledu povolení zřizování adresářové služby Active Directory pro Citrix GoToMeeting.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Na portálu na stránce **Citrix GoToMeeting** aplikace integrace Azure klasické klikněte na **Konfigurovat zřizování uživatelů** otevřete dialogové okno **Konfigurovat zřizování uživatelů** .

    ![Konfigurovat zřizování uživatelů] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769001.png "Konfigurace zřizování uživatelů")

2.  Na stránce **Nastavení a přihlašovací údaje správce** proveďte následující kroky:

    ![Konfigurace zřizování uživatelů] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769002.png "Konfigurace zřizování uživatelů")

    na. Do textového pole **Citrix GoToMeeting správce uživatelské jméno** zadejte uživatelské jméno správce.

    
    b. **Heslo správce GoToMeeting Citrix** textového pole heslo účtu správce.

    
    c. Klikněte na tlačítko **Další**.

3.  Na stránce **potvrzení** klikněte na políčko Uložit konfiguraci.

4.  Kliknutím na tlačítko **Ověřit** pro ověření vaší konfigurace.


##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-citrix-gotomeeting-perform-the-following-steps"></a>Přiřazení uživatelů k Citrix GoToMeeting, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Citrix GoToMeeting** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769003.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC767830.png "Ano")

Teď by měla až 10 minut a ověřte, že účet synchronizování do Dropboxu pro firmy.

Jako první krok ověření můžete zkontrolovat stav zřizovací kliknutím na řídicí panel v D na stránce **Citrix GoToMeeting** aplikace integrace na portálu Azure klasické.

![Řídicí panel] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769004.png "Řídicí panel")

Uživatel úspěšně dokončené zřizování obrázku je označen související stavu:

![Stav integrace] (./media/active-directory-saas-citrix-gotomeeting-tutorial/IC769005.png "Stav integrace")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access.

Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](https://msdn.microsoft.com/library/dn308586).
