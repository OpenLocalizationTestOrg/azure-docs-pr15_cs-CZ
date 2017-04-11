<properties 
    pageTitle="Kurz: Azure Active Directory integrace s bambus HR | Microsoft Azure" 
    description="Naučte se používat bambus HR s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Kurz: Azure Active Directory integrace s bambus HR

Cílem tohoto kurzu je integrace Azure a BambooHR zobrazit.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   BambooHR jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti BambooHR (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili BambooHR.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro BambooHR
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scénář")
##<a name="enabling-the-application-integration-for-bamboohr"></a>Povolení integrace aplikací pro BambooHR

Cílem tento oddíl je přehledu jak povolit integraci aplikace BambooHR.

###<a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace BambooHR, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **BambooHR**.

    ![Galerie aplikace] (./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Galerie aplikace")

7.  V podokně výsledků vyberte **BambooHR**a potom klikněte na **Dokončit** přidat aplikaci.

    ![BambooHR] (./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit BambooHR s účtem v Azure AD pomocí federace na základě SAML protokolu.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **BambooHR** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Scénář] (./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scénář")

2.  Na stránce **jakým způsobem uživatelé přihlásit k BambooHR** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit BambooHR na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci BambooHR (například: https://company.bamboohr.com) a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v BambooHR** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti BambooHR jako správce.

6.  Na domovské stránce proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Jednotné přihlašování")

    1.  Klikněte na **aplikace**.
    2.  V nabídce aplikace na levé straně klikněte na **Jednotné přihlašování**.
    3.  Klikněte na **SAML jednotného přihlašování**.

7.  V části **SAML jednotného přihlašování** proveďte následující kroky:

    ![SAML jednotné přihlašování] (./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML jednotné přihlašování")

    1.  Na portálu na **konfigurovat jednotné přihlašování v BambooHR** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Přihlašovací adresa URL jednotné přihlašování** .
    2.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    3.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **X.509 certifikátu**
    4.  Klikněte na **Uložit**.

8.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit BambooHR, musí být zřízení do BambooHR.  
V případě BambooHR zřizování se ručně naplánovaný úkol.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Zřízení uživatelských účtů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu **BambooHR** jako správce.

2.  Na panelu nástrojů na horní klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Nastavení")

3.  Klikněte na **Přehled**.

4.  V levém navigačním podokně, přejděte na **zabezpečení \> uživatelé**.

5.  Zadejte uživatelské jméno, heslo a e-mailovou adresu platného AAD účtu, který chcete poskytování do související textová pole.

6.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli další BambooHR uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou BambooHR k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>Přiřazení uživatelů k BambooHR, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **BambooHR **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
