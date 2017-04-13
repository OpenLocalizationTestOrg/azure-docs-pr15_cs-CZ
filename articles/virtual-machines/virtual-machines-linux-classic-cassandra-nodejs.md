<properties
    pageTitle="Spustit Cassandra s Linux Azure | Microsoft Azure"
    description="K tomu Cassandra obrázku na Linux ve virtuálních počítačích Azure pomocí funkčně Node.js aplikace"
    services="virtual-machines-linux"
    documentationCenter="nodejs"
    authors="hanuk"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="hanuk;robmcm"/>

# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Spuštěna Cassandra s Linux Azure a otevření ze Node.js

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](https://azure.microsoft.com/documentation/templates/datastax-on-ubuntu/).

## <a name="overview"></a>Základní informace
Microsoft Azure je platforma otevřít cloudu, které se spouští obou Microsoft jako i v jiných výrobců softwarem, který obsahuje operačních systémů, aplikace serverů, zasílání zpráv middleware stejně jako databáze SQL a NoSQL z obou modelů komerční a otevřít zdroje. Vytváření pružné služby na veřejné mračnech včetně Azure vyžaduje opatrní plánování a záměrné architektura pro servery aplikace jako dobře úložiště vrstvy. Architektura distribuované úložiště na Cassandra přirozeným pomáhá při vytváření vysoce dostupných systémů, které jsou odolnost proti chybám pro selhání clusteru. Cassandra je měřítko cloudu NoSQL databáze spravovaná Apache Software Foundation na cassandra.apache.org; Cassandra je napsáno v Java a tedy běží na obou na Windows, jakož i Linux platformách.

Na tento článek se zaměřuje zobrazíte Cassandra nasazení na systémem Ubuntu jako jednoho a více dat centra cluster využívání virtuálních počítačích Microsoft Azure a virtuální sítě. Vyžaduje konfiguraci uzel více disk, návrh topologie odpovídající kroužky na zavěšení a modelování pro podporu replikace potřebné, konzistence dat, výkon a požadavky na dostupnost dat nasazení clusteru výrobní optimalizované úloh je mimo rozsah tohoto článku.

Tento článek přijímá, které základní přístup k zobrazení co je součástí vytváření clusteru Cassandra porovnání Docker, Chef nebo Puppet, které mohou usnadnit nasazení infrastruktury mnohem.  

## <a name="the-deployment-models"></a>Modely nasazení
Microsoft Azure sítí umožňuje nasazení izolace soukromé clusterů přístup z nich může být omezený k dosažení zabezpečení graf nebo barev sítě.  Protože tento článek se zabývá zobrazující nasazení Cassandra základní úrovni, jsme nebude zaměření se na úrovni konzistenci a návrh úložiště optimální výkon. Tady je seznam sítě požadavky pro naše hypotetické obrázku:

- Externí systémy nelze databázi Cassandra z v rámci i mimo ni Azure
- Musí být za vyrovnávání zatížení přenosů thrift Cassandra obrázku
- Nasazení Cassandra uzlů v dvěma skupinami v jednotlivých datacentrem dostupnosti rozšířené obrázku
- Uzamknout clusteru tak, jen aplikace serverové farmy má přístup k databázi přímo
- Žádné veřejné sítě koncové body než SSH
- Každý uzel Cassandra potřebuje pevné interní IP adresa

Cassandra můžete nasadit do jednoho Azure oblasti nebo více oblastí podle distribuované přírodní pracovní zátěž. Více oblastí nasazení modelu můžete využít k vytisknout koncovým uživatelům blízkosti konkrétní zeměpisná oblast prostřednictvím stejné infrastruktury Cassandra. Cassandra na předdefinované uzel replikace přijímá péče synchronizace více předlohy zapíše pocházející z více datacentrech a prezentuje indexy konzistentního zobrazení dat na aplikace. Nasazení více oblastí můžou pomoct taky s zmírnění rizik širší výpadků Azure služby. Optimalizovat konzistence Cassandra společnosti a topologie replikace vám pomůže v různých operace RPO potřebám aplikací na schůzku.

### <a name="single-region-deployment"></a>Nasazení jedné oblasti
Bude začínat jedné oblasti nasazení jsme sklizně learnings při vytváření modelu více oblastí. Azure virtuální sítě se použijí k vytvoření izolovaného podsítí tak, aby sítě zabezpečení zmíněnou může být požadavků.  Proces popsaná v tématu Vytvoření jedné oblasti nasazení používá se systémem Ubuntu 14.04 l a Cassandra 2.08; proces však můžete snadno přijímají na další varianty Linux. Tady jsou uvedené některé systémové vlastností nasazení jedné oblasti.  

**Dostupnost:** Uzly Cassandra obrázku 1 jsou nasazeny dvou množinách dostupnost tak, aby uzly jsou umístěny mezi víc domén poruch vysoké dostupnosti. VMs označena každou sadu dostupnost namapované na 2 poruch domény.  Použití Microsoft Azure koncepci poruch domény ke správě neplánované dolů čas (například hardware a software k chybám) a přitom koncepci upgradu domény (například hostitel nebo Host OS opravy/upgradů, aplikace upgrady) se používá ke správě naplánované dolů čas. Naleznete v tématu [obnovení havárie a dostupnost pro aplikace Azure](http://msdn.microsoft.com/library/dn251004.aspx) role poruch a upgrade domén v dosažení dostupnost.

![Nasazení jedné oblasti](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux1.png)

Obrázek 1: Jedné oblasti nasazení

Všimněte si, že při psaní tohoto textu Azure neumožňuje explicitní mapování skupiny VMs na konkrétní chyby doménu; i při nasazení modelu obrázku 1, je proto statistické pravděpodobné, že virtuálních počítačích může být namapované dvě poruch domény místo čtyři.

**Thrift přenosy Vyrovnávání zatížení:** Připojte se k němu prostřednictvím Vyrovnávání zatížení vnitřní Thrift klienta knihovnám webového serveru. Při této akci musí proces přidávání Vyrovnávání zatížení interní podsítě "data" (viz obrázek 1) v rámci cloudové službě hostingu Cassandra obrázku. Po definování Vyrovnávání zatížení interní jednotlivých uzlech vyžaduje koncový bod rozloženy má být přidán s poznámkami sady rozloženy s názvem Vyrovnávání zatížení dříve definované. Další informace najdete v článku [Azure interní služby Vyrovnávání zatížení ](../load-balancer/load-balancer-internal-overview.md).

**Clusteru semen:** Je důležité vybrat většina uzly vysoce k dispozici pro semena nové uzly komunikovat uzly počáteční hodnota a objevovat topologie clusteru. Jeden uzel ze všech sadách dostupnost označen jako počáteční hodnota uzly chcete-li předejít jediný bod selhání.

**Replikace faktor a konzistence úroveň:** Na Cassandra předdefinované vysoké dostupnosti a data životnosti rozdělení koeficientem replikace (RF - počet kopií každý řádek uložené v clusteru) a konzistence úroveň (počet repliky předcházet číst nebo zapisovat vrácení výsledek volajícímu). Faktor replikace není zadán při vytváření KEYSPACE (podobný relační databáze), že úroveň konzistence není zadán při vydání CRUD dotazu. Dokumentaci Cassandra [Konfigurace konzistence](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) podrobnosti konzistenci a vzorec pro výpočet kvora.

Cassandra podporuje dva typy modelů integrity dat – konzistenci a případné konzistence; Replikace faktor a konzistence úroveň společně určí, pokud data budou konzistentní hned po dokončení operace zápis nebo bude postupně konzistentní. Například určující KVORA stejně jako úroveň konzistence vždy zajišťuje dat konzistence při jakékoliv úrovni konzistence, pod počtu replik k zápisu v případě potřeby k dosažení výsledků KVORA (například jedno) v datech jsou postupně konzistentní.

8 uzel obrázku nahoře, replikace koeficientem 3 a KVORA (2 uzly jsou číst nebo zapisovat konzistence) pro čtení i zápis konzistence úrovně, můžete i po teoretický ztráty nejvíce 1 uzel skupinu replikace před sledováním selhání spuštění aplikace. To předpokládá, že všechny klíčové mezery mít taky rovnoměrně pro čtení i zápis žádosti.  Následující seznam uvádí parametrů, které použijeme pro nasazeném obrázku:

Konfigurace clusteru Cassandra jedné oblasti:

| Parametr obrázku | Hodnota | Poznámky: |
| ----------------- | ----- | ------- |
| Počet uzlů (N) | 8   | Celkový počet uzlů v clusteru |
| Replikace faktoru RF | 3 | Počet kopie daného řádku |
| Úroveň konzistence (zápis) | QUORUM[(RF/2) +1) = 2] výsledek vzorce je zaokrouhlena dolů | Data zapisuje nejvíce 2 repliky před odesláním odpovědi volajícímu; 3 otevřené psaných postupně konzistentní. |
| Úroveň konzistence (pro čtení) | KVORA [(RF/2) + 1 = 2] výsledek vzorce je zaokrouhlena dolů | Přečte 2 repliky před odesláním odpovědi volajícímu. |
| Strategie replikace | NetworkTopologyStrategy [Replikace dat](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) v Cassandra si přečtěte následující dokumentaci pro další informace najdete v článku | Znalost topologii nasazení a umístí repliky uzly tak, aby ostatními není skončily ve stejném regálů |
| Snitch | GossipingPropertyFileSnitch najdete v článku [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) v Cassandra si přečtěte následující dokumentaci pro další informace | NetworkTopologyStrategy používá koncepci snitch pochopit topologii. GossipingPropertyFileSnitch poskytuje lepší kontrolu v jednotlivých uzlech mapování datacentra a regálů. Clusteru použije stálém kontaktu se rozšíří tyto informace. Toto je mnohem jednodušší v dynamické IP nastavení relativní PropertyFileSnitch |


**Azure aspektech pro Cassandra clusteru:** Funkce virtuálních počítačích Microsoft Azure používá úložiště objektů Blob Azure pro trvalé disku; Azure úložiště ukládá 3 kopie každé disku vysoké životnost. To znamená každý řádek dat vložené do tabulky Cassandra je už je uložený ve 3 replikách a tedy konzistence dat se už stará i v případě faktor replikace (RF) jde 1. Hlavní problém s replikace faktor vůbec 1 je, že aplikace bude prostoje i v případě selhání načítání Cassandra. Ale pokud uzel určen problémy odstranit (například hardwaru, systémových selhání software) rozpozná řadiče struktury Azure, bude zřízení nový uzel místo použití stejné jednotky úložiště. Zřízení nového uzel nahradit ten původní může trvat několik minut.  Podobně plánované údržby aktivity jako Host OS změny, Cassandra aktualizuje a změny aplikace Azure struktury řadiče provádí postupných inovací uzlů v clusteru.  Postupné inovace taky může trvat dolů několik uzlů najednou a tedy clusteru setkat stručný výpadek několik oddíly. Však data neztratí kvůli zabudované zálohování Azure úložiště.  

Pro systémy používaný Azure, který nevyžaduje vysoké dostupnosti (například kolem 99,9, která je ekvivalentní 8.76 nulovou/rok; podrobnosti najdete v části [Dostupnost](http://en.wikipedia.org/wiki/High_availability) ) možná budete moct pro práci s RF = 1 a konzistence úroveň = jednu.  U aplikací s požadavky na dostupnost, RF = 3 a konzistence úroveň = KVORA bude tolerovat prostoj některého uzly jednu z těchto replik. RF = 1 v tradiční nasazení (například na místní) se nedají použít kvůli možnou ztrátu dat vzniklé problémy, například selhání disku.   

## <a name="multi-region-deployment"></a>Nasazení více oblastí
Na Cassandra Centrum založených na datech replikace a konzistence modelu jsme je popsali výše pomáhá s nasazením více oblastí mimo pole bez nutnosti externí nástrojů. Toto je velmi liší od tradiční relační databáze, přičemž může platit jsou složité nastavení pro více hlavních zápisy odrážející strukturu databáze. Cassandra ve více oblastech nastavení vám mohou pomoci s příklady použití, včetně následujících:

**Blízkost na základě nasazení:** Více klienta aplikací s Vymazat mapování klienta uživatelů – k-oblasti, můžete prospěch tak, že zhoršeným čekacích dob více oblast obrázku. Například Správa výukové systémů pro vzdělávací instituce nasazení distribuované obrázku ve východoasijských USA a západní US oblastech a bude předávat odpovídajících univerzity pro transakční a analýzy. Data může být místně konzistentní čas čtení a zápis a může být ve obě oblasti postupně konzistentní. Existují další příklady jako distribuční média, obchodování a nic a všechny položky, které bude sloužit geo koncentrovaná uživatele základní je dobré použít případ pro tento model nasazení.

**Dostupnost:** Redundance je klíčovým faktorem k dosažení dostupnost softwaru a hardware. Podrobnosti najdete v části stavebních spolehlivé cloudu systémy na Microsoft Azure. Na Microsoft Azure pouze spolehlivých způsobů dosažení true redundance je nasazení ve více oblastech clusteru. Aplikací může být nasazené v režimu aktivní nebo aktivní pasivní a -li jeden z oblastí dolů, správce přenosy Azure přesměrujete přenosy na aktivní oblast.  S nasazením jedné oblasti, pokud dostupnosti 99,9, nasazení dvou oblastí dosáhnout dostupnost 99.9999 vypočítanou podle vzorce: (1-(1-0.999) *(1-0,999))*100); Viz nahoře papíru podrobnosti.

**Havárie zotavení:** Clusteru Cassandra více oblastech, pokud navržený správně, můžete nebyly výpadků Centrum katastrofické data. Pokud jednom regionu dolů, můžete začít nasazené do jiných oblastí aplikace podávání koncoví uživatelé. Stejně jako jakékoli jiné firmy kontinuitu implementace musí být chybám pro ztrátu dat vyplývající z dat v asynchronní kanálem k odesílání zpráv aplikace. Však Cassandra zajišťuje obnovení mnohem rychlejší než dobu trvání procesů využití tradiční databáze. Obrázek 2 znázorňuje modelu typické nasazení více oblastech pomocí osm uzlů v jednotlivých oblastech. Obě oblasti jsou obrázky zrcadlové navzájem stejné souměrnosti; reálné návrhů závisí na typu pracovního vytížení (například transakční nebo analytické), operace RPO, RTO, konzistence dat a požadavků na dostupnost.

![Více oblastí nasazení](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux2.png)

Obrázek 2: Více oblastí Cassandra nasazení

### <a name="network-integration"></a>Integrace sítě
Sady virtuálních počítačích používaný privátní sítě uložené ve dvou oblastí informuje uživatele o vzájemně pomocí tunelem VPN. Tunelem VPN připojí dvě brány software zřízení během procesu nasazení sítě. Obě oblasti mají podobný architektura sítě z hlediska "www" a "data" podsítí; Azure sítí umožňuje vytváření tolik podsítí podle potřeby a použít ACL podle potřeby zabezpečení sítě. Při navrhování topologii clusteru mimo dat centra komunikace latence a economic vlivu provozu v síti hodit považovat za.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Konzistence dat pro více Datacentrem nasazení
Distribuovat nasazení se musí nějaká clusteru topologie dopad na výkon a dostupnost. RF a konzistence úroveň musíte vybrat tak kvorum nezávisí na dostupnost všem centrům pro data.
Systému, který potřebuje vysoké konzistence LOCAL_QUORUM pro konzistence úrovně (pro čtení a zápis) bude zkontrolujte, že místní čtení a zápis jsou splněny z místní uzlů během asynchronní replikace dat do Center Vzdálená data.  Tabulka 2 shrnuje podrobnosti konfigurace pro více oblast obrázku uvedené dále v zápisu nahoru.

**Konfigurace clusteru Cassandra dvou oblastí**


| Parametr obrázku | Hodnota | Poznámky: |
| ----------------- | ----- | ------- |
| Počet uzlů (N) | 8 + 8 | Celkový počet uzlů v clusteru |
| Replikace faktoru RF | 3 | Počet kopie daného řádku |
| Úroveň konzistence (zápis) | LOCAL_QUORUM [(sum(RF)/2) +1) = 4] výsledek vzorce je zaokrouhlena dolů | 2 uzly zapíše první datacentrem synchronní; Další 2 uzly potřebné pro kvora zapíše asynchronní 2 datacentrem. |
| Úroveň konzistence (pro čtení) | LOCAL_QUORUM ((RF/2) + 1) = 2 výsledek vzorce je zaokrouhlena dolů | Požadavky pro čtení jsou splněny z jedinou oblasti; 2 uzly se budou číst před odesláním odpovědi klientovi. |
| Strategie replikace | NetworkTopologyStrategy [Replikace dat](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) v Cassandra si přečtěte následující dokumentaci pro další informace najdete v článku | Znalost topologii nasazení a umístí repliky uzly tak, aby ostatními není skončily ve stejném regálů |
| Snitch | GossipingPropertyFileSnitch najdete v článku [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) v Cassandra si přečtěte následující dokumentaci pro další informace | NetworkTopologyStrategy používá koncepci snitch pochopit topologii. GossipingPropertyFileSnitch poskytuje lepší kontrolu v jednotlivých uzlech mapování datacentra a regálů. Clusteru použije stálém kontaktu se rozšíří tyto informace. Toto je mnohem jednodušší v dynamické IP nastavení relativní PropertyFileSnitch |


##<a name="the-software-configuration"></a>KONFIGURACE SOFTWARU
Při nasazení se používají následující software verze:

<table>
<tr><th>Software</th><th>Zdroje</th><th>Verze</th></tr>
<tr><td>JRE </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Se systémem Ubuntu  </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 l</td></tr>
</table>

Protože stahování JRE vyžaduje ruční přijetí Oracle licenci, zjednodušit nasazení, stáhněte si všechny příslušný software požadovaný na plochu pro pozdější odeslání pomocí bitové systémem Ubuntu šablonu, kterou jsme bude vytvářet jako prekurzor nasazení obrázku.

Stažení softwaru výše do adresáře známý stažení (například %TEMP%/downloads v systému Windows nebo ~/Downloads na většině Linux rozdělení nebo Mac) v místním počítači.

### <a name="create-ubuntu-vm"></a>VYTVOŘENÍ SYSTÉMEM UBUNTU OM
V tomto kroku procesu vytvoříme obrázek se systémem Ubuntu předem nutné softwarem tak, aby obrázek můžete znovu použít pro zřizování několik Cassandra uzlů.  
####<a name="step-1-generate-ssh-key-pair"></a>Krok 1: Vygenerovat SSH klíč
Azure musí X509 veřejným klíčem, který je PEM nebo DER zakódovaný zřizovací současně. Vygenerovat veřejné a privátní klíč postupujte podle pokynů c:\ způsob použití SSH s Linux na Azure. Pokud chcete použít jako klienta SSH buď ve Windows a v Linuxu putty.exe, budete muset převést PEM zakódovaný RSA privátním klíčem do formátu PPK pomocí puttygen.exe; Pokyny pro tuto najdete na webové stránce výše.

####<a name="step-2-create-ubuntu-template-vm"></a>Krok 2: Vytvoření šablony se systémem Ubuntu OM
Pokud chcete vytvořit šablonu OM, přihlaste se k portálu Azure klasické a použít následující postup: klikněte na nový VÝPOČETNÍM, virtuální počítač, z GALERIE, se systémem UBUNTU, se systémem Ubuntu serveru 14.04 l a potom klikněte na šipku doprava. Kurz, který popisuje postup jejího vytvoření Linux OM najdete v článku Vytvoření virtuálního počítače systém Linux.

Na obrazovce "konfiguraci virtuálního počítače" #1 zadejte následující informace:

<table>
<tr><th>NÁZEV POLE              </td><td>       HODNOTA POLE               </td><td>         POZNÁMKY:                </td><tr>
<tr><td>DATUM VYDÁNÍ VERZE    </td><td> Výběr data drow dolů</td><td></td><tr>
<tr><td>NÁZEV VIRTUÁLNÍHO POČÍTAČE    </td><td> Cass šablony                </td><td> Toto je hostname OM </td><tr>
<tr><td>VRSTVY                     </td><td> STANDARDNÍ                        </td><td> Ponechejte výchozí              </td><tr>
<tr><td>VELIKOST                     </td><td> A1                              </td><td>Vyberte OM podle potřeby vstupu a výstupu; k tomuto účelu ponechejte výchozí </td><tr>
<tr><td> NOVÉ UŽIVATELSKÉ JMÉNO           </td><td> localadmin                      </td><td> "správce" je rezervovaná uživatelského jména v systémem Ubuntu 12. xx a po</td><tr>
<tr><td> OVĚŘOVÁNÍ      </td><td> Zrušte zaškrtnutí políčka                 </td><td>Zkontrolujte, jestli chcete zabezpečené klíčem SSH </td><tr>
<tr><td> CERTIFIKÁT             </td><td> Název souboru certifikát veřejného klíče </td><td> Použití veřejným klíčem generovaného dříve</td><tr>
<tr><td> Nové heslo   </td><td> silné heslo </td><td> </td><tr>
<tr><td> Potvrďte heslo   </td><td> silné heslo </td><td></td><tr>
</table>

Na obrazovce "konfiguraci virtuálního počítače" #2 zadejte následující informace:

<table>
<tr><th>NÁZEV POLE             </th><th> HODNOTA POLE                       </th><th> POZNÁMKY:                                 </th></tr>
<tr><td> CLOUDOVÁ SLUŽBA  </td><td> Vytvoření nového cloudové služby    </td><td>Cloudové služby je kontejneru výpočetním zdrojů, jako jsou virtuálních počítačích</td></tr>
<tr><td> NÁZEV CLOUDOVÉ SLUŽBY DNS </td><td>se systémem Ubuntu template.cloudapp.net   </td><td>Pojmenujte Vyrovnávání zatížení která počítače</td></tr>
<tr><td> OBLAST/SPŘAŽENÍ SKUPINY/VIRTUÁLNÍ SÍTĚ </td><td>    Západ USA </td><td> Vyberte oblast, ze které do webových aplikací přístup clusteru Cassandra</td></tr>
<tr><td>ÚLOŽIŠTĚ ÚČTU </td><td>   Použít výchozí nastavení </td><td>Použít výchozí úložiště účet nebo účet předem vytvořené úložiště v konkrétní oblast</td></tr>
<tr><td>NASTAVENÍ DOSTUPNOSTI </td><td>  Žádná </td><td>  Nechejte prázdné</td></tr>
<tr><td>KONCOVÉ BODY   </td><td>Použít výchozí nastavení </td><td>  Použití výchozí SSH konfigurace </td></tr>
</table>

Klikněte na šipku doprava, ponechejte výchozí hodnoty v dialogovém okně #3 a klikněte na tlačítko "zaškrtnutí" dokončete OM zřizování obrázku. Po několika minutách by měl být OM šablonou název"se systémem ubuntu-" ve stavu "systém".

###<a name="install-the-necessary-software"></a>INSTALACE POTŘEBNÝ SOFTWARE
####<a name="step-1-upload-tarballs"></a>Krok 1: Nahrání tarballs
Stažený software pomocí spojovací bod služby nebo pscp, zkopírujte do adresáře ~/downloads formátu následující příkaz:

#####<a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz

Opakujte výše uvedený příkaz pro JRE stejně jako u Cassandra bitů.

####<a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>Krok 2: Příprava strukturu adresářů a extrahovat archivu
Přihlaste se na OM a vytvořte strukturu adresářů a extrahovat software jako superuživatelů pomocí skriptu flám níže:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Pokud vložíte do okna vim tento skript, zkontrolujte, že odebrat návratu ("\r") pomocí následujícího příkazu:

    tr -d '\r' <infile.sh >outfile.sh

####<a name="step-3-edit-etcprofile"></a>Krok 3: Úprava atd/profilu
Připojte následující na konci:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

####<a name="step-4-install-jna-for-production-systems"></a>Krok 4: Instalace JNA produkčních systémů
Použijte následující příkaz posloupnost: následující příkaz bude nainstalujte jna 3.2.7.jar a jna 3.2.7.jar platformy do adresáře /usr/share.java sudo výstižný get libjna java

Vytvoření symbolického propojení v adresáři $CASS_HOME/knihovny tak, aby Cassandra spouštěcí skript najdete tyto sklenic po g:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

####<a name="step-5-configure-cassandrayaml"></a>Krok 5: Konfigurace cassandra.yaml
Úprava cassandra.yaml na každý OM tak, aby odrážely konfigurace potřeby tak, že všechny virtuálních počítačích [jsme bude doladit to během skutečné zřizování]:

<table>
<tr><th>Název pole   </th><th> Hodnota  </th><th> Poznámky: </th></tr>
<tr><td>název_clusteru </td><td>  "CustomerService"   </td><td> Použijte název, který vystihuje nasazení</td></tr>
<tr><td>listen_address  </td><td>[nechejte pole prázdné]   </td><td> Odstranění "localhost" </td></tr>
<tr><td>rpc_addres   </td><td>[nechejte pole prázdné]  </td><td> Odstranění "localhost" </td></tr>
<tr><td>semena   </td><td>"10.1.2.4, 10.1.2.6 10.1.2.8." </td><td>Seznam všech IP adres, které jsou označeny jako vstupní hodnoty.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> To NetworkTopologyStrateg pro používá odvozováním datového centra a regálů OM</td></tr>
</table>

####<a name="step-6-capture-the-vm-image"></a>Krok 6: Napřed zachytit OM
Přihlaste se k počítači virtuální pomocí hostname (hk čas template.cloudapp.net) a privátním klíčem SSH dřív vytvořili. V tématu Jak používat SSH s Linux na Azure podrobné informace o tom, jak se přihlásit pomocí příkazu ssh nebo putty.exe.

Provedení této akce, které chcete zachytit posloupnosti:
#####<a name="1-deprovision"></a>1. identitách
Použití příkazu "sudo waagent – identitách + uživatele" Odebrat virtuální počítač instance konkrétních informací. Naleznete v tématu [jak zachytit virtuálního počítače Linux](virtual-machines-linux-classic-capture-image.md) použít jako šablonu podrobnosti na snímku procesu.

#####<a name="2-shutdown-the-vm"></a>2: vypnutí OM
Ujistěte se, že je zvýrazněná virtuálního počítače a klikněte na odkaz vypnutí z panelu s příkazy dolní.

#####<a name="3-capture-the-image"></a>3: zachycení obrázku
Ujistěte se, že je zvýrazněná virtuálního počítače a klikněte na SNÍMKU odkaz z panelu s příkazy dolní. Na další obrazovce pojmenujte na OBRÁZEK (například hk-cas-2-08-ub-14-04-2014071), je to vhodné popis OBRÁZKU a klikněte na "" zaškrtnutí dokončete proces sběru.

Bude trvat několik sekund, než a obrázek by měl být k dispozici v části Moje obrázky z Galerie obrázků. Zdroj OM bude automaticky delated po úspěšně nezaznamenávají obrázku.

##<a name="single-region-deployment-process"></a>Proces nasazení jedné oblasti
**Krok 1: vytvoření virtuální sítě** Přihlaste se k portálu Azure klasické a vytvořte virtuální sítě s atributy zobrazení v tabulce. Přečtěte si téma [Konfigurace Cloud-Only virtuální sítě Azure klasické portálu](../virtual-network/virtual-networks-create-vnet-classic-portal.md) podrobný postup procesu.      

<table>
<tr><th>Název atributu OM</th><th>Hodnota</th><th>Poznámky:</th></tr>
<tr><td>Jméno</td><td>vnet-cass západní NAS</td><td></td></tr>
<tr><td>Oblast</td><td>Západní USA</td><td></td></tr>
<tr><td>Servery DNS </td><td>Žádná</td><td>Ignorovat tím jako jsme nepoužíváte serveru DNS</td></tr>
<tr><td>Konfigurace VPN čárky webu</td><td>Žádná</td><td> Ignorovat tím</td></tr>
<tr><td>Konfigurace sítě VPN na webu</td><td>Nnone</td><td> Ignorovat tím</td></tr>
<tr><td>Adresní prostor</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>Počáteční IP</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Přidejte následující podsítí:

<table>
<tr><th>Jméno</th><th>Počáteční IP</th><th>CIDR</th><th>Poznámky:</th></tr>
<tr><td>Web</td><td>10.1.1.0</td><td>/ 24 (251)</td><td>Podsítě pro webové farmy</td></tr>
<tr><td>data</td><td>10.1.2.0</td><td>/ 24 (251)</td><td>Podsítě pro uzly databáze</td></tr>
</table>

Data a Web podsítí mohou být chráněny prostřednictvím sítě skupin zabezpečení do rozsahu je mimo rozsah tohoto článku.  

**Krok 2: zřízení virtuálních počítačích** Pomocí obrázek nevytvořili, jsme vytvoří následující virtuálních počítačích v cloudu server "hk-c-svc západní" a jak je ukázáno v následujícím příkladu je svážete do příslušných podsítí:

<table>
<tr><th>Název počítače    </th><th>Podsítě </th><th>IP adresa </th><th>Nastavení dostupnosti</th><th>Datacentrum/regálů</th><th>Počáteční hodnota?</th></tr>
<tr><td>HK-c1-západní NAS   </td><td>data   </td><td>10.1.2.4   </td><td>HK-c sada-1    </td><td>datacentrum = WESTUS regálů = rack1 </td><td>Ano</td></tr>
<tr><td>HK-c2-západní NAS   </td><td>data   </td><td>10.1.2.5   </td><td>HK-c sada-1    </td><td>datacentrum = WESTUS regálů = rack1 </td><td>Ne </td></tr>
<tr><td>HK-c3-západní NAS   </td><td>data   </td><td>10.1.2.6   </td><td>HK-c sada-1    </td><td>datacentrum = WESTUS regálů = rack2 </td><td>Ano</td></tr>
<tr><td>HK-c4-západní NAS   </td><td>data   </td><td>10.1.2.7   </td><td>HK-c sada-1    </td><td>datacentrum = WESTUS regálů = rack2 </td><td>Ne </td></tr>
<tr><td>HK-c5-západní NAS   </td><td>data   </td><td>10.1.2.8   </td><td>HK-c sada-2    </td><td>datacentrum = WESTUS regálů = rack3 </td><td>Ano</td></tr>
<tr><td>HK-c6-západní NAS   </td><td>data   </td><td>10.1.2.9   </td><td>HK-c sada-2    </td><td>datacentrum = WESTUS regálů = rack3 </td><td>Ne </td></tr>
<tr><td>HK-c7-západní NAS   </td><td>data   </td><td>10.1.2.10  </td><td>HK-c sada-2    </td><td>datacentrum = WESTUS regálů = rack4 </td><td>Ano</td></tr>
<tr><td>HK-c8-západní NAS   </td><td>data   </td><td>10.1.2.11  </td><td>HK-c sada-2    </td><td>datacentrum = WESTUS regálů = rack4 </td><td>Ne </td></tr>
<tr><td>HK B1 západní NAS   </td><td>Web    </td><td>10.1.1.4   </td><td>HK-w sada-1    </td><td>                       </td><td>NENÍ K DISPOZICI</td></tr>
<tr><td>HK-w2-západní NAS   </td><td>Web    </td><td>10.1.1.5   </td><td>HK-w sada-1    </td><td>                       </td><td>NENÍ K DISPOZICI</td></tr>
</table>

Vytvoření seznamu z VMs vyžaduje následujícím způsobem:

1.  Vytvoření prázdného cloudové služby v určité oblasti
2.  Vytvoření virtuálního počítače z dříve zachycený obrázek a připojte ji k virtuální sítě nevytvořili; Tento postup opakujte pro všechny VMs
3.  Přidání Vyrovnávání zatížení vnitřní cloudové služby a připojte ji k podsítě "data"
4.  Pro každý OM nevytvořili přidejte rozloženy koncový bod pro thrift přenosů rozloženy nastavit připojení k vyrovnávání zatížení dříve vytvořené vnitřní

Výše uvedené proces lze provést portálu Azure klasické; použití počítače v systému Windows (Použijte OM na Azure Pokud nebudete mít přístup k počítači Windows), použijte tento skript Powershellu zřízení všechny 8 VMs automaticky.

**Seznam 1: Skript Powershellu pro vytváření virtuálních počítačích**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Krok 3: Nastavení Cassandra na každý OM**

Přihlaste se k OM a proveďte následující kroky:

* Úprava $CASS_HOME/conf/cassandra-rackdc.properties vlastnosti dat centra a regálů:

       dc =EASTUS, rack =rack1

* Úprava cassandra.yaml ke konfiguraci počáteční hodnota uzlů jako níže:

       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Krok 4: VMs spustit a otestovat clusteru**

Přihlaste se na jeden z uzlů (například hk-c1-západní NAS) a spusťte tento příkaz Zobrazení stavu obrázku:

       nodetool –h 10.1.2.4 –p 7199 status

Měli byste vidět podobná té pod 8 uzel clusteru zobrazení:

<table>
<tr><th>Stav</th><th>Adresa  </th><th>Načtení   </th><th>Tokeny </th><th>Vlastní </th><th>ID Host (hostitel)  </th><th>Regálů</th></tr>
<tr><th>ZRUŠENÍM ZAŠKRTNUTÍ  </td><td>10.1.2.4   </td><td>87.81 ZNALOSTNÍ BÁZI KNOWLEDGE BASE   </td><td>256    </td><td>38.0 %  </td><td>Identifikátor GUID (odebrání)</td><td>rack1</td></tr>
<tr><th>ZRUŠENÍM ZAŠKRTNUTÍ  </td><td>10.1.2.5   </td><td>41.08 ZNALOSTNÍ BÁZI KNOWLEDGE BASE   </td><td>256    </td><td>68.9 %  </td><td>Identifikátor GUID (odebrání)</td><td>rack1</td></tr>
<tr><th>ZRUŠENÍM ZAŠKRTNUTÍ  </td><td>10.1.2.6   </td><td>55.29 ZNALOSTNÍ BÁZI KNOWLEDGE BASE   </td><td>256    </td><td>68.8 %  </td><td>Identifikátor GUID (odebrání)</td><td>rack2</td></tr>
<tr><th>ZRUŠENÍM ZAŠKRTNUTÍ  </td><td>10.1.2.7   </td><td>55.29 ZNALOSTNÍ BÁZI KNOWLEDGE BASE   </td><td>256    </td><td>68.8 %  </td><td>Identifikátor GUID (odebrání)</td><td>rack2</td></tr>
<tr><th>ZRUŠENÍM ZAŠKRTNUTÍ  </td><td>10.1.2.8   </td><td>55.29 ZNALOSTNÍ BÁZI KNOWLEDGE BASE   </td><td>256    </td><td>68.8 %  </td><td>Identifikátor GUID (odebrání)</td><td>rack3</td></tr>
<tr><th>ZRUŠENÍM ZAŠKRTNUTÍ  </td><td>10.1.2.9   </td><td>55.29 ZNALOSTNÍ BÁZI KNOWLEDGE BASE   </td><td>256    </td><td>68.8 %  </td><td>Identifikátor GUID (odebrání)</td><td>rack3</td></tr>
<tr><th>ZRUŠENÍM ZAŠKRTNUTÍ  </td><td>10.1.2.10  </td><td>55.29 ZNALOSTNÍ BÁZI KNOWLEDGE BASE   </td><td>256    </td><td>68.8 %  </td><td>Identifikátor GUID (odebrání)</td><td>rack4</td></tr>
<tr><th>ZRUŠENÍM ZAŠKRTNUTÍ  </td><td>10.1.2.11  </td><td>55.29 ZNALOSTNÍ BÁZI KNOWLEDGE BASE   </td><td>256    </td><td>68.8 %  </td><td>Identifikátor GUID (odebrání)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Testování jedné oblasti obrázku
Pomocí následujících kroků k testování clusteru:

1.    Pomocí prostředí PowerShell příkaz Get-AzureInternalLoadbalancer získá IP adresu Vyrovnávání zatížení interní (například  10.1.2.101). Syntaxe příkazu je uveden níže: Get-AzureLoadbalancer – název_služby "hk-c-svc západní NAS" [zobrazí podrobnosti o vyrovnávání zatížení interní spolu s jeho IP adresu]
2.  Přihlaste se k farmě web OM (například hk B1 západní NAS) pomocí nátěrové nebo ssh
3.  Spuštění $CASS_HOME/Koš/cqlsh 10.1.2.101 9160
4.  Pomocí následujících příkazů CQL ověřte, zda clusteru funguje:

        CREATE KEYSPACE customers_ks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');

        SELECT * FROM Customers;

Měli byste vidět zobrazení podobné níže:

<table>
  <tr><th> customer_id </th><th> jméno </th><th> Příjmení </th></tr>
  <tr><td> 1 </td><td> Petr </td><td> Karásek </td></tr>
  <tr><td> 2 </td><td> Petra </td><td> Karásek </td></tr>
</table>

Dejte pozor, aby keyspace vytvořili v kroku 4 používá SimpleStrategy s replication_factor 3. SimpleStrategy je vhodné pro jednu datovou Centrum pro nasazení že NetworkTopologyStrategy více dat na střed nasazení. Replication_factor 3 vám umožní odchylku pro selhání uzlů.

##<a id="tworegion"> </a>Nasazení více oblastí obrázku
Bude využít jednu oblast nasazení dokončení a opakujte stejný postup pro instalaci druhou oblast. Klíčové rozdíl mezi jednoho a více oblastí nasazení je nastavení tunelem virtuální privátní sítě pro komunikaci mezi oblasti; Změníme začít s instalací sítě, zřízení VMs a konfigurace Cassandra.

###<a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Krok 1: Vytvoření virtuální sítě v oblasti 2.
Přihlaste se k portálu Azure klasické a vytvořte virtuální sítě s atributy zobrazení v tabulce. Přečtěte si téma [Konfigurace Cloud-Only virtuální sítě Azure klasické portálu](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) podrobný postup procesu.      

<table>
<tr><th>Název atributu    </th><th>Hodnota    </th><th>Poznámky:</th></tr>
<tr><td>Jméno    </td><td>vnet-cass východ NAS</td><td></td></tr>
<tr><td>Oblast  </td><td>Východní USA</td><td></td></tr>
<tr><td>Servery DNS     </td><td></td><td>Ignorovat tím jako jsme nepoužíváte serveru DNS</td></tr>
<tr><td>Konfigurace VPN čárky webu</td><td></td><td>     Ignorovat tím</td></tr>
<tr><td>Konfigurace sítě VPN na webu</td><td></td><td>      Ignorovat tím</td></tr>
<tr><td>Adresní prostor   </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Počáteční IP </td><td>10.2.0.0   </td><td></td></tr>
<tr><td>CIDR    </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Přidejte následující podsítí:
<table>
<tr><th>Jméno    </th><th>Počáteční IP    </th><th>CIDR   </th><th>Poznámky:</th></tr>
<tr><td>Web </td><td>10.2.1.0   </td><td>/ 24 (251)  </td><td>Podsítě pro webové farmy</td></tr>
<tr><td>data    </td><td>10.2.2.0   </td><td>/ 24 (251)  </td><td>Podsítě pro uzly databáze</td></tr>
</table>


###<a name="step-2-create-local-networks"></a>Krok 2: Vytvoření místní sítě
V místní síti v Azure virtuální sítě je proxy adresní prostor, který odpovídá vzdálený server včetně soukromé cloudu nebo jiné Azure oblasti. Tento proxy adresní prostor je svázán ke vzdálené bráně pro síť směrování do pravého sítě cílů. Přečtěte si téma [Konfigurace VNet VNet připojení](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) pokyny týkající se vytváření VNET VNET připojení.

Vytvořte dvě místní sítě za tyto údaje:

| Síťový název | Adresa brány VPN | Adresní prostor | Poznámky: |
| ------------ | ------------------- | ------------- | ------- |
| HK-lnet-map-to-East-us | 23.1.1.1  | 10.2.0.0/16   | Při vytváření místní síti dát zástupný symbol adresa brány. Adresa skutečné brány se vyplní vytvořené brány. Ujistěte se, že adresní prostor přesně odpovídá odpovídajících vzdálené VNET; v tomto případě VNET vytvořené v oblasti východoasijských USA. |
| HK-lnet-map-to-West-us | 23.2.2.2  | 10.1.0.0/16   | Při vytváření místní síti dát zástupný symbol adresa brány. Adresa skutečné brány se vyplní po vytvoření brány. Ujistěte se, že adresní prostor přesně odpovídá odpovídajících vzdálené VNET; v tomto případě VNET vytvořený v oblastí západní USA. |


###<a name="step-3-map-local-network-to-the-respective-vnets"></a>Krok 3: Mapy "Místní" síti odpovídajících VNETs
Z portálu Microsoft Azure klasické vyberte každý vnet, klikněte na "Konfigurovat", "Připojit k místní síti" Zkontrolujte a vyberte místní sítě za tyto údaje:


| Virtuální sítě | Místní síti |
| --------------- | ------------- |
| HK-vnet západní NAS | HK-lnet-map-to-East-us |
| HK-vnet východ NAS | HK-lnet-map-to-West-us |


###<a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Krok 4: Vytvoření brány na VNET1 a VNET2
Na řídicím panelu virtuálních sítí klikněte na vytvořit brány, který spustí Brána VPN zřizování obrázku. Po několika minutách by měl zobrazit řídicí panel každý virtuální sítě adresu skutečné brány.

###<a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Krok 5: Aktualizace "Místní" sítě s adresami odpovídajících "brány"###
Upravte místní sítě nahrazení zástupného symbolu brány IP adresu s reálným IP adresu jenom zřizování brány. Použijte následující mapování:

<table>
<tr><th>Místní síti    </th><th>Virtuální sítě brány</th></tr>
<tr><td>HK-lnet-map-to-East-us </td><td>Brána hk-vnet západní-nás ostatní</td></tr>
<tr><td>HK-lnet-map-to-West-us </td><td>Brána hk-vnet východ-nás ostatní</td></tr>
</table>

###<a name="step-6-update-the-shared-key"></a>Krok 6: Aktualizujte sdílené klíč
Použijte tento skript Powershellu aktualizovat klávesu IPSec každý VPN brány [použít saké klíč pro obě brány]: hk-lnet-map-to-west-us - LocalNetworkSiteName v Set AzureVNetGatewayKey - VNetName hk-vnet východ NAS - SharedKey hk-lnet-map-to-east-us - LocalNetworkSiteName v D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet západní NAS - SharedKey D9E76BKK

###<a name="step-7-establish-the-vnet-to-vnet-connection"></a>Krok 7: VNET VNET připojení
Z portálu Microsoft Azure klasické použijte nabídku "Řídicího PANELU" virtuálních sítí připojení brány brány. Pomocí příkazů "Připojit" v panelu nástrojů dole. Po několika minutách by měl řídicího panelu zobrazit podrobnosti o připojení graficky.

###<a name="step-8-create-the-virtual-machines-in-region-2"></a>Krok 8: Vytváření virtuálních počítačích v oblasti #2
Vytvořit obrázek se systémem Ubuntu podle popisu v oblasti #1 nasazení pomocí následujících stejné kroky nebo zkopírovat soubor obrázku virtuální pevný disk účet Azure úložiště umístěn v oblasti #2 a obrázek. Použijte tento obrázek a vytvořte následující seznam virtuálních počítačích do nového Cloudová služba hk-c-svc východ NAS:


| Název počítače | Podsítě | IP adresa | Nastavení dostupnosti | Datacentrum/regálů | Počáteční hodnota? |
| ------------ | ------ | ---------- | ---------------- | ------- | ----- |
| HK-c1-východ NAS | data  | 10.2.2.4   | HK-c sada-1      | datacentrum = EASTUS regálů = rack1 | Ano |
| HK-c2-východ NAS | data  | 10.2.2.5   | HK-c sada-1      | datacentrum = EASTUS regálů = rack1 | Ne  |
| HK-c3-východ NAS | data  | 10.2.2.6   | HK-c sada-1      | datacentrum = EASTUS regálů = rack2 | Ano |
| HK-c5-východ NAS | data  | 10.2.2.8   | HK-c sada-2      | datacentrum = EASTUS regálů = rack3 | Ano |
| HK-c6-východ NAS | data  | 10.2.2.9   | HK-c sada-2      | datacentrum = EASTUS regálů = rack3 | Ne  |
| HK-c7-východ NAS | data  | 10.2.2.10  | HK-c sada-2      | datacentrum = EASTUS regálů = rack4 | Ano |
| HK-c8-východ NAS | data  | 10.2.2.11  | HK-c sada-2      | datacentrum = EASTUS regálů = rack4 | Ne  |
| HK B1 východ NAS | Web   | 10.2.1.4   | HK-w sada-1      | NENÍ K DISPOZICI                    | NENÍ K DISPOZICI |
| HK-w2-východ NAS | Web   | 10.2.1.5   | HK-w sada-1      | NENÍ K DISPOZICI                    | NENÍ K DISPOZICI |


Postupujte podle pokynů stejný jako oblast #1, ale použijte 10.2.xxx.xxx adresní prostor.

###<a name="step-9-configure-cassandra-on-each-vm"></a>Krok 9: Konfigurace Cassandra na každý OM
Přihlaste se k OM a proveďte následující kroky:

1. Úprava $CASS_HOME/conf/cassandra-rackdc.properties změnit vlastnosti dat centra a regálů ve formátu: datacentrum = EASTUS regálů = rack1
2. Úprava cassandra.yaml ke konfiguraci uzlů počáteční hodnota: semen: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

###<a name="step-10-start-cassandra"></a>Krok 10: Zahájení Cassandra
Přihlaste se na každý OM a začněte Cassandra na pozadí tohoto příkazu: $CASS_HOME/Koš/cassandra

## <a name="test-the-multi-region-cluster"></a>Testování více oblastí obrázku
Nyní byla nasazena Cassandra 16 uzly s 8 uzlů v jednotlivých oblastech Azure. Tyto uzly jsou ve stejném clusteru na základě názvu běžné obrázku a konfigurace uzel počáteční hodnota. Chcete-li otestovat clusteru použijte následující postup:

###<a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Krok 1: Získání IP Vyrovnávání zatížení vnitřní oblastí pomocí prostředí PowerShell
- Get-AzureInternalLoadbalancer - ServiceName "hk-c-svc západní NAS"
- Get-AzureInternalLoadbalancer - ServiceName "hk-c-svc východ NAS"  

    Poznámka: IP adresy (například západní - 10.1.2.101, východ - 10.2.2.101) zobrazí.

###<a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Krok 2: Spuštění následující v oblasti západní po přihlášení do hk B1 západní NAS
1.    Spuštění $CASS_HOME/Koš/cqlsh 10.1.2.101 9160
2.  Provést následující příkazy CQL:

        CREATE KEYSPACE customers_ks
        WITH REPLICATION = { 'class' : 'NetworkToplogyStrategy', 'WESTUS' : 3, 'EASTUS' : 3};
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Měli byste vidět zobrazení podobné níže:

| customer_id | jméno | Příjmení |
| ----------- | --------- | -------- |
| 1           | Petr      | Karásek      |
| 2           | Petra      | Karásek      |


###<a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Krok 3: Provést následující v oblasti Východ po přihlášení do hk B1 východ NAS:
1.    Spuštění $CASS_HOME/Koš/cqlsh 10.2.2.101 9160
2.  Provést následující příkazy CQL:

        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Jak je vidět oblasti byste měli vidět stejné zobrazení:


| customer_id | jméno | Příjmení   |
|------------ | --------- | ---------- |
| 1           | Petr      | Karásek        |
| 2           | Petra      | Karásek        |


Provést několik dalších vloží a najdete v článku můžou být získat replikovat západ-cz část clusteru.

## <a name="test-cassandra-cluster-from-nodejs"></a>Test Cassandra obrázku z Node.js
Pomocí jedné z VMs Linux vytvořena "webu" úroveň dříve, jsme bude spustit jednoduchý skript Node.js číst dříve vložená data

**Krok 1: Nainstalovat Node.js a Cassandra klienta**

1. Instalace Node.js a npm
2. Instalace uzel balíčku "cassandra klienta" pomocí npm
3. Proveďte následující skript příkazovém řádku prostředí, které zobrazuje řetězce json načtená data:

        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };

        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }

        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }

        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);

           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }

        //update will also insert the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }

        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }

        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)


## <a name="conclusion"></a>Uzavření
Microsoft Azure je flexibilní platformu, která umožňuje fungování Microsoft jak otevřít zdroj softwaru, jak je ukázáno v tomto cvičení. Vysoce dostupných clusterů Cassandra můžete být nasazené na jedno datové centrum prostřednictvím šíření uzlů víc domén poruch. Cassandra clusterů lze také nasadit napříč několika geograficky vzdálené Azure oblastí pro havárie důkaz systémy. Azure a Cassandra společně umožňuje konstrukce vysoce scalable, vysoce dostupné a havárie obnovitelné cloudovým službám potřeby tak, že internet aktuálnímu měřítko služby.  

##<a name="references"></a>Odkazy##
- [http://cassandra.apache.org](http://cassandra.apache.org)
- [http://www.datastax.com](http://www.datastax.com)
- [http://www.nodejs.org](http://www.nodejs.org)
