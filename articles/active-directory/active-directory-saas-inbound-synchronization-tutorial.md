<properties 
    pageTitle="Kurz: Konfigurace pracovní doby pro příchozí synchronizace | Microsoft Azure" 
    description="Naučte se používat příchozí synchronizace s Azure Active Directory povolit jednotné přihlašování, automatické vytváření a další!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="04/06/2016" 
    ms.author="jeedes" />

#<a name="tutorial-configuring-workday-for-inbound-synchronization"></a>Kurz: Konfigurace pracovní doby pro příchozí synchronizace
>[AZURE.NOTE]Azure Active Directory (AD) Premium je k dispozici pro zákazníky v Číně pomocí Celosvětová dostupnost instance Azure AD.    
Azure AD Premium není aktuálně podporován ve službě Microsoft Azure provozovaný společností 21Vianet v Číně.    

Cílem tohoto kurzu je zobrazit kroky, které potřebujete provést v Workday a Microsoft Azure AD pro import lidí z Workday společnosti Microsoft Azure AD.    
 Scénář uvedené v tomto kurzu se předpokládá, že už máte následující položky:  

-   Platné Azure předplatné  
-   Ke klientovi Workday  

Scénář uvedené v tomto kurzu se skládá z následujících stavební bloky:  

1.  Povolení integrace aplikací pro Workday  
2.  Vytvoření uživatele systému integrace  
3.  Vytvoření skupiny zabezpečení  
4.  Přiřazení uživatele systému integrace do skupiny zabezpečení  
5.  Konfigurace možností skupiny zabezpečení  
6.  Aktivace změny zásad zabezpečení  
7.  Konfigurace importem uživatele v aplikaci Microsoft Azure AD  

##<a name="enabling-the-application-integration-for-workday"></a>Povolení integrace aplikací pro Workday

Cílem tento oddíl je přehledu jak povolit integraci aplikace služby Salesforce.    

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Postup povolení integrace aplikace pro Workday, proveďte následující kroky:

1.  Na portálu Správa Azure v levém navigačním podokně klikněte na **Služby Active Directory**.    

    ![Služby Active Directory] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700993.png "Služby Active Directory")  

2.  Ze seznamu **adresáře** vyberte adresář, pro kterou chcete povolit integrace adresářů.    

3.  Otevřete zobrazení aplikací v zobrazení adresáře, klikněte v nabídce horní na **aplikace** .    

    ![Aplikace] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700994.png "Aplikace")  

4.  Otevřete **Aplikaci Galerie**, klikněte na **Přidat aplikaci**a potom klikněte na **Přidat aplikaci pro svoji organizaci používat**.    

    ![Co chcete udělat?] (./media/active-directory-saas-inbound-synchronization-tutorial/IC700995.png "Co chcete udělat?")  

5.  Do **vyhledávacího pole**zadejte **pracovního dne**.    

    ![WORKDAY] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701021.png "WORKDAY")  

6.  V podokně výsledků vyberte **Workday**a potom klikněte na **Dokončit** přidat aplikaci.    

    ![WORKDAY] (./media/active-directory-saas-inbound-synchronization-tutorial/IC701022.png "WORKDAY")  

##<a name="creating-an-integration-system-user"></a>Vytvoření uživatele systému integrace

1.  V **Workday Workbench**vyhledávacího pole zadejte **Vytvořit uživatele** a potom klikněte na odkaz, **Vytvořit uživatele systému integrace**.     

    ![vytvoření uživatele] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750979.png "vytvoření uživatele")  

2.  Provedení úkolu: vytvoření uživatele systému integrace zadáním uživatelské jméno a heslo pro nové uživatele systému integrace.  Nechte požadovat nové heslo na možnost Další přihlásit zrušené zaškrtnutí políčka, protože tento uživatel bude programově přihlášení.    
    Nechte minut časový limit relace s použita výchozí hodnota 0, které zabrání uživatele relace vypršení časového limitu předčasné.    

    ![Vytvoření uživatele systému integrace] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750980.png "Vytvoření uživatele systému integrace")  

##<a name="creating-a-security-group"></a>Vytvoření skupiny zabezpečení

Scénář uvedené v tomto kurzu musíte vytvořit skupinu zabezpečení systému bez omezení integrace a přiřadit uživatele.    

1.  Zadejte vytvořit skupinu uživatelů ve vyhledávacím poli a potom klikněte na odkaz, vytvořit skupinu zabezpečení.     

    ![Skupina CreateSecurity] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750981.png "Skupina CreateSecurity")  

2.  Provedení úkolu: vytvoření skupiny zabezpečení.  Vyberte skupinu zabezpečení systému integrace – aktivace z rozevíracího seznamu typ části nevyužívá dělené tabulky skupiny zabezpečení vytvořit skupinu zabezpečení, ke kterému se členy explicitně přidat.     

    ![Skupina CreateSecurity] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750982.png "Skupina CreateSecurity")  

##<a name="assigning-the-integration-system-user-to-the-security-group"></a>Přiřazení uživatele systému integrace do skupiny zabezpečení

1.  Do vyhledávacího pole zadejte upravit skupinu zabezpečení a potom klikněte na odkaz **Upravit skupinu zabezpečení**.     

    ![Úprava skupiny zabezpečení] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750983.png "Úprava skupiny zabezpečení")  

2.  Vyhledejte a vyberte novou skupinu zabezpečení integrace podle názvu    

    ![Úprava skupiny zabezpečení] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750984.png "Úprava skupiny zabezpečení")  

3.  Přidání nového uživatele systému integrace do nové skupiny zabezpečení.       

    ![Skupiny zabezpečení systému] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750985.png "Skupiny zabezpečení systému")  

##<a name="configuring-security-group-options"></a>Konfigurace možností skupiny zabezpečení

V tomto kroku udělíte do nové skupiny oprávnění pro operace Get a umístění pro objekty zajištěná následující zásady zabezpečení domény:  

-   Zřízení externí účtu  
-   Pracovníka Data: Sestavy veřejné pracovníka  
-   Pracovníka Data: Všechny polohy  
-   Data pracovního: Aktuální pracovníky informace  
-   Pracovní Data: Obchodní názvu na pracovní profilu  

&nbsp;  

1.  Zásady zabezpečení domény zadejte do vyhledávacího pole a potom klikněte na odkaz, zásady zabezpečení domény pro funkčních oblastí.     

    ![Zásady zabezpečení domény] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750986.png "Zásady zabezpečení domény")  

2.  Vyhledejte systému a vyberte oblast funkčnosti systému.  Klikněte na tlačítko označené, OK.     

    ![Zásady zabezpečení domény] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750987.png "Zásady zabezpečení domény")  

3.  Ze seznamu zásady zabezpečení pro oblasti funkčnosti systému rozbalte Správa zabezpečení a vyberte zásady zabezpečení domény externí zřízení účtu.     

    ![Zásady zabezpečení domény] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750988.png "Zásady zabezpečení domény")  

4.  Klikněte na tlačítko Upravit oprávnění a potom v dialogovém okně Upravit oprávnění Přidat novou skupinu zabezpečení na seznam skupin zabezpečení s oprávněním integrace Get a umístění.     

    ![Oprávnění k úpravám] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750989.png "Oprávnění k úpravám")  

5.  Opakujte krok 1, pokud chcete vrátit na obrazovku pro výběr funkčních oblastí a tentokrát hledání pro pracovníky, vyberte funkčních oblastí správy zaměstnanců a klikněte na tlačítko OK označené.    

    ![Zásady zabezpečení domény] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750990.png "Zásady zabezpečení domény")  

6.  V seznamu zásady zabezpečení pro funkčních oblastí správy zaměstnanců, rozbalte Data pracovního: správy zaměstnanců a opakujte krok 4 popsané výše pro každou z nich zbývající zásady zabezpečení:    

    -   Pracovníka Data: Sestavy veřejné pracovníka  
    -   Pracovníka Data: Všechny polohy  
    -   Data pracovního: Aktuální pracovníky informace  
    -   Pracovní Data: Obchodní názvu na pracovní profilu    

    ![Zásady zabezpečení domény] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750991.png "Zásady zabezpečení domény")  

##<a name="activating-security-policy-changes"></a>Aktivace změny zásad zabezpečení

1.  Zadejte aktivovat do vyhledávacího pole a potom klikněte na odkaz, aktivovat pole Čekání na změny zásad zabezpečení.    

    ![Aktivace] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750992.png "Aktivace")  

2.   Zahájení úkolu aktivace pole Čekání na změny zásad zabezpečení zadáním komentáře pro účely auditování a potom kliknete na tlačítko označené, OK.      

    ![Aktivace čeká na vyřízení zabezpečení] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750993.png "Aktivace čeká na vyřízení zabezpečení")  

3.  Dokončení úkolu na další obrazovce zaškrtnutím políčka potvrďte a kliknete na tlačítko označené, OK.     

    ![Aktivace čeká na vyřízení zabezpečení] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750994.png "Aktivace čeká na vyřízení zabezpečení")  

##<a name="configuring-user-import-in-microsoft-azure-ad"></a>Konfigurace importem uživatele v aplikaci Microsoft Azure AD

Cílem tento oddíl je přehledu jak nakonfigurovat Microsoft Azure AD importu lidí z pracovního dne.    

###<a name="to-configure-user-import-in-microsoft-azure-ad-perform-the-following-steps"></a>Konfigurace importem uživatele v aplikaci Microsoft Azure AD, proveďte následující kroky:

1.  Na stránce integrace **Workday** aplikace klikněte na **import konfigurace uživatele** otevřete dialogové okno **Konfigurovat zřizování** .    

2.  Na stránce **Nastavení a přihlašovací údaje správce** proveďte následující kroky a potom klikněte na další:    

    ![Nastavení a přihlašovací údaje správce] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750995.png "Nastavení a přihlašovací údaje správce")    

    1.  Do textového pole **Workday správce uživatelské jméno** zadejte jméno uživatele, který jste vytvořili v části [vytvoření uživatele systému integrace](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) .    
    2.  Do textového pole **heslo správce pracovního dne** zadejte heslo uživatele, kterého jste vytvořili v části [vytvoření uživatele systému integrace](https://msdn.microsoft.com/library/azure/Dn762434.aspx#BKMK_CreateUser) .    
    3.  Do textového pole **Workday klienta URL** zadejte adresu URL nebo Workday klienta.    

3.  Na stránce **Test připojení** klikněte na tlačítko **Spustit test** potvrďte připojení a klikněte na tlačítko **Další**.    

    ![Test připojení] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750996.png "Test připojení")  

4.  Provisioning v **nastaveních** na stránce klikněte na **Další**.    

    ![Možnosti zajišťování] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750997.png "Možnosti zajišťování")  

5.  V dialogovém okně **Spustit zřizování** klikněte na **Dokončit**.    

    ![Spuštění zřizování] (./media/active-directory-saas-inbound-synchronization-tutorial/IC750998.png "Spuštění zřizování")  

Nyní můžete naleznete v části **Uživatelé** a zkontrolujte, jestli byl importován pracovního dne uživateli.    
