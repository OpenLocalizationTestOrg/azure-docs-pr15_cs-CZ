<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Lupa | Microsoft Azure" 
    description="Naučte se používat Lupa s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!." 
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

#<a name="tutorial-azure-active-directory-integration-with-zoom"></a>Kurz: Azure Active Directory integrace s Lupa
  
Cílem tohoto kurzu je zobrazit integrace Azure a Lupa.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Zvětšení klienta
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti lupy (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md) uživatelům Azure AD přiřazené Lupa
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro zvětšení
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-zoom-tutorial/IC784693.png "Scénář")

##<a name="enabling-the-application-integration-for-zoom"></a>Povolení integrace aplikací pro zvětšení
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace pro zvětšení.

###<a name="to-enable-the-application-integration-for-zoom-perform-the-following-steps"></a>Postup povolení integrace aplikace pro zvětšení, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-zoom-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-zoom-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-zoom-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-zoom-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Lupa**.

    ![Galerie aplikace] (./media/active-directory-saas-zoom-tutorial/IC784694.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Lupa**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Zvětšení] (./media/active-directory-saas-zoom-tutorial/IC784695.png "Zvětšení")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit zvětšit s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na **Lupa** aplikace integrace stránce Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zoom-tutorial/IC784696.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jak chcete uživatelům přihlásit k Lupa** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zoom-tutorial/IC784697.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Lupa znak v adresa URL** zadejte adresu URL pomocí následujícího vzorce "*http://company.zoom.us*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-zoom-tutorial/IC784698.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Lupa** na tlačítko **Stáhnout certifikát**a uložte soubor certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zoom-tutorial/IC784699.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se jako správce k webu společnosti Lupa.

6.  Klikněte na kartu **Jednotného přihlašování** .

    ![Jednotné přihlašování] (./media/active-directory-saas-zoom-tutorial/IC784700.png "Jednotné přihlašování")

7.  Klikněte na kartu **Správa zabezpečení** a potom přejděte k části Nastavení **Jednotného přihlašování** .

8.  V části jednotného přihlašování proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-zoom-tutorial/IC784701.png "Jednotné přihlašování")

    1.  Na portálu na **konfigurovat jednotné přihlašování v Lupa** dialogové okno stránce Azure klasické zkopírujte hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Přihlašovací adresa URL stránky** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v Lupa** dialogové okno stránce Azure klasické zkopírujte hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **Adresa URL odhlašovací stránky** .
    3.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **certifikát poskytovatele Identity**
    5.  Na portálu na **konfigurovat jednotné přihlašování v Lupa** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vystavitel** .
    6.  Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-zoom-tutorial/IC784702.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Lupa, musí být zřízení do Lupa.  
V případě Lupa zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Lupa** jako správce.

2.  Klikněte na kartu **Správa účtu** a potom klikněte na **Správa uživatelů**.

3.  V části Správa uživatelů klikněte na **Přidat uživatele**.

    ![Správa uživatelů] (./media/active-directory-saas-zoom-tutorial/IC784703.png "Správa uživatelů")

4.  Na stránce **Přidat uživatele** proveďte následující kroky:

    ![Přidání uživatelů] (./media/active-directory-saas-zoom-tutorial/IC784704.png "Přidání uživatelů")

    1.  Jako **Typ uživatele**vyberte **základní**.
    2.  **E-mailů** do textového pole zadejte e-mailovou adresu platného AAD účtu, který chcete poskytování.
    3.  Klikněte na **Přidat**.

>[AZURE.NOTE] Můžete použít jakékoli další Lupa uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Lupa zřízení AAD uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-zoom-perform-the-following-steps"></a>Přiřazení uživatelů k Lupa, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na **Lupa **integrace stránky aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-zoom-tutorial/IC784705.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-zoom-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).