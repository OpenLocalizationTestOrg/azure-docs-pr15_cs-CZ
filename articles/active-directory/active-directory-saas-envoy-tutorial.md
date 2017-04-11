<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Envoy | Microsoft Azure" 
    description="Naučte se používat Envoy s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-envoy"></a>Kurz: Azure Active Directory integrace s Envoy
  
Cílem tohoto kurzu je zobrazit integrace Azure a Envoy.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Envoy klienta
  
Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili Envoy bude moct jednotného přihlášení do aplikace na webu společnosti Envoy (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Envoy
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-envoy-tutorial/IC776759.png "Scénář")
##<a name="enabling-the-application-integration-for-envoy"></a>Povolení integrace aplikací pro Envoy
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Envoy.

###<a name="to-enable-the-application-integration-for-envoy-perform-the-following-steps"></a>Postup povolení integrace aplikace pro Envoy, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-envoy-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-envoy-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-envoy-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-envoy-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Envoy**.

    ![Galerie aplikace] (./media/active-directory-saas-envoy-tutorial/IC776760.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Envoy**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776777.png "Envoy")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Envoy s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Envoy vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, přečtěte si, [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Envoy** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Povolení jednotné přihlašování] (./media/active-directory-saas-envoy-tutorial/IC776778.png "Povolení jednotné přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Envoy** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-envoy-tutorial/IC776779.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Envoy znak v adresu URL** do textového pole, zadejte adresu URL pomocí následujícího vzorce "*https://\<název klienta\>. Envoy.com*"a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-envoy-tutorial/IC776780.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Envoy** ke stažení certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\Envoy.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-envoy-tutorial/IC776781.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Envoy jako správce.

6.  Na panelu nástrojů na horní klikněte na **Nastavení**.

    ![Envoy] (./media/active-directory-saas-envoy-tutorial/IC776782.png "Envoy")

7.  Klikněte na položku **Společnost**.

    ![Společnosti] (./media/active-directory-saas-envoy-tutorial/IC776783.png "Společnosti")

8.  Klikněte na tlačítko **SAML**.

    ![SAML] (./media/active-directory-saas-envoy-tutorial/IC776784.png "SAML")

9.  V části konfigurace **Ověřování SAML** proveďte následující kroky:

    ![SAML ověřování] (./media/active-directory-saas-envoy-tutorial/IC776785.png "SAML ověřování")

    >[AZURE.NOTE] Hodnota pro Centrála umístění ID je automaticky generované aplikace.

    1.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **otisku** .  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    2.  Na portálu na **konfigurovat jednotné přihlašování v Envoy** dialogové okno stránce Azure klasické zkopírujte hodnotu **SAML jednotného přihlašování k URL** a potom je vložte do textového pole **Adresa URL SAML HTTP poskytovatele Identity** .
    3.  Klikněte na **Uložit změny**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-envoy-tutorial/IC776786.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Neexistuje žádné akce položky konfigurace uživatele zřizování Envoy.  
Při přiřazené uživatel se pokusí přihlásit Envoy pomocí panelu přístup, Envoy kontroluje, zda uživatel vyskytuje.  
Pokud tam ještě žádný uživatelský účet k dispozici, vytvoří se automaticky tak, že Envoy.
##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-envoy-perform-the-following-steps"></a>Přiřazení uživatelů k Envoy, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Envoy **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-envoy-tutorial/IC776787.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-envoy-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).