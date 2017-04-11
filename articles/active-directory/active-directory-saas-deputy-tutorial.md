<properties
    pageTitle="Kurz: Azure Active Directory integrace s zástupce | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a zástupce."
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
    ms.date="09/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Kurz: Azure Active Directory integrace s zástupce

Cílem tohoto kurzu je vidíte, jak integrovat zástupce s Azure Active Directory (Azure AD).

Integrace zástupce s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k zástupce
- Povolení uživatelům, aby automaticky získat přihlášení z na zástupce (jednotného přihlašování) s jejich Azure AD účty
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí zástupce, musíte následující položky:

- Předplatné Azure AD
- Zástupce jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
Cílem tohoto kurzu je umožňují testování Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání zástupce z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-deputy-from-the-gallery"></a>Přidání zástupce z Galerie
Abyste mohli nakonfigurovat integraci zástupce do Azure AD, budete muset přidat zástupce z Galerie do seznamu spravované SaaS aplikací.

**Pokud chcete přidat zástupce z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Pokud chcete otevřít zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .
    
    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.
    
    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **zástupce**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_01.png)

7. V panelu Výsledky vyberte **zástupce**a klikněte na **Dokončit** přidat aplikaci.

    ![Výběr aplikace v galerii](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
Cílem tento oddíl je ukazují, jak konfigurovat a otestujte Azure AD jednotné přihlašování pomocí zástupce založeného na uživateli testu s názvem "Britta Simon".

Azure AD pro jednotné přihlašování pracovat, musí ví, co je databáze odpovídajících funkcí pro uživatele systému zástupce uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v zástupce jinými slovy, je nutné stanovit.

Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v zástupce.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s zástupce, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření zástupce testování uživatele](#creating-a-deputy-test-user)** – a zástupce, která je propojená s Azure AD znázornění jí protějšek Britta Simon.
4. **[Přiřazení Azure AD otestujte uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci zástupce.

**Pokud chcete nakonfigurovat Azure AD jednotné přihlašování s zástupce, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace **zástupce** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce, **jakým způsobem uživatelé přihlásit k zástupce** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_03.png)

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno Pokud chcete nakonfigurovat aplikaci **IDP, které iniciuje režimu**, proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_04.png)

    na. **Identifikátor URI** do textového pole, zadejte adresu URL pomocí následujícího vzorce: `https://<your-subdomain>.<region>.deputy.com`.

    b. V textovém poli **Odpovědět URL** zadejte adresu URL pomocí následujícího vzorce: `https://<your-subdomain>.<region>.deputy.com/exec/devapp/samlacs`.

    c. Klikněte na tlačítko **Další**.

4. Pokud chcete nakonfigurovat aplikaci **SP, které iniciuje režimu** na stránce **Konfigurovat nastavení aplikace** dialogové okno, klikněte na příkaz **"Zobrazit rozšířená nastavení (volitelné)"** zadejte **Znaménko na adresu URL** a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_05.png)

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL pomocí následujícího vzorce: `https://<your-subdomain>.<region>.deputy.com`.

    b. Klikněte na tlačítko **Další**.

    > [AZURE.NOTE] Zástupce oblast přípona je opitional nebo použijte jeden z těchto článků: au | NEDEF | Evropské unie | jako | la | af | | ent au | ent NEDEF | ent eu | ent-jako | ent la | ent af | ent-an, chyby

5. Na stránce **Konfigurace jednotného přihlašování na zástupce** proveďte následující kroky a klikněte na **Další**:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_06.png)

    na. Klikněte na **Stáhnout certifikát**a potom uložit soubor v počítači.

    
6. Přejděte na následující adresu URL: https://(your-subdomain).deputy.com/exec/config/system_config. Přejděte na **Nastavení zabezpečení** a klikněte na **Upravit**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

7. Na portálu v konfigurovat jednotné přihlašování na stránce zástupce Azure klasické zkopírujte adresu URL SAML jednotné přihlašování. 

8. Na této stránce **Nastavení zabezpečení** proveďte pod kroky.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)

    na. Povolte **sociální přihlášení**.

    b. Otevřete ve formátu Base 64 zakódovaný certifikát v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte ho do textového pole **OpenSSL certifikát** .

    c. Adresa URL SAM jednotné přihlašování do textového pole zadejte`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. V textovém poli URL jednotné přihlašování SAM nahraďte `<your subdomain>` s vaší subdoménu.

    e. V textovém poli URL jednotné přihlašování SAM nahraďte `<saml sso url>` s adresou URL jednotné přihlašování SAML jste zkopírovali z portálu Microsoft Azure klasické.

    f. Klikněte na **Uložit nastavení**.

9. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

10. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
    ![Azure AD jednotné přihlášení][11]

### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
Cílem tento oddíl je vytvoření uživatele test na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-deputy-tutorial/create_aaduser_09.png)

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png)

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png)

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-deputy-tutorial/create_aaduser_05.png)

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-deputy-tutorial/create_aaduser_06.png)

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-deputy-tutorial/create_aaduser_07.png)

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-deputy-tutorial/create_aaduser_08.png)

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-deputy-test-user"></a>Vytvoření zástupce zkušební uživatele

Pokud chcete povolit uživatelům Azure AD pro přihlášení na zástupce, musí být zřízení do zástupce. V případě zástupce zřizování se ručně naplánovaný úkol.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Zřízení uživatelský účet, proveďte následující kroky:

1.  Přihlaste se k webu společnosti zástupce jako správce.

2.  Na horním navigačním podokně klikněte na **lidé**.

    ![Lidé] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "Lidé")

3.  Klikněte na tlačítko **Přidat lidi** a klikněte na **Přidat jedna osoba**.

    ![Přidání lidí] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Přidání lidí")

4.  Proveďte následující kroky a klikněte na **Uložit a pozvat**.

    ![Nového uživatele] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "Nového uživatele")

    na. Do textového pole **název** zadejte **Britta** a **Simon**.  

    b. **E-mailu** do textového pole zadejte e-mailovou adresu, kterou chcete zřízení účtu Azure AD.

    c. **Práce ve** do textového pole zadejte název bussniess.

    d. Klikněte na tlačítko **Uložit a pozvat** .

    >[AZURE.NOTE]Majitele účtu AAD dostávat e-mailu a potvrďte svůj účet, než se aktivuje, klepnutím na odkaz. Můžete použít jakékoli další zástupce uživatelského účtu vytváření nástroje nebo rozhraní API poskytovanou zástupce k poskytování AAD uživatelským účtům.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

Cílem tento oddíl je povolení Britta Simon pomocí Azure jednotného přihlašování udělení přístup zástupce.
    
![Přiřazení uživatele][200]

**Pokud chcete přiřadit Britta Simon zástupce, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.
    
    ![Přiřazení uživatele][201]

2. V seznamu aplikací vyberte **zástupce**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_50.png)

3. V nabídce na horní klikněte na **uživatele**.
    
    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.
    
    ![Přiřazení uživatele][205]

### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.
 
Po kliknutí na dlaždici zástupce na panelu aplikace Access můžete by měla získat automaticky přihlášení na zástupce aplikaci.

## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_205.png
