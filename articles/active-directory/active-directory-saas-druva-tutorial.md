<properties 
    pageTitle="Kurz: Azure Active Directory integrace integrace s Druva | Microsoft Azure" 
    description="Naučte se používat Druva s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Kurz: Azure Active Directory integrace integrace s Druva

Cílem tohoto kurzu je zobrazit integrace Azure a Druva.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Druva jednotné přihlašování povolené předplatného

Po dokončení tohoto kurzu, budou moct jednotného přihlášení do aplikace na webu společnosti Druva (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu přístup](active-directory-saas-access-panel-introduction.md)uživatelům Azure AD, které jste přiřadili Druva.

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Druva
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-druva-tutorial/IC795084.png "Scénář")
##<a name="enabling-the-application-integration-for-druva"></a>Povolení integrace aplikací pro Druva

Cílem tento oddíl je přehledu jak povolit integraci aplikace Druva.

###<a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Druva, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-druva-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-druva-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-druva-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-druva-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Druva**.

    ![Galerie aplikace] (./media/active-directory-saas-druva-tutorial/IC795085.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Druva**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Druva] (./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Druva s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

Aplikace Druva očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace **saml tokenu atributy** .  
Následující obrázek ukazuje příklad pro tuto.

![SAML tokenu atributy] (./media/active-directory-saas-druva-tutorial/IC795087.png "SAML tokenu atributy")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Druva** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-druva-tutorial/IC795027.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k Druva** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-druva-tutorial/IC795088.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Druva na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci Druva (například: "*https://cloud.druva.com/home/*") a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-druva-tutorial/IC795089.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v Druva** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-druva-tutorial/IC795090.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se k webu společnosti Druva jako správce.

6.  Přejděte na **Správa \> nastavení**.

    ![Nastavení] (./media/active-directory-saas-druva-tutorial/IC795091.png "Nastavení")

7.  V dialogovém okně Nastavení jednotného přihlašování proveďte následující kroky:

    ![Nastavení jedna jednotné přihlášení] (./media/active-directory-saas-druva-tutorial/IC795092.png "Nastavení jedna jednotné přihlášení")

    1.  Na portálu na **konfigurovat jednotné přihlašování v Druva** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **ID poskytovatele přihlašovací adresa URL** .
    2.  Na portálu na **konfigurovat jednotné přihlašování v Druva** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **ID poskytovatele odhlášení URL** .
    3.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    4.  Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **ID poskytovatele certifikátu**
    5.  Otevřete stránku **Nastavení** , klikněte na **Uložit**.

8.  Na stránce **Nastavení** klikněte na **Generovat tokenu jednotné přihlašování**.

    ![Nastavení] (./media/active-directory-saas-druva-tutorial/IC795093.png "Nastavení")

9.  V dialogovém okně **Jednoho přihlašování ověřování tokenu** proveďte následující kroky:

    ![Jednotné přihlašování tokenu] (./media/active-directory-saas-druva-tutorial/IC795094.png "Jednotné přihlašování tokenu")

    1.  Klikněte na **Kopírovat**.
    2.  Klikněte na tlačítko **Zavřít**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-druva-tutorial/IC795095.png "Konfigurace jednotného přihlašování")

11. V nabídce nahoře klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** .

    ![Atributy] (./media/active-directory-saas-druva-tutorial/IC795096.png "Atributy")

12. Pokud chcete přidat mapování povinný atribut, proveďte následující kroky:

  	|Název atributu|Hodnota atributu|
  	|---|---|
  	|synchronizována\_auth\_tokenu|<*Hodnota schránka*>|

    1.  Pro každý řádek dat v předchozí tabulce klikněte na **Přidat uživatele atribut**.
    2.  Do textového pole **Název atributu** zadejte název atributu zobrazí pro tento řádek.
    3.  **Hodnota atributu** do textového pole zadejte hodnotu atributu zobrazí pro tento řádek.
    4.  Klikněte na **dokončení**.

13. Klikněte na **použít změny**.
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů

Pokud chcete povolit uživatelům Azure AD přihlásit Druva, musí být zřízení do Druva.  
V případě Druva zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Druva** jako správce.

2.  Přejděte na **Správa \> uživatelé**.

    ![Správa uživatelů] (./media/active-directory-saas-druva-tutorial/IC795097.png "Správa uživatelů")

3.  Klikněte na **vytvořit nový**.

    ![Správa uživatelů] (./media/active-directory-saas-druva-tutorial/IC795098.png "Správa uživatelů")

4.  V dialogovém okně Vytvořit nového uživatele proveďte následující kroky:

    ![Vytvoření zpráva] (./media/active-directory-saas-druva-tutorial/IC795099.png "Vytvoření zpráva")

    1.  Zadejte e-mailovou adresu a název platný uživatelský účet Azure Active Directory, který chcete vytvořit do související textová pole.
    2.  Klikněte na **Vytvořit uživatele**.

>[AZURE.NOTE] Můžete použít jakékoli další Druva uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Druva k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-druva-perform-the-following-steps"></a>Přiřazení uživatelů k Druva, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Druva **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-druva-tutorial/IC795100.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-druva-tutorial/IC767830.png "Ano")

Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
