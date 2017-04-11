<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Onit | Microsoft Azure" 
    description="Naučte se používat Onit s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-onit"></a>Kurz: Azure Active Directory integrace s Onit
  
Cílem tohoto kurzu je zobrazit integrace Azure a Onit.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Onit jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Onit (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Onit.
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Onit
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-onit-tutorial/IC791166.png "Scénář")
##<a name="enabling-the-application-integration-for-onit"></a>Povolení integrace aplikací pro Onit
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Onit.

###<a name="to-enable-the-application-integration-for-onit-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Onit, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-onit-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-onit-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-onit-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-onit-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Onit**.

    ![Galerie aplikace] (./media/active-directory-saas-onit-tutorial/IC791167.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Onit**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Onit] (./media/active-directory-saas-onit-tutorial/IC795325.png "Onit")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Onit s svůj účet v Azure AD pomocí federace na základě SAML protokolu.  
Konfigurace jednotného přihlašování pro Onit vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).
  
Aplikace Onit očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace **saml tokenu atributy** .  
Následující obrázek ukazuje příklad pro tuto.

![Jednotné přihlašování] (./media/active-directory-saas-onit-tutorial/IC791168.png "Jednotné přihlašování")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce integrace aplikace **Onit** v nabídce v horní části Azure klasické klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Atributy] (./media/active-directory-saas-onit-tutorial/IC791169.png "Atributy")

2.  Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

    
  	|Název atributu|Hodnota atributu|
  	|---|---|
  	|Jméno|User.userPrincipalName|
  	|e-mailu|User.Mail|

    1.  Pro každý řádek dat v předchozí tabulce klikněte na **Přidat uživatele atribut**.
    2.  Do textového pole **Název atributu** zadejte název atributu zobrazí pro tento řádek.
    3.  **Hodnota atributu** seznamu vyberte hodnotu atributu zobrazí pro tento řádek.
    4.  Klikněte na **dokončení**.

3.  Klikněte na **použít změny**.

4.  V prohlížeči klikněte na **zpět** a znovu otevřete dialogové okno **Rychlý Start** .

5.  Klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-onit-tutorial/IC791170.png "Konfigurace jednotného přihlašování")

6.  Na stránce **jakým způsobem uživatelé přihlásit k Onit** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-onit-tutorial/IC791171.png "Konfigurace jednotného přihlašování")

7.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Onit na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci Onit (například: "*https://ms-sso-test.onit.com*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-onit-tutorial/IC791172.png "Konfigurace aplikace URL")

8.  Na stránce **konfigurovat jednotné přihlašování v Onit** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-onit-tutorial/IC791173.png "Konfigurace jednotného přihlašování")

9.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Onit jako správce.

10. V nabídce nahoře klikněte na **Správa**.

    ![Správa] (./media/active-directory-saas-onit-tutorial/IC791174.png "Správa")

11. Klikněte na **Upravit Corporation**.

    ![Upravit Corporation] (./media/active-directory-saas-onit-tutorial/IC791175.png "Upravit Corporation")

12. Klikněte na kartu **zabezpečení** .

    ![Úprava údajů o společnosti] (./media/active-directory-saas-onit-tutorial/IC791176.png "Úprava údajů o společnosti")

13. Na kartě **zabezpečení** proveďte následující kroky:

    ![Jednotné přihlašování] (./media/active-directory-saas-onit-tutorial/IC791177.png "Jednotné přihlašování")

    1.  Jako **Strategii ověřování**vyberte **jednotného přihlašování a heslo**.
    2.  Na portálu na **konfigurovat jednotné přihlašování v Onit** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Idp cílovou adresu URL** .
    3.  Na portálu na **konfigurovat jednotné přihlašování v Onit** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **URL Idp odhlásit** .
    4.  Zkopírujte hodnotu **Miniatura** z exportovaný certifikát a potom je vložte do textového pole **Idp certifikátu otisku (SHA1)** .  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI)

    5.  Jako **Typ jednotné přihlašování**vyberte **SAML**.
    6.  **Jednotné přihlašování přihlášení tlačítko text** do textového pole zadejte text tlačítka, kterou chcete.
    7.  Vyberte **přihlášení s jednotným Přihlašováním: povinné pro následující domény a uživatelů**do související textového pole zadejte e-mailovou adresu testovací uživatelské jméno a klikněte na tlačítko **Aktualizovat**.
        ![Upravit Corporation] (./media/active-directory-saas-onit-tutorial/IC791178.png "Upravit Corporation")

14. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-onit-tutorial/IC791179.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Onit, musí být zřízení do Onit.  
V případě Onit zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Onit** jako správce.

2.  Klikněte na **Přidat uživatele**.

    ![Správa] (./media/active-directory-saas-onit-tutorial/IC791180.png "Správa")

3.  Na stránce dialogové okno **Přidat uživatele** proveďte následující kroky:

    ![Přidání uživatele] (./media/active-directory-saas-onit-tutorial/IC791181.png "Přidání uživatele")

    1.  Zadejte **jméno** a **E-mailovou adresu** platného AAD účtu, který chcete vytvořit do související textová pole.
    2.  Klikněte na **vytvořit**.  

        >[AZURE.NOTE] Vlastník účtu pošle e-mail a odkaz na před se aktivuje, potvrďte účet.

>[AZURE.NOTE] Můžete použít jakékoli další Onit uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Onit k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-onit-perform-the-following-steps"></a>Přiřazení uživatelů k Onit, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Onit **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-onit-tutorial/IC791182.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-onit-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).