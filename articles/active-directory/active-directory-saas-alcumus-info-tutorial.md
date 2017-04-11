<properties
    pageTitle="Kurz: Azure Active Directory integrace se serverem Exchange informace Alcumus | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Alcumus informace Exchange."
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


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Kurz: Azure Active Directory integrace se serverem Exchange Alcumus informace

Cílem tohoto kurzu je vidíte, jak integrovat Alcumus informace Exchange s Azure Active Directory (Azure AD).  
Integrace Alcumus informace Exchange s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo má přístup k Alcumus informace Exchange 
- Povolení uživatelům, aby automaticky získat přihlášení z k serveru Exchange informace Alcumus (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat Azure AD integrace se serverem Exchange Alcumus informace, musíte následující položky:

- Předplatné [Azure AD](https://azure.microsoft.com/)
- [Exchange informace Alcumus](http://www.alcumusgroup.com/) jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze tří hlavních stavebních bloků:

1. Přidání Exchange Alcumus informace z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Přidání Exchange Alcumus informace z Galerie
Abyste mohli nakonfigurovat integraci Alcumus informace Exchange do Azure AD, potřebujete přidat Exchange Alcumus informace z Galerie do seznamu spravované SaaS aplikace.

**Přidání Exchange Alcumus informace z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Alcumus informace Exchange**.

    ![Aplikace][5]

7. V podokně výsledků vyberte **Alcumus informace Exchange**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Aplikace][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Exchange informace Alcumus založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je protějšek uživatele systému Exchange Alcumus informace pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele systému Exchange informace Alcumus jinými slovy, je nutné stanovit.  
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Exchange Alcumus informace.
 
Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Alcumus informace Exchange, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření Exchange informace Alcumus testování uživatele](#creating-a-alcumus-info-exchange-test-user)** – mít protějšek Britta Simon v Exchange informace Alcumus, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Alcumus informace Exchange.

**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s Alcumus informace Exchange, proveďte následující kroky:**

1. Na portálu na stránce **Exchange Alcumus informace o** aplikaci integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6]

2. Na stránce **jakým způsobem uživatelé přihlásit k serveru Exchange informace Alcumus** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][7]

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky: 

    ![Azure AD jednotné přihlášení][8]
 
    na. v textovém poli **Odpovědět URL** zadejte adresu URL příjemce, který byl nastavení můžete tak, že váš tým podpory Alcumus informace Exchange.

    > [AZURE.NOTE] Pokud nevíte, co je hodnota vpravo, obraťte se na tým podpory Alcumus informace Exchange prostřednictvím [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    b. Klikněte na tlačítko **Další**.
 
4. Na stránce **konfigurovat jednotné přihlašování v Exchange Alcumus informace** klepněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Co je Azure AD Connect][9]

5. Obraťte se na tým podpory Alcumus informace Exchange prostřednictvím [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com), dejte jí souboru metadat a jejich dejte mu to vědět, že by měl umožňují jednotné přihlašování za vás.


6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Co je Azure AD Connect][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Co je Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.  

![Vytvořit Azure AD uživatele][20]

**Pokud chcete vytvořit testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
  
    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.
  
    c. Klikněte na další.



6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    na. Do textového pole **jméno** zadejte **Britta**.  
  
    b. V **Příjmení** txtbox, typ, **Simon**.
  
    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.
  
    d. Vyberte v seznamu **Role** **uživatele**.
  
    e. Klikněte na tlačítko **Další**.


7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.
  
    b. Klikněte na **dokončení**.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Vytvoření uživatele test Alcumus informace Exchange

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v Exchange Alcumus informace.

**Vytvoření uživatele s názvem Britta Simon v Exchange Alcumus informace, proveďte následující kroky:**

1. Obraťte se na tým podpory Alcumus informace Exchange prostřednictvím [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup k serveru Exchange Alcumus informace.

![Přiřazení uživatele][200]

**Pokud chcete přiřadit Britta Simon Alcumus informace Exchange, proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201]

2. V seznamu aplikací vyberte **Exchange Alcumus informace**.

    ![Přiřazení uživatele][202]

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici Alcumus informace o Exchange na panelu aplikace Access můžete by měla získat automaticky přihlášení z Exchange Alcumus informace o aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png