<properties
   pageTitle="Místně sledovat a Diagnostika služby napsané pomocí struktury služby Azure | Microsoft Azure"
   description="Zjistěte, jak sledovat a Diagnostika služeb vytvořených pomocí struktury služby Microsoft Azure na místní rozvoj počítač."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Sledování a Diagnostika služby v nastavení vývoj místního počítače


> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Monitorování, zjišťování diagnostiky a Poradce při potížích s povolit služby pokračujte minimální přerušení uživatelského prostředí. Sledování a diagnostice je považován za kritický v skutečné nasazeném provozním prostředí aplikace. Přijetí podobné modelu průběhu vývoje služby zaručuje, že funguje diagnostiky kanálu obsah přesouvaných provozním prostředí. Služba struktury snadno pro vývojáře služby implementovat diagnostiky, můžete pracovat bez problémů různých jednom počítači místní rozvoj nastavení a nastavení clusteru reálný výrobní.


## <a name="debugging-service-fabric-java-applications"></a>Ladění aplikace služby struktury Java

Aplikace Java jsou k dispozici [více protokolování rámce](http://en.wikipedia.org/wiki/Java_logging_framework) . Protože `java.util.logging` je výchozí možnost s JRE, používá se také [příklady kódu v github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Následující diskusi vysvětluje, jak nakonfigurovat `java.util.logging` framework. 
 
Použití java.util.logging můžete přesměrovat protokoly aplikace výstup proudů, sockets, soubory konzoly či paměti. U každé z těchto možností jsou obsaženy v rámci popisovače výchozí. Můžete vytvořit `app.properties` soubor, který chcete konfigurovat obslužné souboru pro aplikaci přesměrovat všechny protokoly do místní soubor. 

Příklad konfigurace obsahuje následující fragment kódu: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Složku odkazuje `app.properties` soubor musí existovat. Po `app.properties` soubor je vytvořen, musíte taky změnit skriptem čárky položka `entrypoint.sh` v `<applicationfolder>/<servicePkg>/Code/` složku, kterou chcete nastavit vlastnost `java.util.logging.config.file` k `app.propertes` soubor. Položka by měl vypadat takto fragment kódu:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Konfigurace výsledkem protokoly odebraná rotujících způsobem na `/tmp/servicefabric/logs/`. **%U** a **%g** umožňují vytvářet další soubory s názvy souborů mysfapp0.log, mysfapp1.log a tak dál. Ve výchozím nastavení je pokud nakonfigurovaný explicitně žádné rutinu, registrovaná obslužné konzoly. Protokoly jednu můžete zobrazit v syslog v části /var/log/syslog.
 
Další informace najdete v tématu [příklady kódu v github](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Další kroky
Trasování kódem přidán do aplikace funguje taky pomocí diagnostiky aplikace Azure clusteru. Podívejte se na tyto články, které diskutovat o různých možností pro nástroje a popisují, jak můžete nastavit je.
* [Jak získat protokoly pomocí diagnostiky Azure](service-fabric-diagnostics-how-to-setup-lad.md)
