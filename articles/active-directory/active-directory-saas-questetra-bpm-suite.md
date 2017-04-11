<properties
    pageTitle="Kurz: Azure Active Directory integrace s Questetra BPM sadu | Microsoft Aure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Questetra BPM sadu."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Kurz: Azure Active Directory integrace s Questetra BPM Suite

Cílem tohoto kurzu je ukazují, jak integrovat sadu BPM Questetra s Azure Active Directory (Azure AD).  
Integrace Questetra BPM sadu s Azure AD poskytuje následující výhody: 

- Můžete určit v Azure AD, kdo má přístup k sadě BPM Questetra 
- Povolení uživatelům, aby automaticky získat přihlášení na sadě BPM Questetra (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro 

Abyste mohli nakonfigurovat Azure AD integrace s Questetra BPM sady, musíte následující položky:

- Předplatné Azure AD
- [Sady BPM Questetra](https://senbon-imadegawa-988.questetra.net/) jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.  
Scénář uvedené v tomto kurzu se skládá ze tří hlavních stavebních bloků:

1. Přidání Questetra BPM sadu z Galerie 
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Přidání Questetra BPM sadu z Galerie
Abyste mohli nakonfigurovat integraci Questetra BPM sady do Azure AD, potřebujete přidat sadu BPM Questetra z Galerie do seznamu spravované SaaS aplikace.

**Přidat sadu BPM Questetra z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Questetra BPM sadu**.

    ![Aplikace][5]

7. V podokně výsledků vyberte **Questetra BPM sady**a pak klikněte na **Dokončit** přidat aplikaci.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí Questetra BPM sady na základě zkušební uživatele s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co uživatel protějšek sady BPM Questetra uživateli v Azure AD je. Odkaz relace mezi Azure AD uživatel a související sady BPM Questetra jinými slovy, je nutné stanovit.  
Tento odkaz vztah vznikne přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v sadě BPM Questetra.
 
Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Questetra BPM sady, musíte provést následující stavebních bloků:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytvoření sady BPM Questetra otestovat uživatele](#creating-a-questetra-bpm-suite-test-user)** – mít protějšek Britta Simon BPM sady Questetra, který je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlášení

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci sady BPM Questetra.

**Nakonfigurovat Questetra BPM sadu Azure AD jednotné přihlašování, proveďte následující kroky:**

1. Na portálu na stránce integrace aplikace **Sady BPM Questetra** Azure klasické klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][8]

2. Na stránce **jakým způsobem uživatelé přihlásit k sadě BPM Questetra** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Azure AD jednotné přihlášení][9]


3. V okně různých webového prohlížeče Přihlaste se k webu společnosti **Questetra BPM sadu** jako správce.

4. V nabídce nahoře klikněte na **Nastavení systému**. 

    ![Azure AD jednotné přihlášení][10]

5. Otevřete stránku **SingleSignOnSAML** , klikněte na **Jednotné přihlašování SAML ()**. 

    ![Azure AD jednotné přihlášení][11]


6. Na stránce **Konfigurovat nastavení aplikace** dialogové okno Azure klasické portálu, proveďte následující kroky: 

    ![Konfigurace nastavení aplikace][13]
 
    na. Na můžete webu společnosti **Questetra BPM sadu** , klikněte v části informace o SP zkopírujte adresu **ACS URL**a potom je vložte do textového pole **Symbol na adresu URL** .

    b. Na můžete webu společnosti **Questetra BPM sadu** , klikněte v části informace o SP zkopírujte **Entity ID**a potom je vložte do textového pole **Adresa URL vydavatel** .

    c. Na můžete webu společnosti **Questetra BPM sadu** , klikněte v části informace o SP zkopírujte adresu **ACS URL**a potom je vložte do textového pole **Adresa URL odpověď** .

    d. Klikněte na tlačítko **Další**.

 
7. Na stránce **konfigurovat jednotné přihlašování v sadě BPM Questetra** klikněte na tlačítko **Stáhnout certifikát**a uložte soubor certifikátu místně na vašem počítači.

    ![Konfigurace jednotného přihlašování][14]


8. Na můžete **Questetra BPM sadu** společnosti proveďte následující kroky: 

    ![Konfigurace jednotného přihlašování][15]

    na. Zaškrtněte políčko **Povolit jednotného přihlašování**.
     
    b. Na portálu Azure klasické zkopírujte hodnotu **Vystavitel URL** a potom je vložte do textového pole **Entity ID** .

    c. Na portálu Azure klasické zkopírujte hodnotu **Jedné přihlašování adresy URL služby** a potom je vložte do textového pole **Přihlašovací adresa URL stránky** .

    d. Na portálu Azure klasické zkopírujte hodnotu **Jedné adresy URL služby Sign-Out** a potom je vložte do textového pole **Adresa URL odhlašovací stránky** .

    e. **Formát NameID** do textového pole, zadejte **urn: oasis: názvy: tc: SAML:1.1:nameid-formát: emailAddress**.


    f. Vytvoření souboru zakódovaný základu-64 z stažený certifikát. 

    >[AZURE.TIP] Další podrobnosti najdete v tématu [jak převést binární certifikátu do textového souboru](http://youtu.be/PlgrzUZ-Y1o).

    g. Otevření zakódovaný certifikátu základu-64 v poznámkovém bloku, zkopírujte obsah ho do schránky a potom je vložte do textového pole **ověřovací certifikát** . 

    h. Klikněte na **Uložit**.


9. Na portálu Azure klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**. 

    ![Co je Azure AD Connect][17]


10. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  

    ![Co je Azure AD Connect][18]




### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu Azure klasické s názvem Britta Simon.

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvořit Azure AD zkušební uživatele][100] 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvořit Azure AD zkušební uživatele][101] 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**. 

    ![Vytvořit Azure AD zkušební uživatele][102] 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvořit Azure AD zkušební uživatele][103]
 
    na. Jako **Typ uživatele**vyberte **nového uživatele ve vaší organizaci**.
  
    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na další.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: 

    ![Vytvořit Azure AD zkušební uživatele][104] 
  
    na. Do textového pole **jméno** zadejte **Britta**. 
 
    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvořit Azure AD zkušební uživatele][105]  

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvořit Azure AD zkušební uživatele][106]   
  
    na. Poznamenejte si hodnotu **Nové heslo**.
  
    b. Klikněte na **dokončení**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Vytvoření uživatele test Questetra BPM Suite

Cílem tento oddíl je vytvoření uživatele s názvem Britta Simon sady BPM Questetra.

**Vytvoření uživatele s názvem Britta Simon sady BPM Questetra, proveďte následující kroky:**

1.  Přihlášení k vašemu webu společnosti Questetra BPM sadu jako správce.
2.  Přejděte na **Nastavení systému > seznam uživatelů > Nový uživatel**. 
3.  V dialogovém okně Nový uživatel proveďte následující kroky: 

    ![Vytvořte zkušební uživatele][300] 

    na. Do textového pole **název** zadejte uživatelské jméno na Britta v Azure AD.

    b. **E-mailu** do textového pole zadejte jeho Britta uživatelské jméno v Azure AD.

    c. Do textového pole **heslo** zadejte heslo.

4.  Klikněte na **Přidat nového uživatele**.



### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělit přístup Questetra BPM sadě.

![Co je Azure AD Connect][200]

**Přiřazení Britta Simon Questetra BPM sadě, proveďte následující kroky:**

1. Na portálu Azure klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Co je Azure AD Connect][201]

2. V seznamu aplikací vyberte **Questetra BPM sadu**.

    ![Co je Azure AD Connect][205]

1. V nabídce na horní klikněte na **uživatele**.

    ![Co je Azure AD Connect][202]

1. V seznamu uživatelů zvolte **Britta Simon**.

    ![Co je Azure AD Connect][203]

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Co je Azure AD Connect][204]



### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.  
Po kliknutí na dlaždici Questetra BPM sady na panelu aplikace Access můžete by měla získat automaticky přihlášení na aplikaci Questetra BPM sadu.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 