<properties
    pageTitle="Kurz: Azure Active Directory integrace s SciQuest stráví ředitele | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SciQuest věnovat ředitele."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Kurz: Azure Active Directory integrace s SciQuest věnovat Director

Cílem tohoto kurzu je vidíte, jak integrovat SciQuest stráví ředitele s Azure Active Directory (Azure AD).  
Integrace SciQuest věnovat ředitele s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo má přístup k SciQuest stráví Director 
- Povolení uživatelům, aby automaticky získat přihlášení z SciQuest věnovat ředitele (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat Azure AD integrace s SciQuest věnovat ředitele, musíte následující položky:

- Předplatné Azure AD
- SciQuest stráví ředitele jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SciQuest věnovat ředitele z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-sciquest-spend-director-from-the-gallery"></a>Přidání SciQuest stráví ředitele z Galerie
Pokud chcete nakonfigurovat integraci SciQuest stráví ředitele do Azure AD, potřebujete přidat SciQuest stráví ředitele z Galerie do seznamu spravované SaaS aplikace.

**Přidat SciQuest stráví ředitele z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **sciQuest stráví ředitele**.

    ![Aplikace][5]

7. V podokně výsledků vyberte **SciQuest věnovat ředitele**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Aplikace][6]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí SciQuest věnovat ředitele založeného na uživateli testu s názvem "Britta Simon".

Pro jednotné přihlašování pracovat musí Azure AD ví, co je databáze odpovídajících funkcí pro uživatele systému SciQuest věnovat ředitele uživateli v Azure AD. Odkaz relace mezi Azure Active Directory a související uživatelem v aplikaci SciQuest věnovat Director jinými slovy, je nutné stanovit.  
Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v SciQuest stráví ředitele.
 
Ke konfiguraci a otestujte Azure AD jednotné přihlašování s SciQuest věnovat ředitele, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednoho jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření SciQuest věnovat ředitele testování uživatele](#creating-a-halogen-software-test-user)** – a protějšek Britta Simon věnovat ředitele SciQuest, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurace Azure AD jednoduchým jednotné přihlašování

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci SciQuest věnovat ředitele.

**Nakonfigurovat SciQuest stráví ředitele Azure AD jednotné přihlašování, proveďte následující kroky:**

1. Na portálu na stránce **SciQuest věnovat ředitele** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][8]

2. Na stránce **jakým způsobem uživatelé přihlásit k SciQuest věnovat ředitele** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][9]

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky: 

    ![Konfigurace nastavení aplikace][10]
 
     3.1. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci SciQuest věnovat ředitele pomocí následujícího vzorce: *https://.* SciQuest.com/.**

     3,2. **Adresa URL odpověď** do textového pole zadejte stejné hodnoty, které jste zadali do textového pole **Symbol na adresu URL** . 

     3.3. Klikněte na tlačítko **Další**.
 
4. Na stránce **konfigurovat jednotné přihlašování v SciQuest věnovat ředitele** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Co je Azure AD Connect][11]

5. Kontaktní SciQuest domovské stránce podpory pro povolení této metody ověřování pomocí výše uvedených stažený metadat.

6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a potom klikněte na **Dokončit** zavřete dialogové okno **Konfigurace jednotného přihlašování** . 

    ![Co je Azure AD Connect][15]

10. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    




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
  
 
### <a name="creating-a-sciquest-spend-director-test-user"></a>Vytvoření uživatele test SciQuest věnovat Director

Cílem tento oddíl je vytvoření uživatele v SciQuest věnovat ředitele s názvem Britta Simon.

Potřebujete kontaktujte tým podpory SciQuest věnovat ředitele a dejte jí podrobné informace o účtu test ho vytvořili.

Můžete taky můžete taky využít za běhu zřizování jednoho přihlašování funkci, která podporuje SciQuest věnovat ředitele.  
Pokud za běhu zřizování aktivní, uživatelé budou vytvořeny automaticky SciQuest stráví vedoucí během jednoho pokusu o přihlášení Pokud neexistují. Tato funkce nemusí můžete ručně vytvořit protějšek přihlášení uživatele.

Získat za běhu zřizování povoleno, budete muset kontaktovat jste váš tým podpory SciQuest stráví ředitele.
  

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup SciQuest věnovat ředitele.

![Co je Azure AD Connect][200]

**Pokud chcete přiřadit Britta Simon SciQuest věnovat ředitele, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Co je Azure AD Connect][201]

2. V seznamu aplikací vyberte **SciQuest věnovat ředitele**.

    ![Co je Azure AD Connect][202]

1. V nabídce na horní klikněte na **uživatele**.

    ![Co je Azure AD Connect][203]

1. V seznamu uživatelů zvolte **Britta Simon**.

    ![Co je Azure AD Connect][204]

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Co je Azure AD Connect][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici SciQuest stráví ředitele na panelu aplikace Access můžete by měla získat automaticky přihlášení z SciQuest stráví ředitele aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_20.png

