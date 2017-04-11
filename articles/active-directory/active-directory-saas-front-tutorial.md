<properties
    pageTitle="Kurz: Azure Active Directory integrace s přední | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a popředí."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-front"></a>Kurz: Azure Active Directory integrace s dopředu

Cílem tohoto kurzu je vidíte, jak integrovat přední s Azure Active Directory (Azure AD).

Integrace přední s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup do popředí
- Povolení uživatelům, aby automaticky získat přihlášení z dopředu (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí popředí, musíte následující položky:

- Předplatné Azure AD
- Popředí jednotného přihlašování enabled předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání přední z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-front-from-the-gallery"></a>Přidání přední z Galerie
Pokud chcete nakonfigurovat integraci přední do Azure AD, potřebujete přidat přední z Galerie do seznamu spravované SaaS aplikace.

**Přidání přední z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .
    
    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.
    
    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **přední**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-front-tutorial/tutorial_front_01.png)

7. V panelu Výsledky vyberte **přední**a klikněte na **Dokončit** přidat aplikaci.

    ![Výběr aplikace v galerii](./media/active-directory-saas-front-tutorial/tutorial_front_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí přední založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je protějšek uživatelům před uživatelem v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele před jinými slovy, je nutné stanovit.

Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnotu **uživatelské_jméno** před.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s popředí, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření popředí testování uživatele](#creating-a-front-test-user)** – mít protějšek Britta Simon před propojený na Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci popředí.

**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s popředí, proveďte následující kroky:**

1. Na portálu klasické na stránce **popředí** integrace aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jak chcete uživatelům přihlášení do popředí** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_03.png)

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno Pokud chcete nakonfigurovat aplikaci **IDP, které iniciuje režimu**, proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_04.png)

    na. **Identifikátor URI** do textového pole zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.frontapp.com`

    b. V textovém poli **Odpovědět URL** zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.frontapp.com/sso/saml/callback`

    c. Klikněte na tlačítko **Další**

4. Pokud chcete nakonfigurovat aplikaci **SP, které iniciuje režimu** na stránce **Konfigurovat nastavení aplikace** dialogové okno, klikněte na příkaz **"Zobrazit rozšířená nastavení (volitelné)"** zadejte **Znaménko na adresu URL** a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_05.png)

    na. **Přihlaste se na adresu URL** do textového pole zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.frontapp.com`

    b. Klikněte na tlačítko **Další**

    > [AZURE.NOTE] Upozorňujeme, že nejsou skutečnou hodnotou. Budete muset aktualizovat tyto hodnoty skutečné znaménko na adresu URL, identifikátor a adresu URL odpověď. Jak tyto hodnoty podrobnosti naleznete v **kroku 12** nebo kontaktu můžete přední prostřednictvím [support@frontapp.com](emailTo:support@frontapp.com).

5. Na stránce **konfigurovat jednotné přihlašování v přední** proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_06.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.

6. Přihlášení k vašemu tenantovi přední jako správce.

7. Přejděte na **nastavení (ozubené kolo ikona v dolní části Levý postranní panel) > Předvolby**.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

8. Klikněte na odkaz **Jednotné přihlašování** .

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

9. Vyberte **SAML** v rozevíracím seznamu **Jednotné přihlašování**.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

10. **Vstupní bod** textového pole vložte hodnotu **Jedné přihlašování adresy URL služby** ze Průvodce konfigurací Azure AD aplikace.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

11. Zkopírujte obsah souboru stažený certifikát a potom je vložte do textového pole **podpisového certifikátu** .

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

12. Potvrďte, že tyto adresy URL neodpovídá konfiguraci v kroku 3.

    ![Konfigurace straně jednoho přihlašování v aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

13. Klikněte na tlačítko **Uložit** .

14. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

15. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-front-tutorial/create_aaduser_09.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-front-tutorial/create_aaduser_05.png)

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-front-tutorial/create_aaduser_06.png)

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-front-tutorial/create_aaduser_07.png)

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-front-tutorial/create_aaduser_08.png)

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-front-test-user"></a>Vytvoření uživatele test dopředu

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon Front.Please práci se svým týmem podpory přední a přidejte uživatele do popředí účtu.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup do popředí.
    
![Přiřazení uživatele][200]

**Přiřazení Britta Simon do popředí, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
    
    ![Přiřazení uživatele][201]

2. V seznamu aplikací vyberte **přední**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_50.png)

1. V nabídce na horní klikněte na **uživatele**.
    
    ![Přiřazení uživatele][203]

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.
    
    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.
 
Po kliknutí na dlaždici dopředu na panelu aplikace Access můžete by měla získat automaticky přihlášení z přední aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-front-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-front-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-front-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-front-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-front-tutorial/tutorial_general_205.png
