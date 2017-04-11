<properties
 pageTitle="Princip a řešení chyb WebHCat na HDInsight"
 description="Zjistěte, jak o běžné chyby vrácené WebHCat na HDInsight a jejich řešení."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Princip a řešení chyb dostali od WebHCat (Templeton,) na HDInsight

Při použití WebHCat (dříve označované jako Templeton,) pro práci s HDInsight, může se zobrazit chyby. Tento dokument obsahuje pokyny na běžné chyby – Proč se vyskytují a co můžete udělat, abyste je vyřešit.

##<a name="what-is-webhcat"></a>Co je WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) je rozhraní REST API pro správu vrstvu [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tabulky a úložiště pro Hadoop. WebHCat je standardně zapnutá u HDInsight clusterů a pomocí různých nástrojů odeslat úlohy, bez přihlášení do clusteru můžete získat stav úlohy, atd.

##<a name="modifying-configuration"></a>Změna konfigurace

> [AZURE.IMPORTANT] Několik problémů uvedených v tomto dokumentu dochází, protože byla překročena nakonfigurované maximum. Když krok rozlišení zmínky, že můžete změnit hodnoty, použijte jeden z těchto věcí provádět změny:

* **Windows** clusterů: použití akce skript ke konfiguraci hodnotu během vytváření clusteru. Další informace najdete v tématu [akcí vytvořit skript](hdinsight-hadoop-script-actions.md).

* U **Linux** clusterů: použití Ambari (web nebo rozhraní REST API) změňte hodnotu. Další informace najdete v tématu [Správa HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>Výchozí konfigurace

Zde jsou výchozí konfigurace hodnoty, které můžou mít vliv na výkon WebHCat nebo způsobí chybu překročení tyto hodnoty:

| Nastavení | Co dělá | Výchozí hodnota |
| ------- | ------------ | ------------- |
| [yarn.Scheduler.Capacity.maximum aplikace][maximum-applications] | Maximální počet úloh, které mohou být souběžně aktivní (čeká na vyřízení ani pracovního) | 10 000 |
| [templeton.Exec.Max procs][max-procs] | Maximální počet požadavky, které lze zpracovat současně | 20 |
| [mapreduce.jobhistory.Max stáří ms][max-age-ms] | Spočítá počet dnů, které se zachovají historie úlohy | 7 dnů |

##<a name="too-many-requests"></a>Příliš mnoho požadavků

**Nastavit informace HTTP stavový kód**: 429

| Příčina | Rozlišení |
| ----- | ---------- |
| Byla překročena maximální souběžně prováděné žádosti hodit WebHCat za minutu (výchozí nastavení 20) | Zvýšení limit souběžně prováděné žádosti o změnou nebo snížení vytížení zajistit, že jste neposílejte vyšší než maximální počet souběžně prováděné žádosti `templeton.exec.max-procs`. Další informace najdete v tématu [Změna konfigurace](#modifying-configuration) |

##<a name="server-unavailable"></a>Server není dostupný.

**Nastavit informace HTTP stavový kód**: 503

| Příčina | Rozlišení |
| ---------------- | ------------------- |
| K tomu obvykle dojde při selhání mezi primárních a sekundárních HeadNode clusteru | Počkejte dvou minut a pak opakujte |

##<a name="bad-request-content-could-not-find-job"></a>Chybný požadavek obsahu: Nelze najít projektu

**Nastavit informace HTTP stavový kód**: 400

| Příčina | Rozlišení |
| ---------------- | ------------------- |
| Podrobnosti úlohy mít byla vyčistí tak, že historie úlohy Čistič | Výchozí období uchování předchozích je 7 dní. Lze změnit úpravou `mapreduce.jobhistory.max-age-ms`. Další informace najdete v tématu [Změna konfigurace](#modifying-configuration) |
| Vzhledem k selhání byl ukončen projektu | Opakujte odeslání až dvě minuty v rámci projektu |
| Byla použita neplatné id práce. | Kontrola správnosti id práce |

##<a name="bad-gateway"></a>Chybná brána

**Nastavit informace HTTP stavový kód**: 502

| Příčina | Rozlišení |
| ---------------- | ------------------- |
| Vnitřní uvolnění paměti probíhá v rámci procesu WebHCat | Počkejte uvolnění paměti dokončí, nebo restartovat službu WebHCat |
| Čekání na odpověď ze služby ResourceManager časový limit. K tomu může dojít při počet aktivních aplikací nakonfigurované maximum (výchozí nastavení 10 000) | Počkejte aktuálně spuštěných práce k dokončení nebo zvětšit limit souběžné úlohu změnou `yarn.scheduler.capacity.maximum-applications`. Další informace najdete v tématu [Změna konfigurace](#modifying-configuration)  |
| Když se pokusíte načíst všechny úlohy prostřednictvím volání [získat /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) při `Fields` je nastavený na`*` | Není načíst *všechny* podrobnosti projektu, místo toho použít `jobid` k načítání podrobností projektů pouze větší než určité id práce. Nebo nepoužívejte`Fields` |
| Služba WebHCat není dolů během HeadNode překlopení | Až dvě minuty a opakujte |
| Existuje více než 500 čekající úlohy ověřeny pomocí WebHCat | Počkejte, až aktuálně čekající úlohy dokončili před odesláním další úlohy |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
