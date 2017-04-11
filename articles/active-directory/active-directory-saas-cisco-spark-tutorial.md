<properties
    pageTitle="Kurz: Azure Active Directory integrace s Cisco Spark | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Cisco Spark."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Kurz: Azure Active Directory integrace s Cisco Spark

V tomto kurzu se naučíte, jak integrovat Cisco Spark s Azure Active Directory (Azure AD).

Integrace Cisco Spark s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Cisco Spark
- Povolení uživatelům, aby automaticky získat přihlášení z k Cisco Spark (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Cisco Spark, musíte následující položky:

- Předplatné Azure AD
- **Cisco Spark** jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí. Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Cisco Spark z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-cisco-spark-from-the-gallery"></a>Přidání Cisco Spark z Galerie
Pokud chcete nakonfigurovat integraci Cisco Spark do Azure AD, potřebujete přidat Cisco Spark z Galerie do seznamu spravované SaaS aplikace.

**Přidat Cisco Spark z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**. 

    ![Služby Active Directory][1]

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Cisco Spark**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. V podokně výsledků vyberte **Cisco Spark**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Cisco Spark založeného na uživateli testu s názvem "Britta Simon".

Pro jednotné přihlašování pracovat musí Azure AD vědět, co uživatel protějšek v Cisco Spark uživateli v Azure AD. Odkaz vztah mezi Azure Active Directory a související uživatele v Cisco Spark jinými slovy, je nutné stanovit.
Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnota **uživatelské jméno** v Cisco Spark. Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Cisco Spark, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
4. **[Vytváření Cisco Spark testování uživatele](#creating-a-cisco-spark-test-user)** – a protějšek Britta Simon Spark Cisco, která je propojená s Azure AD znázornění jí.
5. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

Cílem tento oddíl je povolit Azure AD jednotné přihlašování v portálu Azure klasické a konfigurace jednotného přihlašování v aplikaci Cisco Spark.

Aplikace Spark Cisco očekává výrazy SAML má být konkrétní atributy. Nakonfigurujte následující atributy pro tuto aplikaci. Na kartě **"Atributy"** aplikace můžete spravovat hodnoty tyto atributy. Následující obrázek ukazuje příklad pro tuto.

![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Nakonfigurovat Cisco Spark Azure AD jednotné přihlašování, proveďte následující kroky:**

1. Na portálu na stránce integrace aplikace **Cisco Spark** v nabídce v horní části Azure klasické klepněte na položku **atributy**.
     
    ![Konfigurace jednotného přihlašování][5]


2. V dialogovém okně **tokenu atributy SAML** proveďte následující kroky:

    na. Klikněte na **Přidat uživatele atribut** otevřete dialogové okno **Přidat Attribure uživatele** .

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. Do textového pole **Název atributu** zadejte **uid**.
    
    c. **Hodnota atributu** seznamu vyberte **user.userprincipal**.
    
    d. Klikněte na **dokončení**. Potom **Použít změny** v dolní části stránky.

3. V nabídce nahoře klikněte na **Rychlý Start**.

    ![Konfigurace jednotného přihlašování][6]

4. Na portálu klasické na stránce integrace **Cisco Spark** aplikace klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .

    ![Konfigurace jednotného přihlašování][7] 

5. Na stránce **jakým způsobem uživatelé přihlásit k Cisco Spark** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.
    
    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    na. Přihlaste se na adresu URL do textového pole, zadejte adresu URL pomocí následujícího vzorce: `https://web.ciscospark.com/#/signin`.

    b. Klikněte na tlačítko **Další**.


7. Na stránce **konfigurovat jednotné přihlašování v Cisco Spark** klikněte na **Stáhnout metadata**a pak uložit soubor v počítači.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Přihlaste se ke [Správě spolupráce cloudu Cisco](https://admin.ciscospark.com/) s oprávněními úplné správce.
9. Klikněte na **Nastavení** a v části **ověřování** , klikněte na **změnit**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Vyberte **Integrace zprostředkovatele identit 3rd. (Upřesnit)** a přejděte na další obrazovce.
11. Klikněte na **Stáhnout soubor metadat** a uložit soubor v počítači.
12. Na stránce **Import Idp metadat** buď přetáhněte soubor metadat Azure AD na stránku nebo pomocí možnosti prohlížeče souborů vyhledejte a nahrajte soubor Azure AD metadat. Potom vyberte **vyžadují certifikát podepsaný renomovanou certifikační autoritou v metadatech (zabezpečení)** a klikněte na tlačítko **Další**. 

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Vyberte **Testovat jednotného přihlašování k připojení**a až se otevře na nové záložce prohlížeče, ověřování s Azure AD tak, že.
14. Vraťte se na kartě **Správa spolupráce cloudu Cisco** prohlížeče. Pokud test byl úspěšný, vyberte **Tento test byl úspěšný. Jednotné přihlašování možnost povolit** a klikněte na tlačítko **Další**.

7. Na portálu klasické Azure AD vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

8. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
    
    ![Azure AD jednotné přihlášení][11]

### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.

![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.
    
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:
 
    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   



### <a name="creating-a-cisco-spark-test-user"></a>Vytvoření uživatele Cisco Spark test

V této části vytvoříte uživatele s názvem Britta Simon v Cisco Spark. V této části vytvoříte uživatele s názvem Britta Simon v Cisco Spark.

1. Přejděte na [Cisco cloudu spolupráce správy](https://admin.ciscospark.com/) s oprávněními úplné správce.
2. Klikněte na **uživatele** a pak **Správa uživatelů**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. V okně **Spravovat uživatele** vyberte **ručně přidat nebo upravit uživatele** a klikněte na tlačítko **Další**.
4. Vyberte **jména a e-mailová adresa**. Potom vyplňte do textového pole jako postupujte:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    na. Do textového pole **jméno** zadejte **Britta**

    b. **Příjmení** do textového pole zadejte **Simon**

    c. Do textového pole **e-mailová adresa** zadejte**britta.simon@contoso.com**

5. Klikněte na znaménko plus a přidání Britta Simon. Potom klikněte na **Další**.
6. V okně **Přidat služby pro uživatele** klikněte na **Uložit** a potom **Dokončit**.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělení přístup Cisco Spark.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Cisco Spark, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Cisco Spark**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203] 

1. V seznamu všichni uživatelé vyberte **Britta Simon**.

2. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

Cílem tento oddíl je vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Cisco Spark na panelu aplikace Access můžete by měla získat automaticky přihlášení z Cisco Spark aplikaci.

## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
