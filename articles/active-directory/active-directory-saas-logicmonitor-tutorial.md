<properties 
    pageTitle="Kurz: Azure Active Directory integrace s LogicMonitor | Microsoft Azure" 
    description="Naučte se používat LogicMonitor s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Kurz: Azure Active Directory integrace s LogicMonitor
  
Cílem tohoto kurzu je zobrazit integrace Azure a LogicMonitor.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   LogicMonitor klienta
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro LogicMonitor
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Scénář")
##<a name="enabling-the-application-integration-for-logicmonitor"></a>Povolení integrace aplikací pro LogicMonitor
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace LogicMonitor.

###<a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace LogicMonitor, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **logicmonitor**.

    ![Galerie aplikace] (./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Galerie aplikace")

7.  V podokně výsledků vyberte **LogicMonitor**a potom klikněte na **Dokončit** přidat aplikaci.

    ![LogicMonitor] (./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit LogicMonitor s účtem v Azure AD pomocí federace podle protokolu SAML.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **LogicMonitor **aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k LogicMonitor** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k LogicMonitor \(e, g,: "*http://company.logicmonitor.com*"\)a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v LogicMonitor** klikněte na tlačítko **Stáhnout metadat**a uložte ho ve vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Konfigurace jednotného přihlašování")

5.  Přihlaste se k vašemu webu společnosti **LogicMonitor** jako správce.

6.  V nabídce na horní klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Nastavení")

7.  V navigaci bat na levé straně klikněte na **Jednotné přihlašování**

    ![Jednotné přihlašování] (./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Jednotné přihlašování")

8.  V části **Nastavení jednotného přihlašování (SSO)** proveďte následující kroky:

    ![Nastavení jednotného přihlašování] (./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Nastavení jednotného přihlašování")

    1.  Zaškrtněte políčko **Povolit jednotného přihlašování**.
    2.  Jako **Výchozí přiřazování rolí**vyberte **jen pro čtení**.
    3.  Otevřete soubor stažený metadat v poznámkovém bloku a potom vložte obsah souboru do textového pole **Metadat zprostředkovatele identit** .
    4.  Klikněte na **Uložit změny**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pro AAD aby uživatelé mohli přihlásit musí být zřízení do aplikace LogicMonitor pomocí jejich jména uživatelů Azure Active Directory.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti LogicMonitor jako správce.

2.  V nabídce v horní klikněte na **Nastavení**a potom klikněte na **rolí a uživatelů**.

    ![Role a uživatelů] (./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Role a uživatelů")

3.  Klikněte na **Přidat**.

4.  V části **Přidat účet** proveďte následující kroky:

    ![Přidat účet] (./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Přidat účet")

    1.  Zadejte **uživatelské jméno**, **e-mailu**, **heslo** a **Zadejte heslo ještě jednou** hodnoty Azure Active Directory uživatele, kterého chcete poskytování do související textová pole.
    2.  Vyberte **role**, **zobrazení oprávnění** a **Stav**.
    3.  Klikněte na **Odeslat**.

>[AZURE.NOTE]Můžete použít jakékoli další LogicMonitor uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou LogicMonitor poskytování služby Azure Active Directory uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>Přiřazení uživatelů k LogicMonitor, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **LogicMonitor** aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).




