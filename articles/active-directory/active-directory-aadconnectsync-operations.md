<properties
   pageTitle="Azure AD Connect synchronizace: provozní úkoly a informace, které | Microsoft Azure"
   description="Toto téma popisuje provozní úkoly pro synchronizaci Azure AD Connect a jak připravit pro provoz této součásti."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect synchronizace: provozu a pozornost
Cílem toto téma je popsat provozní úkoly Azure AD Connect synchronizace.

## <a name="staging-mode"></a>Pracovní režimu
Režim pracovní lze použít pro několik možností, včetně:

-   Dostupnost.
-   Otestujte a nasazení nové změny konfigurace.
-   Dejte novému serveru a vyřadit z provozu původní.

Se serverem v režimu pracovní můžete provést změny konfigurace a zobrazit náhled změn před prováděním active server. Také umožňuje spuštění úplného importu a úplné synchronizaci pro ověření, že všechny změny očekává před provedením těchto změn do provozním prostředí.

Během instalace můžete vybrat serveru být **přípravu režimu**. Tato akce aktivuje serveru pro import a synchronizace, ale nespustí všechny export. Serveru v režimu pracovní neběží synchronizaci hesel nebo zpětným heslo, i když vyberete tyto funkce během instalace. Při vypnutí pracovní režimu serveru začne export umožňuje synchronizaci hesel a umožňuje zpětným heslo.

Můžete vynutit export pomocí Správce služby synchronizace.

Serveru v pracovní režimu stále změny ze služby Active Directory a Azure AD. Má vždy kopii posledním změnám a můžou velmi rychlé pořizování přes povinnosti jiný server. Pokud uděláte změny konfigurace serveru primární, je zodpovědní za to stejné změny na server přípravu režimu.

Výsledky se znalosti starší technologií synchronizace pracovní režimu se liší od obsahuje vlastní databázi SQL serveru. Tato architektura umožňuje pracovní režimu serveru nacházet v různých datacentra.

### <a name="verify-the-configuration-of-a-server"></a>Ověření konfigurace serveru
Chcete-li použít tento postup, postupujte takto:

1. [Příprava](#prepare)
2. [Importovat a synchronizovat](#import-and-synchronize)
3. [Ověření](#verify)
4. [Přepnout active server](#switch-active-server)

#### <a name="prepare"></a>Příprava

1. Instalace Azure AD Connect, vyberte **přípravu režimu**a zrušte zaškrtnutí políčka **Spustit synchronizaci** na poslední stránce Průvodce instalací. Tohoto režimu umožňuje spustit modul synchronizovat ručně.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Znak vypnout/přihlásit a z nabídky start vyberte **Služby synchronizace**.

#### <a name="import-and-synchronize"></a>Importovat a synchronizovat

1. Vyberte **spojovací čáry**a vyberte první spojnice typu **Active Directory Domain Services**. Klikněte na příkaz **Spustit**, vyberte **úplné import**a **OK**. Udělejte tyto kroky pro všechny spojnice tohoto typu.
2. Vyberte spojnici s typem **Azure Active Directory (Microsoft)**. Klikněte na příkaz **Spustit**, vyberte **úplné import**a **OK**.
3. Zkontrolujte, že je stále vybrána karta spojnice. Pro každou spojnici s typem **Active Directory Domain Services**klikněte na příkaz **Spustit**, vyberte **Synchronizace Delta**a **OK**.
4. Vyberte spojnici s typem **Azure Active Directory (Microsoft)**. Klikněte na příkaz **Spustit**, vyberte **Synchronizace Delta**a **OK**.

Nyní máte připravené export se změní na Azure AD a místní AD (Pokud používáte hybridní nasazení Exchange). Další kroky umožňují kontrolovat, co se brzo měnit skutečně začnete exportovat do adresáře.

#### <a name="verify"></a>Ověření

1. Zahájení do příkazového řádku a přejděte na`%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Spusťte:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Název spojnice najdete v synchronizaci služby. Obsahují podobná "contoso.com – AAD" název pro Azure AD.
3. Spusťte:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Máte otevřený soubor ve složce % temp % s názvem export.csv, který může být zkontrolován v aplikaci Microsoft Excel. Tento soubor obsahuje všechny změny, které chcete exportovat.
5. Proveďte potřebné změny dat nebo konfiguraci a spuštění tento postup opakujte (importovat a synchronizovat a ověření) až do změny, které mají být exportovány očekává.

**Principy export.csv souboru**

Většina souboru je zřejmé. Některé zkratky pochopit obsahu:

- OMODT – Změna typu objektu. Určuje, zda je operaci úrovni objekt přidat, aktualizovat nebo odstranit.
- AMODT – typ atributu poslední změny. Určuje, zda je operaci úrovni atribut přidat, aktualizovat nebo odstranit.

Pokud je hodnota atributu s více hodnotami, se zobrazí není každé změně. Zobrazený jenom počet hodnot přidat nebo odebrat.

#### <a name="switch-active-server"></a>Přepnout active server

1. Nyní aktivní serveru vypnout server (DirSync/FIM/Azure AD Sync) tak, aby není exportu Azure AD nebo ji nastavit v pracovní režim (Azure AD Connect).
2. Spusťte Průvodce instalací na serveru **přípravu režimu** a zakažte **přípravu režimu**.
![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Obnovení havárie
Součástí provedení návrhu je plán toho, co dělat, když je selhání kde ztratíte serveru synchronizace. Použití a které závisí na několika faktorech různých modely:

-   Co je odchylku pro není možné provést změny k objektům v Azure AD během prostoje?
-   Pokud používáte synchronizace hesel, uživatelů přijmout mají použít staré heslo v Azure AD pro případ, budou ho změnit místní?
-   Máte závislost na data v reálném operací, jako je zpětným heslo?

V závislosti na odpovědi na tyto otázky a zásady ve vaší organizaci jednu z následujících postupů lze provést:

-   Opětovné sestavení v případě potřeby.
-   Máte náhradní úsporném server jmenoval **přípravu režimu**.
-   Použití virtuálních počítačích.

Pokud nepoužíváte předdefinované databáze SQL serveru Express, byste měli taky zkontrolovat v části [Dostupnost SQL](#sql-high-availability) .

### <a name="rebuild-when-needed"></a>Opětovné sestavení v případě potřeby
Životaschopné strategie je plánování serveru opětovného vytvoření v případě potřeby. Instalace obvykle sync engine a proveďte prováděny počáteční importovat a synchronizovat může být několik hodin. Pokud není k dispozici náhradní serveru, je možné používat dočasně řadiče domény hostovat modul synchronizace.

Server engine synchronizace neukládá každý stát o objekty tak, aby databázi můžete sestaví z dat ve službě Active Directory a Azure AD. Atribut **sourceAnchor** se používá ke spojení od místním prostředím a cloudu. Pokud znovu vytvoříte na server s existující objekty místním prostředím a cloudem, a potom modul synchronizace porovnává tyto objekty společně znovu na přeinstalace. Věci, které potřebujete k dokumentu a uložit se změny konfigurace serveru, například filtry a synchronizace pravidla. Než začnete synchronizace ji znovu použít tyto vlastní konfigurace.

### <a name="have-a-spare-standby-server---staging-mode"></a>Máte náhradní úsporném serveru - přípravu režimu
Pokud máte složitější prostředí, doporučujeme s jedním nebo více úsporném servery. Během instalace můžete povolit serveru být **přípravu režimu**.

Další informace najdete v článku [pracovní režimu](#staging-mode).

### <a name="use-virtual-machines"></a>Použití virtuálních počítačích
Běžná oznámení a podporované metoda je spuštění modul Synchronizace virtuálního počítače. V případě hostiteli problém, můžete obrázek se serverem sync engine migrovat do jiného serveru.

### <a name="sql-high-availability"></a>Dostupnost SQL
Pokud nepoužíváte serveru SQL Server Express, které jsou součástí Azure AD Connect, pak dostupnost pro systém SQL Server uvažovat rovněž. Podporované jenom dostupnost řešením je SQL clusterů. Nepodporované řešení zahrnovat odrážející strukturu a vždy o.

## <a name="next-steps"></a>Další kroky

**Přehled témat**  

- [Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace](active-directory-aadconnectsync-whatis.md)  
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)  
