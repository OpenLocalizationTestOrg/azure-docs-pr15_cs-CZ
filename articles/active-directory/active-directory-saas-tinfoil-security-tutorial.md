<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Tinfoil zabezpečení | Microsoft Azure"
    description="Naučte se používat Tinfoil zabezpečení se službou Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!." 
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

#<a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Kurz: Azure Active Directory integrace s Tinfoil zabezpečení
  
Cílem tohoto kurzu je zobrazit integrace Azure a Tinfoil zabezpečení.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Tinfoil zabezpečení jednotného přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili Tinfoil zabezpečení budou moct jednotného přihlášení do aplikace na webu společnosti Tinfoil zabezpečení (identity poskytovatele, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Tinfoil zabezpečení
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Konfigurace jednotného přihlašování] (./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Konfigurace jednotného přihlašování")

##<a name="enabling-the-application-integration-for-tinfoil-security"></a>Povolení integrace aplikací pro Tinfoil zabezpečení
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Tinfoil cenného papíru.

###<a name="to-enable-the-application-integration-for-tinfoil-security-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Tinfoil cenného papíru, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Tinfoil zabezpečení**.

    ![Galerie aplikace] (./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Tinfoil zabezpečení**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Tinfoil zabezpečení] (./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Tinfoil zabezpečení")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Tinfoil zabezpečení se svým účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Tinfoil zabezpečení vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Tinfoil zabezpečení** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k zabezpečení Tinfoil** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Tinfoil zabezpečení odpovědět URL** do textového pole, zadejte adresu URL Tinfoil zabezpečení vyhodnocení výrazu zákaznické služby ACS () (například: "*https://www.tinfoilsecurity.com/saml/consume*" a potom na tlačítko **Další**.

    >[AZURE.NOTE] Je třeba získat adresu URL ACS z Tinfoil zabezpečení Metadata (https://www.tinfoilsecurity.com/saml/metadata).

    ![Konfigurace aplikace URL] (./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Tinfoil zabezpečení** je možné stáhnout certifikát klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně jako **c:\\Tinfoil Security.cer**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se jako správce k webu společnosti Tinfoil zabezpečení.

6.  Na panelu nástrojů na horní klikněte na **Můj účet**.

    ![Řídicí panel] (./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Řídicí panel")

7.  Klikněte na **zabezpečení**.

    ![Zabezpečení] (./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Zabezpečení")

8.  Na **Jednotné přihlašování** stránce konfigurace proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Jednotné přihlašování")

    1.  Zaškrtněte políčko **Povolit SAML**.
    2.  Klikněte na **Ruční konfigurace**.
    3.  Na portálu na **konfigurovat jednotné přihlašování v zabezpečení Tinfoil** dialogové okno stránce Azure klasické zkopírujte hodnotu **SAML jednotného přihlašování k URL** a potom je vložte do textového pole **SAML Post URL** .
    4.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Otisku SAML certifikát** .  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    5.  Zkopírujte **ID účtu**.
    6.  Klikněte na **Uložit**.

9.  Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Konfigurace jednotného přihlašování")

10. V nabídce nahoře klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Atributy] (./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Atributy")

11. Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

    ![Atributy] (./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Atributy")

    1.  Klikněte na **Přidat uživatele atribut**.
    2.  Do textového pole **Název atributu** zadejte **accountid**.
    3.  **Hodnota atributu** do textového pole vložte hodnotu ID účtu, který jste zkopírovali v předchozí části.
    4.  Klikněte na **dokončení**.

12. Klikněte na **použít změny**.

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Tinfoil zabezpečení, musí být zřízení do Tinfoil zabezpečení.  
V případě Tinfoil zabezpečení zřizování se ručně naplánovaný úkol.

###<a name="to-get-a-user-provisioned-perform-the-following-steps"></a>Jak uživatel zřízení proveďte následující kroky:

1.  Pokud je uživatel součástí účet organizace, budete muset kontaktovat tým podpory Tinfoil zabezpečení získat uživatelský účet vytvořený.

2.  Pokud je uživatel běžný uživatel Tinfoil zabezpečení SaaS, potom uživatele můžete přidat collaborator na všech webech uživatele. To spustí proces, jak poslání pozvánky ke určeného e-mailového vytvořit nový uživatelský účet Tinfoil zabezpečení.

>[AZURE.NOTE] Můžete použít jakékoli další Tinfoil uživatelského účtu vytváření nástroje zabezpečení nebo rozhraní API poskytovanou Tinfoil zabezpečení k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-tinfoil-security-perform-the-following-steps"></a>Přiřazení uživatelů k Tinfoil zabezpečení, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace aplikace **Tinfoil zabezpečení **klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).