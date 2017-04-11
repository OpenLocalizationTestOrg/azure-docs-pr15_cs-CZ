<properties 
    pageTitle="Kurz: Azure Active Directory integrace s tlačí se k | Microsoft Azure" 
    description="Zjistěte, jak pomocí tlačí se k v Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-huddle"></a>Kurz: Azure Active Directory integrace s tlačí se k
  
Cílem tohoto kurzu je zobrazit integrace Azure a tlačí se k.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Tlačí se k jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti tlačí se k (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili tlačí se k.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro tlačí se k
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Konfigurace jednotného přihlašování] (./media/active-directory-saas-huddle-tutorial/IC787830.png "Konfigurace jednotného přihlašování")
##<a name="enabling-the-application-integration-for-huddle"></a>Povolení integrace aplikací pro tlačí se k
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace pro tlačí se k.

###<a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace pro tlačí se k, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-huddle-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-huddle-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-huddle-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-huddle-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **tlačí se k**.

    ![Galerie aplikace] (./media/active-directory-saas-huddle-tutorial/IC787831.png "Galerie aplikace")

7.  V podokně výsledků vyberte **tlačí se k**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Tlačí se k] (./media/active-directory-saas-huddle-tutorial/IC787832.png "Tlačí se k")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit tlačí se k s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na **tlačí se k** aplikaci integrace stránce Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-huddle-tutorial/IC787833.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jak chcete uživatelům přihlásit k tlačí se k** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-huddle-tutorial/IC787834.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Tlačí se k znaménko na adresu URL** do textového pole zadejte adresu URL vašeho klienta tlačí se k pomocí následujícího vzorce "*http://company.huddle.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-huddle-tutorial/IC787835.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v tlačí se k** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-huddle-tutorial/IC787836.png "Konfigurace jednotného přihlašování")

    1.  Klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikát v počítači.
    2.  Zkopírujte hodnotu **Vystavitel URL** , hodnota **SAML jednotné přihlašování URL** a stažený certifikát a pošlete jim týmu podpory tlačí se k.

    >[AZURE.NOTE] Jednotné přihlašování, musí být povolené tým podpory tlačí se k.
Po dokončení konfigurace, zobrazí se vám oznámení.

5.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-huddle-tutorial/IC787837.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit tlačí se k, musí být zřízení do tlačí se k.  
V případě tlačí se k zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **tlačí se k** jako správce.

2.  Klikněte na **pracovní prostor**.

3.  Klikněte na **lidé \> pozvat**.

    ![Lidé] (./media/active-directory-saas-huddle-tutorial/IC787838.png "Lidé")

4.  V části **vytvořit nové pozvání** proveďte následující kroky:

    ![Nové pozvání] (./media/active-directory-saas-huddle-tutorial/IC787839.png "Nové pozvání")

    1.  V seznamu **Zvolte týmu pozvat** vyberte **týmu**.
    2.  Zadejte **E-mailovou adresu** platného AAD účtu, který chcete poskytování do související textové pole.
    3.  Klikněte na **pozvánku**.

    >[AZURE.NOTE] Majitele účtu Azure AD, dostanou e-mailu a odkaz na před se aktivuje, potvrďte účet.

>[AZURE.NOTE] Můžete použít jakékoli další tlačí se k uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou tlačí se k poskytování AAD uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Přiřadit uživatelům tlačí se k, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na **tlačí se k **aplikaci integrace stránky klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-huddle-tutorial/IC787840.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-huddle-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).