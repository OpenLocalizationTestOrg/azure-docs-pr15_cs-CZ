<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Learningpool | Microsoft Azure" 
    description="Naučte se používat Learningpool s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Kurz: Azure Active Directory integrace s Learningpool
  
Cílem tohoto kurzu je integrace Azure a Learningpool zobrazit.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Learningpool jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Learningpool (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Learningpool.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Learningpool
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Scénář")
##<a name="enabling-the-application-integration-for-learningpool"></a>Povolení integrace aplikací pro Learningpool
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Learningpool.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Learningpool, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Learningpool**.

    ![Galerie aplikace] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Learningpool**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Learningpool s účtem v Azure AD pomocí federace podle protokolu SAML.
  
Aplikace Learningpool očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace **saml tokenu atributy** .  
Následující obrázek ukazuje příklad pro tuto.

![SAML tokenu atributy] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "SAML tokenu atributy")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce integrace aplikace **Learningpool** v nabídce v horní části Azure klasické klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Atributy] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Atributy")

2.  Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

    ###  

  	|Název atributu                |Hodnota atributu            |
  	|------------------------------|---------------------------|

     název urn: oid:1.2.840.113556.1.4.221 | User.userPrincipalName
  	|-------------------------------|--------------------------|  
     název urn: oid:2.5.4.42|User.givenName   
  	|název urn: oid:0.9.2342.19200300.100.1.3|User.Mail
  	|název urn: oid:2.5.4.4|User.Surname

    1.  Pro každý řádek dat v předchozí tabulce klikněte na **Přidat uživatele atribut**.
    2.  Do textového pole **Název atributu** zadejte název atributu zobrazí pro tento řádek.
    3.  **Hodnota atributu** seznamu vyberte hodnotu atributu zobrazí pro tento řádek.
    4.  Klikněte na **dokončení**.

3.  Klikněte na **použít změny**.

4.  V prohlížeči klikněte na **zpět** a znovu otevřete dialogové okno **Rychlý Start** .

5.  Klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace Singel jednotné přihlášení] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Konfigurace Singel jednotné přihlášení")

6.  Na stránce **jakým způsobem uživatelé přihlásit k Learningpool** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Konfigurace jednotného přihlašování")

7.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Learningpool na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci Learningpool (například: https://parliament.preview.learningpool.com/auth/shibboleth/index.php) a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "Konfigurace aplikace URL")

8.  Na stránce **konfigurovat jednotné přihlašování v Learningpool** stáhnout metadata, klikněte na tlačítko **Stáhnout metadat**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Konfigurace jednotného přihlašování")

9.  Přepošlete tento soubor metadat pro váš tým podpory Learningpool.

    >[AZURE.NOTE]Jednotné přihlašování musí povolit tak, že tým podpory Learningpool.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Learningpool, musí být zřízení do Learningpool.
  
Neexistuje žádné akce položky konfigurace uživatele zřizování Learningpool.  
Uživatelé musí vytvořit tak, že váš tým podpory Learningpool.

>[AZURE.NOTE]Můžete použít jakékoli další Learningpool uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Learningpool k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Přiřazení uživatelů k Learningpool, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Learningpool **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).