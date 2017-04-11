<properties 
    pageTitle="Kurz: Azure Active Directory integrace s TOPdesk - zabezpečené | Microsoft Azure"
    description="Naučte se používat TOPdesk - zabezpečené službou Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!." 
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

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Kurz: Azure Active Directory integrace s TOPdesk – zabezpečení
  
Cílem tohoto kurzu je zobrazit integrace Azure a TOPdesk – zabezpečení.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   V TOPdesk - zabezpečené jednotného přihlašování povolené předplatného
  
Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili TOPdesk – zabezpečení budou moct jednotného přihlášení do aplikace v TOPdesk - zabezpečené společnosti (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro TOPdesk – zabezpečení
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scénář")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>Povolení integrace aplikací pro TOPdesk – zabezpečení
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace TOPdesk – zabezpečení.

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace TOPdesk - zabezpečené, proveďte následující kroky:

1.  Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **TOPdesk – zabezpečení**.

    ![Galerie aplikace] (./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Galerie aplikace")

7.  V podokně výsledků vyberte **TOPdesk – zabezpečení**a potom klikněte na **Dokončit** přidat aplikaci.

    ![TOPdesk – zabezpečení] (./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk – zabezpečení")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit TOPdesk - zabezpečit s účtem v Azure AD pomocí federace podle protokolu SAML.  
Konfigurace jednotného přihlašování pro TOPdesk - zabezpečené vyžaduje nahrát soubor ikonu logo. Pokud chcete dostat na ikonu soubor, obraťte se na tým podpory TOPdesk.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **TOPdesk - zabezpečené** jako správce.

2.  V nabídce **TOPdesk** klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Nastavení")

3.  Klikněte na **Nastavení přihlášení**.

    ![Nastavení přihlášení] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Nastavení přihlášení")

4.  Rozbalení nabídky **Nastavení přihlášení** a potom klikněte na **Obecné**.

    ![Obecné] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Obecné")

5.  V části **zabezpečené** části konfigurace **SAML přihlášení** proveďte následující kroky:

    ![Technická nastavení] (./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technická nastavení")

    1.  Klikněte na **Stáhnout** pro stažení souboru veřejné metadat a uložte ji místně na vašem počítači.
    2.  Otevřete soubor metadat a vyhledejte uzel **AssertionConsumerService** .
        ![Výraz zákaznické služby] (./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Výraz zákaznické služby")
    3.  Zkopírujte hodnotu **AssertionConsumerService** .  

        >[AZURE.NOTE] Budete potřebovat hodnotu v části **Adresa URL aplikace konfiguraci** dále v tomto kurzu.

6.  V okně různých webového prohlížeče Přihlaste se jako správce k **Azure klasické portál** .

7.  Na stránce integrace aplikace **TOPdesk – zabezpečení** klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Konfigurace jednotného přihlašování")

8.  Na stránce **jakým způsobem uživatelé přihlásit k TOPdesk - zabezpečené** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Konfigurace jednotného přihlašování")

9.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    ![Konfigurace aplikace URL] (./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Konfigurace aplikace URL")

    1.  Do textového pole **TOPdesk - zabezpečené znaménko na adresu URL** zadejte adresu URL používané uživatelů a přihlaste se do TOPdesk - zabezpečené aplikace (například: "*https://qssolutions.topdesk.net*").
    2.  Do textového pole **TOPdesk – veřejná adresa URL odpovědět** vložte **TOPdesk - zabezpečené URL AssertionConsumerService** (například: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Klikněte na tlačítko **Další**.

10. Na stránce **konfigurovat jednotné přihlašování v TOPdesk - zabezpečené** stáhnout soubor metadata, klikněte na tlačítko **Stáhnout metadat**a uložte jej místně na vašem počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Konfigurace jednotného přihlašování")

11. Pokud chcete vytvořit soubor certifikátu, proveďte následující kroky:

    ![Certifikát] (./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Certifikát")

    1.  Otevřete soubor stažený metadat.
    2.  Rozbalte uzel **RoleDescriptor** obsahující **xsi: type** z **krmena: ApplicationServiceType**.
    3.  Zkopírujte hodnotu uzel **509** .
    4.  Uložte hodnotu zkopírovaný **509** místně na svém počítači v souboru.

12. Na TOPdesk - zabezpečené společnosti, v nabídce **TOPdesk** klikněte na **Nastavení**.

    ![Nastavení] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Nastavení")

13. Klikněte na **Nastavení přihlášení**.

    ![Nastavení přihlášení] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Nastavení přihlášení")

14. Rozbalte nabídku **Přihlášení nastavení** a potom klikněte na **Obecné**.

    ![Obecné] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Obecné")

15. V části **veřejné** klikněte na **Přidat**.

    ![Přidání] (./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Přidání")

16. Na stránce dialogové okno **Pomocník pro konfiguraci SAML** proveďte následující kroky:

    ![Pomocník pro konfiguraci SAML] (./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "Pomocník pro konfiguraci SAML")

    1.  Nahrajte soubor stažený metadata, v části **Metadata federace**, klikněte na **Procházet**.
    2.  Nahrajte soubor certifikátu, klikněte v části **Certifikát RSA ()**, klikněte na **Procházet**.
    3.  Nahrání souboru s logem, kterou jste získali od týmu podpory TOPdesk pod **logem ikony**, klikněte na **Procházet**.
    4.  **Atribut uživatelského jména** do textového pole zadejte **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Do textového pole **název zobrazení** zadejte název pro konfiguraci.
    6.  Klikněte na **Uložit**.

17. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Konfigurace jednotného přihlašování")

##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Pokud chcete povolit uživatelům Azure AD přihlásit TOPdesk - zabezpečené, budou musí zřídit do TOPdesk - zabezpečené.  
V případě TOPdesk - zabezpečené, zřizování se ručně naplánovaný úkol.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1.  Přihlaste se k vašemu webu společnosti **TOPdesk - zabezpečené** jako správce.

2.  V nabídce nahoře, klikněte na **TOPdesk \> nový \> podpůrné soubory \> operátor**.

    ![Operátor] (./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operátor")

3.  V dialogovém okně **Nový operátor pro** proveďte následující kroky:

    ![Nový operátor] (./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "Nový operátor")

    1.  Klikněte na kartu Obecné.
    2.  Do textového pole **Příjmení** oddílu **Obecné** zadejte příjmení platný účet Azure Active Directory, který chcete poskytování.
    3.  Na **webu** pro účet vyberte v části **umístění** .
    4.  Do textového pole **Uživatelské jméno** části **TOPdesk přihlášení** zadejte přihlašovací jméno pro vaše uživatele.
    5.  Klikněte na **Uložit**.

>[AZURE.NOTE] Můžete použít jakékoli jiné TOPdesk - zabezpečené uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou TOPdesk - zabezpečené zřízení AAD uživatelské účty.

##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Přiřazení uživatelů k TOPdesk – zabezpečení, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce integrace aplikace **TOPdesk – zabezpečení **klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Ano")
  
Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access. Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).