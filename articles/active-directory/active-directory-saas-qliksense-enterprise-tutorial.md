<properties
    pageTitle="Kurz: Azure Active Directory integrace se službou podnikové smysl Qlik | Microsoft Azure"
    description="Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Enterprise Qlik smysl."
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Kurz: Azure Active Directory integrace s Qlik smysl Enterprise

V tomto kurzu se naučíte, jak integrovat Qlik smysl organizace s Azure Active Directory (Azure AD).

Integrace Qlik smysl organizace s Azure AD poskytuje následující výhody:

- Můžete určit v Azure AD, kdo má přístup k Qlik smysl Enterprise
- Povolení uživatelům, aby automaticky získat přihlášení z na Qlik smysl Enterprise (jednotného přihlašování) pomocí svých účtů Azure AD
- Správa účtů na jednom centrálním místě – klasické portálu Azure

Pokud budete chtít zjistit další informace o SaaS aplikace integrace se službou Azure AD, přečtěte si téma [Co je aplikace access a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Abyste mohli nakonfigurovat integraci Azure AD pomocí Qlik smysl Enterprise, musíte následující položky:

- Předplatné Azure AD
- Pole Qlik smysl organizace jednotného přihlašování povolené předplatného


> [AZURE.NOTE] Chcete-li otestovat kroky v tomto kurzu, není doporučujeme používat provozním prostředí.


Chcete-li otestovat kroky v tomto kurzu, budete by měl těmito doporučeními:

- Pokud je to nutné byste neměli používat provozním prostředí.
- Pokud nemáte prostředí zkušební verzi Azure AD, si můžete stáhnout měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scénář popis
V tomto kurzu abyste je otestovali Azure AD jednotné přihlašování v testovacím prostředí.

Scénář uvedené v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Qlik smysl Enterprise z Galerie
2. Konfigurace a testování Azure AD jednotné přihlášení


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Přidání Qlik smysl Enterprise z Galerie
Pokud chcete nakonfigurovat integraci Qlik smysl Enterprise do Azure AD, potřebujete přidat Qlik smysl Enterprise z Galerie do seznamu spravované SaaS aplikace.

**Přidat Qlik smysl Enterprise z galerie, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory][1]
2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace][2]

4. Klikněte na **Přidat** v dolní části stránky.

    ![Aplikace][3]

5. V dialogovém **Co chcete udělat** klikněte na **Přidat aplikaci z Galerie**.

    ![Aplikace][4]

6. Do pole Hledat zadejte **Qlik smysl organizace**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. V podokně výsledků vyberte **Qlik smysl organizace**a potom klikněte na **Dokončit** přidat aplikaci.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotné přihlášení
V této části Konfigurace a otestujte Azure AD jednotné přihlašování s Enterprise smysl Qlik založeného na uživateli testu s názvem "Britta Simon".

Pro jednotné přihlašování pracovat musí Azure AD vědět, co uživatel protějšek Qlik smysl Enterprise pro uživatele v Azure AD. Odkaz relace mezi Azure AD uživatel a související Qlik smysl Enterprise jinými slovy, je nutné stanovit.

Tento odkaz vztah sídlo přiřazením hodnotu **uživatelské jméno** v Azure AD jako hodnotu **uživatelské_jméno** Qlik smysl Enterprise.

Ke konfiguraci a otestujte Azure AD jednotné přihlašování s Qlik smysl organizace, je potřeba provést následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)** – uživatelům tuto funkci lze použít.
2. **[Vytváření Azure AD testování uživatele](#creating-an-azure-ad-test-user)** – testování Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření Enterprise smysl Qlik testování uživatele](#creating-a-qliksense-enterprise-test-user)** – a protějšek Britta Simon Qlik smysl organizace, která je propojená s Azure AD znázornění jí.
4. **[Přiřazení Azure AD testování uživatele](#assigning-the-azure-ad-test-user)** – povolení Britta Simon používat Azure AD jednotného přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)** – zkontrolujte, zda konfigurace pracuje.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V tomto oddílu povolení Azure AD jednotné přihlašování v portálu klasické a konfigurace jednotného přihlašování v aplikaci Qlik smysl organizace.


**Abyste mohli nakonfigurovat Azure AD jednotné přihlašování s Qlik smysl Enterprise, proveďte následující kroky:**

1. Na portálu klasické na stránce integrace aplikace **Qlik smysl organizace** klikněte na **konfigurovat jednotné přihlašování** otevřete dialogové okno **Konfigurace jednotného přihlašování** .
     
    ![Konfigurace jednotného přihlašování][6] 

2. Na stránce, **jakým způsobem uživatelé přihlásit k Qlik smysl organizace** vyberte **Azure AD jednotného přihlašování**a klikněte na tlačítko **Další**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. Na stránce **Konfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    na. **Přihlaste se na adresu URL** do textového pole, zadejte adresu URL používané vaší uživatelům přihlašování do aplikace Qlik smysl organizace pomocí následujícího vzorce: **https://\<Qlik smysl plně Qualifed Hostname\>: 443 / < virtuální připojit znak před Proxy\>/samlauthn/**.
    
    > [AZURE.NOTE] Poznámka: koncové lomítko na konci tohoto URI.  Je vyžadován.

    b. Klikněte na tlačítko **Další**
 
4. Na stránce **konfigurovat jednotné přihlašování v Qlik smysl organizace** proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    na. Klikněte na tlačítko **Stáhnout metadat**a uložte soubor na svém počítači.  Připravit upravit tento soubor metadat před nahráním na server Qlik smysl.

    b. Klikněte na tlačítko **Další**.

5. Připravte federace Metadata XML soubor tak, aby, můžete nahrávat na server Qlik smysl.

    > [AZURE.NOTE] Před nahráním IdP metadata Qlik smysl server, je třeba soubor upravovat odebrat informace, jak zajistit správnou funkci mezi Azure AD a server Qlik smysl.

    ![QlikSense][qs24]

    na. Otevřete soubor FederationMetaData.xml stahují z Azure v textovém editoru.

    b. Vyhledání hodnoty **RoleDescriptor**.  Nastane čtyři položky (dvě dvojice levých a pravých značek prvků).

    c. Odstranění značek RoleDescriptor a všechny informace o mezi ze souboru.

    d. Uložení souboru a udržovat její přítomnost pro pozdější použití v tomto dokumentu.

6. Přejděte na Qlik smysl Qlik Management Console (QMC) jako uživatel, který můžete vytvořit virtuální proxy konfigurace.

7. V QMC klikněte na položku virtuální Proxy.

    ![QlikSense][qs6] 

8. V dolní části obrazovky klikněte na tlačítko Vytvořit nový.

    ![QlikSense][qs7]

9. Zobrazí se obrazovka virtuální proxy upravit.  Na pravé straně obrazovky je nabídka týkající se uplatňování možnosti konfigurace viditelné.

    ![QlikSense][qs9]

10. S možností nabídky identifikace zaškrtnete zadejte identifikační informace o konfiguraci Azure virtuální proxy.

    ![QlikSense][qs8]  
    
    na. Do pole Popis je popisný název konfiguraci virtuální proxy.  Zadejte popis.
    
    b. Pole Předpona identifikuje virtuální proxy koncový bod pro připojení k Qlik smysl s Azure AD jednotného přihlašování.  Zadejte název jedinečnou předponu tento virtuální proxy.

    c. Časový limit relace nečinnosti (v minutách) je časový limit pro připojení přes tento virtuální proxy.

    d. Název relace souborů cookie záhlaví je název souboru cookie ukládat identifikátor relace Qlik smysl relace, které uživatel obdrží po úspěšném ověření.  Tento název musí být jedinečný.

11. Klikněte na položku ověřování nabídky něho.  Zobrazí se obrazovka ověřování.

    ![QlikSense][qs10]

    na. **Anonymní přístup režimu** rozevírací seznam určuje, pokud anonymní uživatelé mohou přístup k Qlik smysl virtuální proxy.  Anonymní uživatel se výchozí možnost.

    b. **Metody ověřování** rozevírací zjistí, že schéma ověřování proxy virtuální použije.  V rozevíracím seznamu vyberte SAML.  Další možnosti se zobrazí jako výsledek.

    c. **SAML host URI pole**vstupní že budou uživatelé hostname zadávat s přístupem k Qlik smysl přes tento virtuální proxy SAML.  Název hostitele je uri serveru Qlik smysl.

    d. Do pole **SAML entity ID**zadejte stejné hodnota zadaná do pole SAML host URI.

    e. **SAML IdP metadat** je soubor upravovat dříve v části **Upravit Metadata federace z Azure AD konfigurace** .  Odebrání informací pro zajištění správné funkce mezi Azure AD **před nahráním IdP metadata, je třeba soubor upravovat** a server Qlik smysl.  **Získáte výše uvedených pokynů Pokud soubor ještě upravovat.**  Pokud byl upraven soubor klikněte na tlačítko Procházet a vyberte Upravit metadata soubor k nahrání na konfiguraci virtuální proxy.

    f. Zadejte název atributu nebo Přehled schématu atributu SAML představující **ID uživatele** Azure AD odešle server Qlik smysl.  Schéma referenční informace jsou k dispozici v konfiguraci příspěvek obrazovky Azure aplikace.  Použít atribut název **zadejte http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.

    g. Zadejte hodnotu pro **adresáře uživatele** , který budou připojena k uživatele při ověřování serveru Qlik smysl prostřednictvím Azure AD.  Pevně kódovaná hodnoty musí být uzavřen **hranaté závorky []**.  Použít atribut Odeslaná pošta v výrazu Azure AD SAML, zadejte název atributu tohoto textového pole **bez** hranatých závorek.

    h. **Algoritmus podepisování SAML** nastaví certifikát poskytovatele (v tomto případě Qlik smysl serveru) služby přihlašování pro konfiguraci virtuální proxy.  Pokud server Qlik smysl používá důvěryhodnou certifikační generovaný pomocí Microsoft RSA rozšířeného a AES Cryptographic Provider, přejděte do algoritmus podepisování SAML **SHA 256**.

    Můžu. V části mapování atribut SAML umožňuje další atributy jako skupiny nechat zasílat Qlik smysl pro použití v pravidlech zabezpečení.

12. Klikněte na možnost nabídky něho Vyrovnávání zatížení.  Zobrazí se obrazovka Vyrovnávání zatížení.

    ![QlikSense][qs11]

13. Klikněte na Přidat novou serveru uzel tlačítko, vyberte engine uzel nebo uzly Qlik smysl odeslat relací pro účely Vyrovnávání zatížení a klikněte na tlačítko Přidat.

    ![QlikSense][qs12]

14. Klikněte na možnost Upřesnit nabídky něho. Rozšířené obrazovky.

    ![QlikSense][qs13]

    na. Host (hostitel) bílé seznam uvádí názvy hostitelů, které jsou přijaty při připojení k serveru Qlik smysl.  **Zadejte název hostitele uživatelů se připojujete při připojení k serveru Qlik smysl.** Název hostitele se stejnou hodnotu jako host uri SAML bez https://.

15. Klikněte na tlačítko použít.

    ![QlikSense][qs14]

16. Klikněte na OK potvrďte upozornění, které stavy se nerestartuje propojená s virtuální proxy servery proxy.

    ![QlikSense][qs15]

17. Na pravé straně obrazovky zobrazí se nabídka přidružené položky.  Klikněte na možnost nabídky proxy servery.

    ![QlikSense][qs16]

18. Zobrazí se obrazovka proxy serveru.  Klikněte na tlačítko odkaz v dolní části odkaz na proxy server na virtuálních proxy.

    ![QlikSense][qs17]

19. Vyberte uzel proxy, který bude podporovat tento virtuální proxy připojení a klikněte na tlačítko odkaz.  Po propojení, se zobrazí v části související proxy servery proxy server.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. Po přibližně 5 až 10 sekund zobrazí se zpráva aktualizace QMC.  Klikněte na tlačítko Aktualizovat QMC.

    ![QlikSense][qs20]

21. Po aktualizaci QMC klikněte na položku virtuální proxy servery. Nová položka virtuální proxy SAML je uvedený v tabulce na obrazovce.  Jedním klepnutím na položky virtuální proxy.

    ![QlikSense][qs51]

22. V dolní části obrazovky klikněte na tlačítko Stáhnout SP metadat aktivuje.  Tlačítko Stáhnout SP metadat uložit metadata do souboru.

    ![QlikSense][qs52]

23. Otevřete soubor sp metadat.  Sledujte položky **ID entit** a **AssertionConsumerService** .  Tyto hodnoty jsou ekvivalentní **identifikátor** a **Přihlásit se adresa URL** konfigurace aplikace Azure AD. Pokud nejsou odpovídající měli byste je nahradit v Průvodci konfigurací Azure AD aplikace.

    ![QlikSense][qs53]

24. Na portálu klasické vyberte potvrzení jednotné přihlašování a klikněte na tlačítko **Další**.
    
    ![Azure AD jednotné přihlášení][10]

25. Na stránce **Potvrzení přihlášení** klepněte na **Dokončit**.  
 
    ![Azure AD jednotné přihlášení][11]


### <a name="creating-an-azure-ad-test-user"></a>Vytvoření uživatele služby Azure AD test
V této části vytvoříte testovací uživatelské jméno na portálu klasické s názvem Britta Simon.


![Vytvořit Azure AD uživatele][20]

**Vytvoření testovací uživatelské jméno v Azure AD, proveďte následující kroky:**

1. Na **portálu Azure klasické**, v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3. Zobrazení seznamu uživatelů, v nabídce v horní, klikněte na **uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. Na panelu nástrojů v dolní části otevřete dialogové okno **Přidat uživatele** , klikněte na **Přidat uživatele**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. Na stránce dialogové okno **námi o tohoto uživatele** proveďte následující kroky:  ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    na. Jako typ uživatele vyberte nového uživatele ve vaší organizaci.

    b. Do **textového pole**uživatelské jméno zadejte **BrittaSimon**.

    c. Klikněte na tlačítko **Další**.

6.  Na stránce dialogové okno **Uživatelského profilu** proveďte následující kroky: ![vytváření uživatele služby Azure AD test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    na. Do textového pole **jméno** zadejte **Britta**.  

    b. **Příjmení** textového pole Typ **Simon**.

    c. Do textového pole **Název zobrazení** zadejte **Britta Simon**.

    d. Vyberte v seznamu **Role** **uživatele**.

    e. Klikněte na tlačítko **Další**.

7. Na stránce **stažení dočasné heslo** dialogového okna klikněte na **vytvořit**.

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. Na stránce **stažení dočasné heslo** dialogové okno proveďte následující kroky:

    ![Vytvoření uživatele služby Azure AD test](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    na. Poznamenejte si hodnotu **Nové heslo**.

    b. Klikněte na **dokončení**.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Vytvoření uživatele test Qlik smysl Enterprise

V této části vytvoříte uživatele s názvem Britta Simon Qlik smysl Enterprise. Zkontrolujte pracujte se na tým podpory Qlik smysl organizace přidáte uživatele v platformu Qlik smysl organizace.


### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení uživatelské test Azure AD

V této části povolíte Britta Simon pomocí Azure jednotného přihlašování udělení přístup Qlik smysl organizace.

![Přiřazení uživatele][200] 

**Pokud chcete přiřadit Britta Simon Qlik smysl Enterprise, proveďte následující kroky:**

1. Na portálu klasické otevřete zobrazení aplikací v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.

    ![Přiřazení uživatele][201] 

2. V seznamu aplikací vyberte **Qlik smysl organizace**.

    ![Konfigurace jednotného přihlašování](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. V nabídce na horní klikněte na **uživatele**.

    ![Přiřazení uživatele][203]

4. V seznamu uživatelů zvolte **Britta Simon**.

5. Na panelu nástrojů dole klepněte na tlačítko **přiřadit**.

    ![Přiřazení uživatele][205]


## <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jednoho přihlašování konfiguraci pomocí panelu aplikace Access.

Po kliknutí na dlaždici Qlik smysl Enterprise na panelu aplikace Access můžete by měla získat automaticky přihlášení z Qlik smysl podnikové aplikaci.


## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png