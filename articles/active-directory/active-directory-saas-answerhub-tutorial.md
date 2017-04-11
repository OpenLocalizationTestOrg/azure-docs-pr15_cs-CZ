<properties 
    pageTitle="Kurz: Azure Active Directory integrace s AnswerHub | Microsoft Azure" 
    description="Naučte se používat AnswerHub s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Kurz: Azure Active Directory integrace s AnswerHub

Cílem tohoto kurzu je zobrazit integrace Azure a [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software).  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   [AnswerHub](http://www.dzonesoftware.com/products/answerhub-question-answer-software) jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, které jste přiřadili AnswerHub uživatelům Azure AD bude moct jednotného přihlášení do aplikace na webu společnosti AnswerHub (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro AnswerHub
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-answerhub-tutorial/IC785165.png "Scénář")

##<a name="enabling-the-application-integration-for-answerhub"></a>Povolení integrace aplikací pro AnswerHub

Cílem tento oddíl je přehledu jak povolit integraci aplikace AnswerHub.

###<a name="to-enable-the-application-integration-for-answerhub-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace AnswerHub, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-answerhub-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-answerhub-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-answerhub-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-answerhub-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **AnswerHub**.

    ![Galerie aplikace] (./media/active-directory-saas-answerhub-tutorial/IC785166.png "Galerie aplikace")

7.  V podokně výsledků vyberte **AnswerHub**a potom klikněte na **Dokončit** přidat aplikaci.

    ![AnswerHub] (./media/active-directory-saas-answerhub-tutorial/IC785167.png "AnswerHub")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit AnswerHub s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tento postup, přečtěte si, [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **AnswerHub** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-answerhub-tutorial/IC785168.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k AnswerHub** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-answerhub-tutorial/IC785169.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **AnswerHub znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*https://company.answerhub.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-answerhub-tutorial/IC785170.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v AnswerHub** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-answerhub-tutorial/IC785171.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti AnswerHub jako správce.
    >[AZURE.NOTE] Pokud potřebujete ke konfiguraci AnswerHub nápovědu, obraťte se na [tým podpory AnswerHub společnosti](mailto:success@answerhub.com. ).








6.  Přejděte na **stránku Správa**.

7.  Klikněte na kartu **uživatelů a skupin** .

8.  V navigačním podokně na levé straně v části **Sociální nastavení** klikněte na příkaz **Vzhled SAML**.

9.  Klikněte na kartu **IDP konfigurace** .

10. Na kartě **IDP konfigurace** proveďte následující kroky:

    ![Nastavení SAML] (./media/active-directory-saas-answerhub-tutorial/IC785172.png "Nastavení SAML")

    1.  Na portálu na **konfigurovat jednotné přihlašování v AnswerHub** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **IDP přihlašovací adresa URL** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v AnswerHub** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **URL IDP odhlásit** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v AnswerHub** dialogové okno stránce Azure klasické zkopírujte hodnoty **Název identifikátor formát** a potom je vložte do textového pole **Formát názvu identifikátor IDP** .
    4.  Klikněte na **klíče a certifikáty**.

11. Na kartě klíče a certifikáty, proveďte následující kroky:

    ![Klíče a certifikáty] (./media/active-directory-saas-answerhub-tutorial/IC785173.png "Klíče a certifikáty")

    1.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    2.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **IDP veřejný klíč (x 509 Format)** .
    3.  Klikněte na **Uložit**.

12. Na kartě **IDP konfigurace** klikněte na **Uložit**.

13. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-answerhub-tutorial/IC785174.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit AnswerHub, musí být zřízení do AnswerHub.  
V případě AnswerHub zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **AnswerHub** jako správce.

2.  Přejděte na **stránku Správa**.

3.  Klikněte na kartu **Uživatelé a skupiny** .

4.  V navigačním podokně na levé straně v části **Spravovat uživatele** klikněte na **Vytvořit nebo importovat uživatele**.

    ![Uživatelé a skupiny] (./media/active-directory-saas-answerhub-tutorial/IC785175.png "Uživatelé a skupiny")

5.  Zadejte **e-mailová adresa**, **uživatelské jméno** a **heslo** platný účet Azure Active Directory, který chcete poskytování do související textová pole a potom klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další AnswerHub uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou AnswerHub k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-answerhub-perform-the-following-steps"></a>Přiřazení uživatelů k AnswerHub, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **AnswerHub **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-answerhub-tutorial/IC785176.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-answerhub-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
