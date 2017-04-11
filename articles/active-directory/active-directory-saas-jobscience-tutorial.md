<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Jobscience | Microsoft Azure" 
    description="Naučte se používat Jobscience s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Kurz: Azure Active Directory integrace s Jobscience
  
Cílem tohoto kurzu je zobrazit integrace Azure a Jobscience.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Předplatné Jobscience jednotné přihlašování povolena
  
Po dokončení tohoto kurzu, které jste přiřadili Jobscience uživatelům Azure AD bude moct jednotného přihlášení do aplikace na webu společnosti Jobscience (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro Jobscience
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Scénář")
##<a name="enabling-the-application-integration-for-jobscience"></a>Povolení integrace aplikací pro Jobscience
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Jobscience.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace Jobscience, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **jobscience**.

    ![Galerie aplikace] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Jobscience**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Jobscience s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro Jobscience vyžaduje, abyste načíst Miniatura hodnotu z certifikát.  
Pokud znáte není tento postup, najdete v článku [jak načíst hodnotu Miniatura certifikátu](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti Jobscience jako správce.

2.  Přejděte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Nastavení")

3.  V levém navigačním podokně v části **Spravovat** klikněte na **Domain Management** rozbalte části související a klikněte na **My Domain** otevřete stránku **My Domain** . 

    ![Mé domény] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Mé domény")

4.  K ověření, že svoji doménu správně byl nastavení, ujistěte se, že je v "**Krok 4 používaný uživatele**" a zkontrolujte "**Nastavení domény**".

    ![Doména používaný uživatele] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "Doména nasazené na uživatele")

5.  V okně různých webového prohlížeče Přihlaste se k portálu Azure klasické.

6.  Na stránce integrace **Jobscience** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Konfigurace jednotného přihlašování")

7.  Na stránce **jakým způsobem uživatelé přihlásit k Jobscience** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Konfigurace jednotného přihlašování")

8.  Na stránce **Konfigurace adresa URL aplikace** v textovém poli **Jobscience znak v adrese URL** zadejte adresu URL pomocí následujícího vzorce "*http://company.my.salesforce.com*" a klikněte na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "Konfigurace aplikace URL")

9.  Na stránce **konfigurovat jednotné přihlašování v Jobscience** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Konfigurace jednotného přihlašování")

10. Na webu společnosti Jobscience klikněte na **Ovládací prvky zabezpečení**a potom klikněte na **Nastavení jednotného přihlašování**.

    ![Ovládací prvky zabezpečení] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Ovládací prvky zabezpečení")

11. V části **Nastavení jednotného přihlašování** proveďte následující kroky:

    ![Nastavení jednotného přihlašování] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Nastavení jednotného přihlašování")

    1.  Vyberte **SAML povolené**.
    2.  Klikněte na **Nový**.

12. V dialogovém okně **SAML jednoho přihlašování nastavení upravit** proveďte následující kroky:

    ![SAML jednoho přihlašování nastavení] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML jednoho přihlašování nastavení")

    1.  Do textového pole **název** zadejte název pro konfiguraci.
    2.  Na portálu na **konfigurovat jednotné přihlašování v Jobscience** dialog stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Vydavatel**
    3.  Do textového pole **Entity Id** zadejte **https://salesforce-jobscience.com**
    4.  Klikněte na tlačítko **Procházet** k nahrání Azure AD certifikát.
    5.  Jako **Typ identit SAML**zaškrtněte políčko **výraz obsahuje Identifikátor federace z objektu uživatele**.
    6.  Je **Umístění identit SAML**vyberte položku **Identity v elementu NameIdentfier příkazu předmět**.
    7.  Na portálu na **konfigurovat jednotné přihlašování v Jobscience** dialog stránce Azure klasické zkopírujte hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL přihlašovací poskytovatele Identity**
    8.  Na portálu na **konfigurovat jednotné přihlašování v Jobscience** dialog stránce Azure klasické zkopírujte hodnotu **Adresa URL vzdálené odhlásit** a potom je vložte do textového pole **Adresa URL odhlášení poskytovatele Identity**
    9.  Klikněte na **Uložit**.

13. V levém navigačním podokně v části **Spravovat** klikněte na **Domain Management** rozbalte části související a klikněte na **My Domain** otevřete stránku **My Domain** . 

    ![Mé domény] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Mé domény")

14. Na stránce **Moje doménu** v části **Přihlašovací stránky Branding** klikněte na **Upravit**.

    ![Přihlašovací stránka Branding] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Přihlašovací stránka Branding")

15. Na stránce **Branding přihlašovací stránky** v části **Služba ověřování** název **SAML jednotného přihlašování k nastavení** se zobrazí. Vyberte ho a pak klikněte na **Uložit**.

    ![Přihlašovací stránka Branding] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Přihlašovací stránka Branding")

16. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Konfigurace jednotného přihlašování")
  
Pokud chcete získat SP iniciovanému jednotné přihlašování přihlašovací adresa URL klepněte na **Nastavení jednotného přihlašování** v části **Ovládací prvky zabezpečení** nabídky.

![Ovládací prvky zabezpečení] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Ovládací prvky zabezpečení")
  
Klepněte na jednotné přihlašování profil, který jste vytvořili v předchozím kroku.  
Tato stránka zobrazuje jednotného přihlášení na adresu URL pro svou společnost (například *https://companyname.my.salesforce.com?so=companyid*).
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit Jobscience, musí být zřízení do Jobscience.  
V případě Jobscience zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **Jobscience** jako správce.

2.  Přejděte na nastavení

    ![Nastavení] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Nastavení")

3.  Přejděte na **Správa uživatelů \> uživatelé**.

    ![Uživatelé] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Uživatelé")

4.  Klikněte na **Nový uživatel**.

    ![Všichni uživatelé] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Všichni uživatelé")

5.  V dialogovém okně **Upravit uživatele** proveďte následující kroky:

    ![Úpravy uživatele] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Úpravy uživatele")

    1.  Zadejte jméno, příjmení, alias, e-mailů, uživatelské jméno a přezdívek vlastnosti Azure AD uživatele, kterého chcete poskytování do související textová pole.
    2.  Klikněte na **Uložit**.

    >[AZURE.NOTE] Majitele účtu Azure AD pošle e-mail obsahující odkaz potvrďte účet předtím, než je aktivován.

>[AZURE.NOTE] Můžete použít jakékoli další Jobscience uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou Jobscience k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Přiřazení uživatelů k Jobscience, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Jobscience **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).