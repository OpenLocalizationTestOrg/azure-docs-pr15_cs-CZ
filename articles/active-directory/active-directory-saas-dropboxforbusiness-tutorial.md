<properties 
    pageTitle="Kurz: Azure Active Directory integrace s Dropboxu pro firmy | Microsoft Azure" 
    description="Informace o používání Dropboxu pro firmy s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Kurz: Azure Active Directory integrace s Dropboxu pro firmy
  
Cílem tohoto kurzu je zobrazit integrace Azure a Dropboxu pro firmy.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Test klienta v Dropboxu pro firmy
  
Po dokončení tohoto kurzu, společnost uživatelům Azure AD jste přiřadili Dropboxu pro firmy budete moci jednotného přihlášení do aplikace v Dropboxu pro firmy web (service provider, které iniciuje přihlášení) nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení aplikace integraci dropboxu pro firmy
2.  Konfigurace jednotného přihlašování
3.  Konfigurace zřizování uživatelů
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Scénář")



##<a name="enabling-the-application-integration-for-dropbox-for-business"></a>Povolení aplikace integraci dropboxu pro firmy
  
Cílem tento oddíl je přehledu povolení aplikace integraci dropboxu pro firmy.

###<a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>Chcete-li povolit integraci aplikace dropboxu pro firmy, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  Do **vyhledávacího pole**zadejte **Dropboxu pro firmy**.

    ![Galerie aplikace] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Galerie aplikace")

7.  V podokně výsledků vyberte **Dropboxu pro firmy**a pak klikněte na **Dokončit** přidat aplikaci.

    ![Dropboxu pro firmy] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropboxu pro firmy")

##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověřit Dropboxu pro firmy s účtem v Azure AD pomocí federace podle protokolu SAML.

V rámci tohoto postupu jsou potřeba k nahrání na Dropboxu pro firmy klienta zakódovaný certifikát základu-64. Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Azure klasické portálu na stránce aplikace integraci **Dropboxu pro firmy** klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Konfigurace jednotného přihlašování")

2.  Na stránce, **jakým způsobem uživatelé přihlásit k Dropboxu pro firmy** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** proveďte následující kroky:

    na. Přihlášení k vaší Dropboxu pro firmy klienta. 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Konfigurace jednotného přihlašování")

    b. V navigačním podokně na levé straně klikněte na **Konzole pro správu**. 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Konfigurace jednotného přihlašování")

    c. V **Konzole pro správu**klikněte v levém navigačním podokně na **ověření** . 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Konfigurace jednotného přihlašování")

    d. V části **jednotného přihlašování** zaškrtněte políčko **Povolit jednotného přihlašování**a potom klikněte na **Další** rozbalte tuto část.  

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Konfigurace jednotného přihlašování")

    e. Zkopírujte adresu URL u **mohli uživatelé přihlašovat zadáním e-mailovou adresu nebo můžete přejít přímo do**. 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Konfigurace jednotného přihlašování")

    f. Na portálu Azure klasické **Dropboxu pro firmy přihlášení** URL textového pole vložte adresu URL. 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Konfigurace jednotného přihlašování")  



4. Na stránce **konfigurovat jednotné přihlašování v Dropboxu pro firmy** klikněte na tlačítko **Stáhnout certifikát**a uložte jej certifikát v počítači.  

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Konfigurace jednotného přihlašování")


5. V Dropboxu pro firmy klienta, v části **jednotného přihlašování** na stránce **ověření** proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Konfigurace jednotného přihlašování")

    na. Klikněte na **požadované**.

    b. Na portálu na **konfigurovat jednotné přihlašování v Dropboxu pro firmy** dialog stránce Azure klasické zkopírujte hodnotu **Přihlašovací adresa URL stránky** a potom je vložte do **přihlášení adresu URL** do textového pole.


    c. Vytvoření souboru **kódování Base-64** z stažený certifikát. 

    > [AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).


    d. Klikněte na tlačítko **"Zvolit certifikát"** a pak přejděte k vaší **základu-64 zakódovaný soubor certifikátu**.


    e. Tlačítko **"Uložte změny"** dokončete konfiguraci v Dropboxu pro firmy klienta.


6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** . 

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Konfigurace jednotného přihlašování")



##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Cílem tento oddíl je přehledu povolení uživatele zřizování adresářové služby Active Directory do Dropboxu pro firmy.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Abyste mohli nakonfigurovat zřizování uživatelů, proveďte následující kroky:

1. Azure klasické portálu na stránce aplikace integraci **Dropboxu pro firmy** klikněte na **Konfigurovat zřizování uživatelů** otevřete dialogové okno **Konfigurovat zřizování uživatelů** .

2. Na povolit zřizování uživatele do Dropboxu pro firmy, klikněte na povolit zřizování uživatele otevřete přihlásit se ke Dropbox k propojení s dialogovým oknem Azure AD.  

    ![Zřizování uživatelů] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Zřizování uživatelů")

3. V dialogovém okně **Přihlásit se k Dropbox k propojení s Azure AD** přihlaste do Dropboxu pro firmy klienta. 

    ![Zřizování uživatelů] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Zřizování uživatelů")



4. Klikněte na **Povolit** udělit Azure AD pro přístup ke Dropboxu. 

    ![Zřizování uživatelů] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Zřizování uživatelů")



5. Dokončete konfiguraci, klikněte na tlačítko **Dokončit** .  

    ![Zřizování uživatelů] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Zřizování uživatelů")




##<a name="assigning-users"></a>Přiřazení uživatelů
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>Přiřazení uživatelů do Dropboxu pro firmy, proveďte následující kroky:

1.  Na portálu Azure klasické vytvořte testovací účet.

2.  Na stránce aplikace integraci **Dropboxu pro firmy **klikněte na **přiřadit uživatelům**.

    ![Přiřazení uživatelů] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení.

    ![Ano] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Ano")
  


Teď by měla až 10 minut a ověřte, že účet synchronizování do Dropboxu pro firmy.

Jako první krok ověření můžete zkontrolovat stav zřizovací kliknutím na **řídicí panel** na stránce aplikace integrace **Dropboxu pro firmy** na portálu Azure klasické.

![Přiřazení uživatelů] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Přiřazení uživatelů")


Uživatel úspěšně dokončené zřizování obrázku je označen související stav.

![Přiřazení uživatelů] (./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Přiřazení uživatelů")


Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel aplikace Access.
Podrobné informace o panelu přístupu najdete v článku [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).




## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)