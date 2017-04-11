<properties 
    pageTitle="Kurz: Konfigurace pracovní doby pro příchozí synchronizace | Microsoft Azure" 
    description="Informace o použití Workday jako zdroj dat identity pro službu Azure Active Directory." 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />


#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Kurz: Konfigurace pracovní doby pro příchozí synchronizace


Cílem tohoto kurzu je zobrazit kroky, které potřebujete provést v Workday a Azure AD pro import lidí z Workday Azure AD. 

Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:

-   Platné předplatné Azure AD Premium
-   Ke klientovi v Workday
  
Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:

1. Povolení integrace aplikací pro Workday 


2. Vytvoření uživatele systému integrace 


3. Vytvoření skupiny zabezpečení 


4. Přiřazení uživatele systému integrace do skupiny zabezpečení 


5. Konfigurace možností skupiny zabezpečení 


6. Aktivace změny zásad zabezpečení 


7. Konfigurace importem uživatele v Azure AD 



##<a name="enabling-the-application-integration-for-workday"></a>Povolení integrace aplikací pro Workday
  
Cílem tento oddíl je přehledu jak povolit integraci aplikace Workday.

### <a name="steps"></a>Pomocí následujících kroků:

1.  Azure klasický portálu v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Služby Active Directory] (./media/active-directory-saas-workday-inbound-tutorial/IC700993.png "Služby Active Directory")

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .

    ![Aplikace] (./media/active-directory-saas-workday-inbound-tutorial/IC700994.png "Aplikace")

4.  Klikněte na **Přidat** v dolní části stránky.

    ![Přidat aplikaci] (./media/active-directory-saas-workday-inbound-tutorial/IC749321.png "Přidat aplikaci")

  
5. Do pole Hledat zadejte **pracovního dne**.

    ![Přidat aplikaci z gallerry] (./media/active-directory-saas-workday-inbound-tutorial/IC701021.png "Přidat aplikaci z gallerry")

6. V podokně výsledků vyberte Workday a potom klikněte na Dokončit přidat aplikaci.

    ![Galerie aplikace] (./media/active-directory-saas-workday-inbound-tutorial/IC701022.png "Galerie aplikace")





## <a name="creating-an-integration-system-user"></a>Vytvoření uživatele systému integrace

### <a name="steps"></a>Pomocí následujících kroků:


1. Vyplňte **Workday Workbench**vytvořit uživatele do vyhledávacího pole a potom klikněte na **Vytvořit integrace systém uživatele**. 

    ![Vytvoření uživatele] (./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "Vytvoření uživatele")



2. Provedení úkolu: **Vytvoření uživatele systému integrace** zadáním uživatelské jméno a heslo pro nové uživatele systému integrace.  Nechte požadovat nové heslo na možnost Další přihlásit zaškrtnuté, protože tento uživatel bude programově přihlášení. Nechte minut časový limit relace s použita výchozí hodnota 0, které zabrání uživatele relace vypršení časového limitu předčasné. 

    ![Vytvoření uživatele systému integrace] (./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "Vytvoření uživatele systému integrace")
 




## <a name="creating-a-security-group"></a>Vytvoření skupiny zabezpečení

Scénář uvedených v tomto kurzu musíte vytvořit skupinu zabezpečení systému bez omezení integrace a přiřadit uživatele.

### <a name="steps"></a>Pomocí následujících kroků:

1. Zadejte vytvořit skupinu uživatelů ve vyhledávacím poli a potom klikněte na **Vytvořit skupinu zabezpečení**. 

    ![Skupina CreateSecurity] (./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "Skupina CreateSecurity")
 

2. Provedení úkolu: vytvoření skupiny zabezpečení.  Vyberte skupinu zabezpečení systému integrace – aktivace z rozevíracího seznamu typ části nevyužívá dělené tabulky skupiny zabezpečení vytvořit skupinu zabezpečení, ke kterému se členy explicitně přidat. 

    ![Skupina CreateSecurity] (./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "Skupina CreateSecurity")
 



## <a name="assigning-the-integration-system-user-to-the-security-group"></a>Přiřazení uživatele systému integrace do skupiny zabezpečení

### <a name="steps"></a>Pomocí následujících kroků:


1. Do vyhledávacího pole zadejte upravit skupinu zabezpečení a potom klikněte na **Upravit skupinu zabezpečení**. 

    ![Úprava skupiny zabezpečení] (./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "Úprava skupiny zabezpečení")
 
 

2. Vyhledejte a vyberte Nová skupina zabezpečení integrace podle názvu. 

    ![Úprava skupiny zabezpečení] (./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "Úprava skupiny zabezpečení")

 

3. Přidání nového uživatele systému integrace do nové skupiny zabezpečení. 

    ![Skupiny zabezpečení systému] (./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Skupiny zabezpečení systému")  




## <a name="configuring-security-group-options"></a>Konfigurace možností skupiny zabezpečení

V tomto kroku udělíte do nové skupiny oprávnění pro **získání** a **kde si ji** operace pro objekty zajištěná následující zásady zabezpečení domény:

- Zřízení externí účtu

- Pracovníka Data: Sestavy veřejné pracovníka

- Pracovníka Data: Všechny polohy

- Data pracovníka: Aktuální pracovníky informace

- Pracovní Data: Obchodní názvu na pracovní profilu

 
### <a name="steps"></a>Pomocí následujících kroků:

1. Do vyhledávacího pole zadejte zásady zabezpečení domény a potom klikněte na odkaz, zásady zabezpečení domény pro funkčních oblastí.  

    ![Zásady zabezpečení domény] (./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "Zásady zabezpečení domény")  
 

2. Vyhledejte systému a vyberte oblast funkčnosti **systému** .  Klikněte na **OK**.  

    ![Zásady zabezpečení domény] (./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "Zásady zabezpečení domény")  


3. V seznamu zásady zabezpečení pro oblasti funkčnosti systému rozbalte Správa zabezpečení a vyberte zásady zabezpečení domény externí zřízení účtu.  

    ![Zásady zabezpečení domény] (./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "Zásady zabezpečení domény")  


4. Klikněte na **Upravit oprávnění**a potom na stránce **Upravit oprávnění**dialogové okno Přidat novou skupinu zabezpečení do seznamu skupin zabezpečení s oprávněním integrace **získat** a **umístění** . 

    ![Oprávnění k úpravám] (./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "Oprávnění k úpravám")  

 

5. Opakujte krok 1, pokud chcete vrátit na obrazovku pro výběr funkčních oblastí a tentokrát hledání pro pracovníky, vyberte funkčních oblastí správy zaměstnanců a klikněte na tlačítko **OK**.

    ![Zásady zabezpečení domény] (./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "Zásady zabezpečení domény")  
 

6. V seznamu zásady zabezpečení pro funkčních oblastí správy zaměstnanců, rozbalte pracovníka dat: správy zaměstnanců a opakujte krok 4 popsané výše pro každou z nich zbývající zásady zabezpečení:

     - Pracovníka Data: Sestavy veřejné pracovníka

     - Pracovního Data: Všechny polohy

     - Data pracovního: Aktuální pracovníky informace

     - Pracovní Data: Obchodní názvu na pracovní profilu


    ![Zásady zabezpečení domény] (./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "Zásady zabezpečení domény")  







## <a name="activating-security-policy-changes"></a>Aktivace změny zásad zabezpečení

### <a name="steps"></a>Pomocí následujících kroků:

1. Zadejte aktivovat do vyhledávacího pole a potom klikněte na odkaz, aktivovat pole Čekání na změny zásad zabezpečení. 

    ![Aktivace] (./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "Aktivace") 
 

2. Zahájení úkolu aktivace pole Čekání na změny zásad zabezpečení zadáním komentáře pro účely auditování a klikněte na **OK**. 

    ![Aktivace čeká na vyřízení zabezpečení] (./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "Aktivace čeká na vyřízení zabezpečení")   
 

3. Dokončení úkolu na další obrazovce zaškrtnutím políčka označené potvrdit a potom klikněte na **OK**. 

    ![Aktivace čeká na vyřízení zabezpečení] (./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "Aktivace čeká na vyřízení zabezpečení")  





## <a name="configuring-user-import-in-azure-ad"></a>Konfigurace importem uživatele v Azure AD

Cílem tento oddíl je přehledu jak nakonfigurovat Azure AD importu lidí z pracovního dne.


### <a name="steps"></a>Pomocí následujících kroků:


1. Na stránce integrace **Workday** aplikace klikněte na **import konfigurace uživatele** otevřete dialogové okno **Konfigurovat zřizování** .


2. Na stránce **Nastavení a přihlašovací údaje správce** proveďte následující kroky a potom klikněte na **Další**: 

    ![Nastavení a přihlašovací údaje správce] (./media/active-directory-saas-workday-inbound-tutorial/IC750995.png "Nastavení a přihlašovací údaje správce")  
 
    na. Workday správce uživatelské jméno do textového pole, zadejte jméno uživatele, jste vytvořili v části Vytvoření oddíl integrace systém uživatele.

    b. Do textového pole Workday správce heslo zadejte heslo uživatele jste vytvořili v části Vytvoření oddílem integrace systém uživatele.

    c. Do textového pole Workday klienta adresa URL zadejte adresu URL nebo Workday klienta.


3. Na stránce **Test připojení** klikněte na tlačítko **Spustit test** potvrďte připojení a klikněte na tlačítko **Další**. 

    ![Test připojení] (./media/active-directory-saas-workday-inbound-tutorial/IC750996.png "Test připojení")  
 

4. **Provisioning** v nastaveních na stránce klikněte na **Další**. 

    ![Možnosti zajišťování] (./media/active-directory-saas-workday-inbound-tutorial/IC750997.png "Možnosti zajišťování")


5. V dialogovém okně **Spustit zřizování** klikněte na **Dokončit**. 

    ![Spuštění zřizování] (./media/active-directory-saas-workday-inbound-tutorial/IC750998.png "Spuštění zřizování")
 

Nyní můžete naleznete v části **Uživatelé** a zkontrolujte, jestli byl importován pracovního dne uživateli.



## <a name="additional-resources"></a>Další zdroje informací

* [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je aplikace access a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)
