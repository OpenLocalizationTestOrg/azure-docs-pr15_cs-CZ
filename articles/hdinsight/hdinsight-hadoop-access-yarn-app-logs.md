<properties
    pageTitle="Aplikace Access Hadoop vláken protokoly programově | Microsoft Azure"
    description="Aplikace Access se přihlásí programově clusteru Hadoop v HDInsight."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Aplikace Access vláken přihlásí HDInsight serveru s Windows

Toto téma vysvětluje, jak získat přístup ke protokoly pro aplikace vláken (ještě jiného zdroje Vyjednávač), které dokončili clusteru Hadoop v Azure HDInsight

> [AZURE.NOTE] Informace v tomto dokumentu platí pouze pro clusterů serveru s Windows HDInsight. Informace o přístupu k vláken přihlášení na základě Linux HDInsight clusterů, najdete v [aplikaci Access vláken přihlášení na základě Linux Hadoop na HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Zjistit předpoklady pro

- Shluk serveru s Windows HDInsight.  V tématu [vytvoření systému Windows Hadoop clusterů HDInsight](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>Server vláken časové osy

<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">Vláken časovou osu serveru</a> obsahuje obecné informace o dokončených aplikací, jakož i aplikace framework konkrétní informace pomocí funkce dva různé rozhraní. Konkrétně:

* Ukládání a vyhledávání informací obecné aplikace na HDInsight clusterů není povolené s verzí 3.1.1.374 nebo vyšší.
* Součást aplikace framework specifické informace o serveru časovou osu není momentálně neexistuje na clusterů HDInsight.


Obecné informace o aplikacích obsahuje následující typů dat:

* ID aplikace jedinečný identifikátor aplikace
* Uživatele, který spustil aplikace
* Informace o pokusů dokončete aplikace
* Kontejnery používané jakýkoli pokus dané aplikace

Ve vaší HDInsight tyto informace budou uloženy Azure správcem úložiště historie v kontejneru výchozí výchozí účet Azure úložiště. Tento obecný údaje o dokončených aplikací můžete získat prostřednictvím rozhraní REST API:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Protokoly aplikací vláken a

VLÁKEN podporuje více programovací modelů (MapReduce jeden z nich) tak, že oddělení řízení zdrojů z plánování a sledování aplikací. Důvodem je prostřednictvím globální *ResourceManager* (SV), uzel jednoho pracovníka *NodeManagers* (NMs) a za aplikace *ApplicationMasters* (AMs). Dopoledne jednotlivé aplikace vyjednávání zdroje (procesoru paměti, disku, sítě) pro spuštění aplikace se RM. Správce prostředků spolupracuje NMs udělit tyto materiály, které jsou poskytovány jako *kontejnery*. AM je zodpovědný za sledování průběhu kontejnery přiřazenou RM. Aplikace může vyžadovat mnoho kontejnery podle povahy aplikace.

Kromě toho jednotlivých aplikací může obsahovat více *aplikace pokusí o* k dokončení přítomnosti dojde k chybě nebo z důvodu ztráty komunikaci mezi dop a RM. Kontejnery jsou proto udělena konkrétní pokus o aplikace. Znamená kontejneru poskytuje kontext pro základní jednotka práci prováděnou aplikace vláken a všechny práce, která probíhá v rámci kontejneru probíhá na uzel jednoho pracovníka, na kterém byla přidělit kontejner. V tématu [Vláken koncepty] [ YARN-concepts] další kdykoliv při ruce.

Protokoly aplikace (a protokoly přidružené kontejneru) je považován za kritický v ladění problematický Hadoop aplikací. VLÁKEN poskytuje hodní rámec pro shromažďování, agregaci a ukládání protokolů aplikace s [Protokolu agregace] [ log-aggregation] funkce. Funkce agregace protokolu zajišťuje přístup k aplikaci protokoly deterministického, sloučí protokoly přes všechny kontejnery uzlu pracovní a ukládá jako jeden agregované soubor protokolu jednoho pracovníka uzel v systému souborů výchozí po ukončení aplikace. Aplikace může používat stovky nebo tisíce kontejnerů, ale protokoly pro všechny kontejnery spustit na jednoho pracovníka uzel bude vždycky agregované jednoho souboru s výsledkem jednoho souboru protokolu jednoho pracovníka uzel používané aplikací. Agregace protokolu je standardně zapnutá u HDInsight clusterů (verze 3.0 a nahoře), a agregovaná protokoly najdete ji v kontejneru výchozí svůj cluster v následujícím umístění:

    wasbs:///app-logs/<user>/logs/<applicationId>

V tomto umístění, je *uživatelské* jméno uživatele, který spustil aplikace a *applicationId* je jedinečný identifikátor aplikace jako přidělil RM. vláken

Souhrnné protokoly nejsou přímo čitelné zapsaný v [TFile][T-file], [binárním formátu] [ binary-format] indexované v kontejneru. VLÁKEN poskytuje nástroje rozhraní příkazového řádku pro výpis tyto protokoly jako prostý text u aplikací nebo kontejnery zájmu. Tyto protokoly můžete zobrazit jako prostý text tak, že některý z následujících vláken příkazy přímo na uzlů (po připojení k němu prostřednictvím RDP):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>Uživatelské rozhraní ResourceManager vláken

Uživatelské rozhraní ResourceManager vláken běží na headnode obrázku a můžete k nim získat přístup prostřednictvím řídicím panelu Azure portálu: 

1. Přihlaste se k [portálu Azure](https://portal.azure.com/). 
2. V nabídce nalevo klikněte na tlačítko **Procházet**, klikněte na **HDInsight clusterů**, klikněte na clusteru serveru s Windows, který chcete mít přístup protokoly aplikace vláken.
3. V nabídce začátek klikněte na **řídicí panel**. Zobrazí se stránka otevřít na nové prohlížeče karta jen **Konzoly dotazu HDInsight**.
4. Z **Konzoly HDInsight dotazu**klikněte na **Uživatelské rozhraní vláken**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
