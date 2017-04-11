<properties 
    pageTitle="Kurz: Azure Active Directory integrace s ArcGIS | Microsoft Azure" 
    description="Naučte se používat ArcGIS s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Kurz: Azure Active Directory integrace s ArcGIS

Cílem tohoto kurzu je integrace Azure a ArcGIS zobrazit. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   ArcGIS jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti ArcGIS (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili ArcGIS.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro ArcGIS
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Scénář")
##<a name="enabling-the-application-integration-for-arcgis"></a>Povolení integrace aplikací pro ArcGIS

Cílem tento oddíl je přehledu jak povolit integraci aplikace ArcGIS.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace ArcGIS, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **ArcGIS**.

    ![Galerie Applcation] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Galerie Applcation")

7.  V podokně výsledků vyberte **ArcGIS**a potom klikněte na **Dokončit** přidat aplikaci.

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit ArcGIS s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **ArcGIS** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k ArcGIS** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **ArcGIS znak v adrese URL** zadejte adresu URL používané uživatelů se přihlásit pomocí následujícího vzorce "*https://company.maps.arcgis.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v ArcGIS** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti ArcGIS jako správce.

6.  Klikněte na **Upravit nastavení**.

    ![Úprava nastavení] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Úprava nastavení")

7.  Klikněte na **zabezpečení**.

    ![Zabezpečení] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Zabezpečení")

8.  V části **Přihlášení Enterprise**klikněte na **Nastavit poskytovatele Identity**.

    ![Pole organizace přihlášení] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Pole organizace přihlášení")

9.  Na stránce konfigurace **Nastavení zprostředkovatele identit** proveďte následující kroky:

    ![Nastavení poskytovatele Identity] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Nastavení poskytovatele Identity")

    1.  Do textového pole Název zadejte název vaší organizace.
    2.  **Že metadata pro zprostředkovatele identit Enterprise poskytnou pomocí**vyberte **Soubor**.
    3.  Nahrajte soubor stažený metadata, klikněte na **Zvolit soubor**.
    4.  Klepněte na tlačítko **nastavit poskytovatele Identity**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit ArcGIS, musí být zřízení do ArcGIS.  
V případě ArcGIS zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu tenantovi **ArcGIS** .

2.  Klikněte na možnost **požádat členy**.

    ![Pozvání členů] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "Pozvání členů")

3.  Vyberte **Přidat členy automaticky bez odeslání e-mailu**a potom na tlačítko **Další**.

    ![Automatické přidání členů] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Automatické přidání členů")

4.  Na stránce dialogové okno **členů** proveďte následující kroky:

    ![Naučte se přidávat a revize] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Naučte se přidávat a revize")

    1.  Zadejte **křestního jména**, **Příjmení** a **e-mailu** platný účet AAD, který chcete k poskytování.
    2.  Klikněte na **Přidat a zobrazit**.

5.  Zkontrolujte data, která jste zadali a potom klikněte na **Přidat členy**.

    ![Přidání člena] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Přidání člena")

>[AZURE.NOTE] Můžete použít jakékoli další ArcGIS uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou ArcGIS k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>Přiřazení uživatelů k ArcGIS, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **ArcGIS **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
