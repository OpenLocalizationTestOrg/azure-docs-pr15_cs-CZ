<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Picturepark | Microsoft Azure" 
    description="Naučte se používat Picturepark s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Kurz: Azure Active Directory integrace s Picturepark
  
Cílem tohoto kurzu je zobrazit integrace Azure a Picturepark.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Picturepark klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Picturepark (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Picturepark.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Picturepark
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scénář")

##<a name="enabling-the-application-integration-for-picturepark"></a>Povolení integrace aplikací pro Picturepark
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Picturepark.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Picturepark, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Picturepark**.

    ![Galerie aplikace] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Picturepark**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Picturepark s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Picturepark vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, přečtěte si, [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Picturepark** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Picturepark** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Sign Picturepark na adresu URL** do textového pole zadejte svoji adresu URL pomocí následujícího vzorce "*http://company.picturepark.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Picturepark** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Picturepark jako správce.

6.  Na panelu nástrojů na horní klikněte na **Nástroje pro správu**a potom klikněte na **Konzola pro správu**.

    ![Konzola pro správu] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Konzola pro správu")

7.  Klikněte na položku **ověření**a potom klikněte na **Zprostředkovatelé identit jiní**.

    ![Ověřování] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Ověřování")

8.  V části **Konfigurace zprostředkovatele identit** proveďte následující kroky:

    ![Konfigurace zprostředkovatele identit] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Konfigurace zprostředkovatele identit")

    1.  Klikněte na **Přidat**.
    2.  Zadejte název pro konfiguraci.
    3.  Vyberte **nastavit jako výchozí**.
    4.  Na portálu na **konfigurovat jednotné přihlašování v Picturepark** dialogové okno stránce Azure klasické zkopírujte hodnotu **SAML jednotné přihlašování URL** a potom je vložte do textového pole **Identifikátor URI vydavatel** .
    5.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Důvěryhodný vydavatel palec tisk** .  

        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    6.  Klikněte na tlačítko **JoinDefaultUsersGroup**.
    7.  Do textového pole **deklaraci identity** nastavit atribut **Emailaddress** , zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
        ![Konfigurace] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Konfigurace")
    8.  Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Picturepark, musí být zřízení do Picturepark.  
V případě Picturepark zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **Picturepark** .

2.  Na panelu nástrojů na horní klikněte na **Nástroje pro správu**a potom klikněte na **uživatele**.

    ![Uživatelé] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Uživatelé")

3.  Na kartě **Přehled uživatelů** klikněte na **Nový**.

    ![Správa uživatelů] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Správa uživatelů")

4.  V dialogovém okně **Vytvořit uživatele** proveďte následující kroky:

    ![Vytvoření uživatele] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Vytvoření uživatele")

    1.  Typ: **e-mailovou adresu**, **heslo**, **potvrďte heslo**, **jméno**, **Příjmení**, **firma**, **země**, **ZIP**a **Město** platného Azure Active Directory uživatele, který má typ objektu poskytování do související textová pole.
    2.  Vyberte požadovaný **jazyk**.
    3.  Klikněte na **vytvořit**.

>[AZURE.NOTE]Můžete použít jakékoli další Picturepark uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Picturepark k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Přiřazení uživatelů k Picturepark, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Picturepark **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).