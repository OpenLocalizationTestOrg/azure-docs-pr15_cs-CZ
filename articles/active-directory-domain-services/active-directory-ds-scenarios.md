<properties
    pageTitle="Azure Active Directory Domain Services: Scénáře nasazení | Microsoft Azure"
    description="Scénáře nasazení služby Azure AD domény"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>


# <a name="deployment-scenarios-and-use-cases"></a>Scénáře nasazení a případy použití
V tomto oddílu se podíváme na několik scénářů a případy použití využívajících služby Domain Azure Active Directory (AD).

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Zabezpečení, snadná správa Azure virtuálních počítačích
Azure Active Directory Domain Services můžete použít ke správě Azure virtuálních počítačích optimalizovaného způsobem. Azure virtuálních počítačích můžete připojit k spravovaných doméně, tedy umožňuje podnikové AD pověření k přihlášení. Tento přístup pomáhá zabránit potíže Správa přihlašovacích údajů například zachování místním účtem správce ve všech Azure virtuálních počítačích.

Virtuálních počítačích serveru připojených k spravované domény lze také spravovat a zabezpečit pomocí zásad skupiny. Můžete použít standardní hodnoty vyžaduje zabezpečení na Azure virtuálních počítačích a i zamknout, je v souladu s pokyny pro podnikovou zabezpečení. Pomocí možnosti správy zásad skupiny můžete například omezit přístup k aplikacím, které lze spustit na tyto virtuálních počítačích.

![Zjednodušená správa Azure virtuálních počítačích](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Jak servery a další infrastrukturu dosáhne koncového životnost, Contoso pohybuje mnoho aplikací právě hostovaný místně do cloudu. Jejich aktuální standard IT vyžaduje, aby servery hostování podnikových aplikací musí být doméně a spravovanou pomocí zásad skupiny. Společnosti Contoso správce IT dává přednost domény spojení virtuálních počítačích nasazenou v Azure, usnadnit správy. V důsledku toho správcům a uživatelům se přihlásit pomocí své přihlašovací údaje společnosti. Ve stejnou dobu je možné konfigurovat počítačích dodržovat směrné plány vyžaduje zabezpečení prostřednictvím zásad skupiny. Contoso nechcete mít nasadit, sledovat a spravovat domény řadiče v Azure zajistit Azure virtuálních počítačích. Azure AD Domain Services tedy skvěle hodí tento případ použití.

**Poznámky k nasazení**

Vezměte v úvahu následující fakta pro tento scénář nasazení:

- Spravované domény poskytovanou Azure AD Domain Services poskytují jedné paušální strukturu OU (organizační jednotka) ve výchozím nastavení. Všechny počítače doméně uložena v jedné paušální OU. Můžete však vytvářet vlastní organizačních jednotek.

- Azure AD Domain Services podporuje jednoduché zásad skupiny ve formě předdefinované objekt zásad skupiny jednotlivých uživatelů a počítačů kontejnery. Nelze obrázku obecná OU/oddělením, provést filtrování WMI nebo vytvořit vlastní objekty zásad skupiny.

- Azure AD Domain Services podporuje základní schématu objektu AD počítače. Nelze rozšířit schématu objekt počítače.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Výtah a směny místní aplikace, která používá ověřování vazbu LDAP infrastruktury služby Azure

![Vazbu protokolu LDAP](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso je místní aplikace, která jste si koupili od nezávislý mnoha lety. Aplikace je aktuálně v režimu údržby tak, že nezávislí výrobci softwaru a vyžádání změn do aplikace je výtažkovými pro Contoso. Tato aplikace má frontend založené na webu, který shromáždí přihlašovací údaje uživatele pro pomocí webového formuláře a potom ověřuje uživatele provedením vazbu protokolu LDAP ke službě podnikové Active Directory. Migrace tato aplikace služby Azure infrastruktury libovolný text contoso. Je vhodné funkčnost aplikace je, aniž by bylo všechny změny. Kromě toho měli uživatelé ověření pomocí své existující podnikové přihlašovací údaje a bez nutnosti retrain uživatelům věci dělat jinak. Jinými slovy koncoví uživatelé musí být oblivious kde spuštění aplikace a migraci by měly být průhledné na ně.

**Poznámky k nasazení**

Vezměte v úvahu následující fakta pro tento scénář nasazení:

- Ujistěte se, že aplikace není nutné změnit i zápis do adresáře. Zápis LDAP na spravované domény poskytovanou Azure AD Domain Services nepodporuje.

- Není možné změnit hesla přímo proti spravované domény. Koncoví uživatelé mohli změnit svoje heslo buď pomocí Azure AD samoobslužné heslo změnit mechanismus nebo proti místního adresáře. Tyto změny se automaticky synchronizované k dispozici v spravované domény.


## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Výtah a shift místní aplikace, která používá LDAP číst pro přístup k adresáři infrastruktury služby Azure
Contoso je spuštěna aplikace řádek obchodní (LOB) místní vyvinutý skoro deset let před. Tato aplikace je adresář vědět a byla navržený pro práci s Windows Server AD. Aplikace používá LDAP (Lightweight Directory Access Protocol) zobrazíte informace/atributy o uživatelích ze služby Active Directory. Aplikace není změnit atributy nebo jinak zapisovat do adresáře. Contoso chcete migrovat tato aplikace služby Azure infrastruktury a vyřadit místní hardwaru stárnutí aktuálně hostingu této aplikace. Aplikace nelze přepsat moderní adresář rozhraní API například rozhraní API Azure AD grafu založeného na ostatních. Možnost výtah a shift je požadované proto, kterým aplikace můžete migrovat spustit v cloudu, bez úpravy kód nebo přepsání aplikace.

**Poznámky k nasazení**

Vezměte v úvahu následující fakta pro tento scénář nasazení:

- Ujistěte se, že aplikace není nutné změnit i zápis do adresáře. Zápis LDAP na spravované domény poskytovanou Azure AD Domain Services nepodporuje.

- Ujistěte se, že aplikace nemusí vlastní/rozšířené schématu služby Active Directory. Rozšíření schématu nejsou podporované ve službách Azure AD Domain.


## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Migrace místní služba démon aplikace nebo infrastruktury služby Azure
Některé aplikace se skládají z více úrovní, kde ze úrovní třeba provádět ověřené volání do back-end osy například vrstva databáze. Účty služby Active Directory se běžně používají pro tyto případy použití. Je možné výtah a shift tyto aplikace služby Azure infrastruktury a použití Azure AD Domain Services pro identitu potřeby tyto aplikace. Můžete použít stejný účet služby, která se synchronizuje ze svého místního adresáře Azure AD. Případně můžete nejdřív vytvořit vlastní OU a vytvořit účet samostatný služby v této OU nasadit tyto aplikace.

![Použití WIA účet služby](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso je uživatelském softwarová trezoru aplikace, která obsahuje webových front-end, SQL server a back-end FTP serveru. Integrované v systému Windows ověření účtů služeb slouží k ověření webových front-end k serveru FTP. Webový front-end nastaven na Spustit jako účet služby. Je back-end server nakonfigurovaný k povolit přístup z účtu služby front-end webu. Není k dispozici pro nasazení domény řadiče domény virtuálního počítače v cloudu, aby přesunout tato aplikace služby Azure infrastruktury dává přednost contoso. Společnosti Contoso správce IT můžete nasadit servery hostingu webových front-end, SQL server a serveru FTP Azure virtuálních počítačích. Tyto počítače se pak připojen k spravovaných domain Azure AD Domain Services. Pak můžete používají stejný účet služby v jeho místního adresáře pro účely ověření v aplikaci. Tento účet služby se synchronizuje s spravovaných domain Azure AD Domain Services a je k dispozici.

**Poznámky k nasazení**

Vezměte v úvahu následující fakta pro tento scénář nasazení:

- Ujistěte se, že aplikace používá uživatelského jména a hesla pro ověřování. Ověřování na základě certifikát/čipové karty se nepodporuje Azure AD Domain Services.

- Není možné změnit hesla přímo proti spravované domény. Koncoví uživatelé mohli změnit svoje heslo buď pomocí Azure AD samoobslužné heslo změnit mechanismus nebo proti místního adresáře. Tyto změny se automaticky synchronizované k dispozici v spravované domény.


## <a name="azure-remoteapp"></a>Azure RemoteApp
Azure RemoteApp umožňuje správcům společnosti Contoso vytvořit kolekci doméně. Tato funkce umožňuje vzdálené aplikace hodit Azure RemoteApp spustit v doméně počítačích a získat přístup ke dalších zdrojů pomocí ověřování integrované v systému Windows. Contoso zajistit spravovaných doménu používané kolekcí doméně Azure RemoteApp použít Azure AD Domain Services.

![Azure RemoteApp](./media/active-directory-domain-services-scenarios/azure-remoteapp.png)

Další informace o tento scénář nasazení, najdete v článku blogu služby vzdálené plochy s názvem [výtah a shift úloh v Azure RemoteApp a Azure AD doménových služeb](http://blogs.msdn.com/b/rds/archive/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services.aspx).
