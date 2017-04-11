<properties
    pageTitle="Kurz: Azure Active Directory integrace s Amazon webové služby AWS | Microsoft Azure"
    description="Naučte se používat Amazon webové služby AWS s Azure Active Directory povolit jednotné přihlašování, automatizované zřizování a další!"
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


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Kurz: Azure Active Directory integrace s Amazon webové služby AWS

Cílem tohoto kurzu je vidíte, jak integrovat Amazon webové služby AWS s Azure Active Directory (Azure AD).  
Integrace Amazon webové služby AWS s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo bude mít přístup k Amazon webové služby AWS 
- Povolení uživatelům, aby automaticky získat přihlášení z pro Amazon webové služby AWS (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat integraci Azure AD pomocí Amazon webové služby AWS, musíte následující položky:

- Předplatné Azure AD
- Amazon webové služby AWS jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze tří hlavních stavebních bloků:

1. Přidání Amazon webové služby AWS z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Přidání Amazon webové služby AWS z Galerie
Pokud chcete nakonfigurovat integraci z Amazon webové služby AWS do Azure AD, potřebujete přidat Amazon webové služby AWS z Galerie do seznamu spravované SaaS aplikace.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>Chcete-li přidat Amazon webové služby AWS z galerie, proveďte následující kroky:

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1] 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** . 
   
    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky. 
   
    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**. 
   
    ![Aplikace][4]

6. Do pole Hledat zadejte **Amazon webové služby AWS**.
   
    ![Aplikace][5]

7. V podokně výsledků vyberte **Amazon webové služby AWS**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Aplikace][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování s Amazon webové služby AWS založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je protějšek uživatelům v Amazon webové služby AWS uživatele v Azure AD. Odkaz relace mezi Azure Active Directory a související uživatelem v Amazon webové služby AWS jinými slovy, je nutné stanovit.  
Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Amazon webové služby AWS.
 
Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Amazon webové služby AWS, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednoho jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření Amazon webové služby AWS otestujte uživatele](#creating-a-halogen-software-test-user)** – mít protějšek Britta Simon v Amazon webové služby AWS, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurace Azure AD jednoduchým jednotné přihlašování

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Amazon webové služby AWS.  
Aplikace Amazon webové služby AWS očekává výrazy SAML v určitém formátu, které budete požádáni o přidejte mapování vlastních atributů konfigurace **saml tokenu atributy** . Následující obrázek ukazuje příklad pro tuto.


![Konfigurace jednotného přihlašování][27]

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s Amazon webové služby AWS, proveďte následující kroky:**

1. Na portálu na stránce **Amazon webové služby AWS** aplikace integrace Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][7]

2. Na stránce **jakým způsobem uživatelé přihlásit k Amazon webové služby AWS** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování][8]

3. Na stránce **Konfigurovat nastavení aplikace** dialogového okna klikněte na další. 

    ![Konfigurace nastavení aplikace][9]
 
4. Na stránce **konfigurovat jednotné přihlašování v Amazon webové služby AWS** klikněte na tlačítko **Stáhnout metadat**a uložte soubor metadat místně na vašem počítači.

    ![Konfigurace jednotného přihlašování][10]

5. V jiném okně prohlížeče přihlášení k vašemu webu společnosti Amazon webové služby AWS jako správce.

6. Klikněte na **Domů konzoly**.

    ![Konfigurace jednotného přihlašování][11]

7. Klikněte na **identit a správu přístupu**. 

    ![Konfigurace jednotného přihlašování][12]

8. Klikněte na **Zprostředkovatelé identit jiní**a potom klikněte na **Vytvořit poskytovatele**. 

    ![Konfigurace jednotného přihlašování][13]

9. Na stránce dialogové okno **Konfigurovat zprostředkovatele** proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování][14]

     na. Jako **Typ zprostředkovatele**vyberte **SAML**.

     b. Do textového pole **Název poskytovatele** zadejte název poskytovatele (například: *uvedené*).

     c. Nahrajte soubor stažený metadata, klikněte na **Zvolit soubor**.

     d. Klikněte na **Další krok**.


10. Na stránce **Ověření informace o poskytovatelích** dialogového okna klikněte na **vytvořit**. 

    ![Konfigurace jednotného přihlašování][15]

11. Klikněte na položku **role**a potom klikněte na **Vytvořit novou roli**. 

    ![Konfigurace jednotného přihlašování][16]

12. V dialogovém okně **Pojmenovat Role** proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování][17]

    na. Do textového pole **Role název** zadejte název role (například: *TestUser*).

    b. Klikněte na **Další krok**.

13. V dialogovém okně **Vybrat typ Role** proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování][18]

    na. Vyberte **roli připojovat poskytovatele Identity**.

    b. V části **přístup udělit Web jednotné přihlašování (WebSSO) poskytovatelům SAML** klikněte na **Výběr**.


14. V dialogovém okně **Vytvořit důvěřovat** proveďte následující kroky:  

    ![Konfigurace jednotného přihlašování][19]
     
     na. Jako SAML poskytovatele vyberte poskytovatele SAML nevytvoříte previousley (například: *uvedené*) 

     b. Klikněte na **Další krok**.


15. V dialogovém okně **Ověření Role zabezpečení** klikněte na **Další krok**. 

    ![Konfigurace jednotného přihlašování][32]


16. V dialogovém okně **Připojení zásady** klikněte na **Další krok**.  

    ![Konfigurace jednotného přihlašování][33]


17. V dialogovém okně **Kontrola** proveďte následující kroky:   

    ![Konfigurace jednotného přihlašování][34]

     na. Zkopírujte hodnotu **Role informace** .

     b. Zkopírujte hodnotu informace **Důvěryhodné entity** .

     c. Klikněte na **vytvořit Role**. 

18. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.

    ![Co je Azure AD Connect][20]

19. Na stránce **potvrzení přihlašování** klikněte na **Dokončit** zavřete dialogové okno **Konfigurovat jednotného přihlašování** .

    ![Co je Azure AD Connect][22]


20. V nabídce nahoře klikněte na **atributy** otevřete dialogové okno **SAML tokenu atributy** . 

    ![Konfigurace jednotného přihlašování][21]

21. Klikněte na **Přidat uživatele atribut**. 

    ![Konfigurace jednotného přihlašování][23]

22. V dialogovém okně Přidat uživatele atribut proveďte následující kroky. 

    ![Konfigurace jednotného přihlašování][24] 

    na. Do textového pole **Název atribut** zadejte **https://aws.amazon.com/SAML/Attributes/Role**.

    b. **Hodnota atributu** do textového pole, zadejte **[Role informace hodnota], [hodnotu důvěryhodné informace Entity]**.

    >[AZURE.TIP] Toto jsou hodnoty, které jste zkopírovali v dialogovém okně Kontrola po vytvoření vaše role. 

    c. Klikněte na **Dokončit** zavřete dialogové okno **Přidat atribut uživatele** .

23. Klikněte na **Přidat uživatele atribut**. 

    ![Konfigurace jednotného přihlašování][23]


24. V dialogovém okně Přidat uživatele atribut proveďte následující kroky. 

    ![Konfigurace jednotného přihlašování][25]


     na. Do textového pole **Název atributu** zadejte **https://aws.amazon.com/SAML/Attributes/RoleSessionName**.

     b. **Hodnota atributu** do textového pole, zadejte nebo vyberte **user.userprincipalname** z rozevíracího seznamu.
     
    ![Konfigurace jednotného přihlašování][35]
    

     c. Klikněte na **Dokončit** zavřete dialogové okno **Přidat atribut uživatele** .


25. Klikněte na **použít změny**. 

    ![Konfigurace jednotného přihlašování][26]





### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test

Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.

![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.
  2. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.
  3. Klikněte na další.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. V **Příjmení** txtbox, typ, **Simon**.
  
    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.
  
    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.
  
    b. Klikněte na **dokončení**.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Vytvoření uživatele Amazon webové služby AWS test

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon v Amazon webové služby AWS.

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Vytvoření uživatele s názvem Britta Simon v Amazon webové služby AWS, proveďte následující kroky:

1. Přihlaste se k vašemu webu společnosti **Amazon webové služby AWS** jako správce.

2. Klikněte na ikonu **Domů konzoly** . 

    ![Konfigurace jednotného přihlašování][11]

3. Klikněte na identitu a přístup k řízení. 

    ![Konfigurace jednotného přihlašování][28]

4. Na řídicím panelu klikněte na uživatele a potom klikněte na vytvořit nového uživatele. 

    ![Konfigurace jednotného přihlašování][29]

5. V dialogovém okně Vytvořit uživatele proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování][30]

     na. V textových polích **Zadejte jména uživatelů** zadejte v Azure AD Brita Simon uživatelské jméno (userprincipalname).

     b. Klikněte na **vytvořit**.




### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup k Amazon webové služby AWS.

![Přiřazení uživatele][31]

**Pokud chcete přiřadit Britta Simon CloudPassage, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][26]

2. V seznamu aplikací vyberte **Amazon webové služby AWS**.

    ![Přiřazení uživatele][27]

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][25]

1. V seznamu uživatelů zvolte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][29]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici Amazon webové služby AWS na panelu aplikace Access můžete by měla získat automaticky přihlášení z Amazon webové služby AWS aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















