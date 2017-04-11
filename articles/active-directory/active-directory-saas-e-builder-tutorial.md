<properties 
    pageTitle="Kurz: Azure Active Directory integrace s e Tvůrce | Microsoft Azure" 
    description="Informace o používání e Tvůrce s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Kurz: Azure Active Directory integrace s e Tvůrce
  
Cílem tohoto kurzu je zobrazit integrace Azure a e Tvůrce.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   E Tvůrce klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti e Tvůrce (service provider, které iniciuje přihlášení) nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD přiřazené k e Tvůrce.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro e Tvůrce
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Scénář")
##<a name="enabling-the-application-integration-for-e-builder"></a>Povolení integrace aplikací pro e Tvůrce
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace pro e Tvůrce.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>Postup povolení integrace aplikace pro e tvůrce, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **E Tvůrce**.

    ![Galerie aplikace] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Galerie aplikace")

7.  V podokně výsledků vyberte **E Tvůrce**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Nástroje pro sestavení e] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "Nástroje pro sestavení e")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověření e-nástroje pro sestavení se svým účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **E Tvůrce** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Konfigurace jednotného přihlašování")

2.  Na stránce, **jakým způsobem uživatelé přihlásit k e Tvůrce** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** do textového pole **e Tvůrce přihlaste do pole Adresa URL** zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>.e Builder.com*" a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "Konfigurace aplikace URL")

4.  V **konfigurovat jednotné přihlašování v e Tvůrce** stránky, pokud si chcete stáhnout metadata, klepněte na tlačítko **Stáhnout metadata**, a potom datový soubor, místně jako **c:\\e-BuilderMetaData.xml**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Konfigurace jednotného přihlašování")

5.  Tento soubor metadat pro e Tvůrce vpřed tým podpory. Potřeb týmu podpory nakonfiguruje jednotného přihlašování pro vás.

6.  Vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Neexistuje žádné akce položky konfigurace uživatele vytváření e Tvůrce.  
Při přiřazené uživatel se pokusí přihlásit do e Tvůrce pomocí panelu přístup, e Tvůrce kontroluje, zda uživatel vyskytuje.  
Pokud tam ještě žádný uživatelský účet k dispozici, automaticky se vytvoří pomocí e Tvůrce.
##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Přiřazení uživatelů k e tvůrce, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **E Tvůrce **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
