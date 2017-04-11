<properties
    pageTitle="Kurz: Azure Active Directory integrace s DocuSign | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a DocuSign."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Kurz: Azure Active Directory integrace s DocuSign

Cílem tohoto kurzu je zobrazit integrace Azure a DocuSign.
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

- Platné Azure předplatné
- Ke klientovi DocuSign



Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1. [Povolení integrace aplikací pro DocuSign](#enabling-the-application-integration-for-docusign) 


2. [Konfigurace jednotného přihlašování](#configuring-single-sign-on) 


3. [Konfigurace zřízení účtu](#configuring-account-provisioning) 


4. [Přiřazení uživatelů](#assigning-users) 

    ![Konfigurace jednotného přihlašování][0]
 

## <a name="enabling-the-application-integration-for-docusign"></a>Povolení integrace aplikací pro DocuSign

Cílem tento oddíl je přehledu jak povolit integraci aplikace DocuSign.

### <a name="to-enable-the-application-integration-for-docusign-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace DocuSign, proveďte následující kroky:

1. Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Konfigurace jednotného přihlašování][1]

2. Ze seznamu adresáře vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Konfigurace jednotného přihlašování][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. Na stránce co chcete dialogové okno kliknutím na **Přidat aplikaci z Galerie**.

    ![Konfigurace jednotného přihlašování][4]


6. Do pole Hledat zadejte **DocuSign**.

    ![Konfigurace jednotného přihlašování][5]

7. V podokně výsledků vyberte **DocuSign**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Konfigurace jednotného přihlašování][6]


## <a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování

Cílem tento oddíl je přehledu jak povolit uživatelům ověřit DocuSign s účtem v Azure AD pomocí federace podle protokolu SAML.


### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1. Na portálu na stránce **integraci aplikace DocuSign** Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno Konfigurace jednotného přihlašování.

    ![Konfigurace jednotného přihlašování][7]

2. Na stránce **jakým způsobem uživatelé přihlásit k DocuSign** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko Další.

    ![Konfigurace jednotného přihlašování][8]

3. Na stránce **Konfigurovat nastavení aplikace** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování][61]

    na. **Přihlaste se na adresu URL** do textového pole, zadejte `https://account.docusign.com/*`.  

    b. Do textového pole **identifikátor** zadejte `https://account.docusign.com/*`.  
   
    c. Klikněte na tlačítko **Další**. 


    > [AZURE.TIP] Přihlaste se na adresu URL a identifikátor URI hodnoty jsou pouze zástupné symboly. Pokyny k načtení správných hodnot ve vašem prostředí jsou zahrnuty dál v tomto tématu.
 

4. Na stránce **konfigurovat jednotné přihlašování v DocuSign** klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování][10]


5. V okně prohlížeče jiný web Přihlaste se k **portálu správy DocuSign** jako správce.


6. V nabídce navigace na levé straně klikněte na **domény**.

    ![Konfigurace jednotného přihlašování][51]

7. V pravém podokně klikněte na **Deklaraci identity doménu**.

    ![Konfigurace jednotného přihlašování][52]

8. V dialogovém okně **deklarace doménu** do textového pole **Název domény** zadejte doménu společnosti a klikněte na **deklaraci identity**. Ujistěte se, že ověření domény a stav je aktivní.

    ![Konfigurace jednotného přihlašování][53]

9. V nabídce na levé straně klikněte na **Zprostředkovatelé identit jiní**  

    ![Konfigurace jednotného přihlašování][54]

10. V pravém podokně klikněte na **Přidat zprostředkovatele identit**. 
    
    ![Konfigurace jednotného přihlašování][55]

11. Na stránce **Nastavení zprostředkovatele identit** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování][56]


    na. Do textového pole **název** zadejte jedinečný název pro konfiguraci. Nepoužívejte mezery.

    b. Na portálu Azure klasické zkopírujte adresu URL Vystavitel a potom je vložte do textového pole **Vystavitel poskytovatele Identity** .

    c. Na portálu Azure klasické zkopírujte **Vzdálené přihlašovací adresa URL**a potom je vložte do textového pole **Adresa URL přihlašovací poskytovatele Identity** .

    d. Na portálu Azure klasické zkopírujte adresu **URL vzdálené odhlásit**a potom je vložte do textového pole **Adresa URL odhlášení poskytovatele Identity** .

    e. Vyberte **Znaménko AuthN žádosti**.

    f. **Odeslání AuthN žádost**vyberte **Publikovat**.

    g. Jako, **Odeslat žádost odhlásit**vyberte **Publikovat**. 


12. V části **Vlastní atribut mapování** vyberte pole, které chcete propojit s Azure AD deklarace. V tomto příkladu deklaraci **emailaddress** namapovala s hodnotou **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**. Toto je výchozí název deklarace z Azure AD pro deklarace e-mailu. 

    > [AZURE.NOTE] Pomocí příslušných **identifikátor uživatele** namapovat z Azure AD Docusign uživatele mapování. Vyberte správné pole a zadejte příslušné hodnoty na základě nastavení organizace.

    ![Konfigurace jednotného přihlašování][57]

13. V části **Certifikátů zprostředkovatele identit** klikněte na **Přidat certifikát**a pak nahrajte certifikát, který jste stáhli z portálu klasické Azure AD.   

    ![Konfigurace jednotného přihlašování][58]

14. Klikněte na **Uložit**.

15. V části **Zprostředkovatelé identit jiní** klikněte na možnost **Akce**a potom klikněte na **koncové body**.   

    ![Konfigurace jednotného přihlašování][59]



10. Na portálu Azure klasické vraťte se na stránce **Konfigurovat nastavení aplikace** . 

16. Na **portálu správce DocuSign**v části **Zobrazení SAML 2.0 koncové body** provedete následující kroky:

    ![Konfigurace jednotného přihlašování][60]

    na. Zkopírujte adresu **URL Vystavitel poskytovatele služby**a potom je vložte do textového pole **identifikátor** na portálu Azure klasické.

    b. Zkopírujte adresu **URL přihlašovací poskytovatele služby**a potom je vložte do textového pole **Symbol na adresu URL** na portálu Azure klasické.

    c.  Klikněte na tlačítko **Zavřít**  


10. Na portálu Azure klasické klikněte na **Další**. 


15. Na portálu Azure klasické vyberte **potvrzení jednotné přihlašování**a klikněte na tlačítko **Další**.

    ![Aplikace][14]

10. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.

    ![Aplikace][15]
 

## <a name="configuring-account-provisioning"></a>Konfigurace zřízení účtu

Cílem tento oddíl je přehledu povolení uživatele zřizování adresářové služby Active Directory pro DocuSign.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1. V **Azure klasické portál**, na stránce **DocuSign integraci aplikace** klikněte na **konfigurovat účet zřizování** otevřete dialogové okno Konfigurovat zřizování uživatelů.

    ![Konfigurace zřízení účtu][30]

2. Na stránce **Nastavení a přihlašovací údaje správce** povolit automatické zřizování uživatelů, přihlašovací údaje účtu DocuSign poskytnout dostatečná oprávnění a klikněte na tlačítko **Další**. 

    ![Konfigurace zřízení účtu][31]

3. V dialogovém okně **Test připojení** klikněte na tlačítko **Start test**a na úspěšné podmínce, klikněte na tlačítko **Další**.

    ![Konfigurace zřízení účtu][32]

3. Na stránce **potvrzení** klikněte na **Dokončit**.

    ![Konfigurace zřízení účtu][33]
 

## <a name="assigning-users"></a>Přiřazení uživatelů

Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

### <a name="to-assign-users-to-docusign-perform-the-following-steps"></a>Přiřazení uživatelů k DocuSign, proveďte následující kroky:

1. **Azure klasické portál**vytvořte testovací účet.

2. Na stránce **DocuSign integraci aplikace** klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů][40]
 

3. Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Přiřazení uživatelů][41]


Teď by měl až 10 minut a ověřte, že účet synchronizování k DocuSign.

Jako první krok ověření můžete zkontrolovat stav zřizovací kliknutím na řídicí panel v D na stránce DocuSign aplikace integrace na portálu Azure klasické.

![Přiřazení uživatelů][42]

Uživatel úspěšně dokončené zřizování obrázku je označen související stavu:

![Přiřazení uživatelů][43]


Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access.

Podrobné informace o panelu přístupu najdete v článku Úvod do panelu aplikace Access.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png