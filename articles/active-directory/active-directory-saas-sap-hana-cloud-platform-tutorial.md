<properties 
    pageTitle="Kurz: Azure Active Directory integrace s SAP HANA cloudu platformy | Microsoft Azure" 
    description="Naučte se používat SAP HANA cloudu platformy s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Kurz: Azure Active Directory integrace s SAP HANA cloudu platformy
  
Cílem tohoto kurzu je zobrazit integrace Azure a SAP HANA cloudu platformy.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   SAP HANA cloudu platformy účtu
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD přiřazené k modelům SAP HANA cloudu platformu.

>[AZURE.IMPORTANT]Budete muset nasadit vlastní aplikaci nebo přihlášení k odběru aplikace ve vašem účtu SAP HANA cloudu platformu otestovat jednotného přihlášení na. V tomto kurzu aplikace nasazený v okně účet.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro platformu cloudu HANA SAP
2.  Konfigurace jednotného přihlašování
3.  Přiřazení role pro uživatele
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scénář")
##<a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Povolení integrace aplikací pro platformu cloudu HANA SAP
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace pro platformu cloudu HANA SAP.

###<a name="to-enable-the-application-integration-for-sap-hana-cloud-platform-perform-the-following-steps"></a>Postup povolení integrace aplikace SAP HANA cloudu platformy, proveďte následující kroky:

1.  Na portálu Správa Azure v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **SAP HANA cloudu platformu**.

    ![Galerie aplikace] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Galerie aplikace")

7.  V podokně výsledků vyberte **SAP HANA cloudu platformy**a potom klikněte na **Dokončit** přidat aplikaci.

    ![SAP Hana] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit SAP HANA cloudu platformu s účtem v Azure AD pomocí federace na základě SAML protokolu.  
V rámci tohoto postupu je třeba odeslat certifikát zakódovaný základu-64 ke tenantovi SAP HANA cloudu platformu.  
Pokud znáte není tento postup, přečtěte si, [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce integrace aplikace **SAP HANA cloudu platformu** Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Konfigurace jednotného přihlašování")

2.  Na stránce, **jakým způsobem uživatelé přihlásit k SAP HANA cloudu platformy** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Konfigurace jednotného přihlašování")

3.  V okně různých webového prohlížeče Přihlaste se k modelům SAP HANA cloudu platformy řídicí panel na https://account. \<orientace na šířku hostitele\>.ondemand.com/cockpit (například: *https://account.hanatrial.ondemand.com/cockpit*).

4.  Klikněte na kartu **zabezpečení** .

    ![Zabezpečení] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Zabezpečení")

5.  V části Správa zabezpečení proveďte následující kroky:

    ![Získat Metadata] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Získat Metadata")

    1.  Klikněte na kartu **Místní poskytovatele služeb** .
    2.  SAP HANA cloudu platformy metadat budou moct soubor stáhnout, klikněte na **Získat Metadata**.

6.  Na portálu na stránce **Konfigurace adresa URL aplikace** Azure Active klasické proveďte následující kroky a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Konfigurace aplikace URL")

    1.  **Přihlaste se na adresu URL** do textového pole zadejte adresu URL používané uživatelů a přihlaste se do aplikace **SAP HANA cloudu platformu** . Toto je adresa URL specifické pro účet chráněného zdroj v aplikaci SAP HANA cloudu platformu. Adresa URL se podle následujícího vzorce: *https://\<applicationName\>\<název účtu\>.\< Host (hostitel) na šířku\>.ondemand.com/\<cestu\_k\_chráněné\_zdroje\> * (například: *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*)

        >[AZURE.NOTE]Toto je adresa URL aplikace SAP HANA cloudu platformy, která vyžaduje, aby uživatel k ověření.

    2.  Stažený soubor metadat SAP HANA cloudu platformy otevřít a vyhledejte **ns3:AssertionConsumerService** značku.
    3.  Zkopírujte hodnotu atributu **umístění** a potom je vložte do textového pole **Adresa URL SAP HANA cloudu platformy odpověď** .

7.  Na stránce **konfigurovat jednotné přihlašování v SAP HANA cloudu platformy** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte jej ve vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Konfigurace jednotného přihlašování")

8.  V části **Místní poskytovatele** na SAP HANA cloudu platformy řídicím panelu, proveďte následující kroky:

    ![Správa zabezpečení] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Správa zabezpečení")

    1.  Klikněte na **Upravit**.
    2.  Jako **Typ konfigurace**vyberte položku **vlastní**.
    3.  Jako **Název místní poskytovatelem**ponechte výchozí hodnotu.
    4.  K vytvoření **Podepisování klíče** a pár klíče **Podpisový certifikát** , klikněte na **Generovat pár klíče**.
    5.  Jako **Hlavní šíření**vyberte **Zakázáno**.
    6.  Jako **Platnost ověřování**vyberte **Zakázáno**.
    7.  Klikněte na **Uložit**.

9.  Klikněte na kartě **Důvěryhodní zprostředkovatele identit** a potom klikněte na **Přidat zprostředkovatele identit důvěryhodné**.

    ![Správa zabezpečení] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Správa zabezpečení")

    >[AZURE.NOTE]Správa seznamu důvěryhodní poskytovatelé identity, budete muset zvolili typ konfigurace vlastní v části místní poskytovatele služeb. Výchozí konfigurace typu máte neupravitelný a implicitní zabezpečení ve službě ID SAP. Pro žádný nemáte žádné nastavení zabezpečení.

10. Klikněte na kartu **Obecné** a potom klikněte na **Procházet** k nahrání souboru stažený metadat.

    ![Správa zabezpečení] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Správa zabezpečení")

    >[AZURE.NOTE] Po odeslání souboru metadat, jsou hodnoty **URL přihlašování**, **Jedné adresy URL odhlásit** a **Podpisový certifikát** vyplněné automaticky.

11. Klikněte na kartu **atributy** .

12. Na kartě **atributy** proveďte následující kroky:

    ![Atributy] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Atributy")

    1.  Po kliknutí na **Add Assertion-Based atribut**, přidejte následující atributy na základě výraz:

        |Atribut výrazu| Hlavní atributu|
        |-------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/givenName|   jméno|--------------------|--------------------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Surname|        Příjmení|-----------|
        |http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/EmailAddress|e-mailu|

    >[AZURE.NOTE]Konfigurace atributy závisí na způsobu aplikací na HCP vytvářejí, tedy které atributy očekávají v odpovědi SAML a v části název (jistinu atribut) přistupují Tenhle atribut v kódu.
    >  
    >na.  **Výchozí atribut** v obrazovky je určené pro účely obrázku. Je není nutné scénáře práce.  
    >
    >b.  Jména a hodnoty pro **Atribut jistinu** vidět snímek, závisí na způsobu vyvinuté aplikace. Je možné, že aplikace požaduje jiný mapování.

13. Na portálu na **konfigurovat jednotné přihlašování v SAP HANA cloudu platformy** dialog stránce Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Konfigurace jednotného přihlašování")
  
Volitelný krok může konfigurovat na základě výraz skupiny poskytovatele Identity Azure Active Directory

>[AZURE.NOTE]Používání skupiny na SAP HANA cloudu platformu umožňuje dynamicky přiřadit jeden nebo víc uživatelů do jedné nebo více rolí v aplikacích SAP HANA cloudu platformy určený podle hodnoty atributů ve výrazu SAML 2.0. Řekněme, že výraz obsahuje atribut "*smlouvy = dočasné*", je vhodné všechny ovlivněné uživatele, které budou přidány do skupiny "*dočasné*". Skupina "*dočasné*" může obsahovat jednu nebo více rolí z jedné nebo více nasazené účtu SAP HANA cloudu platformu.
>  
>Na základě výraz skupin použijte, pokud chcete přiřadit hmotnost většího počtu uživatelů k rolím jeden nebo více aplikací ve vašem účtu SAP HANA cloudu platformu. Pokud chcete přiřadit jeden nebo malé několik uživatelů (a) určité role doporučujeme přiřazení přímo na kartách "**Povolení**" řídicím panelu SAP HANA cloudu platformu.

##<a name="assigning-a-role-to-a-user"></a>Přiřazení role pro uživatele
  
Pokud chcete povolit uživatelům Azure AD přihlásit SAP HANA cloudu platformu, musíte přiřadit role v platformu SAP HANA cloudu je.

###<a name="to-assign-a-role-to-a-user-perform-the-following-steps"></a>Pokud chcete přiřadit roli uživatele, proveďte následující kroky:

1.  Přihlaste se k řídicí panel **SAP HANA cloudu platformu** .

2.  Proveďte následující kroky:

    ![Povolení] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Povolení")

    1.  Klikněte na tlačítko **se tak mohli ověřovat**.
    2.  Klikněte na kartu **uživatelů** .
    3.  **Uživatele** do textového pole zadejte e-mailovou adresu uživatele.
    4.  Klikněte na tlačítko uživateli přiřadit roli **přiřadit** .
    5.  Klikněte na **Uložit**.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-sap-hana-cloud-platform-perform-the-following-steps"></a>Přiřazení uživatelů k SAP HANA cloudu platformy, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace aplikace **SAP HANA cloudu platformy** klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).