<properties
    pageTitle="Kurz: Azure Active Directory integrace Halogen softwarem"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Halogen Software."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Kurz: Azure Active Directory integrace Halogen softwarem

Cílem tohoto kurzu je vidíte, jak integrovat Halogen Software s Azure Active Directory (Azure AD).

Integrace s Azure AD Halogen Software poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo má přístup k Halogen Software 
- Povolení uživatelům, aby automaticky získat přihlášení na Halogen software (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat integraci Azure AD Halogen softwarem, musíte následující položky:

- Předplatné Azure AD
- Halogen Software jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí. 

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Halogen Software z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-halogen-software-from-the-gallery"></a>Přidání Halogen Software z Galerie
Pokud chcete nakonfigurovat integraci Halogen Software do Azure AD, potřebujete přidat Halogen Software z Galerie do seznamu spravované SaaS aplikace.

**Přidání Halogen softwaru z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky. 

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **halogen software**.

    ![Aplikace][5]

7. V podokně výsledků vyberte **Halogen Software**a potom klikněte na **Dokončit** přidat aplikaci.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Halogen softwaru na základě zkušební uživatele s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému Halogen Software pro uživatele v Azure AD. Odkaz relace mezi Azure Active Directory a související uživatelem softwaru Halogen jinými slovy, je nutné stanovit.

Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Halogen Software.
 
Ke konfiguraci a otestujte Azure AD jednotného přihlašování Halogen softwarem, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednoho jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření softwaru Halogen testování uživatele](#creating-a-halogen-software-test-user)** – a protějšek Britta Simon Halogen Software, který je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurace Azure AD jednoduchým jednotné přihlašování

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Halogen Software.


**Abyste mohli nakonfigurovat Azure AD jednotného přihlašování Halogen softwarem, proveďte následující kroky:**

1. Na portálu na stránce **Halogen Software** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][8]

2. Na stránce **jakým způsobem uživatelé přihlásit k softwaru Halogen** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][9]

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:  ![konfigurovat nastavení aplikace][10]
 
     na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci Halogen Software pomocí následujícího vzorce: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Klikněte na tlačítko **Další**.
 
4. Na stránce **konfigurovat jednotné přihlašování v článku Software Halogen** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.
    
    ![Co je Azure AD Connect][11]

5. V jiném okně prohlížeče přihlašování do aplikace **Halogen Software** jako správce.

6. Klikněte na kartu **Možnosti** . 

    ![Co je Azure AD Connect][12]


7. V levém navigačním podokně klikněte na **SAML konfigurace**. 

    ![Co je Azure AD Connect][13]

8. Na stránce **Konfigurace SAML** proveďte následující kroky:  ![co je Azure AD Connect][14]

    na. **Jedinečný identifikátor**vyberte **NameID**.

    b. Jako **Mapy jedinečný identifikátor k**vyberte **uživatelské jméno**.

    c. Nahrajte soubor stažený metadata, klikněte na **Procházet** a vyberte soubor a potom **Nahrát soubor**.

    d. Konfigurace, klikněte na **Spustit otestujte**. 

    > [AZURE.NOTE] Budete muset počkat zprávy "*SAML test je dokončeno. Zavřete okno*". Zavřete okno otevřené prohlížeče. Zaškrtávací políčko **Povolit SAML** je dostupné jenom v případě dokončení testu.

    e. Zaškrtněte políčko **Povolit SAML**.
    
    f. Klikněte na **Uložit změny**. 


9. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** . 

    ![Co je Azure AD Connect][15]

10. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Co je Azure AD Connect][16]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Co je Azure AD Connect][100] 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Co je Azure AD Connect][101] 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Co je Azure AD Connect][102] 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Co je Azure AD Connect][103] 
 
    na. Jako **Typ uživatele**vyberte **nového uživatele ve vaší organizaci**.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na další.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Co je Azure AD Connect][104] 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. V **Příjmení** txtbox, typ, **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Co je Azure AD Connect][105]  

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Co je Azure AD Connect][106]   

    na. Poznamenejte si hodnotu **Nové heslo**.
    b. Klikněte na **dokončení**.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Vytvoření uživatele test Halogen Software

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon Halogen softwaru.

**Pokud chcete vytvořit uživatele s názvem Britta Simon Halogen softwaru, proveďte následující kroky:**

1. Přihlaste se do aplikace **Halogen Software** jako správce.

2. Klikněte na kartu **Centrum uživatele** a potom klikněte na **Vytvořit uživatele**.

    ![Co je Azure AD Connect][300]  

3. Na stránce dialogové okno **Nový uživatel** proveďte následující kroky:

    ![Co je Azure AD Connect][301]

    na. Do textového pole **jméno** zadejte **Britta**. 
  
    b. **Příjmení** do textového pole zadejte **Simon**.
  
    c. Do textového pole **uživatelské jméno** zadejte **uživatelské jméno Brita Simon Azure klasické portálu**.
  
    d. Do textového pole **heslo** zadejte heslo pro Britta.
  
    e. Klikněte na **Uložit**.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup Halogen Software.

![Co je Azure AD Connect][200]

**Pokud chcete přiřadit Britta Simon Halogen Software, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Co je Azure AD Connect][201]

2. V seznamu aplikací vyberte **Halogen Software**.

    ![Co je Azure AD Connect][202]

1. V nabídce na horní klikněte na **uživatele**.

    ![Co je Azure AD Connect][203]

1. V seznamu uživatelů zvolte **Britta Simon**.

    ![Co je Azure AD Connect][204]

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Co je Azure AD Connect][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Halogen Software na panelu aplikace Access můžete by měla získat automaticky přihlášení na aplikaci Halogen Software.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png