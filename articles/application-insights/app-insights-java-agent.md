<properties 
    pageTitle="Sledování závislosti, výjimky a čas spuštění ve webových aplikacích pro Java" 
    description="Rozšířené sledování webu Java s přehledy aplikace" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Sledování závislosti, výjimky a čas spuštění ve webových aplikacích pro Java

*Přehledy aplikace je v náhledu.*

Pokud máte [vybavit přístroji webovou aplikaci Java s aplikací přehledy][java], můžete použít agenta Java a dozvědět se víc hlubší, bez jakýchkoli změn kód:


* **Závislosti:** Údaje o hovory, které aplikace umožňuje další součásti, včetně:
 * **ZBÝVAJÍCÍ hovory** prostřednictvím HttpClient OkHttp a RestTemplate (jarní).
 * **Redis** volání přes klienta Jedis. Pokud chcete hovor trvá déle než 10s, agent taky načte argumenty volání.
 * **[JDBC hovory](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, databáze Oracle nebo Apache Derby DB. podporuje volání "executeBatch". MySQL a PostgreSQL Pokud chcete hovor trvá déle než 10s, agent sestavy plán dotazů. 
* **Zachyceny výjimky:** Data o výjimek, které jsou uskutečněných jednotlivými kódu.
* **Čas spuštění metoda:** Data o dobu potřebnou k provedení určité metody.

Použití jazyka Java agent, nainstalujte na server. Webové aplikace musí rozdvojovaných s [Aplikací přehledy Java SDK][java].

## <a name="install-the-application-insights-agent-for-java"></a>Instalace aplikace přehledy agent jazyka Java

1. V počítači spuštěná Java serveru, [Stáhněte si agent](https://aka.ms/aijavasdk).
2. Úprava skriptu serveru při spuštění aplikace a přidejte následující JVM:

    `javaagent:`*úplnou cestu k souboru SKLENICE agent*

    Například v Tomcat na počítač Linux:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Restartujte aplikaci server.

## <a name="configure-the-agent"></a>Konfigurace agenta

Vytvoření souboru s názvem `AI-Agent.xml` a vložte ho do stejné složky jako soubor SKLENICE agent.

Nastavte obsah souboru xml. Následující příklad zahrnout nebo vynechat funkce, která se má úprav 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Je třeba povolit výjimce sestavy a způsob časování pro jednotlivé metody.

Ve výchozím nastavení `reportExecutionTime` platí a `reportCaughtExceptions` vyhodnotí jako NEPRAVDA.

## <a name="view-the-data"></a>Zobrazení dat

V aplikaci přehledy zdroje agregované vzdálené závislost a způsob spuštění časy se zobrazí [v části dlaždice výkonu][metrics]. 

Vyhledat jednotlivé instance závislosti, výjimky a způsob sestav, otevřete [hledání][diagnostic]. 

[Diagnostikování závislost problémy – Další informace](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Otázky? Máte problémy?

* Žádná data? [Nastavení výjimky brány firewall](app-insights-ip-addresses.md)
* [Poradce při potížích s Java](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
