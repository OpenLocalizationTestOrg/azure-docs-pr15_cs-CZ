<properties
    pageTitle="Co je nového v Azure sada nástrojů pro Eclipse"
    description="Další informace o nejnovějších funkcích v Azure sada nástrojů pro Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh694270.aspx -->

# <a name="whats-new-in-the-azure-toolkit-for-eclipse"></a>Co je nového v Azure sada nástrojů pro Eclipse

## <a name="azure-toolkit-for-eclipse-releases"></a>Azure sada nástrojů zatmění verzích

Tento článek obsahuje informace o různých verzí a nejnovější aktualizace Azure sada nástrojů pro Eclipse.

> [AZURE.NOTE] Je také Azure sada nástrojů pro integrovaném vývojovém IntelliJ prostředí. Další informace najdete v tématu [Azure nástrojů pro IntelliJ].

### <a name="august-26-2016"></a>26 srpen 2016

Azure sada nástrojů pro Eclipse – 2016 srpen vydání zahrnuje tyto výhody:

* **Vlastní JDK rozdělení**. Azure sada nástrojů pro Eclipse teď podporuje určující nasazení libovolného verze JDK na váš web Appu Azure kontejner:
  - Kromě JDKs poskytovanou Azure můžete také z široké výběru zulština OpenJDK verzí k dispozici v Azure Azul systémy.
  - Můžete taky určit vlastní JDK rozdělení, pokud ho uložte jako soubor ZIP ke svému účtu úložiště.
* **Vylepšení do listu Exploreru Azure**:
  - Podpora správy virtuálního počítače pomocí nový model správce prostředků v Azure: můžete seznam, vytvářet a odstraňovat zdroje na základě Správce virtuálních počítačích aniž byste museli opustit integrovaném vývojovém prostředí.
  - Podpora správy účtu úložiště objektů blob správcem v Azure zdroje, které doplňuje existujících funkcí pro správu účtů "klasické" úložiště.
* **Ovladač Microsoft JDBC 6.0 pro systém SQL Server**. Poslední ovladači JDBC obsahuje tato aktualizace pro Microsoft SQL Server (verze 6.0), což je teď součástí knihovně, ve které můžete snadno přidat k projektům jazyka Java, čímž nahrazení starší verze.

### <a name="june-29-2016"></a>29 června 2016

Azure sada nástrojů pro Eclipse – červen 2016 vydání zahrnuje tyto výhody:

* **Požadavek Java 8**. Azure sada nástrojů pro Eclipse vyžaduje Java 8, i když tento požadavek se týká jen této sady - aplikace můžete dál používat všechny verze Java podporovaných službami Azure.
* **Podpora pro nejnovější JDKs Java**. Nejnovější verze Java JDKs jsou teď nepodporuje Azure sada nástrojů pro Eclipse.
* **Podpora pro Azure SDK v2.9.1**. Nejnovější verzi Azure SDK je teď minimální předpokladem pro Azure sada nástrojů pro Eclipse.
* **Integrované vzorky**. Azure sada nástrojů pro Eclipse nabízí několik ukázkové aplikace aby vývojáři začít pracovat.
* **Integrace nástroj HDInsight**. V Azure HDInsight nástroje jsou teď součástí Azure sada nástrojů pro Eclipse. Další informace najdete v tématu [HDInsight modul plug-in nástroje pro Eclipse].
* **Vzdálené ladění Java webových aplikací**. Azure sada nástrojů pro Eclipse nyní podporuje vzdálené ladění Java web apps na aplikaci služby Azure.
* **Podpora pro vzhled zatmění Luna verze.** Minimální požadovaný zatmění integrovaném vývojovém prostředí je nová verze vzhled Luna.

### <a name="april-12-2016"></a>12 duben 2016

Azure sada nástrojů pro Eclipse – 2016 s dubnovou vydání zahrnuje tyto výhody:

* **Podpora pro Azure SDK v2.9.0**. Nejnovější verzi Azure SDK je teď minimální předpoklad pro Azure sada nástrojů pro Eclipse.
* **Různé použitelnosti, rychlostí reakce nejnovější aktualizace související s podpory Azure v prohlížeči**. Celá řada optimalizaci výkonu v jak této sady informuje uživatele o s Azure výsledek ve více neodpovídá uživatelského rozhraní.
* **Možnost odstranit existující kontejner webové aplikace v Azure z v rámci zatmění**. Azure sada nástrojů pro Eclipse umožňuje odstranit existující Azure Web kontejner aniž byste museli opustit zatmění.

### <a name="march-7-2016"></a>7 březen 2016

Azure sada nástrojů pro Eclipse – 2016 březen vydání zahrnuje tyto výhody:

* **Podpora rychlého nasazení lightweight Java aplikací**. Azure sada nástrojů pro Eclipse nyní podporuje rychlého nasazení lightweight aplikace Java do Azure webové aplikace kontejnery tak nasazení aplikace Java trvá sekund místo minut.
* **Podpora pro správu webové aplikace pomocí zobrazení Průzkumníka Azure**. Zobrazení Průzkumníka Azure v této sady nyní podporuje pro seznam, spuštění a ukončení Azure Web Apps.
* **Aktualizace Tomcat, molo a zulština OpenJDK rozdělení**. Azure sada nástrojů pro Eclipse podporuje aktualizované verze Tomcat, molo a zulština OpenJDK Java nasazení do služby Azure cloud services.

### <a name="january-4-2016"></a>4 leden 2016

Azure sada nástrojů pro Eclipse – 2016 leden vydání zahrnuje tyto výhody:

* **Podpora pro zulština OpenJDK aktualizace**. Další informace najdete v tématu [Azul systémy webovou stránku pro OpenJDK zulština].
* **Aktualizace Tomcat a molo rozdělení**. Byly aktualizovány molo a Tomcat distribuce, které jsou k dispozici na Microsoft Azure pomocí nástrojů Azure pro Eclipse.
* **Funkce dostupná mezi zatmění a IntelliJ sadách pro Azure**. Sada nástrojů Azure zatmění a [Azure sada nástrojů pro IntelliJ] podporuje stejnou sadu funkcí.

### <a name="september-1-2015"></a>1 dne 2015

Azure sada nástrojů pro Eclipse – vydání září 2015 zahrnuje tyto výhody:

* **Podpora pro zulština OpenJDK aktualizace**. Další informace najdete v tématu [Azul systémy webovou stránku pro OpenJDK zulština].
* **Aktualizace Tomcat a molo rozdělení**. Byly aktualizovány molo a Tomcat distribuce, které jsou k dispozici na Microsoft Azure pomocí nástrojů Azure pro Eclipse. (Tyto distribuce umožňuje vývojářům vytvořit rychlý vývoj a otestovat projektů s Azure sada nástrojů pro Eclipse.
* **Podpora pro automaticky aktualizované Tomcat a molo odkazy**. Kromě konkrétní verzí Tomcat a molo, které jsou k dispozici v Azure, vývojáři teď odkazovat na rozdělení jen "nejnovější (automaticky aktualizován)", která se automaticky aktualizuje a nejnovější rozdělení každý hlavní verzi molo nebo Tomcat při příštím role instance se z koše. (Recyklace automaticky, ale vývojáři můžete spustit ručně Koš portálu Azure.) Nová funkce znamená, aby vývojáři nemusíte přeinstalujte aplikaci, která bude mít možnost jejich serverový software aktualizace. (
*  Tato funkce je momentálně určeny pouze pro účely vývoj a testování nebo jiných velice důležité aplikace a se nedoporučuje výroby.)
* **Zobrazení Průzkumníka azure pro objekty BLOB, dotazů a tabulek v Azure úložiště**. Vývojáři provádět sadu běžné úkoly s jejich úložiště artefakty přímo z integrovaném vývojovém zatmění prostředí. Příklad: odstranění, nahrávání nebo stahování objektů BLOB.

### <a name="august-1-2015"></a>1 srpen 2015

Azure sada nástrojů pro Eclipse – vydání srpen 2015 zahrnuje tyto výhody:

* **Aplikace přehledy přístrojového vybavení správy klíčů**. Tuto aktualizaci můžete získat, vytvářet a spravovat své aplikace přehledy přístrojového vybavení klíče přímo z integrovaném vývojovém zatmění prostředí.
* **Ovladač Microsoft JDBC 4.1 pro systém SQL Server**. Tato aktualizace podporuje nejnovější ovladače JDBC Microsoft SQL Server.
* **Verze 2.7 Azure SDK**. Tato nejnovější aktualizace SDK Azure je nové předpoklad sada nainstalovaná ve Windows. (Poznámka: Toto není potřeba v systému Windows operačních systémech.)
* **Podpora pro v7 zulština OpenJDK aktualizovat**. Další informace najdete v tématu [Azul systémy webovou stránku pro OpenJDK zulština].

### <a name="may-1-2015"></a>1 květen 2015

Azure sada nástrojů pro Eclipse – vydání květen 2015 zahrnuje tyto výhody:

* **Vylepšení uživatelského rozhraní pro výběr serveru**. Tato verze zjednodušuje používání nástrojů v operačních systémech systému Windows.
* **Podpora pro Maven projekty**. Tato verze podporuje Maven projekty jako aplikace, které tato sada můžete nasadit u Azure a konfigurovat přehledy aplikace.
* **Verze 2.6 Azure SDK**. Tato nejnovější aktualizace SDK Azure je nové předpoklad sada nainstalovaná ve Windows. (Poznámka: Toto není potřeba v systému Windows operačních systémech.)
* **Upgrade nasazení namísto publikovat**. Pokud projekt nasazení jsou publikování, když už je živou v předchozí verzi, této sady teď používá funkce upgradu na Azure nasazení místo vypnutí předchozí nasazení a publikování úplně stejně jako v minulosti. To umožňuje cloudové služby na spuštění bez přerušování kdykoli je to možné, pomáhají dosáhnout dostupnost i během aktualizace a znovu publikování zrychluje.
* **Podpora nejnovější v8 zulština OpenJDK: - aktualizovat 40**. Další informace najdete v tématu [Azul systémy webovou stránku pro OpenJDK zulština].

### <a name="march-9-2015"></a>9. březnem 2015

Azure sada nástrojů pro Eclipse – vydání březnem 2015 zahrnuje tyto výhody:

* **Podpora pro Mac, se systémem Ubuntu a další provedeních Linux**. Této verzi Azure sada nástrojů pro Eclipse přidá podporu pro Mac OS a několik platformy Unix tak, aby vývojáři můžou nainstalovat sadu k vytváření, konfigurace a publikování projektů Java ke Cloudovým službám Azure (PaaS) z zatmění spuštěné v operačních systémech než Windows.

>[AZURE.NOTE] Tato funkce není ve verzi preview a není vhodné pro použití v provozním prostředí. Žádná Zákaznická podpora smlouva úrovni služeb (SLA), ale všechny názory si vážíme a podporovat.

* **Modul plug-in nové přehledy aplikace**. Vývojáři můžou teď konfigurace telemetrie automatické serveru pomocí aplikace přehledy v Azure.
* **Automatizace nasazení a systémem příkazového řádku**. Tato funkce umožňuje vývojářům automatizovat publikování pro novějších verzích jejich nasazení pomocí a mimo zatmění. Skript předem vygenerovaných se konfigurují projektu po prvním nasazení z zatmění a následné nasazení umožňuje skript plně automatizovat nasazení prostřednictvím příkazového řádku pouze.
* **Dostupnost tomcat a molo na Azure jednodušší a rychlejší nasazení**. Vývojáři teď můžete odkázat různé verze Tomcat a molo, které jsou k dispozici v Azure přímo místo toho muset nahrát Java server svých účtů (nebo prostřednictvím této sady), takže je potřeba nahrát server Java rychlé, Začínáme – scénářích.
* **Metoda klávesových zkratek pro publikování jazyka Java webových aplikací web apps ke Azure cloudovým službám**. Snížit křivky výukové jednoduché vývoj a testování scénáře, můžete vývojáři nyní publikovat aplikací Java více přímo na Azure. Místo museli proces vytváření a konfigurace Azure nasazení projektu, má být nasazené aplikací s výchozí instanci v8 Tomcat: a zulština JVM (OpenJDK).

### <a name="january-30-2015"></a>30 ledna 2015

Azure sada nástrojů pro Eclipse – vydání ledna 2015 zahrnuje tyto výhody:

* **Podpora pro IBM® WebSphere® aplikace serveru Svoboda Core**. Tato verze přidá IBM WebSphere aplikace Server Svoboda Core do seznamu podporovaných aplikací servery ze kterých je možné instalovat Azure této sady. Toto nejnovější přidání rozbalí aktuální seznam aplikací servery, které podporují &quot;mimo pole&quot; pomocí nástrojů, které už zahrnutý v různých verzích Tomcat molo, JBoss a GlassFish.
* **Zařazení aplikace přehledy SDK**. Tuto knihovnu nově vydaná klientského rozhraní API (v0.9.0) je teď součástí balíček pro Azure knihovny Java.
* **Aktualizace balíček pro Azure knihovny Java**. Tato aktualizace zahrnuje Azure knihovny Java v0.7.0 a rozhraní API úložiště klienta v2.0.0, i aplikace přehledy SDK v0.9.0 nově vydaná.

### <a name="november-12-2014"></a>12 listopadu 2014

Azure sada nástrojů pro Eclipse – vydání listopadu 2014 zahrnuje tyto výhody:

* **Podpora pro Azure SDK 2,5**. Tato nejnovější aktualizace SDK Azure je nové předpokladem pro tuto sadu nástrojů.
* Domovské stránce **podpory pro aktualizovanou verzi zulština OpenJDK v1.8, v1.7 a v1.6 balíčků**. Další informace najdete v tématu [Azul systémy webovou stránku pro OpenJDK zulština].
* **Podpora pro nové velikosti standardní D pro cloudové služby**, které nabídky lepší výkon a další paměť zdroje. Další informace najdete v tématu [virtuálního počítače a cloudové služby velikosti Azure].

### <a name="october-17-2014"></a>17 října 2014

Azure sada nástrojů pro Eclipse – vydání října 2014 zahrnuje tyto výhody:

* **Zvýšení výkonu v okně Publikovat do cloudu scénáře**. Načtení informace o předplatném je rychleji, když uživatelé mají víc předplatných a úložiště účtů.
* **Podpora pro aktualizovanou verzi balíček v1.8 zulština OpenJDK**. Další informace najdete v tématu [Azul systémy webovou stránku pro OpenJDK zulština].
* **Podpora platnosti starší verze aplikace 3 JDKs stran**. Nepoužívají se balíčků JDK už zobrazí v rozevírací nabídce nových projektů nasazení. Referenční zastaralé balíčků JDK zůstanou můžete provést přihlášení se, ale doporučuje se upgradovat tyto projekty spolehnout se na nejnovější existující projekty.
* **Aktualizované verze balíček pro Azure knihovny Java klientského rozhraní API knihovny**. Další informace najdete v tématu [Microsoft Azure klientského rozhraní API].
* **Opravy chyb.** Tato verze obsahuje několik různých opravy chyb, které byly založeny na sestavy uživatele a testování.

### <a name="august-5-2014"></a>5 srpen 2014

Azure sada nástrojů pro Eclipse – vydání srpen 2014 zahrnuje tyto výhody

* **Podpora Azure SDK 2.4.** Starší verze aplikace sady nástrojů zatmění nebude fungovat u tohoto nově vydanou SDK.
* **Aktualizované verze v1.6 zulština OpenJDK 1.7 a v1.8 balíčků.** Další informace najdete v tématu [Azul systémy webovou stránku pro OpenJDK zulština].
* **Aktualizovanou verzi balíček pro knihovny Azure Java klientského rozhraní API knihovny.** Další informace najdete v tématu [Microsoft Azure klientského rozhraní API].
* **Podpora pro nejnovější publikovat nastavení formátu.** Podpora jsme přidali verze 2.0 formátu nastavení publikování.
* **Změny architektuře za publikovat do cloudu funkce.** Tato sada teď používá nově vydanou Microsoft Azure klientského rozhraní API jazyka Java pro podporu publikovat do cloudu.
* **Opravy chyb.** Tato verze obsahuje počet uživatel požadoval opravy chyb.

### <a name="june-12-2014"></a>12. června 2014

Azure sada nástrojů pro Eclipse – vydání června 2014 je menší údržby aktualizacích, které poskytuje má tyto výhody:

* **Podpora v1.8 balíčku zulština OpenJDK.** Další informace najdete v tématu [Azul systémy webovou stránku pro OpenJDK zulština].
* **Aktualizované verze v1.6 zulština OpenJDK a 1.7 balíčků.** Další informace najdete v tématu [Azul systémy webovou stránku pro OpenJDK zulština].
* **Aktualizovanou verzi balíček pro knihovny Azure Java klientského rozhraní API knihovny.** Další informace najdete v tématu [Microsoft Azure klientského rozhraní API].
* **Opravy chyb.** Tato verze obsahuje počet uživatel požadoval opravy chyb.

### <a name="april-4-2014"></a>4 dubna 2014

Modul plug-in Azure pro Eclipse – vydání dubna 2014 vydala. Jedná se o aktualizaci doprovodným verzi Azure 2.3 SDK, která jsou předpokladem a bude možné automaticky stáhnout při instalaci modulu plug-in. Protože náhled únor 2014 obsahuje tato aktualizace nových funkcí, opravy chyb a některé vylepšení použitelnosti názory řízené úsilím:

* **Podpora pro verzi Azure SDK 2.3.** Modul plug-in Azure pro Eclipse – vydání dubna 2014 vyžaduje Azure SDK 2.3. Pokud chcete použít nové modul plug-in, pokud už nemáte Azure SDK 2.3, se výzva umožňuje instalaci. Nepoužívejte Azure SDK 2.3 s předchozími verzemi aplikace modulu plug-in.
* **Upgrade aplikace bez dokončení balíčku nasazení.** Při nasazení aplikace Java, které jsou součástí projektu, modul plug-in teď automaticky uloží je do svého účtu vybrané úložiště, aby mohli aktualizovat a odpadkového instance role pro nasazení nejnovější aplikaci bitů aniž by bylo nutné znovu vytvořit a přeinstalujte celého balíčku.
* **Tomcat 8 je nyní rozpoznaný aplikační server.** Pokud vyberete adresář instalace Tomcat 8 v počítači na kartě **Server** obrazovky s dialogem **Azure nasazení projektu** , modul plug-in teď automaticky rozpozná ho a nebude možné instalovat Tomcat 8 automatizovaných způsobem, podobně jako ke starším verzím Tomcat už v seznamu.
* **Azul zulština OpenJDK balíček aktualizace: 47 aktualizace v1.7 aktualizace 51 a v1.6.** Efektivní s touto verzí systému Azul otevřít JDK zulština v7 balíček aktualizace 51 neexistuje. Otevřít JDK zulština v6 balíčků spustit také, jsou k dispozici, počínaje aktualizace 47. Tyto aktualizace jsou kromě balíček dřív k dispozici v7 otevřít JDK zulština aktualizovat 45, aktualizujte 40 a aktualizovat 25.
* **Podpora A8 a A9 Microsoft Azure Virtual Machine velikost.** Teď nasazením do cloudové služby na vysoké paměti A8 a velikosti A9 virtuálního počítače. Další informace o těchto velikostí OM najdete v článku [virtuální počítač a velikosti cloudové služby Azure].
* **Automatické přesměrování z HTTP na HTTPS pro SSL s podporou role.** Cloudové služby obsahuje pouze role HTTPS, pokud žádost o uživatele HTTP, bude automaticky přesměrovávat na HTTPS. Je potřeba vytvořit samostatné role pro zpracování žádostí o HTTP.
* **Expresní emulátoru používaný pro místní emulace.** Azure Express emulátoru teď slouží jako emulátor při ladění aplikací místně.
* **Azure je přejmenovaný jako Microsoft Azure.** Uživatelské rozhraní obrazovky teď odrážela, že Azure byl přejmenované a už s názvem Azure.

### <a name="february-6-2014"></a>6 února 2014

Modul plug-in Azure pro Eclipse: únor 2014 vydala náhled. Přináší nových funkcí, opravy chyb a některé vylepšení použitelnosti názory řízené úsilím od náhled říjen 2013:

* **Podpora pro odstranění SSL.** Odstranění Secure Sockets Layer (SSL) je přidán jako funkce, která umožňují snadno podporu HTTPS Hypertext Transfer Protocol zabezpečené () v nasazením Java na Azure, aniž by bylo potřeba nakonfigurovat protokol SSL ve aplikační server Java. To platí zejména spřažení relace a/nebo ověřený scénáře komunikace. Třeba při používání služby Řízení přístupu (ACS) filtr, který už sada nástrojů nepodporuje. Další informace najdete v tématu [Odstranění SSL] a [jak použít protokol SSL odstranění].
* **GlassFish 4 je nyní rozpoznané aplikační server.** Pokud vyberete adresář instalace GlassFish 4 v počítači na kartě **Server** obrazovky s dialogem **Azure nasazení projektu** , modul plug-in teď automaticky rozpozná ho a nebude možné instalovat GlassFish OSE 4 automatické způsobem, podobně jako verze GlassFish OSE 3 už v seznamu.
* **Azul zulština OpenJDK balíček aktualizace 45.** Efektivní s touto verzí systému Azul zulština (otevřít JDK v7 balíček) 45 je k dispozici aktualizace; je to kromě dřív k dispozici aktualizace 40 a aktualizace 25.
* **Podporu "automatické" pro soukromé porty koncového bodu.** Privátní port můžete nastavit, aby automatické pro zadávání koncové body a interní koncové body chcete, aby Azure automaticky přiřadit port této koncový bod. Je dříve pouze přiřadit konkrétní číslo portu.
* **Podpora pro přizpůsobení název certifikátu (CN) k vytvoření certifikátu podepsaného svým držitelem uživatelského rozhraní.** Dříve se stejným názvem pevně používal pro všechny nové certifikáty; Teď můžete použít vlastní název certifikátu k rozlišení mezi více certifikátů na portálu Azure používá pro jiné účely.
* **Azure nástrojů:** Azure nástrojů má aktualizovaný s následující změny: 
    * ![][ic710876]Tato ikona jsme přidali **Nové Azure nasazení projektu**.
    * ![][ic710877]Tato ikona jsme přidali jako zkratku dialog Vytvoření certifikátu podepsaného svým držitelem.
* **Podpora velikost A5 Azure virtuálního počítače.** Teď můžete nasadit do cloudové služby vysoké paměti počítače virtuální A5 velikost. Další informace o této velikosti OM najdete v článku [virtuální počítač a velikosti cloudové služby Azure].
* **Podpora pro systém Microsoft Windows serveru 2012 R2.** Teď můžete vybrat Windows serveru 2012 R2 jako operačního systému cloudu.

### <a name="october-22-2013"></a>22 října 2013

Modul plug-in Azure pro Eclipse: říjen 2013 Preview vydala. Přináší nových funkcí, opravy chyb a některé vylepšení použitelnosti názory řízené úsilím od náhled září 2013:

* **Podpora pro verze Azure SDK 2.2.** Modul plug-in Azure pro Eclipse - říjen 2013 náhled podporuje Azure SDK 2.2. Modul plug-in dál fungovat s Azure SDK 2.1 a nainstaluje automaticky Azure SDK 2.2 Pokud už nemáte aspoň Azure SDK 2.1 nainstalovaný.
* **Azul zulština OpenJDK balíček aktualizace 40.** Podle dne 2013 se ohlásí zobrazit náhled, umožňuje modul plug-in nyní pomocí třetích stran-za předpokladu, že JDK přímo na Azure, aniž by bylo potřeba nahrát vlastní JDK. Ve verzi 2013 říjen Azul systému zulština (otevřít JDK v7 balíček) aktualizace 40 je teď k dispozici. Toto je kromě původně publikovaných aktualizace 25.
* **Odkaz na nasazení cloudu v protokolu činnosti.** V rámci protokol aktivitu Azure nasazení má stav **Publikovaná**, můžete kliknutím **publikované** takové alternativní řešení je teď odkaz na nasazení; nasazení potom otevře se v prohlížeči. (Stav **Publikováno** byla dříve označena **spuštěný**.)
* **Výběr OS cílové jsou dostupné na webu publikování.** Dialogové okno **Publikovat Azure** obsahuje nové pole, **S operačním systémem cílové**, který umožňuje intuitivnější můžete nastavit cíl operační systém.
* **Automatické přepsat předchozí nasazení.** Dialogové okno **Publikovat Azure** obsahuje nové zaškrtávací políčko **předchozím nasazení přepsat**. Pokud toto políčko zaškrtnuté, kdy je publikován nový nasazení automaticky přepíše předchozí nasazení; by zaznamenáte &quot;409 konflikt&quot; problémy při publikování do stejného umístění bez první zrušení publikování předchozí nasazení.
* **Molo 9 je nyní rozpoznaný aplikační server.** Pokud vyberete adresář instalace 9 molo v počítači na kartě **Server** obrazovky s dialogem **Azure nasazení projektu** , modul plug-in teď automaticky rozpozná ho a nebude možné instalovat molo 9 automatické způsobem, podobně jako ke starším verzím molo už v seznamu.
* **Přidání role v místní nabídce projektu.** Místní nabídka projektu **Azure** nyní obsahuje nová položka nabídky **Přidat roli**, která obsahuje rychlejší a další zjistitelný způsob, jak lze přidat novou roli Azure projektu.
* **Aktualizace balíček pro knihovny Azure Java knihovny.** To je založena na 0.4.6 verzi [Microsoft Azure klientského rozhraní API].

### <a name="september-25-2013"></a>25 září 2013

Modul plug-in Azure pro Eclipse - září 2013 Preview vydala. Přináší nových funkcí, opravy chyb a některé vylepšení použitelnosti názory řízené úsilím od náhled srpen 2013:

* **Možnost nasadit balíček Azul zulština OpenJDK k dispozici v Azure.** Nové možnosti bylo přidáno při určování JDK pomocí služby Azure nasazení. Pomocí této možnosti nástroje můžete nasazovat balení JDK třetích stran přímo na Azure cloudu, aniž byste museli nahrát vlastní. Systémy Azul poskytuje první takové balíček jen zulština podle OpenJDK, které můžete nyní nasazených pomocí této možnosti.
* **Aktualizace balíček pro knihovny Azure Java knihovny.** To je založena na 0.4.5 verzi [Microsoft Azure klientského rozhraní API].

### <a name="august-1-2013"></a>1 srpen 2013

Modul plug-in Azure pro Eclipse - srpen 2013 Preview vydala. Jedná se o aktualizaci doprovodným verze 2.1 SDK Azure, což je předpoklad a bude možné automaticky stáhnout při instalaci modulu plug-in. Protože náhled červenec 2013 obsahuje tato aktualizace nových funkcí, opravy chyb a některé vylepšení použitelnosti názory řízené úsilím:

* **Odebrání možností zahrnout místní JDK a místní aplikační server v rámci balíček pro nasazení.** Stahování JDK a aplikační server z cloudového úložiště během nasazení je vhodnější vkládání tyto součásti v okně balíček od stahování položek výsledky ve zmenšené nasazení balíčku, nasazení rychlejší a líp údržbu. Výsledkem možnosti Zahrnout JDK a aplikační server v balíček pro nasazení mohly být odebrány. Existující projekty, které byly nakonfigurované tak, aby zahrnoval místní JDK a místní aplikační server jako součást balíček pro nasazení se automaticky převedou na automatické uložení JDK a aplikační server cloudového úložiště.
* **Podpora pro verze Azure SDK 2.1.** Modul plug-in Azure pro Eclipse - srpen 2013 Preview vyžaduje Azure SDK 2.1. Do not use srpen 2013 preview se staršími verzemi Azure SDK a nepoužívejte Azure SDK 2.1 s předchozími verzemi aplikace Azure modul plug-in pro Eclipse.
* **Podpora pro zatmění Kepler verze.** Související s tím, nové minimální požadovaný zatmění integrovaném vývojovém prostředí verze je indigově modré. Modul plug-in Azure pro Eclipse zkouší už úředně na Helios.

### <a name="july-3-2013"></a>3 červenec 2013

Modul plug-in Azure pro Eclipse – červenec 2013 Preview vydala. Protože náhled květen 2013 obsahuje tato aktualizace nových funkcí, opravy chyb a některé vylepšení použitelnosti názory řízené úsilím:

* **Možnost vytvoření nového účtu úložiště.** Tlačítko **Nový** přidala dialogové okno **Přidat účet úložiště** . Umožňuje vytvořit účet úložiště v modulu plug-in zatmění, aniž by bylo potřeba přihlásit k portálu Správa Azure. (Třeba už máte předplatné Azure tuto funkci lze použít.) Další informace o vytvoření nového účtu úložiště najdete v tématu [Vytvoření nového účtu úložiště].
* **Nové &quot;(automaticky)&quot; možnosti pro úložiště účet použili automatické nasazení JDK a server a pro ukládání do mezipaměti.** Při použití možnost **automaticky nahrávat** JDK a aplikační server teď můžete určit **(automaticky)** pro adresu URL a úložiště účet, který chcete použít při odesílání JDK a aplikační server nebo při používání, ukládání do mezipaměti Azure. Pak tyto funkce bude automaticky používat stejný účet úložiště jako ten, který jste vybrali v dialogovém okně **Publikovat Azure** . Kurz [Vytvoření Ahoj prezentace aplikace pro Azure v zatmění] byl aktualizovaný možnost nové **(automaticky)** .
* **Možnost nastavit koncové body Azure služeb.** Zadejte koncové body služeb, které určuje, že zda používaný a spravuje globální Azure platformu aplikace Azure provozovaný společností 21Vianet v Číně nebo osobní Azure platformu. Další informace najdete v tématu [Koncové body služby Azure].
* **Velké nasazení můžete určit místní úložiště zdroje.** V případě, že nasazení je příliš velký mají být obsaženy v approot výchozí složky, teď můžete určit místní úložiště zdroje jako cíle nasazení pro vaše JDK a aplikační server. Další informace najdete v tématu [Nasazení velké nasazení].
* **Podpora velikosti A6 a A7 Azure virtuálního počítače.** Teď nasazením do cloudové služby na vysoké paměti A6 a A7 virtuálního počítače velikosti. Další informace o těchto velikosti najdete v článku [virtuální počítač a velikosti cloudové služby Azure].
* **Aktualizace balíček pro knihovny Azure Java knihovny.** To je založena na 0.4.4 verzi [Microsoft Azure klientského rozhraní API].

### <a name="may-1-2013"></a>1 květen 2013

Modul plug-in Azure pro Eclipse - může 2013 Preview vydala. Jedná se o hlavní aktualizaci doprovodným verze 2.0 Azure SDK, která jsou předpokladem a bude možné automaticky stáhnout při instalaci modulu plug-in. Tato verze obsahuje nových funkcí, opravy chyb a některé vylepšení použitelnosti názory řízené úsilím od náhled únor 2013:

* **Automatické odesílání JDK a aplikační server a nasazení z Azure úložiště.** Nová možnost, která automaticky uloží na server vybrané JDK a aplikační server, v případě potřeby, k účtu zadaný Azure úložiště a nasadí tyto součásti odtud místo vložení do balíček pro nasazení nebo s uživateli nahrávání potom ručně. Tato běžně požadované funkce můžete výrazně zvýšit usnadnění nasazení součásti JDK a serveru, zejména u novým uživatelům. Walk-through, který používá tyto možnosti najdete v článku [Vytvoření Ahoj prezentace aplikace pro Azure v zatmění].
* **Centralizované účtu úložiště sledování a možnost referenční úložiště účty další snadno (pomocí rozevíracího seznamu ovládacího prvku).** Toto nastavení se projeví několik funkcí, které jsou závislé na úložiště, jako je JDK a nasazení serveru component a ukládání do mezipaměti. Další informace najdete v tématu [Seznamu účet Azure úložiště].
* **Zjednodušené nastavení vzdálený přístup v okně Publikovat do cloudu průvodce.** Je třeba udělat stačí zadejte uživatelské jméno a heslo pro povolení vzdáleného přístupu, nebo ho nechte pole prázdné a používejte vzdáleného přístupu zakázané.
* **Aktualizace balíček pro knihovny Azure Java knihovny.** To je založena na 0.4.2 verzi [Microsoft Azure klientského rozhraní API].
* **Podpora rychlých relace v systému Windows Server 2012.** Dříve rychlých relace se pracuje pouze Windows Server 2008 R2, teď i cloudových operační systém cílů podpory relace spřažení.
* **Zvýšení výkonu nahrání balíčku.** I když JDK a aplikační server jsou vložené do balíček pro nasazení, nahrát část procesu nasazení může být přibližně dvakrát tak rychlé ve srovnání s předchozími verzemi.

### <a name="february-8-2013"></a>8 únor 2013

Modul plug-in Azure pro Eclipse: únor 2013 Preview vydala. Toto je menší aktualizacích, které obsahuje opravy chyb, vylepšení použitelnosti názory řízené úsilím a několik nových funkcí od náhled listopad 2012:

* Podpora zavádění JDKs aplikační servery a libovolné další součásti z veřejné nebo soukromé Azure objektů blob úložiště stahování místo včetně při nasazení do cloudu v balíček pro nasazení.
* Možnost změnit pořadí, ve kterém jsou zpracovány uživatelem definovaných součásti roli, po přidání tlačítek **Nahoru** a **Dolů** v části **součásti** **Vlastnosti rolí Azure**.
* Aktualizace **balíček pro knihovny Azure Java** knihovně založené na 0.4.0 verzi [Microsoft Azure klientského rozhraní API].

### <a name="november-5-2012"></a>5 listopad 2012

Modul plug-in Azure pro Eclipse - listopad 2012 vydala náhled. Toto je hlavní aktualizacích, které obsahuje několik nových funkcí, jakož i další opravy chyb a vylepšení použitelnosti názory řízené úsilím od 2012 září náhled:

* Podpora pro Microsoft Windows Server 2012 jako operačního systému cloudu.
* Podpora Azure společně umístit mezipaměti podporu pro klienty memcached.
* Zahrnutí knihoven klienta Apache Qpid JMS využít založené na Azure AMQP zasílání zpráv.
* Vylepšené průvodce **Nový projekt** s novou stránku na konci, která uživatelům nabízí možnost rychle nastavit několik běžných hlavními funkcemi jejich projektu: rychlé relací, ukládání do mezipaměti a vzdálené ladění.
* Automatické snížení instancí role 1 při spuštění ve výpočetním emulátoru, abyste se vyhnuli konfliktům port vazeb mezi instancemi serveru.

### <a name="september-28-2012"></a>Září 28. únorem 2012

Modul plug-in Azure pro Eclipse - září 2012 vydala náhled. Služba přináší řadu dalších opravy chyb od srpen 2012 náhled i některé vylepšení použitelnosti názory řízené úsilím v existujících funkcí:

* Podpora pro Microsoft Windows 8 a Microsoft Windows Server 2012 jako operačního systému vývoj řešení problémů, které dřív zabráněno v modulu plug-in správně funguje v těchto operačních systémech.
* Lepší podpora pro zadávání rozsahy portů koncového bodu.
* Opravy chyb souvisejících s mezerami cesty k souboru.
* Role místní nabídka vylepšení pro rychlejší přístup k specifické nastavení.
* Menší upřesnění v průvodci **publikovat do cloudu** a počet další opravy chyb.

### <a name="august-28-2012"></a>28 srpen 2012

Modul plug-in Azure pro Eclipse - srpen 2012 vydala náhled. Služba přináší další opravy chyb od 2012 červenec náhled i několik vylepšení použitelnosti názory řízené úsilím existujících funkcí:

* V dialogovém okně Azure přístup ovládací prvek služeb filtr:
    * **Možnost Vložit podpisového certifikátu** v souboru WAR aplikace zjednodušit cloudu nasazení.
    * **Možnost vytvoření certifikátu podepsaného svým držitelem** ve filtru ACS uživatelského rozhraní. Další informace o filtr služby Azure přístup ovládací prvek najdete v článku [jak ověřování uživatelů webu s Azure přístup ovládací prvek služby pomocí zatmění].
* V Průvodci Azure nasazení projektu (také platí pro na roli konfigurace serveru vlastností stránky):
    * **Automatické zjišťování JDK umístění** na svém počítači (které je možné přepsat v případě potřeby).
    * **Automatické rozpoznávání typu server** po výběru adresáři aplikace serveru instalace.

### <a name="july-15-2012"></a>15 červenec 2012

Modul plug-in Azure pro Eclipse – červenec 2012 vydala náhled, který řeší celá řada nejvyšší prioritou chyby najít a/nebo vykázaného uživatelé po uvolnění. června 2012. Jedná se o aktualizaci service pouze, jsou obsaženy žádné nové funkce.

### <a name="june-7-2012"></a>7 června 2012

Azure modul plug-in pro Eclipse – červen 2012 CTP vydala. Nové funkce patří:

* **Průvodce nový projekt nasazení Azure:** Umožňuje vybrat váš JDK Java aplikační server a aplikace Java přímo v Průvodci vylepšení uživatelského rozhraní. V seznamu konfigurace serveru mimo pole můžete vybírat jsou zahrnuty Tomcat 6, Tomcat 7, GlassFish OSE 3, molo 7, 8 molo, JBoss 6 a 7 JBoss (samostatný). Kromě toho můžete přizpůsobit seznam konfigurace serveru. Toto vylepšení uživatelského rozhraní je alternativy přetažením a přetažením komprimované soubory a kopírování přes spouštěcího, která byla dříve hlavní přístup. Tuto metodu stále funguje, ale se pravděpodobně používat jenom pro pokročilejší scénáře.
* **Konfigurace serveru role vlastností stránky:** Umožňuje snadno přepínat mezi JDKs Java aplikace servery a aplikace související s nasazením po vytvoření projektu. Další informace najdete v tématu [Konfigurace vlastností serveru].
* **&quot;Publikovat do cloudu&quot; průvodce:** poskytuje snadný způsob, jak nasazení projektu Azure přímo z zatmění, automatizaci dříve ručně těžké zrušení načítání přihlašovací údaje, přihlášení k portálu Správa Azure nahrání balíčku apod. Příklad přímo nasazení projektu Azure najdete v článku [Vytvoření Ahoj prezentace aplikace pro Azure v zatmění].
* **Azure nástrojů:** Azure nástrojů je teď dostupná v zatmění, která obsahuje tlačítka, která vyvolat tyto funkce:
    * ![][ic710879]**Spuštění v Azure emulátoru**: spuštění projektu v emulátor.
    * ![][ic710880]**Obnovení emulátoru Azure**: Obnoví emulátor.
    * ![][ic710881]**Vytvořit balíček cloudu pro Azure**: kompilaci balíček pro nasazení.
    * ![][ic710876]**Nový projekt nasazení Azure**: vytvoří nový projekt Azure nasazení.
    * ![][ic710882]**Publikovat na Azure cloudu**: publikuje projektu na Azure.
    * ![][ic710883]**Zrušit publikování**: Odstraní nasazení.
    * Spoustu tyto Azure tlačítka se používají při [vytváření Ahoj prezentace aplikace pro Azure v zatmění].
* **Azure knihovny pro Java:** Teď k dispozici v rámci jednoho balíček pro knihovny Azure Java knihovny v zatmění doprovodným instalace modulu plug-in a obsahující všechny potřebné závislosti. Jednoduše přidat jeden odkaz do knihovny v projektu Java a nemusíte stahovat žádné samostatně. Další informace najdete v tématu [instalace Azure sada nástrojů pro Eclipse].
* **Microsoft JDBC ovladač 4.0 pro systém SQL Server dostupné při instalaci modulu plug-in:** Během instalace nových modul plug-in můžete nainstalovat nejnovější verzi ovladač JDBC Microsoft SQL Server.
* **Filtr služby ovládací prvek azure přístup je k dispozici při instalaci modulu plug-in:** Tento novou součást, jako zatmění knihovny v této sady umožňuje webové aplikace Java Bezproblémová umožní využít výhod ověřování služby Azure přístup řízení (ACS) pomocí různých Zprostředkovatelé identit jiní, například Google, Live.com a Yahoo!. Nebudete muset napsat logiku ověřování sami, stačí konfigurace několik možností a nechat filtr udělejte lifting těžké umožnit uživatelům přihlásit pomocí ACS. Můžete jenom zaměřit na psaní kód, který umožňuje uživatelům přístup k zdroje na základě jejich identity jako vrácené filtr uvnitř objektu žádosti o aplikaci. Návod na pomocí filtru ACS najdete v článku [jak ověřování uživatelů webu s Azure přístup ovládací prvek služba pomocí zatmění].
* **Automatické rozpoznávání předpoklad Azure SDK 1.7:** Při vytváření nového projektu nasazení Azure Azure SDK 1.7 bude automaticky stahovat Pokud už není nainstalovaná.
* **Instance koncové body:** Umožňuje přístup koncový bod přímé port pro komunikaci s rozloženy role instance. Koncové body instance dají přidat s použitím koncové body uživatelské rozhraní aplikace dostupné prostřednictvím stránce [koncové body vlastnosti] . Díky povolení vzdáleného ladění a JMX Diagnostika pro konkrétní výpočet instance spuštěna v cloudu v scénáře s více-instance nasazení. 
* **Součásti uživatelského rozhraní:** Usnadňuje zkušeným uživatelům nastavit projektové závislosti mezi jednotlivé Azure role v projektu a ostatní externí zdroje, jako jsou projekty aplikací Java; také usnadňuje popisují jejich použití logických operátorů nasazení. Další informace najdete v tématu [součásti vlastnosti].
* **Automatický upgrade z předchozích verzí projektů:** Při otevření pracovní prostor, který byl vytvořený pomocí předchozí verze modulu plug-in Azure projektu staré projekty v zobrazí zatmění jako zavřeli, protože předchozích verzí projektů nejsou kompatibilní s novou verzi. Pokud se pokusíte otevřít jeden z těchto staré projektů, spustí se upgrade průvodce. Pokud jste souhlasili s upgradu, nový projekt s **_Upgraded** přidaným k jeho jméno, vytváření a automaticky aktualizován pro práci s novou verzi. V případě potřeby můžete přejmenovat nový projekt. V rámci upgradu původní projektu se nezmění (a bude platit skrytých).

### <a name="december-10-2011"></a>10 prosinec 2011

Azure modul plug-in pro Eclipse - prosinec 2011 CTP vydala. Nové funkce patří:

* **Relace spřažení (&quot;rychlé relace&quot;) podpory:** Pomáhá povolit stavovou skupinový aplikace Java jenom jeden zaškrtávacím políčkem. Další informace najdete v tématu [Spřažení relace].
* **Předem udělali ukázky skriptů při spuštění:** Nejoblíbenější Java serverů (Tomcat molo, JBoss, GlassFish), které je můžete jenom zkopírování a vložení z adresáře ukázky projektu do spouštěcího skriptu.
* **Emulátoru spuštění výstup v reálném čase:** Nyní můžete sledovat provádění všech kroků ze spouštěcího skriptu v okně vyhrazené konzoly vám ukážou, průběhu a chyby skriptu jako prováděný Azure.
* **Sledování automatické, lehká java.exe:** Vynutí Koš rolí při java.exe se zastaví, pomocí skriptu lightweight, předpřipravených automaticky zahrnuty nasazení.
* **Vzdálené Java aplikace ladění konfigurace uživatelského rozhraní:** Umožňuje snadno povolit na zatmění vzdálené ladění pro přístup k aplikaci Java spuštěné v emulátor nebo Azure cloudu, abyste mohli projít a ladění kódu jazyka Java v reálném čase. Další informace najdete v tématu [Ladění Azure aplikací v zatmění].
* **Konfiguraci zdroje místní úložiště uživatelského rozhraní:** Takže už musíte nakonfigurovat místních zdrojů pomocí XML přímo. Tato funkce umožňuje přístup k efektivní cestu místních zdrojů po nasazení prostřednictvím proměnné prostředí, které můžete odkázat přímo ze spouštěcího skriptu. Další informace najdete v tématu [místní úložiště vlastnosti].
* **Proměnnou konfigurací prostředí uživatelského rozhraní:** Takže už musíte nastavit proměnné prostřednictvím ruční úpravy konfigurace XML. Další informace najdete v tématu [prostředí proměnné vlastnosti].
* **JDBC ovladače SQL Azure:** Získá nainstalovaný prostřednictvím modulu plug-in jako plně integrovaného zatmění knihovnu, povolení snadněji programování proti SQL Azure. 
* **Rychlá nabídka přístup ke konfiguraci role uživatelského rozhraní**: jenom klikněte pravým tlačítkem myši na složku rolí a klikněte na **Vlastnosti**.
* **Ikony složek vlastní Azure projektu a role:** Pro lepší viditelnost a snadnější navigace na pracovní prostor a projektu.

## <a name="see-also"></a>Viz taky ##

Další informace o sadách Azure pro Java IDEs najdete v následujících tématech:

- [Azure sada nástrojů pro Eclipse]
  - [Instalace Azure sada nástrojů pro Eclipse]
  - [Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění]
  - *Co je nového v Azure sada nástrojů pro Eclipse (Tento článek)*
- [Azure sada nástrojů pro IntelliJ]
  - [Instalace Azure sada nástrojů pro IntelliJ]
  - [Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ]
  - [Co je nového v Azure sada nástrojů pro IntelliJ]

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

<!-- URL List -->

[Azure sada nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure sada nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace Azure sada nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace Azure sada nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Co je nového v Azure sada nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547

[Webová stránka Azul systémů pro OpenJDK zulština]: http://go.microsoft.com/fwlink/?LinkId=402457
[Koncové body služby Azure]: http://go.microsoft.com/fwlink/?LinkID=699526
[Seznam účet Azure úložiště]: http://go.microsoft.com/fwlink/?LinkID=699528
[Součásti vlastnosti]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699533
[Ladění Azure aplikací v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699535
[Nasazení velké nasazení]: http://go.microsoft.com/fwlink/?LinkID=699536
[Vlastnosti koncové body]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[Vlastnosti proměnné prostředí]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[Modul plug-in HDInsight nástroje pro Eclipse]: ./hdinsight/hdinsight-apache-spark-eclipse-tool-plugin.md
[Jak ověřit uživatelů webu se službou Azure přístup ovládací prvek prostřednictvím zatmění]: http://go.microsoft.com/fwlink/?LinkID=264703
[Jak používat odstranění SSL]: http://go.microsoft.com/fwlink/?LinkID=699545
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Místní uložení vlastnosti]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[Microsoft Azure klientského rozhraní API]: http://go.microsoft.com/fwlink/?LinkId=280397
[Vlastnosti konfigurace serveru]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Spřažení relace]: http://go.microsoft.com/fwlink/?LinkID=699548
[Odstranění SSL]: http://go.microsoft.com/fwlink/?LinkID=699549
[Vytvořte nový účet úložiště]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
[Virtuální počítač a velikosti cloudové služby Azure]: http://go.microsoft.com/fwlink/?LinkId=466520

<!-- IMG List -->

[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710877]: ./media/azure-toolkit-for-eclipse-whats-new/ic710877.png
[ic710879]: ./media/azure-toolkit-for-eclipse-whats-new/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-whats-new/ic710880.png
[ic710881]: ./media/azure-toolkit-for-eclipse-whats-new/ic710881.png
[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710882]: ./media/azure-toolkit-for-eclipse-whats-new/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-whats-new/ic710883.png
