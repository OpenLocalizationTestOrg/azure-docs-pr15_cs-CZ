<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Work.com | Microsoft Azure" 
    description="Naučte se používat Work.com s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!." 
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

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Kurz: Azure Active Directory integrace s Work.com
  
Cílem tohoto kurzu je zobrazit integrace Azure a Work.com.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Work.com jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, AAD uživatelů, kterým máte přiřadit Work.com přístup bude moct jeden znak do aplikace na webu společnosti Work.com (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Work.com
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Scénář")

##<a name="enabling-the-application-integration-for-workcom"></a>Povolení integrace aplikací pro Work.com
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Work.com.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Work.com, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-work-com-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Work.com**.

    ![Galerie aplikace] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Work.com**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Work.com s účtem v Azure AD pomocí federace podle protokolu SAML.  
V rámci tohoto postupu musíte do Work.com.com nahrát certifikát.

>[AZURE.NOTE] Konfigurace jednotného přihlašování, budete muset nastavit vlastní název domény Work.com ještě. Potřebujete definovat aspoň název domény, otestujte váš název domény a nasazení pro celou organizaci.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Přihlaste se ke klientovi Work.com jako správce.

2.  Přejděte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Nastavení")

3.  V levém navigačním podokně v části **Spravovat** klikněte na **Domain Management** rozbalte části související a klikněte na **My Domain** otevřete stránku **My Domain** . 

    ![Mé domény] (./media/active-directory-saas-work-com-tutorial/IC767825.png "Mé domény")

4.  K ověření, že svoji doménu správně byl nastavení, ujistěte se, že je v "**Krok 4 používaný uživatele**" a zkontrolujte "**Nastavení domény**".

    ![Doména používaný uživatele] (./media/active-directory-saas-work-com-tutorial/IC784377.png "Doména nasazené na uživatele")

5.  V okně různých webového prohlížeče Přihlaste se k portálu Azure klasické.

6.  Na stránce integrace **Work.com **aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Konfigurace jednotného přihlašování")

7.  Na stránce **jakým způsobem uživatelé přihlásit k Work.com** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Konfigurace jednotného přihlašování")

8.  Na stránce **Konfigurace adresa URL aplikace** **Odhlásit Work.com na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci Work.com (například: " *http://company.my.salesforce.com*") a potom klikněte na **Další**: 

    ![Konfigurace aplikace URL] (./media/active-directory-saas-work-com-tutorial/IC794111.png "Konfigurace aplikace URL")

9.  Na stránce **konfigurovat jednotné přihlašování v Work.com** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Konfigurace jednotného přihlašování")

10. Přihlaste se k vašemu tenantovi Work.com.

11. Přejděte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Nastavení")

12. Rozbalte nabídku **Ovládací prvky zabezpečení** a potom klikněte na **Nastavení jednotného přihlašování**.

    ![Nastavení jednotného přihlašování] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Nastavení jednotného přihlašování")

13. Na stránce dialogové okno **Nastavení jednotného přihlašování** proveďte následující kroky:

    ![SAML povolena] (./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML povolena")

    1.  Vyberte **SAML povolené**.
    2.  Klikněte na **Nový**.

14. V části **SAML přihlašování nastavení jednotného** proveďte následující kroky:

    ![SAML jednoho přihlašování nastavení] (./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML jednoho přihlašování nastavení")

    1.  Do textového pole **název** zadejte název pro konfiguraci.  

        >[AZURE.NOTE] Poskytování hodnoty **název** automatické vyplnění textového pole **Název rozhraní API** .

    2.  Na portálu na **konfigurovat jednotné přihlašování v Work.com** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vydavatel** .
    3.  K nahrání stažený certifikát, klikněte na **Procházet**.
    4.  Do textového pole **Entity Id** zadejte **https://salesforce-work.com**.
    5.  Jako **Typ identit SAML**zaškrtněte políčko **výraz obsahuje Identifikátor federace z objektu uživatele**.
    6.  Je **Umístění identit SAML**vyberte položku **Identity v elementu NameIdentfier příkazu předmět**.
    7.  Na portálu na **konfigurovat jednotné přihlašování v Work.com** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL přihlašovací poskytovatele Identity** .
    8.  Na portálu na **konfigurovat jednotné přihlašování v Work.com** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlášení poskytovatele Identity** .
    9.  Jako **Služby, které iniciuje požádat o vazbu zprostředkovatele**vyberte **Publikovat HTTP**.
    10. Klikněte na **Uložit**.

15. V klasickém portálu Work.com v levém navigačním podokně klikněte na položku **Domain Management** rozbalte části související a klikněte **My Domain** otevřete stránku **My Domain** . 

    ![Mé domény] (./media/active-directory-saas-work-com-tutorial/IC794115.png "Mé domény")

16. Na stránce **Moje doménu** v části **Přihlašovací stránky Branding** klikněte na **Upravit**.

    ![Přihlašovací stránka Branding] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Přihlašovací stránka Branding")

17. Na stránce **Branding přihlašovací stránky** v části **Služba ověřování** název **SAML jednotného přihlašování k nastavení** se zobrazí. Vyberte ho a pak klikněte na **Uložit**.

    ![Přihlašovací stránka Branding] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Přihlašovací stránka Branding")

18. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Uživatelům Azure Active Directory mohli přihlásit musí být zřízení k Work.com.  
V případě Work.com zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti Work.com jako správce.

2.  Přejděte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Nastavení")

3.  Přejděte na **Správa uživatelů \> uživatelé**.

    ![Správa uživatelů] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Správa uživatelů")

4.  Klikněte na **Nový uživatel**.

    ![Všichni uživatelé] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Všichni uživatelé")

5.  V části Upravit uživatele proveďte následující kroky:

    ![Úpravy uživatele] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Úpravy uživatele")

    1.  Zadejte **Příjmení**, **aliasu**, **e-mailu**, **uživatelské jméno** a **přezdívek** atributy platný účet Azure Active Directory, který chcete poskytování do související textová pole.
    2.  Vyberte **roli**, **uživatelskou licenci** a **profil**.
    3.  Klikněte na **Uložit**.  

        >[AZURE.NOTE] Majitele účtu služby Azure Active Directory pošle e-mail a odkaz na před se aktivuje, potvrďte účet.

>[AZURE.NOTE] Můžete použít jakékoli další Work.com uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Work.com k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Přiřazení uživatelů k Work.com, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace Work.com aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Ano")
  
Teď by měl až 10 minut a ověřte, že účet synchronizování k Work.com.com.
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).