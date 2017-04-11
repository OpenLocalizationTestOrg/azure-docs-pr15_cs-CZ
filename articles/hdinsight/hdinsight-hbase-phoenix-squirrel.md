<properties 
   pageTitle="Použití Apache Phoenix a SQuirreL v HDInsight | Microsoft Azure" 
   description="Zjistěte, jak používat Apache Phoenix v HDInsight a jak nainstalovat a nakonfigurovat SQuirreL na počítači se připojit k obrázku HBase v HDInsight." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Použití Apache Phoenix a SQuirreL se systémem Windows HBase clusterů HDinsight  

Zjistěte, jak používat [Apache Phoenix](http://phoenix.apache.org/) v HDInsight a jak nainstalovat a nakonfigurovat SQuirreL na počítači se připojit k obrázku HBase v HDInsight. Další informace o Phoenix najdete v článku [Phoenix v 15 minut a méně](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Gramatiky Phoenix najdete v článku [Phoenix gramatiky](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Informace o verzi Phoenix v HDInsight, najdete v článku [Co je nového v verze obrázku Hadoop poskytovanou HDInsight?] [hdinsight-versions].
>
> Informace v tomto dokumentu jsou specifické pro clusterů serveru s Windows HDInsight. Informace o použití Phoenix na základě Linux HDInsight najdete v tématu [Použití Apache Phoenix s na základě Linux HBase clusterů HDinsight](hdinsight-hbase-phoenix-squirrel-linux.md).

##<a name="use-sqlline"></a>Použití SQLLine
[SQLLine](http://sqlline.sourceforge.net/) je nástroj příkazového řádku pro spuštění SQL. 

###<a name="prerequisites"></a>Zjistit předpoklady pro
Než budete moct použít SQLLine, musíte mít takto:

- **A HBase obrázku v HDInsight**. Další informace o poskytování HBase clusteru najdete v článku [Začínáme s Apache HBase v HDInsight][hdinsight-hbase-get-started].
- **Připojit k obrázku HBase prostřednictvím protokolu vzdálené plochy**. Pokyny najdete v tématu [Správa Hadoop clusterů HDInsight pomocí portálu klasické Azure][hdinsight-manage-portal].

**Pokud chcete zjistit, název hostitele**

1. Otevřete **Příkazový řádek Hadoop** ze stolního počítače.
2. Spusťte tento příkaz získat přípona DNS:

        ipconfig

    Poznamenejte si **Přípony DNS specifické pro připojení**. Například *myhbasecluster.f5.internal.cloudapp.net*. Když se připojíte ke HBase clusteru, musíte jedné Zookeepers pomocí plně kvalifikovaný název domény. Každý cluster HDInsight má 3 Zookeepers. Jsou *zookeeper0* *zookeeper1*a *zookeeper2*. Úplný název domény budou nějak *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**Použití SQLLine**

1. Otevřete **Příkazový řádek Hadoop** ze stolního počítače.
2. Spusťte následující příkazy Otevřít SQLLine:

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![hdinsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    Příkazy ve výběru:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
        
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;

Další informace najdete v tématu [Ruční SQLLine](http://sqlline.sourceforge.net/#manual) a [Phoenix gramatiky](http://phoenix.apache.org/language/index.html).


















##<a name="use-squirrel"></a>Použití SQuirreL

[Klient SQL sQuirreL](http://squirrel-sql.sourceforge.net/) je grafický program Java, která vám umožní zobrazit se strukturou databáze kompatibilní se standardem JDBC, procházet data v tabulkách, příkazy SQL atd. Slouží k připojení k Apache Phoenix na HDInsight.

V této části se dozvíte, jak nainstalovat a nakonfigurovat SQuirreL na počítači se připojit k obrázku HBase v HDInsight přes VPN. 

###<a name="prerequisites"></a>Zjistit předpoklady pro

Před postupů, musíte mít takto:

- Clusteru služby HBase používaný Azure virtuální sítě s virtuálního počítače DNS.  Pokyny najdete v tématu [poskytování HBase clusterů Azure virtuální síti se systémem][hdinsight-hbase-provision-vnet]. 

    >[AZURE.IMPORTANT] Virtuální sítě je nutné nainstalovat serveru DNS. Pokyny najdete v tématu [Konfigurace DNS mezi dvěma Azure virtuálních sítí](hdinsight-hbase-geo-replication-configure-dns.md)

- Pokud potřebujete přípona připojení DNS HBase clusteru obrázku. K získání, RDP do clusteru a pak spusťte IPConfig.  Přípona DNS je podobný:

        myhbase.b7.internal.cloudapp.net
- Stáhněte a nainstalujte [Microsoft Visual Studio Express 2013 pro počítač s Windows](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) v počítači. Budete potřebovat makecert z balíčku vytvořit certifikát.  
- Stáhněte a nainstalujte [Prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) k počítači.  SQuirreL SQL klienta verze 3.0 a vyšší vyžaduje JRE verze 1,6 nebo vyšší.  


###<a name="configure-a-point-to-site-vpn-connection-to-the-azure-virtual-network"></a>Konfigurace připojení VPN čárky webu Azure virtuální sítě

Zahrnuje 3 kroky konfigurace připojení VPN čárky webu:

1. [Konfigurace virtuální sítě a dynamické směrování brány](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [Vytvoření vlastních certifikátů](#Create-your-certificates)
3. [Konfigurace klienta VPN](#Configure-your-VPN-client)

Další informace najdete v tématu [Konfigurace připojení VPN čárky webu k virtuální sítě Azure](../vpn-gateway/vpn-gateway-point-to-site-create.md) .

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Konfigurace virtuální sítě a dynamické směrování brány

Dosažení mít zřízení HBase obrázku v Azure virtuální sítě (viz požadavky pro tento oddíl). Dalším krokem je třeba nakonfigurovat připojení čárky webu.

**Konfigurace připojení čárky webu**

1. Přihlaste se k [Azure klasické portál][azure-portal].
2. Na levé straně klikněte na **sítích**.
3. Klikněte na virtuální sítě nevytvoříte (najdete v článku [poskytnutí HBase clusterů Azure virtuální síti se systémem][hdinsight-hbase-provision-vnet]).
4. Klikněte na **KONFIGUROVAT** shora.
5. V části **připojení čárky webu** vyberte **Konfigurovat čárky webu připojení**. 
6. Konfigurace **Spuštění IP** a **CIDR** určit rozsah IP adres, ze kterého klienty VPN dostanou IP adresy když připojení. Oblast nemůže překrývat pomocí kterékoli z oblastí v místní síti a Azure virtuální sítě, ke kterému se připojují. Příklad. Pokud jste vybrali 10.0.0.0/20 virtuální sítě, můžete vybrat 10.1.0.0/24 adresní prostor klienta. V tématu [Připojení čárky webu] [ vnet-point-to-site-connectivity] Další informace o.
7. V části mezery virtuální sítě adresa klikněte na **Přidat podsítě brány**.
7. Klikněte na **Uložit** v dolní části stránky.
8. Klikněte na **Ano** potvrďte změny. Počkejte, dokud systému proběhla provádějícího změnu před pokračováním k dalšímu postupu.


**Vytvoření dynamického směrování brány**

1. Z portálu Microsoft klasické Azure klikněte na **řídicí panel** v horní části stránky.
2. Klikněte na položku **Vytvořit brány** v dolní části stránky.
3. Klikněte na **Ano** potvrďte. Počkejte, až se vytvoří brány.
4. Klikněte na **řídicí panel** v horní.  Zobrazí se vizuální zobrazení virtuální sítě:

    ![Azure virtuální síťový diagram virtuální čárky webu][img-vnet-diagram] 

    Diagram zobrazuje 0 připojení klientů. Po vytvoření připojení k síti virtuální třeba číslo aktualizovat jednu. 

#### <a name="create-your-certificates"></a>Vytvoření vlastních certifikátů

Jedním ze způsobů vytvořit certifikát X.509 je pomocí certifikátu nástroji pro vytváření (makecert.exe), které jsou součástí [Microsoft Visual Studio Express 2013 pro počítač s Windows](https://www.visualstudio.com/products/visual-studio-express-vs.aspx). 


**Vytvoření certifikátu podepsaného svým držitelem kořenového**

1. V počítači otevřete okno příkazového řádku.
2. Přejděte do složky nástrojů Visual Studia. 
3. Příkaz v následujícím příkladu vytvoříte a instalace kořenového certifikátu v úložišti osobních certifikátů k počítači a také vytvořit odpovídající soubor .cer později nahrajete na portál klasické Azure. 

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    Přejděte na adresář, který se má soubor .cer umístit, kde HBaseVnetVPNRootCertificate je název, který chcete použít pro certifikát. 

    Nechejte příkazový řádek.  Je třeba v následujícím postupu.

    >[AZURE.NOTE] Vzhledem k tomu, že jste vytvořili kořenového certifikátu, ze které bude vytvořen certifikátů, můžete exportovat certifikát spolu s jeho privátním klíčem a uložte bezpečných umístění, kde může obnovit. 

**Chcete-li vytvořit certifikát klienta**

- Na stejném příkazovém řádku (musí být v počítači, které jste vytvořili kořenového certifikátu. Klientský certifikát musí být vygeneruje z kořenového certifikátu), spusťte tento příkaz:

        makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate je název kořenového certifikátu.  Musí shodovat s názvem kořenového certifikátu.  

    Kořenového certifikátu a certifikát klienta jsou uložené v úložišti osobních certifikátů na vašem počítači. Použijte příkaz certmgr.msc k ověření.

    ![Certifikát Azure virtuální sítě vpn čárky webu][img-certificate]

    Klientský certifikát musí být nainstalovaný na každém počítači, který chcete připojit k virtuální sítě. Doporučujeme vytvořit jedinečný klienta certifikáty pro každý počítač, který chcete připojit k virtuální sítě. Export certifikátů klienta, použijte příkaz certmgr.msc. 

**Nahrání kořenového certifikátu do portálu klasické Azure**

1. Z portálu Microsoft klasické Azure klikněte na **síť** na levé straně.
2. Klikněte na místo, kam svůj cluster HBase nasazených na virtuální síť.
3. Na začátek klikněte na položku **certifikáty** .
4. Klikněte na **Odeslat** ze spodního okraje a zadejte soubor kořenového certifikátu, kterou jste vytvořili v řízení před poslední. Počkejte, dokud máte Import certifikátu.
5. Klikněte na **řídicí panel** nahoře.  Virtuální diagram znázorňuje stav.


#### <a name="configure-your-vpn-client"></a>Konfigurace klienta VPN



**Stažení a instalace balíček klienta VPN**

1. Na řídicím panelu stránky virtuální sítě, v části první pohled dával buď **Stáhnout balíček pro 64bitovou verzi klienta VPN** klepněte na tlačítko **Stáhnout balíček pro 32bitovou verzi klienta VPN** podle workstation operační systém verze.
2. Klikněte na tlačítko **Spustit** a nainstalujte balíček.
3. Při zobrazení výzvy zabezpečení klikněte na **Další informace**a potom klikněte na **Spustit přesto**.
4. Dvakrát klikněte na **Ano** .

**Připojení k VPN**

1. Na ploše počítači, klikněte na ikonu sítí na hlavním panelu. Zobrazí se připojení VPN s názvem vaší virtuální sítě.
2. Klikněte na název připojení VPN.
3. Klikněte na **Připojit**.

**Chcete-li otestovat VPN připojení a domény překlad**

- Z workstation, otevřete příkazový řádek a ping, jednu z následujících názvů uvedené přípony DNS HBase obrázku je myhbase.b7.internal.cloudapp.net:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

###<a name="install-and-configure-squirrel-on-your-workstation"></a>Nainstalujte a nakonfigurujte SQuirreL na počítači

**Chcete-li nainstalovat SQuirreL**

1. SQuirreL SQL klienta sklenice moct soubor stáhněte z [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).
2. Otevření nebo spuštění sklenice soubor. Vyžaduje [Prostředí Java Runtime](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).
3. Dvakrát klikněte na **Další** .
4. Zadejte cestu, kde máte oprávnění k zápisu a klikněte na tlačítko **Další**.
    >[AZURE.NOTE] Výchozí složky instalace je ve složce C:\Program Files\squirrel-sql-3.6.  Chcete-li zapsat tyto materiály, instalační program musí mít oprávnění správce. Otevřete okno příkazového řádku jako správce, přejděte do složky tříd jazyka Java a znovu spusťte 
    >
    >     java.exe -jar [the path of the SQuirreL jar file] 
5. Kliknutím na **OK** potvrďte vytváření cílový adresář.
6. Ve výchozím nastavení je instalace Base a standardní balíčků.  Klikněte na tlačítko **Další**.
7. Dvakrát klikněte na **Další** a potom klikněte na tlačítko **Hotovo**.


**Chcete-li nainstalovat ovladač Phoenix**

Soubor sklenice phoenix ovladače se nachází na HBase obrázku. Cesta je podobná podle verze:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
Je potřeba ji zkopírujte počítači složce [SQuirreL instalace] / lib cestu.  Nejjednodušším způsobem je RDP do clusteru a potom použijte souborů zkopírujte (CTRL + C a CTRL + V) zkopírovat na počítači.

**Chcete-li přidat ovladač Phoenix SQuirreL**

1. Otevřete klienta SQL SQuirreL v počítači.
2. Klikněte na kartu **ovladač** na levé straně.
2. V nabídce **ovladače** klepněte na tlačítko **Nový**.
3. Zadejte následující informace:

    - **Název**: Phoenix
    - **Ukázková adresa URL**: jdbc:phoenix:zookeeper2.contoso hbase eu.f5.internal.cloudapp.net
    - **Název třídy**: org.apache.phoenix.jdbc.PhoenixDriver

    >[AZURE.WARNING] Uživatel všechna malá písmena v ukázková adresa URL. Můžete se úplné zookeeper kvora v případě, jedna z nich je dolů.  Názvy hostitelů jsou zookeeper0, zookeeper1 a zookeeper2.

    ![Ovladač HDInsight HBase Phoenix SQuirreL][img-squirrel-driver]
4. Klikněte na **OK**.

**Vytvoření aliasu clusteru HBase**

1. SQuirreL klikněte na kartu **aliasy** na levé straně.
2. V nabídce **aliasy** klikněte na **Nový Alias**.
3. Zadejte následující informace:

    - **Název**: název HBase obrázku nebo názvů dáváte přednost.
    - **Ovladač**: Phoenix.  To musí shodovat s názvem ovladač, který jste vytvořili v posledním postupu.
    - **Adresa URL**: URL se zkopírují z konfigurace ovladače. Nejdřív musíte mít uživatel všechna malá písmena.
    - **Uživatelské jméno**: je možné žádný text.  Vzhledem k tomu, že používáte připojení k síti VPN, uživatelské jméno se nepoužívá.
    - **Heslo**: může být žádný text.

    ![Ovladač HDInsight HBase Phoenix SQuirreL][img-squirrel-alias]
4. Klikněte na **Testovat**. 
5. Klikněte na **Připojit**. Při připojení, SQuirreL vypadá takto:

    ![HBase Phoenix SQuirreL][img-squirrel]

**Spusťte test**

1. Klikněte na kartu **SQL** hned vedle kartu **objekty** .
2. Zkopírujte a vložte následující kód:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Kliknutím na tlačítko spustit.

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. Přejděte zpět na kartu **objekty** .
5. Rozbalte název aliasu a potom rozbalte **tabulku**.  Zobrazí se nové tabulky zařazený do kategorie.
 
##<a name="next-steps"></a>Další kroky
V tomto článku se naučili použití Apache Phoenix v HDInsight.  Další informace najdete v tématu

- [Přehled HDInsight HBase][hdinsight-hbase-overview]: HBase je databáze NoSQL otevřít zdroje, Apache, založená na Hadoop poskytující RAM a silných konzistence pro velké objemy dat nestrukturovaná a semistructured.
- [Zřízení HBase clusterů Azure virtuální síti se systémem][hdinsight-hbase-provision-vnet]: integrace virtuální sítě HBase clusterů může být nasazené na stejné virtuální síti aplikace tak, aby aplikace komunikovat s HBase přímo.
- [Konfigurace HBase replikace v HDInsight](hdinsight-hbase-geo-replication.md): Přečtěte si, jak konfigurovat HBase replikace mezi dvěma Azure datacentrech. 
- [Analýza Twitter myšlenkou s HBase v HDInsight][hbase-twitter-sentiment]: Přečtěte si, jak to v reálném čase [myšlenkou analýza](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých dat pomocí HBase v clusteru Hadoop v HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
