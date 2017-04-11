<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Thoughtworks Mingle | Microsoft Azure" 
    description="Zjistěte, jak pomocí Thoughtworks Mingle v Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Kurz: Azure Active Directory integrace s Thoughtworks Mingle
  
Cílem tohoto kurzu je zobrazit integrace Azure a Thoughtworks Mingle.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Thoughtworks Mingle klienta
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Thoughtworks Mingle
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Scénář")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>Povolení integrace aplikací pro Thoughtworks Mingle
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace pro Thoughtworks Mingle.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace pro Thoughtworks Mingle, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **thoughtworks mingle**.

    ![Galerie aplikace] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Thoughtworks Mingle**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Thoughtworks Mingle] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Mingle")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Thoughtworks Mingle s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu musíte Thoughtworks Mingle odeslat certifikát.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na **Thoughtworks Mingle **aplikace integrace stránce Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Thoughtworks Mingle** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Thoughtworks Mingle klienta URL** zadejte adresu URL pomocí následujícího vzorce "*http://company.mingle.thoughtworks.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Thoughtworks Mingle** klikněte na stáhnout metadat a uložte ho ve vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Konfigurace jednotného přihlašování")

5.  Přihlaste se k vašemu webu společnosti **Thoughtworks Mingle** jako správce.

6.  Klikněte na kartu **Správce** a potom klikněte na **Jednotné přihlašování Config**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "Konfigurace jednotného přihlašování")

7.  V části **Config jednotné přihlašování** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "Konfigurace jednotného přihlašování")

    1.  Pokud chcete odeslat soubor metadat, klikněte na **Zvolit soubor**.
    2.  Klikněte na **Uložit změny**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pro AAD aby uživatelé mohli přihlásit musí být zřízení do aplikace Thoughtworks Mingle pomocí jejich jména uživatelů Azure Active Directory.  
V případě Thoughtworks Mingle zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti Thoughtworks Mingle jako správce.

2.  Klikněte na **profil**.

    ![První projektu] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "První projektu")

3.  Klikněte na kartu **Správce** a potom klikněte na **uživatele**.

    ![Uživatelé] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Uživatelé")

4.  Klikněte na **Nový uživatel**.

    ![Nového uživatele] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Nového uživatele")

5.  Na stránce dialogové okno **Nový uživatel** proveďte následující kroky:

    ![Nového uživatele] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Nového uživatele")

    1.  Zadejte **přihlašovací jméno**, **zobrazované jméno**, **Zvolte heslo**, **potvrďte heslo** platný AAD účet, který chcete poskytování do související textová pole.
    2.  Jako **Typ uživatele**vyberte **Úplné uživatelské**.
    3.  Klikněte na **vytvořit profil**.

>[AZURE.NOTE] Můžete použít jakékoli další Thoughtworks Mingle uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Thoughtworks Mingle k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Přiřadit uživatelům Thoughtworks Mingle, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na **Thoughtworks Mingle** integrace stránky aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
