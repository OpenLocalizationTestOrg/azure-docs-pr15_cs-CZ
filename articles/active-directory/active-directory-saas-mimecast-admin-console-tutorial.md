<properties 
    pageTitle="Kurz: Azure Active Directory integrace s konzole pro správu Mimecast | Microsoft Azure" 
    description="Naučte se používat konzole pro správu Mimecast s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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

#<a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Kurz: Azure Active Directory integrace s konzole pro správu Mimecast
  
Cílem tohoto kurzu je zobrazit integrace Azure a konzole pro správu Mimecast.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Konzole pro správu Mimecast jednotné přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili konzole pro správu Mimecast bude moct jednotného přihlášení do aplikace na webu společnosti konzole pro správu Mimecast (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro konzole pro správu Mimecast
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Scénář")
##<a name="enabling-the-application-integration-for-mimecast-admin-console"></a>Povolení integrace aplikací pro konzole pro správu Mimecast
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace konzole pro správu Mimecast.

###<a name="to-enable-the-application-integration-for-mimecast-admin-console-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace konzole pro správu Mimecast, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Konzole pro správu Mimecast**.

    ![Galerie aplikace] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Mimecast konzole pro správu**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Konzole pro správu Mimecast] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Konzole pro správu Mimecast")
##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit konzole pro správu Mimecast s účtem ve Azure AD pomocí federace na základě SAML protokolu.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **Konzole pro správu Mimecast** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k konzole pro správu Mimecast** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** **Mimecast správce konzoly Sign na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci konzole pro správu Mimecast (například: "https://webmail-uk.mimecast.com" nebo "https://webmail-us.mimecast.com") a potom na tlačítko **Další**.

    >[AZURE.NOTE] Přihlašovací adresa URL je konkrétní oblasti.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v konzole pro správu Mimecast** stáhnout certifikátu, klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Konfigurace jednotného přihlašování")

5.  V okně různých webového prohlížeče Přihlaste se jako správce k konzole Mimecast správce.

6.  Přejděte na **služby \> aplikace**.

    ![Služby] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Služby")

7.  Klikněte na **profily ověřování**.

    ![Ověřování profilů] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Ověřování profilů")

8.  Klikněte na **Nový profil ověřování**.

    ![Nové ověřování profilů] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Nové ověřování profilů")

9.  V části **Ověřování profilu** proveďte následující kroky:

    ![Ověřování profilu] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Ověřování profilu")

    1.  Do textového pole **Popis** zadejte název pro konfiguraci.
    2.  Vyberte možnost **Vynutit SAML ověřování pro konzole pro správu Mimecast**.
    3.  Jako **poskytovatele**vyberte **Azure Active Directory**.
    4.  Na portálu na **konfigurovat jednotné přihlašování v konzole pro správu Mimecast** dialogové okno stránce Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Adresa URL vydavatel** .
    5.  Na portálu na **konfigurovat jednotné přihlašování v konzole pro správu Mimecast** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Přihlašovací adresa URL** .
    6.  Na portálu na **konfigurovat jednotné přihlašování v konzole pro správu Mimecast** dialogové okno stránce Azure klasické zkopírujte její hodnotu **Vzdálené přihlašovací adresa URL** a potom je vložte do textového pole **Adresa URL odhlásit** .  

        >[AZURE.NOTE]Přihlašovací adresa URL adresy URL odhlášení hodnot a jsou pro konzole pro správu Mimecast stejné.

    7.  Vytvoření souboru **kódování base-64** z stažený certifikát.  

        >[AZURE.TIP]Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

    8.  Otevřít zakódovaný certifikátu základu-64 v poznámkovém bloku odebrat první řádek ("*--*") a poslední řádek ("*--*"), zkopírujte zbývající obsah ho do schránky a vložte ho do textového pole **Certifikátů zprostředkovatele identit (Metadata)** .
    9.  Zaškrtněte políčko **Povolit jednotného přihlášení na**.
    10. Klikněte na **Uložit**.

10. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit konzole pro správu Mimecast, musí být zřízení v konzole pro správu Mimecast.  
V případě konzole pro správu Mimecast zřizování se ručně naplánovaný úkol.
  
Musíte si zaregistrovat doménu, než budete moct vytvářet uživatelů.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se ke **Konzole pro správu Mimecast** jako správce.

2.  Přejděte na **adresáře \> interní**.

    ![Adresáře] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Adresáře")

3.  Klikněte na **zaregistrovat novou doménu**.

    ![Registrace novou doménu] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Registrace novou doménu")

4.  Po vytvoření vaši novou doménu klikněte na **Nová adresa**.

    ![Nová adresa] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "Nová adresa")

5.  V dialogovém okně Nový adresu proveďte následující kroky:

    ![Uložení] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Uložení")

    1.  Zadejte **E-mailovou adresu**, **Globální jméno**, **heslo** a **Potvrďte heslo** atributy platného AAD účtu, který chcete poskytování do související textová pole.
    2.  Klikněte na **Uložit**.

>[AZURE.NOTE]Může použít jakékoli jiné konzole pro správu Mimecast uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou konzole pro správu Mimecast k poskytování AAD uživatelským účtům.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-mimecast-admin-console-perform-the-following-steps"></a>Přiřazení uživatelů k konzole pro správu Mimecast, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace **Konzole pro správu Mimecast **aplikace klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).