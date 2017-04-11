<properties
    pageTitle="Začínáme s katalogem dat | Microsoft Azure"
    description="Kurz začátku do konce prezentace scénáře a funkcí katalogu dat Azure."
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>Začínáme s katalogem dat Azure
Azure katalog dat je plně spravovaných cloudové služby, které bude sloužit jako systém registrace a systémem zjišťování majetku data organizace. Podrobné informace najdete v tématu [Co je katalog dat Azure](data-catalog-what-is-data-catalog.md).

Tento kurz vám pomůže začít s katalogem dat Azure. Postupujte podle následujících pokynů v tomto kurzu:

| Postup | Popis |
| :--- | :---------- |
| [Katalog dat poskytování](#provision-data-catalog) | V tomto postupu zřízení nebo nastavení katalogu dat Azure. V tomto kroku provedete jenom v případě, že katalogu nebyla nastavena před. Jedinou katalog dat na organizaci (Microsoft Azure Active Directory domain) můžete mít, i když jsou víc předplatných přidruženého k vašemu účtu Azure. |
| [Registrace prostředky dat](#register-data-assets) | V tomto postupu zaregistrujte majetku dat z databáze ukázkové AdventureWorks2014 v katalogu dat. Registrace je proces extrahování klíčové strukturální metadat například názvy, typů a umístění ze zdroje dat a kopírování že metadata v katalogu. Zdroje dat a datové prostředky zůstat kde jsou, ale metadata používanou katalogem pro jejich snadněji zjistitelný a srozumitelná. |
| [Seznamte se s prostředky dat](#discover-data-assets) | V tomto postupu používat portál katalog dat Azure zjistit datového prostředky zaregistrované v předchozím kroku. Po registrované zdroje dat s katalogem dat Azure, jeho metadata indexováno službou tak, aby uživatelé mohli snadno najít data, včetně obsahu vaší. |
| [Pořizovat poznámky v datové prostředky](#annotate-data-assets) | V tomto postupu zadejte poznámky (informace například popisy, značky, si přečtěte následující dokumentaci nebo odborníky) pro datové prostředky. Tyto informace doplňující metadata extrahuje ze zdroje dat a aby zdroje dat srozumitelnější další lidi. |
| [Připojení k datové prostředky](#connect-to-data-assets) | V tomto postupu otevřete prostředky dat v integrovaných klientských nástrojích (třeba Excel a SQL Server Data Tools) a nástroji zjistili (SQL Server Management Studio). |
| [Správa aktiv dat](#manage-data-assets) | V tomto postupu nastavení zabezpečení pro vaše data prostředky. Katalog dat není uživatelům přístup k datům samotné. Vlastníka zdroje dat mezery nastavuje přístup k datům. <br/><br/> S katalogem dat můžete zjišťovat zdroje dat a zobrazení **metadat** související s zdroje registrované v katalogu. Může nastat situace, ale místo, kam by měly být viditelné jenom pro určitého uživatele nebo členům skupiny určitého zdroje dat. Katalog dat podobnému sledu slouží k převzetí vlastnictví majetku registrovaná data v rámci katalogu a řídit viditelnost prostředky, které vlastníte. |
| [Odebrání data majetku](#remove-data-assets) | V tomto postupu se naučíte, jak odebrat majetku data z katalogu dat. |  

## <a name="tutorial-prerequisites"></a>Výuková požadavky

### <a name="azure-subscription"></a>Azure předplatného
Pokud chcete nastavit katalog dat Azure, musíte být vlastníkem nebo spoluvlastníka Azure předplatného.

Azure předplatná lépe tak uspořádat přístup ke cloudové služby zdrojů, jako jsou katalog dat Azure. Jsou taky nápovědy můžete řídit, jak je vykázaného používání zdrojů, vám účtovat poplatky a zaplacené. Každého předplatného může obsahovat různé platba a fakturace vzhled tak, abyste měli různých předplatných a různých plánech oddělení, project, místní office a tak dál. Každý cloudové služby součástí předplatného a musíte mít předplatné před nastavením katalog dat Azure. Další informace najdete v tématu [Správa účtů, předplatná a správní role](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Pokud nemáte předplatné, můžete vytvořit bezplatný účet zkušební jenom několika minut. Další informace najdete v článku [Bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) .

### <a name="azure-active-directory"></a>Azure Active Directory
Pokud chcete nastavit katalog dat Azure, jste se nepřihlásili pod uživatelský účet Azure Active Directory (Azure AD). Musíte být vlastníkem nebo spoluvlastníka Azure předplatného.  

Azure AD poskytují snadný způsob, jak pro svoji firmu správy identit a přístup, jak v cloudu a místní. Můžete jeden pracovní nebo školní účet pro přihlášení k libovolné cloudu a místní webové aplikace. Azure katalog dat používá Azure AD ověření přihlášení. Další informace najdete v tématu [Co je Azure Active Directory](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Konfigurace zásad Azure Active Directory

Jestliže se můžete přihlásit k portálu katalog dat Azure, ale při pokusu o přihlášení k registraci nástroj pro zdroj dat může nastat situace, zobrazit chybová zpráva, které brání přihlášení. K této chybě může dojít při práci v podnikové síti nebo když se připojujete z mimo podnikové síti.

Registrační nástroj používá *ověřování pomocí formulářů* pro ověřování přihlášení uživatele před Azure Active Directory. Správce služby Azure Active Directory pro úspěšně přihlásit, musí povolit ověřování pomocí formulářů v *zásady globální ověřování*.

Zásada globální ověřování můžete povolit ověřování samostatně intranetové nebo extranetové připojení k jak je znázorněno na následujícím obrázku. Pokud není povolený ověřování pomocí formulářů pro síť, ze kterého se připojujete může docházet chyb při přihlášení.

 ![Zásady globální ověřování Azure Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Další informace najdete v tématu [Konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Katalog dat poskytování
Můžete vytvořit jedinou katalog dat na organizaci (Azure Active Directory domény). Proto pokud může vlastník nebo spoluvlastníka předplatné Azure, kteří patří do této domény Azure Active Directory vytvořil už katalog, nebude moct znovu vytvořte katalog, i když máte víc předplatných Azure. K ověření, zda byl vytvořen katalog dat uživatele ve vaší doméně služby Azure Active Directory, přejděte na [domovskou stránku katalog dat Azure](http://azuredatacatalog.com) a ověřte, jestli se zobrazí v katalogu. Pokud ke katalogu již byly vytvořeny za vás, následujícím postupem přeskočit a přejít v další části.    

1. Přejděte na [stránku služby katalog dat](https://azure.microsoft.com/services/data-catalog) a klikněte na **Začínáme**.

    ![Azure katalog dat – marketing cílovou stránku](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Přihlaste se pomocí účtu, který se může vlastník nebo spoluvlastníka Azure předplatného. Po přihlášení se zobrazí na následující stránce.

    ![Azure katalog dat – poskytování katalogu dat](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Zadejte **název** v katalogu dat, **předplatné** , které chcete použít a **umístění** pro katalogu.
4. Rozbalte **ceny** a vyberte katalog dat Azure **edition** (Free nebo standardní).
    ![Azure katalog dat – vyberte edition](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Rozbalte **Katalogu uživatele** a klikněte na **Přidat** k přidání uživatelů do katalogu dat. Automaticky se přidají do této skupiny.
    ![Katalog dat Azure – uživatelů](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Rozbalte **Katalogu správci** a klikněte na tlačítko **Přidat** pro přidání dalších správců v katalogu dat. Automaticky se přidají do této skupiny.
    ![Katalog dat Azure – správce](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Klikněte na **Vytvořit katalog** vytvořit v katalogu dat pro vaši organizaci. Po vytvoření otevřela vidíte domovské stránky katalogu dat.
    ![Katalog dat Azure – vytvořili](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Vyhledání katalogu dat na portálu Azure
1. Na samostatné kartě v prohlížeči nebo v okně prohlížeče samostatnou webovou přejděte na [portál Azure](https://portal.azure.com) a přihlaste se pomocí stejný účet, který jste použili k vytvoření katalogu dat v předchozím kroku.
2. Vyberte **Procházet** a pak klikněte na **Katalog dat**.

    ![Katalog dat Azure – procházet Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) se zobrazí v katalogu dat, který jste vytvořili.

    ![Azure katalog dat – katalogu zobrazení v seznamu](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Klikněte na katalog, který jste vytvořili. Zobrazí zásuvné **Katalog dat** na portálu.

    ![Azure katalog dat – zásuvné portálu ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. Můžete zobrazit vlastnosti v katalogu dat a jejich aktualizaci. Například klikněte na **ceny osy** a změňte edici.

    ![Azure katalog dat – ceny osy](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Databáze ukázkové Adventure Works
V tomto kurzu zaregistrovat majetku dat (tabulky) z databáze ukázkové AdventureWorks2014 pro databázový stroj SQL serveru, ale pokud chcete pracovat s daty, která je známé a jsou pro vaše role, můžete použít libovolný podporovaný zdroj dat. Seznam podporovaných zdrojů dat najdete v článku [podporované zdroje dat](data-catalog-dsr.md).

### <a name="install-the-adventure-works-2014-oltp-database"></a>Instalace Adventure Works 2014 OLTP databáze
Databáze Adventure Works podporuje standardní online zpracování transakcí scénáře pro fiktivní kolo výrobce (Adventure Works cyklů) obsahující produkty, prodeje a nákupu. V tomto kurzu zaregistrovat informace o produktech do katalogu dat Azure.

Chcete-li nainstalovat databáze ukázkové Adventure Works:

1. Stáhněte si [Adventure Works 2014 celé databáze Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) na webu CodePlex.
2. Pokud chcete obnovit databázi v počítači, postupujte podle pokynů v [Obnovení záložní databáze pomocí SQL Server Management Studio](http://msdn.microsoft.com/library/ms177429.aspx)nebo pomocí následujících kroků:
    1. Otevřete SQL Server Management Studio a připojte k SQL serveru databázový stroj.
    2. Klikněte pravým tlačítkem na **databáze** a klikněte na **Obnovit databázi**.
    3. V části **Obnovit databázi**klikněte na možnost **zařízení** pro **zdroj** a klikněte na tlačítko **Procházet**.
    4. V části **Vyberte zálohovací zařízení**klikněte na **Přidat**.
    5. Přejděte na složku, kam jste soubor **AdventureWorks2014.bak** , vyberte soubor a klikněte na **OK** zavřete dialogové okno **Vyhledejte záložní soubor** .
    6. Kliknutím na **OK** zavřete dialogové okno **Vyberte zálohovací zařízení** .    
    7. Kliknutím na **OK** zavřete dialogové okno **Obnovit databázi** .

Nyní můžete zaregistrovat prostředky dat z databáze ukázkové Adventure Works pomocí katalogu dat Azure.

## <a name="register-data-assets"></a>Registrace prostředky dat

V tomto cvičení používáte nástroj registrace zaregistrovat prostředky dat z databáze Adventure Works pomocí katalogu. Registrace je proces extrahování klíčové strukturální metadat například názvy, typů a umístění zdroje dat a prostředky, které obsahuje a kopírování že metadata v katalogu. Zdroje dat a datové prostředky zůstat kde jsou, ale metadata používanou katalogem pro jejich snadněji zjistitelný a srozumitelná.

### <a name="register-a-data-source"></a>Registrace zdroje dat

1.  Přejděte na [domovskou stránku katalog dat Azure](https://azuredatacatalog.com) a klikněte na **Publikovat Data**.

    ![Katalog dat Azure – publikování dat tlačítko](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  Klikněte na **Spustit aplikaci** ke stažení, instalace a spuštění nástroje pro registraci ve vašem počítači.

    ![Azure katalog dat – tlačítko Spustit](media/data-catalog-get-started/data-catalog-launch-application.png)

3. Na stránce **Vítejte** klikněte na tlačítko **přihlásit** a zadejte svoje přihlašovací údaje.    

    ![Azure katalog dat – úvodní stránka](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. Na stránce **Katalog dat Microsoft Azure** klikněte na **Serveru SQL Server** a **Další**.

    ![Azure katalog dat – zdroje dat](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  Zadejte vlastnosti připojení SQL serveru **AdventureWorks2014** (viz následující příklad) a klikněte na **Připojit**.

    ![Azure katalog dat – nastavení připojení SQL serveru](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  Zaregistrujte metadat dat materiálů. V tomto příkladu je zaregistrovat **Výrobní/produktu** objekty z AdventureWorks výrobní názvů:

    1. Ve stromové struktuře **Hierarchie Server** rozbalte **AdventureWorks2014** a klikněte na **výroby**.
    2. Vyberte **produkt**, **sloupec ProductCategory**, **ProductDescription**a **ProductPhoto** pomocí klávesy Ctrl a kliknutím.
    3. Klikněte na příkaz **Přesunout vybrané šipku** (**>**). Tato akce přesunutí všech vybraných objektů do seznamu **objektů k registraci** .

        ![Azure kurz katalog dat – Procházet a vyberte objektů](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. Vyberte možnost **Zahrnout v náhledu** , zobrazil náhled snímku data. Snímek obsahuje až 20 záznamy ze všech tabulek a zkopírována do katalogu.
    5. Vyberte **Zahrnout Data profilu** zahrnout snímek statistiku objekt profilu dat (třeba: minimální, maximální a průměr hodnot ve sloupci počet řádků).
    6. Do pole **Přidat značky** zadejte **adventure pracuje, cyklů**. Tato akce přidá Hledat značky pro tyto datové prostředky. Značky jsou skvělý způsob, jak umožnit uživatelům nalézt zdroje dat registrované.
    7. Zadejte název **odborník** těchto údajů (volitelné).

        ![Azure kurz katalog dat – objektů k registraci](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. Klikněte na **ZAREGISTROVAT**. Katalog dat Azure zaregistruje vybraných objektů. V tomto cvičení jsou registrovány vybraných objektů z Adventure Works. Registrační nástroj extrahuje metadat z dat materiálů a zkopíruje tato data do katalogu dat Azure služby. Data zůstanou uložena se právě nachází, kde zůstane v části ovládací prvek správců a zásady aktuální systému.

        ![Azure katalog dat – registrovaných objektů](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. Objekty zdroje registrovaných dat zobrazíte kliknutím na **Portál zobrazení**. Na portálu katalog dat Azure potvrďte, že se všechny čtyři tabulky a databází v zobrazení tabulky.

        ![Objekty na portálu katalog dat Azure ](media/data-catalog-get-started/data-catalog-view-portal.png)


V tomto cvičení registrované objekty z databáze ukázkové Adventure Works tak, aby bylo možné snadno rozpoznat uživatelé ve vaší organizaci. V dalším cvičení se naučíte, jak zjistit registrovaná data prostředky.

## <a name="discover-data-assets"></a>Seznamte se s prostředky dat
Vyhledávání v katalogu dat Azure používá dva hlavní mechanismy: hledání a filtrování.

Hledání je navržen intuitivní a výkonných. Ve výchozím nastavení hledané termíny splnění proti libovolného vlastnosti v katalogu, včetně uživatelů – za předpokladu, že poznámky.

Filtrování navržen jako doplněk vyhledávání. Můžete vybrat konkrétní vlastností, jako je třeba odborníci zdroj dat, typ objektu a značky zobrazíte odpovídající aktiva dat a chcete omezit výsledky hledání na odpovídající prostředky.

Pomocí kombinace hledání a filtrování, můžete rychle přejít zdroje dat, které byly registrovaný u katalog dat Azure zjistit datového prostředky, které potřebujete.

V tomto cvičení používat portál katalog dat Azure zjistit dat prostředky, které jste zaregistrovali v předchozím cvičení. Další informace o syntaxi hledání najdete v článku [hledání v katalogu dat syntaktické reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) .

Následuje několik příkladů pro zjišťování prostředky dat v katalogu.  

### <a name="discover-data-assets-with-basic-search"></a>Seznamte se s dat majetku s základní hledání
Základní hledaný umožňuje hledat katalog pomocí jedné nebo více hledaných slov. Výsledky jsou prostředky, které splňují všechny vlastnost s jedním nebo víc podmínek definovaných.

1. Na portálu katalog dat Azure klikněte na kartu **Domů** . Pokud zavřeli webového prohlížeče, přejděte na [domovskou stránku katalog dat Azure](https://www.azuredatacatalog.com).
2. Do pole Hledat zadejte `cycles` a stiskněte klávesu **ENTER**.

    ![Azure katalog dat – základní text hledání](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Potvrďte, že se všechny čtyři tabulky a databáze (AdventureWorks2014) ve výsledcích. Pomocí tlačítek na panelu nástrojů, jak je znázorněno na následujícím obrázku můžete přepínat mezi **zobrazením mřížky** a **zobrazení seznamu** . Všimněte si, že klíčové slovo pro hledání zvýrazněná ve výsledcích hledání **Vyberte** možnost je **připojen k**. Můžete taky určit počet **výsledků na stránku** ve výsledcích hledání.

    ![Azure katalog dat – základní text výsledků hledání](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    Panel **Vlastnosti** je na pravé straně panelu **hledání** je na levé straně. Na panelu **hledání** můžete změnit kritéria hledání a filtrování výsledků. Panel **Vlastnosti** zobrazí vlastnosti vybraného objektu v tabulce nebo seznam.

4. Ve výsledcích hledání klikněte na **Product** . Klikněte na **Náhled**, **sloupců**, **Dat profilu**a **si přečtěte následující dokumentaci** karty nebo klikněte na šipku pro rozbalení podokna dolní.  

    ![Azure katalog dat – dolním podokně](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    Na kartě **Náhled** uvidíte náhled dat v tabulce **Product** .  
5. Klikněte na kartu **sloupce** podrobné informace o sloupcích (třeba **název** a **datový typ**) najdete v materiálů dat.
6. Klikněte na kartu **Data profilu** najdete v článku vytváření profilů dat (třeba: počet řádků, velikost dat nebo minimální hodnotu ve sloupci) v materiálů data.
7. Filtrování výsledků pomocí **filtrů** na levé straně. Například pro **Typ objektu**klikněte na **tabulku** a zobrazí pouze čtyři tabulky, ne databázi.

    ![Azure katalog dat – filtrování výsledků hledání](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Seznamte se s dat majetku s vlastnost oborů
Definování rozsahu vlastnost pomůže objevit majetku dat místo, kam hledaný výraz porovnána s určitou vlastnost.

1. Vymažte filtr **tabulky** v seznamu **Typ objektu** v dialogovém okně **filtry**.  
2. Do pole Hledat zadejte `tags:cycles` a stiskněte klávesu **ENTER**. Všechny vlastnosti, které můžete použít pro vyhledávání v katalogu dat najdete v článku [hledání v katalogu dat syntaktické reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) .
3. Potvrďte, že se všechny čtyři tabulky a databáze (AdventureWorks2014) ve výsledcích.  

    ![Katalog dat – vlastnosti definování rozsahu výsledků hledání](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Uložení pole Hledat
1. V podokně **hledání** v části **Aktuální hledání** zadejte název pro hledání a klikněte na tlačítko **Uložit**.

    ![Azure katalog dat – uložit hledání](media/data-catalog-get-started/data-catalog-save-search.png)
2. Potvrďte, že uložené hledání se zobrazí v části **– Uložená hledání**.

    ![Azure katalog dat – uložená hledání](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Vyberte jednu z akce, které lze provádět se uložená hledání (**Přejmenování**, **Odstranění**, **Uložení jako výchozí** hledání).

    ![Azure katalog dat – uložení možnosti hledání](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Logické operátory
Můžete rozšířit nebo zpřesnění hledání logické operátory.

1. Do pole Hledat zadejte `tags:cycles AND objectType:table`, a stiskněte klávesu **ENTER**.
2. Potvrďte, že se zobrazí pouze tabulky (ne databáze) ve výsledcích.  

    ![Azure katalog dat – logický operátor ve vyhledávání](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Seskupení pomocí závorek
Seskupením pomocí závorek, můžete seskupovat pohled na část dotazu dosáhnout logické izolace, zejména spolu s logické operátory.

1. Do pole Hledat zadejte `name:product AND (tags:cycles AND objectType:table)` a stiskněte klávesu **ENTER**.
2. Potvrďte, že se zobrazí pouze v tabulce **Product** ve výsledcích hledání.

    ![Azure katalog dat – seskupení hledání](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Relační operátory
Relační operátory můžete pomocí porovnání než rovnosti vlastností, které mají číselná a datum datové typy.

1. Do pole Hledat zadejte `lastRegisteredTime:>"06/09/2016"`.
2. Vymažte filtr **tabulky** v seznamu **Typ objektu**.
3. Stiskněte klávesu **ENTER**.
4. Potvrďte, že se **produktu**, **sloupec ProductCategory**, **ProductDescription**a **ProductPhoto** tabulkami a AdventureWorks2014 databázi, kterou jste zaregistrovali ve výsledcích hledání.

    ![Azure katalog dat – porovnání výsledků hledání](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Zjistěte, [Jak zjistit datového prostředky](data-catalog-how-to-discover.md) podrobné informace o syntaxi hledání objevují ostatní data majetek i [hledání v katalogu dat syntaktické reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) .

## <a name="annotate-data-assets"></a>Pořizovat poznámky v datové prostředky
V tomto cvičení používat portál katalog dat Azure opatřit (zadejte informace jako popisy, značky nebo odborníky) jste dříve registrované v katalogu dat prostředky. Poznámky dodatku k vylepšení strukturální metadata extrahuje ze zdroje dat při registraci a jednodušší prostředky data zjišťovat a interpretaci.

V tomto cvičení sledovat jednu datovou materiálů (ProductPhoto). Přidejte popisný název a popis majetku ProductPhoto data.  

1.  Přejděte na [domovskou stránku katalog dat Azure](https://www.azuredatacatalog.com) a prohledávání s `tags:cycles` k vyhledání dat prostředky jste si zaregistrovali.  
2. Ve výsledcích hledání klikněte na tlačítko **ProductPhoto** .  
3. **Popis**zadejte **Product obrázky** **Popisný název** a **fotografie produktu pro marketingových materiálů** .

    ![Azure katalog dat – ProductPhoto popis](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    **Popis** pomůže ostatním zjišťovat a porozumět tomu důvod, proč a jak používat materiálů vybraná data. Můžete také přidat více značek a zobrazit sloupce. Teď můžete zkusit hledání a filtrování a objevovat majetku dat pomocí popisná jste přidali do katalogu.

Můžete taky udělat následující na této stránce:

- Přidání odborníků majetku data. Klikněte na **Přidat** do oblasti **odborníkům** .
- Přidání značek na úrovni datovou sadu. Klikněte na **Přidat** do oblasti **značky** . Značky může být značku uživatele nebo Glosář značku. Standardní vydání z katalogu dat obsahuje firmy slovník, který pomáhá správcům katalogu definovat taxonomie střední firmy. Uživatelé katalogu může pořizovat poznámky klikněte v dat aktiv pomocí Glosář. Další informace najdete v tématu [jak nastavit Glosář firmy řídit značek](data-catalog-how-to-business-glossary.md)
- Přidání značek na úrovni sloupců. Klikněte na **Přidat** v části **značky** u sloupce, který chcete opatřit poznámkami.
- Přidání popisu na úrovni sloupců. Zadejte **Popis** sloupce. Popis metadata ze zdroje dat můžete taky zobrazit.
- Přidejte informace **žádosti o přístup** , které ukazuje uživatelům, jak požádat o přístup k materiálů data.

    ![Katalog dat Azure – přidání značek, popisy](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- Zvolte kartu **si přečtěte následující dokumentaci** a připojte dokumentaci položka data. Si přečtěte následující dokumentaci katalog dat Azure můžete pomocí katalogu dat jako obsahu úložiště vytvořit kompletní popisů datové prostředky.

    ![Azure katalog dat – karta si přečtěte následující dokumentaci](media/data-catalog-get-started/data-catalog-documentation.png)


Můžete také přidat poznámku k více dat prostředky. Můžete například vyberte všechna data prostředky zaregistrovaný a zadejte odborník pro ně.

![Katalog dat Azure – pořizovat poznámky v několika prostředky dat](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Katalog dat Azure podporuje původ DAV přístup k poznámky. Všichni uživatelé katalogu dat můžete přidat značky (uživatele nebo glosář), popis a ostatních metadatech tak, aby všichni uživatelé s pohledu na data materiálů a jeho použití může mít tento perspektivy zachycení a k dispozici ostatním uživatelům.

Zjistěte, [jak opatřit dat prostředky](data-catalog-how-to-annotate.md) podrobné informace o přidávání poznámek dat prostředky.

## <a name="connect-to-data-assets"></a>Připojení k datové prostředky
V tomto cvičení otevřete majetku dat v integrovaných klientské nástroje (Excel) a nástroji zjistili (SQL Server Management Studio) pomocí informace o připojení.

> [AZURE.NOTE] Je důležité si pamatovat, že katalog dat Azure nemá vám umožní získat přístup ke zdroji dat – to jednoduše usnadňuje zjišťovat a přečetli. Když se připojíte ke zdroji dat, klientské aplikace, které zvolíte používá pověření systému Windows nebo zobrazí výzvu k zadání přihlašovacích údajů podle potřeby. Pokud jste předtím nemáte přístup ke zdroji dat, musíte mít udělený přístup, abyste se mohli připojit.

### <a name="connect-to-a-data-asset-from-excel"></a>Připojení k materiálů dat z aplikace Excel

1. Ve výsledcích hledání vyberte **produkt** . Na panelu nástrojů klikněte na **Otevřít v** a na položku **aplikace Excel**.

    ![Katalog dat Azure – připojení k datové materiálů](media/data-catalog-get-started/data-catalog-connect1.png)
2. Klikněte na **Otevřít** v automaticky otevírané okno stáhnout. Toto chování se může lišit v závislosti na prohlížeči.

    ![Azure katalog dat – Pokud chcete stažený soubor připojení aplikace Excel](media/data-catalog-get-started/data-catalog-download-open.png)
3. V okně **Upozornění zabezpečení aplikace Microsoft Excel** zaškrtněte políčko **Povolit**.

    ![Azure katalog dat – překryvné okno zabezpečení aplikace Excel](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Ponechejte výchozí hodnoty v dialogovém okně **Importovat Data** a klikněte na **OK**.

    ![Azure katalog dat – importu dat aplikace Excel](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Zobrazení zdroje dat v aplikaci Excel.

    ![Azure katalog dat – product tabulky v Excelu](media/data-catalog-get-started/data-catalog-connect2.png)

V tomto cvičení jste připojení k datové prostředky zjistil pomocí katalogu dat Azure. Pomocí portálu Azure katalogu dat můžete připojit přímo pomocí integrována do nabídky **Otevřít v** klientských aplikacích. Taky můžete připojit u jakékoli aplikace, jaká jste pomocí informace o připojení umístění součástí metadata materiálů. Můžete třeba SQL Server Management Studio pro připojení k databázi AdventureWorks2014 pro přístup k datům v datové prostředky registrované v tomto kurzu.

1. Otevřete **SQL Server Management Studio**.
2. V dialogovém okně **připojit k serveru** zadejte název serveru z podokna **Vlastnosti** na portálu katalog dat Azure.
3. Pomocí příslušné ověřovací a přihlašovací údaje pro přístup k materiálů data. Pokud nemáte přístup, pomocí informací v poli **Požádat o přístup** ho.

    ![Azure katalog dat – vyžádání přístupu](media/data-catalog-get-started/data-catalog-request-access.png)

Klikněte na **Zobrazení připojovací řetězec** pro zobrazení a kopírování ADF.NET, ODBC a OLEDB řetězce připojení do schránky pro použití v aplikaci.

## <a name="manage-data-assets"></a>Správa aktiv dat
V tomto kroku zjistěte, jak nastavit zabezpečení majetku data. Katalog dat není uživatelům přístup k datům samotné. Vlastníka zdroje dat mezery nastavuje přístup k datům.

Katalog dat můžete zjišťovat zdroje dat a zobrazit metadata související s zdroje registrované v katalogu. Může nastat situace, ale místo, kam zdroje dat by měl být pouze určitým uživatelům nebo členům skupiny konkrétní viditelný. Podobnému sledu použijete katalogu dat k převzetí vlastnictví majetku registrovaná data v rámci katalogu a pak ovládací prvek viditelnost aktiva vlastníte.

> [AZURE.NOTE] Správa možnosti popisované v tomto cvičení jsou k dispozici pouze v standardní vydání z Azure katalogu dat, ale ne v bezplatné verzi.
V katalogu dat Azure můžete převzetí vlastnictví dat prostředky, přidání dalších vlastníci prostředky dat a nastavení viditelnosti dat prostředky.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Převzetí vlastnictví dat prostředky a omezit viditelnost

1. Přejděte na [domovskou stránku katalog dat Azure](https://www.azuredatacatalog.com). Do textového pole **Hledat** zadejte `tags:cycles` a stiskněte klávesu **ENTER**.
2. Klikněte na některou položku v seznamu výsledků a na panelu nástrojů klikněte na **Převzít vlastnictví** .
3. V části **Správa** panelu **Vlastnosti** klikněte na **Převzít vlastnictví**.

    ![Azure katalog dat – převzetí vlastnictví](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Chcete omezit zobrazení, vyberte v části **viditelnost** **Vlastníci a tyto uživatele** a klikněte na **Přidat**. Zadejte e-mailové adresy uživatelů do textového pole a stiskněte **ENTER**.

    ![Katalog dat Azure – omezit přístup](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Odebrání data majetku

V tomto cvičení používat portál katalog dat Azure odebrat náhled dat z registrovaná data prostředky a odstranění prostředky data z katalogu.

V katalogu dat Azure můžete odstranit jednotlivé materiálů nebo odstranit více prostředky.

1. Přejděte na [domovskou stránku katalog dat Azure](https://www.azuredatacatalog.com).
2. Do textového pole **Hledat** zadejte `tags:cycles` a klepněte na **ENTER**.
3. Vyberte nějakou položku v seznamu výsledků a klikněte na **Odstranit** na panelu nástrojů jak je znázorněno na následujícím obrázku:

    ![Azure katalog dat – odstranit položku mřížky](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    Pokud používáte zobrazení seznamu, je zaškrtnutí políčka nalevo od položky, jak je znázorněno na následujícím obrázku:

    ![Azure katalog dat – položky seznamu odstranit](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    Můžete také vybrat více zdrojů dat a jejich odstranění, jak je znázorněno na následujícím obrázku:

    ![Katalog dat Azure – odstranit více prostředky dat](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Výchozí chování katalogu je umožnit všichni uživatelé zaregistrovat všechny zdroje dat a povolit všem uživatelům odstranit všechny datový zdroj dat, registrované. Možnosti správy součástí standardní vydání z Azure katalogu dat poskytují další možnosti pro převzetí vlastnictví majetku, omezení, které můžete zjistit prostředky, a omezení, který můžete odstranit prostředky.


## <a name="summary"></a>Souhrn

V tomto kurzu prozkoumat základních funkcí katalogu dat Azure, včetně registrace poznámky, zjišťování a Správa datových zdrojů dat enterprise. Teď, když dokončíte kurzu je čas na Začínáme. Můžete začít dnes zaregistrováním zdroje dat, které využívají týmové a pozvání kolegům používání katalogu.

## <a name="references"></a>Odkazy

- [Jak si zaregistrovat prostředky dat](data-catalog-how-to-register.md)
- [Jak zjistit datového prostředky](data-catalog-how-to-discover.md)
- [Jak opatřit prostředky dat](data-catalog-how-to-annotate.md)
- [Jak k dokumentu datové prostředky](data-catalog-how-to-documentation.md)
- [Jak se připojit k prostředky dat](data-catalog-how-to-connect.md)
- [Správa aktiv dat](data-catalog-how-to-manage.md)
