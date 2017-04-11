<properties
    pageTitle="Kurz: Azure Active Directory integrace s Lifesize cloudu | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Lifesize cloudu."
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Kurz: Azure Active Directory integrace s Lifesize cloudu

V tomto kurzu se naučíte, jak integrovat Lifesize cloudu s Azure Active Directory (Azure AD).

Integrace Lifesize cloudu s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup ke cloudu Lifesize
- Povolení uživatelům, aby automaticky získat přihlášení z cloudu Lifesize (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Lifesize cloudu, musíte následující položky:

- Předplatné Azure AD
- Cloudu Lifesize jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Lifesize cloudu z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-lifesize-cloud-from-the-gallery"></a>Přidání Lifesize cloudu z Galerie
Pokud chcete nakonfigurovat integraci Lifesize cloudu do Azure AD, potřebujete přidat Lifesize cloudu z Galerie do seznamu spravované SaaS aplikace.

**Přidání Lifesize cloudu z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]
2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Lifesize cloudu**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. V podokně výsledků vyberte **Lifesize cloudu**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části můžete nakonfigurovat a testovací Azure AD jednotné přihlašování s Lifesize cloudu založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek v cloudu Lifesize je pro uživatele v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v cloudu Lifesize jinými slovy, je nutné stanovit.

Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v cloudu Lifesize.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Lifesize cloudu, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Obláčkem Lifesize testování uživatele](#creating-a-lifesize-cloud-test-user)** – mít protějšek Britta Simon v cloudu Lifesize, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci Lifesize cloudu.


**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s Lifesize cloudu, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **Lifesize cloudu** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce **jakým způsobem uživatelé přihlásit k Lifesize cloudu** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Lifesize cloudu pomocí následujícího vzorce: **https://login.lifesizecloud.com/ls/?acs**.
    
    b. Klikněte na tlačítko **Další**
 
4. Na stránce **konfigurovat jednotné přihlašování v cloudu Lifesize** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    b. Klikněte na tlačítko **Další**.


5. Chcete-li získat SSO nakonfigurován pro aplikaci přihlášení do aplikace Lifesize cloudu s oprávněními správce.

6. V pravém horním rohu klikněte na své jméno a potom klikněte na **Upřesňující nastavení**

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. V části kliknutím nyní upřesňující nastavení na odkaz **Nastavení jednotného přihlašování** . Tím se otevře na stránce Konfigurace jednotného přihlašování pro instanci aplikace.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. Konfigurace jednotného přihlašování uživatelského rozhraní nakonfigurujte následující hodnoty.    

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • Zkopírujte hodnotu Vystavitel URL z Azure AD a text vložte do textového pole **Vystavitel poskytovatele Identity** .

    • Zkopírujte hodnotu adresa URL vzdálené přihlášení z Azure AD a text vložte do textového pole **Přihlašovací adresa URL** .

    • Otevřete stažený certifikát v poznámkovém bloku a kopírování obsahu certifikát, bez řádků zahájení a ukončení osvědčení, vložte takto do textového pole **X.509 certifikát** .

    • V mapování SAML atributu textového pole **jméno** zadejte hodnotu jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**

    • V mapování SAML atribut **Příjmení** textového pole zadejte hodnotu jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    • V mapování SAML atributu textového pole **e-mailu** zadejte hodnotu jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. Kontrola konfigurace kliknete na tlačítko **Testovat** .

    > [AZURE.NOTE] Pro úspěšné testování musíte dokončit Průvodce konfigurací v Azure AD a taky poskytují přístup uživatelům nebo skupinám, kteří mohou provádět test.
    
10. Povolte jednotné přihlašování kontrolou na tlačítko **Povolit jednotné přihlašování** .

11. Nyní klikněte na tlačítko **Update** tak, aby se všechna nastavení uloží. To vygeneruje hodnotu RelayState. Zkopírujte hodnotu RelayState, který je vytvořen do textového pole. Potřebujeme tuto hodnotu v dalším krokům.

12. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

13. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
 
    ![Azure AD jednotné přihlášení][11]

14. Teď přihlášení do portálu Správa Azure **https://portal.azure.com** pomocí přihlašovacích údajů správce

15. Klikněte na odkaz **Další služby** v levém navigačním podokně
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Vyhledávání Azure Active Directory a klikněte na odkaz **Azure Active Directory**
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. Bude hledat všechny aplikace SaaS pod tlačítkem **Podnikových aplikací** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. Teď kliknou na odkaz pro **Všechny aplikace** v dalším zásuvné
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. Hledání aplikace Lifesize, u kterého chcete nastavit RelayState. 
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. Nyní klikněte na odkaz **jednotné přihlašování** v zásuvné

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. Zobrazí se zaškrtněte políčko **Zobrazit rozšířená nastavení adresy URL** . Zaškrtněte políčko.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. Nakonfigurujte RelayState aplikace, která se zobrazí na stránce Lifesize jednotného přihlašování k aplikaci konfigurace. 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. Uložení nastavení.

### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.


![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:  ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-an-lifesize-cloud-test-user"></a>Vytvoření uživatele test Lifesize cloudu

V této části vytvoříte uživatele s názvem Britta Simon v cloudu Lifesize. Lifesize cloud podporuje zřizování automatické uživatele. Po úspěšném ověření v Azure AD bude automaticky vytvořena uživatele v aplikaci. 


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělit přístup do cloudu Lifesize.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Lifesize cloudu, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Lifesize cloudu**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Lifesize cloudu na panelu aplikace Access můžete by měla získat automaticky přihlášení z cloudu Lifesize aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
