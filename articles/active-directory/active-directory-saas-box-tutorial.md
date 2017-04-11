<properties 
    pageTitle="Kurz: Azure Active Directory políčko integraci s | Microsoft Azure" 
    description="Informace o použití pole se službou Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
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




#<a name="tutorial-azure-active-directory-integration-with-box"></a>Kurz: Azure Active Directory integrace s polem


  
Cílem tohoto kurzu je zobrazit integrace Azure a pole.  
Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné Azure předplatné
-   Test klienta do pole
  
Po dokončení tohoto kurzu, Azure AD uživatelů, které jste přiřadili pole bude moct jednotného přihlášení do aplikace na webu společnosti pole (service provider, které iniciuje přihlášení), nebo pomocí [Úvod do panelu aplikace Access](active-directory-saas-access-panel-introduction.md).
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1.  Povolení integrace aplikací pro pole
2.  Konfigurace jednotného přihlašování
3.  Konfigurace uživatele a zřízení skupiny
4.  Přiřazení uživatelů

![Scénář] (./media/active-directory-saas-box-tutorial/IC769537.png "Scénář")



##<a name="enabling-the-application-integration-for-box"></a>Povolení integrace aplikací pro pole
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace pro pole.

###<a name="to-enable-the-application-integration-for-box-perform-the-following-steps"></a>Pokud chcete povolit integraci aplikace pro pole, proveďte následující kroky:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-box-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-box-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-box-tutorial/IC749321.png "Přidat aplikaci")

5.  V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-box-tutorial/IC749322.png "Přidat aplikaci z gallerry")

6.  **Do **vyhledávacího pole**zadejte.**

    ![Galerie aplikace] (./media/active-directory-saas-box-tutorial/IC701023.png "Galerie aplikace")

7.  V podokně výsledků zaškrtněte **políčko**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Pole] (./media/active-directory-saas-box-tutorial/IC701024.png "Pole")



##<a name="configuring-single-sign-on"></a>Konfigurace jednotného přihlašování
  
Cílem tento oddíl je přehledu jak povolit uživatelům ověření pole s účtem v Azure AD pomocí federace podle protokolu SAML. V rámci tohoto postupu musíte nahrajete metadat Box.com.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1.  Na portálu na stránce **pole** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-box-tutorial/IC769538.png "Konfigurace jednotného přihlašování")

2.  Na stránce **jakým způsobem uživatelé přihlásit k pole** vyberte **Microsoft Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-box-tutorial/IC769539.png "Konfigurace jednotného přihlašování")

3.  Na stránce **Konfigurace adresa URL aplikace** do textového **Pole klienta URL** zadejte adresu URL svého pole klienta (například: https://<mydomainname>. box.com) a potom na tlačítko **Další**.

    ![Konfigurace aplikace URL] (./media/active-directory-saas-box-tutorial/IC669826.png "Konfigurace aplikace URL")

4.  Na stránce **konfigurovat jednotné přihlašování v poli** ke stažení metadata, klikněte na **Stáhnout metadata**a potom soubor data místně na svém počítači.

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-box-tutorial/IC669824.png "Konfigurace jednotného přihlašování")

5.  Tým podpory vpřed tento soubor metadat pro pole. Potřeb týmu podpory nakonfiguruje jednotného přihlašování pro vás.

6.  Vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování] (./media/active-directory-saas-box-tutorial/IC769540.png "Konfigurace jednotného přihlašování")
##<a name="configuring-user-provisioning"></a>Konfigurace zřizování uživatelů
  
Cílem tento oddíl je přehledu povolení zřizování adresářové služby Active Directory do pole.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Konfigurace jednotného přihlašování, proveďte následující kroky:

1. Na portálu na stránce **pole** aplikace integrace Azure klasické klikněte na **Konfigurovat zřizování uživatelů** otevřete dialogové okno **Konfigurovat zřizování uživatelů** . 

    ![Povolení zřizování automatické uživatele] (./media/active-directory-saas-box-tutorial/IC769541.png "Povolení zřizování automatické uživatele")

2. Na stránce **uživatele zřizování políčko Povolit** dialogového okna klikněte na **Povolit zřizování uživatelů**. 

    ![Povolení zřizování automatické uživatele] (./media/active-directory-saas-box-tutorial/IC769544.png "Povolení zřizování automatické uživatele")

3. Na stránce **přihlášení k udělit přístup k pole** požadovaných pověření a potom klikněte na **Ověřit**. 

    ![Povolení zřizování automatické uživatele] (./media/active-directory-saas-box-tutorial/IC769546.png "Povolení zřizování automatické uživatele")


4. Klikněte na **udělit přístup k políčko** povolit tuto operaci a vraťte se do portálu Azure klasické. 

    ![Povolení zřizování automatické uživatele] (./media/active-directory-saas-box-tutorial/IC769549.png "Povolení zřizování automatické uživatele")


5. Na stránce **Možnosti zajišťování** zaškrtávací políčka **typy objektů zřízení** umožňují vyberte, zda jsou seskupení objektů zřízení pole současně s objekty uživatelů.  V části "přiřazení uživatelů a skupin" pod další informace.


6. Dokončete konfiguraci kliknutím na tlačítko dokončeno. 

    ![Povolení zřizování automatické uživatele] (./media/active-directory-saas-box-tutorial/IC769551.png "Povolení zřizování automatické uživatele")



##<a name="assigning-a-test-user"></a>Přiřazení zkušební uživatele
  
Testování konfigurace, budete muset udělit uživatelům Azure AD, že kterou chcete povolit používání aplikace access do ní přiřazením.

###<a name="to-assign-users-to-box-perform-the-following-steps"></a>Přiřazení uživatelů do pole, proveďte následující kroky:

1. Na portálu Azure klasické vytvořte testovací účet.

2. Na stránce integrace aplikace **pole **klikněte na **přiřadit uživatelům**. 

    ![Přiřazení uživatelů] (./media/active-directory-saas-box-tutorial/IC769552.png "Přiřazení uživatelů")

3.  Vyberte svůj testovací uživatelské jméno, klikněte na **přiřadit**a potom na tlačítko **Ano** potvrďte přiřazení. 

    ![Ano] (./media/active-directory-saas-box-tutorial/IC767830.png "Ano")
  
Teď by měla až 10 minut a ověřte, že účet synchronizování pole.

Jako první krok ověření můžete zkontrolovat stav zřizovací kliknutím na řídicí panel v D na stránce pole aplikace integrace na portálu Azure klasické.

![Řídicí panel] (./media/active-directory-saas-box-tutorial/IC769553.png "Řídicí panel")

Uživatel úspěšně dokončené zřizování obrázku je označen související stavu:

![Stav integrace] (./media/active-directory-saas-box-tutorial/IC769555.png "Stav integrace")


Ve vašem klientovi pole synchronizovaní uživatelé jsou uvedené v části **Spravovat uživatele** v **Konzole pro správu**.

![Stav integrace] (./media/active-directory-saas-box-tutorial/IC769556.png "Stav integrace")


##<a name="assigning-users-and-groups"></a>Přiřazení uživatelů a skupin

**Pole > Uživatelé a skupiny** kartu Azure klasické portálu umožňuje určit, kteří uživatelé a skupiny by měl mít udělený přístup do pole. Přiřazení uživatele nebo skupiny způsobí, že následujících akcí:

* Azure AD umožňuje přiřazené uživateli (buď tak, že přímé přiřazení nebo členství ve skupinách) pro ověření pole. Pokud není přiřazené uživateli, Azure AD nepovolí Přihlaste se k pole a vrátí chybu na přihlašovací stránce Azure AD.

* Dlaždici aplikace pro pole se přidá na uživatele [spouštěč aplikací](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

* Pokud je automatické vytváření zapnuto, pak přiřazených uživatelům nebo skupinám se přidají do zřizovací fronty automaticky zřízení.

    * Pokud k zřídit byly nakonfigurována objekty uživatelů, pouze pak všechny přímo přiřazení uživatelé umístěné ve frontě zřizovací a všichni uživatelé, které jsou součástí všech přiřazených skupinám budou umístěny ve frontě zřizovací. 
    
    * Pokud seskupit objekty byly konfigurace zřídit, jsou všechny přiřazené seskupit objekty zřízení na pole, stejně jako všichni uživatelé, které jsou součástí těchto skupin. Členství ve skupině a uživateli se zachovají po zápisu pole.
    
Můžete použít **atributy > jednotného přihlašování** kartu a nakonfigurujte, které atributy uživatele (nebo deklarací) mají být zobrazena pole během ověřování na základě SAML a **atributy > Provisioning** kartu a nakonfigurovat způsob atributy uživatelů a skupin flow z Azure AD pole během vytváření operace. Najdete v tématech pod další informace.


## <a name="additional-resources"></a>Další zdroje informací

* [Přizpůsobení deklarované Vystavitel tokenu SAML](active-directory-saml-claims-customization.md)
* [Zřizování: Přizpůsobení mapování atributů](active-directory-saas-customizing-attribute-mappings.md)
* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
