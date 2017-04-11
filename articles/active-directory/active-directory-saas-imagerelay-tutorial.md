<properties
    pageTitle="Kurz: Azure Active Directory integrace s ImageRelay | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ImageRelay."
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
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-imagerelay"></a>Kurz: Azure Active Directory integrace s ImageRelay

Cílem tohoto kurzu je vidíte, jak integrovat ImageRelay s Azure Active Directory (Azure AD).  
Integrace ImageRelay s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k ImageRelay
- Povolení uživatelům, aby automaticky získat přihlášení na k ImageRelay (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí ImageRelay, musíte následující položky:

- Předplatné Azure AD
- ImageRelay jednotné přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ImageRelay z Galerie

2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-imagerelay-from-the-gallery"></a>Přidání ImageRelay z Galerie
Pokud chcete nakonfigurovat integraci ImageRelay do Azure AD, potřebujete přidat ImageRelay z Galerie do seznamu spravované SaaS aplikace.

**Pokud chcete přidat ImageRelay z galerie, proveďte následující kroky:**

1. Azure klasické portálu v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **ImageRelay**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_01.png)

7. V podokně výsledků vyberte **ImageRelay**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí ImageRelay založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí uživatelský účet, který představuje související uživatele v ImageRelay.  Odkaz vztah mezi Azure Active Directory a související uživatele v ImageRelay jinými slovy, je nutné stanovit.  
Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v ImageRelay.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s ImageRelay, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření ImageRelay testování uživatele](#creating-a-userecho-test-user)** – a protějšek Britta Simon ImageRelay, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci ImageRelay.


**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s ImageRelay, proveďte následující kroky:**

1. Na portálu na stránce **ImageRelay** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

     ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k ImageRelay** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

     ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaši uživatelé přihlásit k aplikaci ImageRelay (například: *https://fabrikam.ImageRelay.com/*).

    b. Klikněte na tlačítko **Další**.

4. Na stránce **konfigurovat jednotné přihlašování v ImageRelay** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.

5. V jiném okně prohlížeče Přihlaste se k vašemu webu společnosti ImageRelay jako správce.

1. Na panelu nástrojů na horní klikněte na **Uživatelé a oprávnění** úlohu.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. Klikněte na **vytvořit novou oprávnění**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png) 

1. V pracovní zátěž **Přihlaste se na nastavení jednotného** zaškrtněte políčko **této skupiny můžou jenom přihlášení pomocí jednotného přihlašování** a pak klikněte na **Uložit**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. Přejděte na **Nastavení účtu**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. Přejděte na pracovní zátěž **Jeden znak na nastavení** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

1. V dialogovém okně **Nastavení SAML** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)

    na. Na portálu Azure klasické zkopírujte **Jednoho přihlašování adresy URL služby**a potom je vložte do textového pole **Přihlašovací adresa URL** .


    b. Na portálu Azure klasické zkopírujte **Jedné adresy URL služby Sign-Out**a potom je vložte do textového pole **Adresa URL odhlásit** .

    c. Jako **Název Id formát**, vyberte **urn: oasis: názvy: tc: SAML:1.1:nameid-formát: emailAddress**.

    
    d. **Vazba možností pro žádosti o poskytovateli služeb (obrázek přenosu)**vyberte **Vazba příspěvek**.
   

    e. V části **x.509 certifikát**klikněte na **Aktualizovat certifikát**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Otevřete stažený certifikát v poznámkovém bloku, zkopírujte obsah a potom je vložte do textového pole x.509 certifikát.
  
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. V části **JIT zřizování uživatelů** vyberte **Povolit zřizování uživatelů JIT**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Vyberte (například **Základní SSO**) oprávnění skupinu, která bude moct přihlášení pouze prostřednictvím jednotného přihlašování.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    Můžu. Klikněte na **Uložit**.

6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.

    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasného hesla** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-imagerelay-test-user"></a>Vytvoření uživatele ImageRelay test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v ImageRelay.

**Vytvoření uživatele s názvem Britta Simon v ImageRelay, proveďte následující kroky:**

1. Přihlášení k vašemu webu společnosti ImageRelay jako správce.

1. Přejděte na **Uživatelé a oprávnění** a vyberte **Vytvořit jednotné přihlašování uživatele**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. Zadejte **e-mailu**, **křestního jména**, **Příjmení** a **společnosti** uživatele, který chcete vytvořit a vyberte (například jednotné přihlašování základní) oprávnění skupinu, která je skupina, která se můžete přihlásit jenom prostřednictvím jednotného přihlašování.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. Klikněte na **vytvořit**.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k ImageRelay.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon ImageRelay, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v nabídce horním.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **ImageRelay**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_23.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici ImageRelay na panelu aplikace Access můžete by měla získat automaticky přihlášení z ImageRelay aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_205.png
