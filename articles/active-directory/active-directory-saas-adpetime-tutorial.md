<properties
    pageTitle="Kurz: Azure Active Directory integrace s ADP eTime | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ADP eTime."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Kurz: Azure Active Directory integrace s ADP eTime

Cílem tohoto kurzu je předvedení ADP eTime integrace se službou Azure Active Directory (Azure AD).  
Integrace ADP eTime s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k ADP eTime
- Povolení uživatelům, aby automaticky získat přihlášení z k ADP eTime (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure


Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí ADP eTime, musíte následující položky:

- Předplatné Azure AD
- ADP eTime jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání ADP eTime z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-adp-etime-from-the-gallery"></a>Přidání ADP eTime z Galerie
Pokud chcete nakonfigurovat integraci ADP eTime do Azure AD, potřebujete přidat ADP eTime z Galerie do seznamu spravované SaaS aplikace.

**Přidání ADP eTime z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **ADP eTime**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)

7. V podokně výsledků vyberte **ADP eTime**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí ADP eTime založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému ADP eTime uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v ADP eTime jinými slovy, je nutné stanovit.  
Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v ADP eTime.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s ADP eTime, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření ADP eTime otestovat uživatele](#creating-a-adpetime-test-user)** – mít protějšek Britta Simon v eTime ADP, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci eTime ADP.

Aplikace eTime ADP očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace SAML tokenu atributy. Následující obrázek ukazuje příklad pro tuto. Název deklarace vždycky **"PersonImmutableID"** a hodnoty, které jsme změnili na ExtensionAttribute2, která obsahuje EmployeeID uživatele. Tady popředí mapování uživatele Azure AD pro ADP eTime bude možné provést číslo zaměstnance, ale je to namapovat na jinou hodnotu také podle nastavení aplikace. Takže prosím práce s ADP eTime týmu nejdřív použijte správné identifikátor uživatele a zmapovat daná hodnota s deklaraci **"PersonImmutableID"** .  

![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

Před můžete nakonfigurovat výrazu SAML, budete muset kontaktovat tým podpory eTime ADP a požádat o přínosu atribut jedinečný identifikátor pro vašeho klienta. Budete muset tuto hodnotu pro nastavení vlastních nárok na aplikace.


**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s ADP eTime, proveďte následující kroky:**

1. Na portálu na stránce **ADP eTime** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k ADP eTime** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno takto:.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 


    na. V textovém poli **Odpovědět URL** zadejte adresu URL používá uživatelů k přihlašování k aplikaci eTime ADP pomocí následujícího vzorce: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.

    b. Klikněte na tlačítko **Další**.

4. Na stránce **konfigurovat jednotné přihlašování v ADP eTime** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.

    b. Klikněte na tlačítko **Další**.


5. Jednotné přihlašování nakonfigurován pro aplikaci, kontaktujte tým podpory eTime ADP a e-mailu připojit staženého souboru metadata, tak, aby je možné konfigurovat integrace jednotného přihlašování.

    > [AZURE.NOTE] Po **ADP eTime** týmu nakonfigurovat instance, si hodnotu **RelayState** z nich. Postupujte podle níže uvedené kroky ji nakonfigurovat. Po této konfiguraci můžete otestovat integrace. Takže Poznámka: Toto je důležité konfigurace pro integraci aplikace pro práci.

6. Abyste mohli nakonfigurovat hodnotu RelayState v Azure AD, proveďte následující kroky: 
    
    na. Přihlášení k [Portálu Správa Azure](https://portal.azure.com) jako správce.

    b. V levém navigačním podokně klikněte na **Další služby**. 
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)

    c. Do textového pole **Hledat** zadejte **Azure Active Directory**a klikněte na odkaz na související.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)

    d. Klikněte na **podnikových aplikací**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)

    e. V části **Spravovat** klikněte na **Všechny aplikace**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)

    f. Do textového pole **Hledat** zadejte **ADP eTime**a klikněte na odkaz na související. 
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)

    g. V části **Spravovat** klikněte na **jednotné přihlašování**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)

    h. Zaškrtněte políčko **Zobrazit rozšířené nastavení adresy URL**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
    
    Můžu. **Stav směrování** do textového pole zadejte hodnotu pomocí následující vzorce:
    
    - Provozním prostředí:`https://fed.adp.com/saml/fedlanding.html?<id>` 
    - Pracovní prostředí:`https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)

    j. Uložení nastavení.

7. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][10]

8. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Azure AD jednotné přihlášení][11]



### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.  
V seznamu uživatelů zvolte **Britta Simon**.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 

    na. Jako **Typ uživatele**vyberte **nového uživatele ve vaší organizaci**.

    b. Do textového pole **Uživatelské jméno** zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasného hesla** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-adp-etime-test-user"></a>Vytvoření uživatele ADP eTime test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v ADP eTime. Zkontrolujte pracujte se na tým podpory eTime ADP přidáte uživatele v okně ADP eTime účet. 


> [AZURE.NOTE]Pokud je potřeba ručně vytvořit uživatelem, budete muset kontaktovat tým podpory eTime ADP.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k ADP eTime.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon ADP eTime, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **ADP eTime**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici eTime ADP na panelu aplikace Access můžete by měla získat automaticky přihlášení na aplikaci eTime ADP.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png
