<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Veracode | Microsoft Azure" 
    description="Naučte se používat Veracode s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>Kurz: Azure Active Directory integrace s Veracode
  
Cílem tohoto kurzu je zobrazit integrace Azure a Veracode. Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Veracode jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Veracode.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Veracode
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-veracode-tutorial/IC802903.png "Scénář")

##<a name="enabling-the-application-integration-for-veracode"></a>Povolení integrace aplikací pro Veracode
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Veracode.

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Veracode, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-veracode-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-veracode-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-veracode-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-veracode-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Veracode**.

    ![Galerie aplikace] (./media/active-directory-saas-veracode-tutorial/IC802904.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Veracode**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Veracode] (./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Veracode s účtem v Azure AD pomocí federace podle protokolu SAML.  
Aplikace Veracode očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace **saml tokenu atributy** .  
Následující obrázek ukazuje příklad pro tuto.

![Atributy] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Atributy")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Veracode** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-veracode-tutorial/IC802907.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Veracode** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-veracode-tutorial/IC802908.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurovat nastavení aplikace** klikněte na **Další**.

    ![Konfigurace nastavení aplikace] (./media/active-directory-saas-veracode-tutorial/IC802909.png "Konfigurace nastavení aplikace")

4.  Na stránce **konfigurovat jednotné přihlašování v Veracode** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-veracode-tutorial/IC802910.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Veracode jako správce.

6.  V nabídce na horní klikněte na **Nastavení**a potom klikněte na **Správce**.

    ![Správa] (./media/active-directory-saas-veracode-tutorial/IC802911.png "Správa")

7.  Klikněte na kartu **SAML** .

8.  V části **Nastavení SAML organizaci** proveďte následující kroky:

    ![Správa] (./media/active-directory-saas-veracode-tutorial/IC802912.png "Správa")

    1.  Na portálu na **konfigurovat jednotné přihlašování v Veracode** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vydavatel**
    2.  Pokud chcete odeslat stažený certifikát, klikněte na **Zvolit soubor**.
    3.  Zaškrtněte políčko **Povolit vlastní registrace**.

9.  V části **Nastavení samoobslužné registrace** proveďte následující kroky a potom klikněte na **Uložit**:

    ![Správa] (./media/active-directory-saas-veracode-tutorial/IC802913.png "Správa")

    1.  Jako **Nový aktivaci uživatele**vyberte **Ne aktivace nutná**.
    2.  **Aktualizace dat uživatele**vyberte **Předvoleb Veracode User Data**.
    3.  **Podrobnosti atributu SAML**pak udělejte toto:
        -   **Role uživatelů**
        -   **Správce zásad**
        -   **Revidující**
        -   **Potenciální zákazník zabezpečení**
        -   **Vedoucími**
        -   **Odesílatele**
        -   **Poznámkové bloky pro školy**
        -   **Všechny prohledávat typy**
        -   **Členství týmu**
        -   **Výchozí tým**

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-veracode-tutorial/IC802914.png "Konfigurace jednotného přihlašování")

11. V nabídce nahoře klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Atributy] (./media/active-directory-saas-veracode-tutorial/IC795920.png "Atributy")

12. Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

    ![Atributy] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Atributy")

  	| Název atributu | Hodnota atributu |
  	|:---------------|:----------------|
  	| jméno      | User.givenName  |
  	| Příjmení       | User.Surname    |
  	| e-mailu          | User.Mail       |

    1.  Pro každý řádek dat v předchozí tabulce klikněte na **Přidat uživatele atribut**.
    
    2.  Do textového pole **Název atributu** zadejte název atributu zobrazí pro tento řádek.

    3.  **Hodnota atributu** do textového pole vyberte hodnotu atributu zobrazí pro tento řádek.

    4.  Klikněte na **dokončení**.

13. Klikněte na **použít změny**.

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Veracode, musí být zřízení do Veracode.  
V případě Veracode zřizování je automatizované úkolu.  
Existuje žádné úkol za vás.
  
Automatické vytvoření uživatelů v případě potřeby při prvním jednoho přihlašování pokusu o.

>[AZURE.NOTE] Můžete použít jakékoli další Veracode uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Veracode k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>Přiřazení uživatelů k Veracode, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Veracode **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-veracode-tutorial/IC802915.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-veracode-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).