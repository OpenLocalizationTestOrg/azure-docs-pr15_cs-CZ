<properties
    pageTitle="Kurz: Azure Active Directory integrace s Litmos | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Litmos."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Kurz: Azure Active Directory integrace s Litmos

Cílem tohoto kurzu je vidíte, jak integrovat Litmos s Azure Active Directory (Azure AD).  
Integrace Litmos s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo má přístup k Litmos 
- Povolení uživatelům, aby automaticky získat přihlášení na k Litmos (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – Azure Active Directory 

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat integraci Azure AD pomocí Litmos, musíte následující položky:

- Předplatné Azure AD
- Litmos jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze tří hlavních stavebních bloků:

1. Přidání Litmos z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-litmos-from-the-gallery"></a>Přidání Litmos z Galerie
Pokud chcete nakonfigurovat integraci Litmos do Azure AD, potřebujete přidat Litmos z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat Litmos z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Litmos**.

    ![Aplikace][5]

7. V podokně výsledků vyberte **Litmos**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Aplikace][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Litmos založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému Litmos uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Litmos jinými slovy, je nutné stanovit.  
Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Litmos.
 
Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Litmos, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření Litmos otestujte uživatele](#creating-a-halogen-software-test-user)** – a protějšek Britta Simon Litmos, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu klasické Azure AD a konfigurace jednotného přihlašování v aplikaci Litmos.  
V rámci tohoto postupu je nutné vytvořit soubor zakódovaný certifikátu základu-64.  
Pokud znáte není tohoto postupu, naleznete v článku [Převedení binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

V rámci konfigurace budete muset přizpůsobit **Tokenu atributy SAML** Litmos aplikace.  

![Azure AD jednotné přihlášení][17] 

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Litmos, proveďte následující kroky:**

1. V klasickém portálu Azure AD na stránce **Litmos** aplikace integrace klikněte **Konfigurace jednotného přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k Litmos** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
 
    ![Azure AD jednotné přihlášení][7] 


1. Přihlášení k vašemu webu společnosti Litmos (například: *https://azureapptest.litmos.com/account/Login*) jako správce.

    ![Azure AD jednotné přihlášení][21] 


1. Na navigačním panelu na levé straně klikněte na **účty**.

    ![Azure AD jednotné přihlášení][22] 


1. Klikněte na kartu **Integrace** .

    ![Azure AD jednotné přihlášení][23] 


1. Na kartě **Integrace** posuňte se dolů na **3 integrace stran**a potom klikněte na kartu **SAML 2.0** .

    ![Azure AD jednotné přihlášení][24] 

1. Zkopírujte hodnotu v části **je SAML endoiint pro litmos:**.

    ![Azure AD jednotné přihlášení][26] 


3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno Azure klasické portálu, proveďte následující kroky:

    ![Azure AD jednotné přihlášení][8] 
 
    na. **Identifikátor URI** do textového pole, zadejte adresu URL používá uživatelů k přihlašování k aplikaci Litmos (například: *https://azureapptest.litmos.com/account/Login*).
     
    b. V textovém poli **Odpovědět URL** vložte hodnotu, kterou jste zkopírovali z aplikace Litmos v předchozím kroku.

    c. Klikněte na tlačítko **Další**.
 
4. Na stránce **konfigurovat jednotné přihlašování v Litmos** proveďte následující kroky:

    ![Azure AD jednotné přihlášení][2] 

    na. Klikněte na Stáhnout certifikát a uložte soubor na svém počítači.


1. V aplikaci **Litmos** proveďte následující kroky:

    ![Azure AD jednotné přihlášení][25] 

    na. Zaškrtněte políčko **Povolit SAML**.

    b. Vytvoření souboru **kódování base-64** z stažený certifikát.  

    >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o)

    c. Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **SAML X.509 certifikát** .

    d. Klikněte na **Uložit změny**.


6. Na portálu klasické Azure AD vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
  
    ![Azure AD jednotné přihlášení][11]


20. V nabídce nahoře klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** . 

    ![Konfigurace jednotného přihlašování][12]


24. V dialogovém okně **Přidat uživatele atribut** proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování][14]

  	| Název atributu | Hodnota atributu |
  	| ---            | ---             |
  	| E-mailu          | User.Mail       |
  	| Jméno      | User.givenName  |
  	| Příjmení       | User.Surname    |

    Pro každý řádek dat v předchozí tabulce proveďte následující kroky:
   
    na. Klikněte na **Přidat uživatele atribut**. 

    ![Konfigurace jednotného přihlašování][15]


    na. **Název atributu** do textového pole zadejte **Název atributu** se zobrazí pro tento řádek.

    b. Vyberte **Hodnotu atributu** zobrazí pro tento řádek.

    c. Klikněte na **Dokončit** zavřete dialogové okno **Přidat atribut uživatele** .


25. Klikněte na **použít změny**. 

    ![Konfigurace jednotného přihlašování][16]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.  

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure clasic**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
 
4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  

    na. Jako **Typ uživatele**vyberte **nového uživatele ve vaší organizaci**.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
 
    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.
    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
 
8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
  
    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   

  
 
### <a name="creating-a-litmos-test-user"></a>Vytvoření uživatele Litmos test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v Litmos.  
Aplikace Litmos podporuje pouze za běhu zřizování. To znamená, účet uživatele se automaticky vytvoří v případě potřeby při pokusu o přístup k aplikaci pomocí panelu aplikace Access.

**Vytvoření uživatele s názvem Britta Simon v Litmos, proveďte následující kroky:**


1. Přihlášení k vašemu webu společnosti Litmos (například: *https://azureapptest.litmos.com/account/Login*) jako správce.

    ![Azure AD jednotné přihlášení][21] 


1. Na navigačním panelu na levé straně klikněte na **účty**.

    ![Azure AD jednotné přihlášení][22] 


1. Klikněte na kartu **Integrace** .

    ![Azure AD jednotné přihlášení][23] 


1. Na kartě **Integrace** posuňte se dolů na **3 integrace stran**a potom klikněte na kartu **SAML 2.0** .

    ![Azure AD jednotné přihlášení][24] 

1. Vyberte **automaticky vytvořit uživatelů:**.

    ![Azure AD jednotné přihlášení][27] 


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Litmos.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Litmos, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Litmos**.

    ![Přiřazení uživatele][202] 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici Litmos na panelu aplikace Access můžete by měla získat automaticky přihlášení z Litmos aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png





