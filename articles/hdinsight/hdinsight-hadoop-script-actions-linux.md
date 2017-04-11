<properties
    pageTitle="Skript akce vývoj na základě Linux HDInsight | Microsoft Azure"
    description="Jak upravit clusterů na základě Linux HDInsight pomocí skriptu akce. Skript akce jsou způsob, jak přizpůsobit Azure HDInsight clusterů zadání nastavení konfigurační obrázku nebo instalaci dalších službách, nástroje nebo jiného softwaru na clusteru. "
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="larryfr"/>

# <a name="script-action-development-with-hdinsight"></a>Vývoj akce skriptů s HDInsight

Skript akce jsou způsob, jak přizpůsobit Azure HDInsight clusterů zadání nastavení konfigurační obrázku nebo instalaci dalších službách, nástroje nebo jiného softwaru na clusteru. Skript akce můžete použít při vytváření obrázku nebo pracovního clusteru.

> [AZURE.NOTE] Informace v tomto dokumentu je specifické pro clusterů na základě Linux HDInsight. Informace o použití akce skriptu se systémem Windows clusterů najdete v tématu [vývoj akce skriptů s HDInsight (Windows)](hdinsight-hadoop-script-actions.md).

## <a name="what-are-script-actions"></a>Jaké akce skriptu?

Skript akce jsou skripty flám Azure běží na uzlů provést změny konfigurace nebo instalace softwaru. Skript akce se spustí jako kořenovou a nabízí plný přístup práva uzlů.

Skript akce se dají použít následujícími způsoby:

| Použijte tento skript... | Během clusteru vytváření... | Průběžný clusteru... |
| ----- |:-----:|:-----:|
| Azure portálu | ✓ | ✓ |
| Azure Powershellu | ✓ | ✓ |
| Azure rozhraní příkazového řádku | &nbsp; | ✓ |
| HDInsight .NET SDK | ✓ | ✓ |
| Azure správce prostředků šablony | ✓ | &nbsp; |

Další informace o použití těchto postupů použít skript akce najdete v článku [přizpůsobení HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Doporučené postupy pro vývoj skriptů

Při vytváření vlastní skript pro HDInsight clusteru jsou mít na paměti několik doporučené postupy:

- [Cílová Hadoop verze](#bPS1)
- [Operační systém verze obrázku](#bps10)
- [Stabilní odkazy na prostředky skriptu](#bPS2)
- [Použijte předem zkompilované prostředky](#bPS4)
- [Zajistěte, aby byl clusteru přizpůsobení skriptu idempotent](#bPS3)
- [Zajištění dostupnost architektury obrázku](#bPS5)
- [Konfigurace vlastních součástí používat úložiště objektů Blob Azure](#bPS6)
- [Napište informace STDOUT a STDERR](#bPS7)
- [Ukládat soubory ve formátu ASCII s LF konce řádků](#bps8)
- [Obnovení ze přechodná chyby pomocí Logika opakování](#bps9)

> [AZURE.IMPORTANT] Skript akce musí dokončit 60 minut nebo bude časový limit. Během vytváření uzel skript spustil současně další nastavení a konfigurace procesy. Soutěže pro zdroje, jako jsou šířka pásma procesoru čas nebo síti může způsobovat skript trvat déle než vývojové prostředí.

### <a name="bPS1"></a>Cílová Hadoop verze

Různé verze aplikací HDInsight používají různé verze služby Hadoop a součásti nainstalovaný. Pokud skript očekává určité verze služby nebo součásti, používejte pouze skriptu pomocí verze Hdinsightu, který obsahuje požadované součásti. Můžete najít informace o součást verzích zahrnutý v sadě HDInsight pomocí [HDInsight součást správy verzí](hdinsight-component-versioning.md) dokumentu.

###<a name="bps10"></a>Operační systém verze obrázku

Na základě Linux HDInsight vychází z rozdělení se systémem Ubuntu Linux. Různé verze aplikací HDInsight přes v různých verzích se systémem Ubuntu, která může provést chování skriptem. Například vycházejí HDInsight 3.4 a starší verze se systémem Ubuntu, které používají Upstart. Verze 3.5 je založena na 16.04 systémem Ubuntu, který využívá Systemd. Systemd a Upstart spolehnout různé příkazy, tak by se měly zapisovat skriptu pro práci s obojí.

Další důležité rozdíl mezi HDInsight 3.4 a 3.5 je to `JAVA_HOME` teď odkazuje na Java 8.

Operační systém verze můžete zkontrolovat pomocí `lsb_release`. Následující fragmenty kódu z skriptem nainstalovat odstín ukazuje, jak zjistit, jestli je spuštěný skript na 14 systémem Ubuntu nebo 16:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi
    ...
    if [[ $OS_VERSION == 16* ]]; then
        echo "Using systemd configuration"
        systemctl daemon-reload
        systemctl stop webwasb.service    
        systemctl start webwasb.service
    else
        echo "Using upstart configuration"
        initctl reload-configuration
        stop webwasb
        start webwasb
    fi
    ...
    if [[ $OS_VERSION == 14* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
    elif [[ $OS_VERSION == 16* ]]; then
        export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    fi

Můžete najít celou skript, který obsahuje tyto fragmenty kódu v https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Verze se systémem Ubuntu používanou Hdinsightu najdete v článku [HDInsight součásti verze](hdinsight-component-versioning.md) dokumentu.

Rozdíly mezi Systemd a Upstart, najdete v tématu [Systemd Upstart uživatelům](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Stabilní odkazy na prostředky skriptu

Nezapomeňte, že skripty a zdrojů použitých skriptem zůstávají k dispozici po celou dobu clusteru a verzích tyto soubory se nezmění po celou dobu. Pokud nové uzly se přidají do clusteru během měřítka operace je nutné tyto materiály.

Doporučený postup je stáhnout a archivovány všechny položky v úložišti Azure účet předplatného.

> [AZURE.IMPORTANT] Úložiště účet použili musí být na výchozí úložiště účtu clusteru nebo kontejneru veřejné, jen pro čtení na jiný účet úložiště.

Například ukázky poskytované společností Microsoft ukládají do účtu úložiště [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) , který je kontejner veřejnosti, jen pro čtení zachovány týmem HDInsight.

### <a name="bPS4"></a>Použijte předem zkompilované prostředky

Snížit časové náročnosti následujícím způsobem, vyhněte se operacích, které kompilaci zdroje ze zdrojového kódu. Místo toho předem kompilaci zdrojů a uložte binární verze úložiště objektů Blob Azure tak, aby je mohli rychle stáhnout clusteru z skriptu.

### <a name="bPS3"></a>Zajistěte, aby byl clusteru přizpůsobení skriptu idempotent

Skripty musí navržené tak, aby idempotent znamená, pokud je skript spustili tisknutím, by měly zajistit, že clusteru je vrácena stejný stát pokaždé, když se přehrávala.

Pokud vlastní skript nainstaluje aplikace na /usr/local/bin třeba jeho první spuštění a pak na každý další spustit skript zaškrtněte zda aplikace již existuje v umístění /usr/local/bin před pokračováním další kroky v skript.

### <a name="bPS5"></a>Zajištění dostupnost architektury obrázku

Na základě Linux clusterů HDInsight poskytuje dva hlavní uzlů, které jsou aktivní v rámci clusteru a skript, který akce jsou byl spuštěn pro oba uzly. Pokud součásti očekává pouze jeden hlavy uzel, je nutné vytvořit skript, který bude pouze nainstalujte na jeden z dva hlavní uzlů v clusteru.

> [AZURE.IMPORTANT] Výchozí služby nainstalovali jako součást sady HDInsight slouží k selhání mezi dva hlavní uzlů v případě potřeby, ale není tato funkce rozšířit na vlastní součástí prostřednictvím skriptu ctions. V případě potřeby součástí prostřednictvím skriptu akce vysoce přístupný je nutné implementovat vlastní mechanismus přepnutí, využívající dvou hlavy uzly k dispozici.

### <a name="bPS6"></a>Konfigurace vlastních součástí používat úložiště objektů Blob Azure

Součásti instalované na clusteru pravděpodobně výchozí konfigurace, který používá úložiště Hadoop Distributed soubor systému (HDFS). HDInsight používá jako výchozí úložiště úložiště objektů Blob Azure (WASB). To umožňuje systém kompatibilní souborů HDFS, které ukládá dat, i když se odstraní clusteru. Součásti nainstalovat pro práci s WASB místo HDFS, byste měli označit.

Například následující zkopíruje giraph examples.jar soubor z místního systému souborů do WASB:

    hadoop fs -copyFromLocal /usr/hdp/current/giraph/giraph-examples.jar /example/jars/

### <a name="bPS7"></a>Napište informace STDOUT a STDERR

Aby došlo k zápisu STDOUT a STDERR během spouštění skriptů informace zaznamenané a lze zobrazit pomocí webového Ambari uživatelského rozhraní.

> [AZURE.NOTE] Ambari bude k dispozici pouze pokud clusteru je úspěšně vytvořen. Pokud používáte akci skriptu během vytváření clusteru a vytvoření se nezdaří, naleznete v části Poradce při potížích [že přizpůsobení HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) pro jiné způsoby přístupu k protokolované informace.

Většině nástrojů a instalační balíčky se už informace k zápisu STDOUT a STDERR, ale budete chtít přidat další protokolování. Odeslání text na článek použití STDOUT `echo`. Příklad:

        echo "Getting ready to install Foo"

Ve výchozím nastavení `echo` odešle řetězec STDOUT. Pokud chcete odkázat ho STDERR, přidejte `>&2` před `echo`. Příklad:

        >&2 echo "An error occurred installing Foo"

To přesměruje informace odesílané do STDOUT (1, je to není uvedený tady ve výchozím nastavení) na STDERR (2). Další informace o přesměrování vstupu a výstupu najdete v tématu [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Další informace o prohlížení informace zaznamenané skriptem akce najdete v článku [přizpůsobení HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

###<a name="bps8"></a>Ukládat soubory ve formátu ASCII s LF konce řádků

Flám skripty je uložit jako formát ASCII, s řádky ukončí LF. Pokud soubory jsou uloženy jako UTF-8, která může obsahovat značku pořadí bajtů na začátku souboru, nebo společně s konců Line FEED, což je běžné editory Windows, se nepovede skript s chybami podobná této:

    $'\r': command not found
    line 1: #!/usr/bin/env: No such file or directory

###<a name="bps9"></a>Obnovení ze přechodná chyby pomocí Logika opakování

Při stahování souborů, instalaci balíčků pomocí výstižný get nebo jiné akce, které přenášet data přes internet, akce může selhat důsledku přechodná sítě chyb. Například vzdálené zdrojů komunikujete pravděpodobně probíhá při přechodu na záložní uzel.

Aby skript pružné přechodná chyb, můžete používat Logika opakování. Následující obrázek je příkladem funkci, která poběží příkazům předaný k němu a (Pokud se příkaz nezdaří,) až třikrát zopakovat. Počká dvě sekundy mezi jednotlivými opakováními.

    #retry
    MAXATTEMPTS=3

    retry() {
        local -r CMD="$@"
        local -i ATTMEPTNUM=1
        local -i RETRYINTERVAL=2

        until $CMD
        do
            if (( ATTMEPTNUM == MAXATTEMPTS ))
            then
                    echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                    return 1
            else
                    echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                    sleep $(( RETRYINTERVAL ))
                    ATTMEPTNUM=$ATTMEPTNUM+1
            fi
        done
    }

Následující příklady použití této funkce.

    retry ls -ltr foo

    retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh

## <a name="helpermethods"></a>Pomocné metody pro vlastních skriptů

skript akce pomocné metody jsou nástroje, které můžete použít při vytváření vlastních skriptů. Tyto se definují [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)a mohou být součástí skriptů pomocí následujících akcí:

    # Import the helper method module.
    wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh

Díky následující nápovědy k dispozici pro použití v skript:

| Použití pomocníka | Popis |
| ------------ | ----------- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` | Stahování souboru z zdrojová adresa URL zadaná cesta k souboru. Ve výchozím nastavení nepřepíše existující soubor. |
| `untar_file TARFILE DESTDIR` | Extrahuje vkládání souboru (pomocí `-xf`,) do cílového adresáře. |
| `test_is_headnode` | Pokud spuštění na hlavní clusteru, vrátí hodnotu 1; v opačném 0. |
| `test_is_datanode` | Pokud je aktuální uzel uzel dat (pracovní), vrátí hodnotu 1; v opačném 0. |
| `test_is_first_datanode` | Pokud je aktuální uzel uzel první dat (pracovní) (pojmenovali workernode0), vrátí hodnotu 1; v opačném 0. |
| `get_headnodes` | Vrátí plně kvalifikovaný název domény headnodes clusteru. V názvech se oddělený středníkem. Prázdný řetězec je vrácena chyba. |
| `get_primary_headnode` | Získá plně kvalifikovaný název domény primární headnode. Prázdný řetězec je vrácena chyba. |
| `get_secondary_headnode` | Získá plně kvalifikovaný název domény sekundární headnode. Prázdný řetězec je vrácena chyba. |
| `get_primary_headnode_number` | Získá číselné přípony primární headnode. Prázdný řetězec je vrácena chyba. |
| `get_secondary_headnode_number` | Získá číselné přípony sekundární headnode. Prázdný řetězec je vrácena chyba. |

## <a name="commonusage"></a>Společné použití vzorky

Tato část obsahuje pokyny na některé běžné vzorce použití, které můžou nastat při psaní vlastní skript implementace.

### <a name="passing-parameters-to-a-script"></a>Předávání parametrů skriptu

V některých případech může vyžadovat skript parametry. Například možná budete muset heslo správce pro clusteru za účelem získání informací z rozhraní REST API Ambari.

Parametry předaný skript označují jako _Parametry pozice_a přiřazené `$1` jako první parametr `$2` parametru second a tak, aby aktivní. `$0`obsahuje název samotného skriptu.

Hodnoty předávány skriptu jako parametry by měl být uzavřeny jednoduchých uvozovek (') tak, aby předaná hodnota je považována za literál a zvláštní pozornost není věnovat však započítávány znaky jako '! ".

### <a name="setting-environment-variables"></a>Nastavení prostředí proměnných

Nastavení proměnné prostředí se provádí následující:

    VARIABLENAME=value

Kde je PROMPTTEXT název proměnné. Přístup k proměnné po uplynutí tohoto, můžete `$VARIABLENAME`. Například pro přiřazení hodnoty zadané pozice parametrem jako proměnnou prostředí s názvem heslo, použijete takto:

    PASSWORD=$1

Pak můžete použít následující přístupu k informacím `$PASSWORD`.

Proměnné nastavená skript existovat pouze v rozsahu skript. V některých případech budete muset přidat systém široké proměnné, které bude trvat po dokončení skriptu. Obvykle jde tak, aby uživatelé připojující se k obrázku prostřednictvím SSH můžete využívat součástí skriptu. Je možné provádět přidáním proměnnou prostředí `/etc/environment`. Například následující přidá __HADOOP\_CONF\_DIR__:

    echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Přístup k místa, kde jsou uloženy vlastních skriptů

Slouží k přizpůsobení clusteru skripty potřebuje buď lze v okně výchozí úložiště účet u obrázku nebo, pokud na jiný účet úložiště v kontejneru veřejné jen pro čtení. Pokud skript odkazuje na prostředky umístěné jinde tyto musejí být také v veřejně přístupný (minimálně veřejné jen pro čtení). Můžete například chtít stažení souboru obrázku pomocí `download_file`.

Uložení souboru v účet Azure úložiště přístupných osobám s postižením cluster (například výchozí úložiště účet), poskytne rychlý přístup, je toto úložiště v rámci Azure sítě.

### <a name="checking-the-operating-system-version"></a>Kontrola verze operačního systému

Různé verze aplikací HDInsight přes v jednotlivých verzích se systémem Ubuntu. Doména může obsahovat rozdílů mezi verzemi OS, které je třeba změnami pro skript. Například budete muset nainstalovat binární soubor, který je svázané se verzi systémem Ubuntu.

Kontrola verze operačního systému, použijte `lsb_release`. Například následující ukazuje, jak odkazovat na jiný vkládání soubor v závislosti na operační systém verze:

    OS_VERSION=$(lsb_release -sr)
    if [[ $OS_VERSION == 14* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
        HUE_TARFILE=hue-binaries-14-04.tgz
    elif [[ $OS_VERSION == 16* ]]; then
        echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
        HUE_TARFILE=hue-binaries-16-04.tgz
    fi

## <a name="deployScript"></a>Kontrolní seznam pro nasazení akci skriptu

Tady je postup, trvání jsme přípravu nasazení tyto skripty:

- Umístění souborů, které obsahují vlastních skriptů na místě, přístupný pomocí uzlů během nasazení. Může to být v kterémkoliv výchozích nebo další úložiště účty zadané v době nasazení obrázku nebo jiné kontejner veřejně přístupný úložiště.

- Přidání kontroly do skripty a ujistěte se, že provádějí impotently, tak, aby skript jde spouštět tisknutím ve stejném uzlu.

- Pomocí /tmp adresáře dočasný soubor zachovat stažené soubory používané skripty a potom vyčistit je po provedení mít skriptů.

- V případě, že byly změněny nastavení na úrovni OS nebo souborů Hadoop služby konfigurace, je vhodné restartování HDInsight služby, které můžete vybrat všechny úrovně s operačním systémem nastavení, například proměnné nastavení v skripty.

## <a name="runScriptAction"></a>Jak spustit akci skriptu

Akce skriptu slouží k přizpůsobení clusterů HDInsight pomocí portálu Azure Azure Powershellu, šablony Azure zdroje správce (ARM) nebo HDInsight .NET SDK. Pokyny najdete v tématu [jak používat akci skriptu](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Vlastní skript vzorky

Microsoft poskytuje ukázky skriptů instalace součásti clusteru HDInsight. Ukázky skriptů a pokyny, jak je používat jsou dostupné v těchto odkazů:

- [Instalace a používání odstín v HDInsight clusterů](hdinsight-hadoop-hue-linux.md)
- [Instalace a používání R v HDInsight Hadoop clusterů](hdinsight-hadoop-r-scripts-linux.md)
- [Instalace a používání Solr v HDInsight clusterů](hdinsight-hadoop-solr-install-linux.md)
- [Instalace a používání Giraph v HDInsight clusterů](hdinsight-hadoop-giraph-install-linux.md)  

> [AZURE.NOTE] Dokumenty propojené nad jsou specifické pro clusterů na základě Linux HDInsight. Skripty, které spolupracují s HDInsight serveru s Windows najdete v článku [vývoj akce skriptů s HDInsight (Windows)](hdinsight-hadoop-script-actions.md) nebo pomocí odkazů k dispozici v horní části každé článek.

##<a name="troubleshooting"></a>Řešení potíží

Následující seznam uvádí chyby, ke kterým může dojít při použití skriptů, kterou jste vytvořili:

__Error__: `$'\r': command not found`. Někdy následovaný `syntax error: unexpected end of file`.

_Příčina_: k této chybě dojde, pokud řádky v skript končit Line FEED. Systémy UNIX počítat pouze LF jako pole Konec řádku.

Tento problém nejčastěji bude proveden poté skript vytvořeny v prostředí Windows je Line FEED běžné řádku končícím pro mnoho textovém editoru v systému Windows.

_Řešení_: Pokud jednu z možností v textovém editoru, vyberte formát Unix nebo LF pro pole Konec řádku. Může taky pomocí následujících příkazů systémem Unix přejděte Line FEED LF:

> [AZURE.NOTE] Následující příkazy se zhruba ekvivalentní, že je potřeba změnit konce řádků Line FEED na LF. Vyberte jednu podle nástrojů k dispozici v počítači.

| Příkaz | Poznámky |
| ------- | ----- |
| `unix2dos -b INFILE` | Původní soubor zálohovat pomocí. Rozšíření BAK |
| `tr -d '\r' < INFILE > OUTFILE` | VÝSTUPNÍ_SOUBOR bude obsahovat verzí použít pouze LF zakončení |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Tento soubor upraví přímo bez vytvoření nového souboru |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` | VÝSTUPNÍ_SOUBOR bude obsahovat verzí použít pouze LF konce.

__Error__: `line 1: #!/usr/bin/env: No such file or directory`.

_Příčina_: k této chybě dochází při uložení skript jako UTF-8 s označit pořadí bajt (BOM).

_Řešení_: uložte soubor jako ASCII nebo jako UTF-8 bez Kusovník. K vytvoření nového souboru bez Kusovníku může taky použít tento příkaz Linux nebo Unix systému:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

K příkazu výše nahraďte __VSTUPNÍ_SOUBOR__ soubor, ve kterém Kusovníku. __VÝSTUPNÍ_SOUBOR__ by měl být nový název souboru, který bude obsahovat skriptem bez Kusovníku.

## <a name="seeAlso"></a>Další kroky

* Zjistěte, jak [Přizpůsobit HDInsight clusterů pomocí skriptu akce](hdinsight-hadoop-customize-cluster-linux.md)

* Další informace o vytváření .NET aplikací, které spravují HDInsight použít [odkaz HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx)

* Naučte se používat ZBÝVAJÍCÍ provádět akce správy clusterů HDInsight pomocí rozhraní [HDInsight REST API](https://msdn.microsoft.com/library/azure/mt622197.aspx) .
