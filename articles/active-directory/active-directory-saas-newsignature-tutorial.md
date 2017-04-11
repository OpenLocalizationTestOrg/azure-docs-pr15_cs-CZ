<properties
    pageTitle="Kurz: Azure Active Directory integrace s portálem Správa cloudu pro Microsoft Azure | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a portálu Správa cloudu pro Microsoft Azure."
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


# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Kurz: Azure Active Directory integrace s portálem Správa cloudu pro Microsoft Azure

Cílem tohoto kurzu je vidíte, jak integrovat portálu Správa cloudu pro Microsoft Azure pomocí služby Azure Active Directory (Azure AD).  
Integrace portálu Správa cloudu pro Microsoft Azure s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k portálu Správa cloudu pro Microsoft Azure
- Povolení uživatelům, aby automaticky získat podepsané aktivní na portálu Správa cloudu pro Microsoft Azure (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure


Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí portálu Správa cloudu pro Microsoft Azure, musíte následující položky:

- Předplatné Azure AD
- Portálu Správa cloudu pro Microsoft Azure jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání portálu Správa cloudu pro Microsoft Azure z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a>Přidání portálu Správa cloudu pro Microsoft Azure z Galerie
Pokud chcete nakonfigurovat integraci portálu Správa cloudu pro Microsoft Azure do Azure AD, potřebujete přidat portálu Správa cloudu pro Microsoft Azure z Galerie do seznamu spravované SaaS aplikace.

**Přidání portálu Správa cloudu pro Microsoft Azure z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Portálu Správa cloudu pro Microsoft Azure**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_01.png)

7. V podokně výsledků vyberte **Portálu Správa cloudu pro Microsoft Azure**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí portálu Správa cloudu pro Microsoft Azure založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele v portálu pro správu cloudu pro Microsoft Azure uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v portálu pro správu cloudu pro Microsoft Azure jinými slovy, je nutné stanovit.  
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v portálu pro správu cloudu pro Microsoft Azure.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování pomocí portálu Správa cloudu pro Microsoft Azure, musíte provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření portálu Správa cloudu pro Microsoft Azure otestujte uživatele](#creating-a-newsignature-test-user)** – mít protějšek Britta Simon v portálu pro správu cloudu pro Microsoft Azure, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v portálu správy cloudu pro aplikaci Microsoft Azure.



**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování pomocí portálu Správa cloudu pro Microsoft Azure, proveďte následující kroky:**

1. Na portálu na stránce **Portálu Správa cloudu pro Microsoft Azure** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k portálu Správa cloudu pro Microsoft Azure** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno takto:.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole zadejte adresu URL používané uživatelům přihlášení k portálu Správa cloudu pro aplikaci Microsoft Azure pomocí následujícího vzorce:`https://portal.igcm.com/<instance name>`

    b. Klikněte na tlačítko **Další**.


4. Na stránce **konfigurovat jednotné přihlašování v portálu pro správu cloudu pro Microsoft Azure** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_05.png) 

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5. Jednotné přihlašování nakonfigurován pro aplikaci získáte obraťte se na portálu správy cloudu pro Microsoft Azure tým podpory na [jczernuszka@newsignature.com](mailTo:jczernuszka@newsignature.com) a e-mailu připojit soubor stažený certifikát. Taky zadejte adresu URL vystavitel, SAML jednotného přihlašování k URL a jeden znak, adresy URL služby tak, aby je možné konfigurovat pro jednotné přihlašování integraci.


6. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

7. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.  

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-newsignature-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Vytvoření portálu Správa cloudu pro uživatele Microsoft Azure test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v portálu pro správu cloudu pro Microsoft Azure. Prosím pracujte s portálem Správa cloudu pro tým podpory na Microsoft Azure přidáte uživatele v portálu pro správu cloudu pro účet Microsoft Azure. 


> [AZURE.NOTE]Pokud je potřeba ručně vytvořit uživatelem, budete muset kontaktovat portálu pro správu cloudu pro tým podpory na Microsoft Azure.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k portálu Správa cloudu pro Microsoft Azure.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon na portálu Správa cloudu pro Microsoft Azure, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Portálu Správa cloudu pro Microsoft Azure**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Když kliknete na portálu Správa cloudu pro Microsoft Azure dlaždice na panelu aplikace Access, můžete by měla získat automaticky přihlášení z na portálu Správa cloudu pro aplikaci Microsoft Azure.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_205.png
