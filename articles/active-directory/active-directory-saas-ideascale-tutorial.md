<properties 
    pageTitle="Kurz: Azure Active Directory integrace s IdeaScale | Microsoft Azure" 
    description="Naučte se používat IdeaScale s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Kurz: Azure Active Directory integrace s IdeaScale
  
Cílem tohoto kurzu je zobrazit integrace Azure a IdeaScale.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   IdeaScale jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili IdeaScale.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro IdeaScale
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-ideascale-tutorial/IC790838.png "Scénář")
##<a name="enabling-the-application-integration-for-ideascale"></a>Povolení integrace aplikací pro IdeaScale
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace IdeaScale.

###<a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace IdeaScale, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-ideascale-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-ideascale-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-ideascale-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-ideascale-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **IdeaScale**.

    ![Galerie aplikace] (./media/active-directory-saas-ideascale-tutorial/IC790841.png "Galerie aplikace")

7.  V podokně výsledků vyberte **IdeaScale**a potom klikněte na **Dokončit** přidat aplikaci.

    ![IdeaScale] (./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit IdeaScale s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro IdeaScale vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **IdeaScale** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-ideascale-tutorial/IC790843.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k IdeaScale** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-ideascale-tutorial/IC790844.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit IdeaScale na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci IdeaScale (například: "*https://company.IdeaScale.com*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-ideascale-tutorial/IC790845.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v IdeaScale** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-ideascale-tutorial/IC790846.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti IdeaScale jako správce.

6.  Přejděte na **Nastavení komunity**.

    ![Nastavení komunity] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Nastavení komunity")

7.  Přejděte na **zabezpečení \> jedním přihlášení nastavení**.

    ![Nastavení jedné přihlášení] (./media/active-directory-saas-ideascale-tutorial/IC790848.png "Nastavení jedné přihlášení")

8.  Jako **Typ jednotné přihlášení**vyberte **SAML 2.0**.

    ![Jeden typ přihlášení] (./media/active-directory-saas-ideascale-tutorial/IC790849.png "Jeden typ přihlášení")

9.  V dialogovém okně **Nastavení jednotného přihlášení** proveďte následující kroky:

    ![Nastavení jedné přihlášení] (./media/active-directory-saas-ideascale-tutorial/IC790850.png "Nastavení jedné přihlášení")

    1.  Na portálu na **konfigurovat jednotné přihlašování v IdeaScale** dialogové okno stránce Azure klasické zkopírujte hodnotu **ID entita** a potom je vložte do textového pole **SAML IdP Entity ID** .
    2.  Kopírování obsahu metadat stažený soubor a potom je vložte do textového pole **SAML IdP Metadata** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v IdeaScale** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **URL úspěch odhlásit** .
    4.  Klikněte na **Uložit změny**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-ideascale-tutorial/IC790851.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit IdeaScale, musí být zřízení do IdeaScale.  
V případě IdeaScale zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **IdeaScale** jako správce.

2.  Přejděte na **Nastavení komunity**.

    ![Nastavení komunity] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Nastavení komunity")

3.  Přejděte na **základní nastavení \> Správa členů**.

4.  Klikněte na **Přidat člena**.

    ![Správa členů] (./media/active-directory-saas-ideascale-tutorial/IC790852.png "Správa členů")

5.  V části přidat nového člena proveďte následující kroky:

    ![Přidání nového člena] (./media/active-directory-saas-ideascale-tutorial/IC790853.png "Přidání nového člena")

    1.  **E-mailových adres** do textového pole zadejte e-mailovou adresu platného AAD účtu, který chcete poskytování.
    2.  Klikněte na **Uložit změny**.

    >[AZURE.NOTE] Majitele účtu služby Azure Active Directory pošle e-mail s odkazem pro potvrzení účtu před z něj stal aktivní.

>[AZURE.NOTE] Můžete použít jakékoli další IdeaScale uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou IdeaScale k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>Přiřazení uživatelů k IdeaScale, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **IdeaScale **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-ideascale-tutorial/IC790854.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-ideascale-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).