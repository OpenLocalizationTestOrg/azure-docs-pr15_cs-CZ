<properties
    pageTitle="Podpora databáze SQL starších klientů a koncovém změní auditování | Microsoft Azure"
    description="Informace o databáze SQL podpora klientů a IP změny koncový bod pro auditování."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>Databáze SQL – podpora klientů a změny IP koncový bod pro auditování


[Auditování](sql-database-auditing-get-started.md) automaticky spolupracuje SQL klienti, které podporují Neuvedeno přesměrování.


##<a id="subheading-1"></a>Podpora klientů

Klient, Neuvedeno 7.4, které by také podporují přesměrování. Výjimky zahrnout JDBC 4.0 ve kterém není plně podporované funkce přesměrování a Tedious Node.JS v které přesměrování nebyl implementovaná.

"Starších klientů" tedy který podpory Neuvedeno verze 7.3 a pod - server plně kvalifikovaný název domény v připojení řetězec třeba změnit:

Původní serveru plně kvalifikovaný název domény v připojovacím řetězci: <*název serveru*>. database.windows.net

Změněné serveru plně kvalifikovaný název domény v připojovacím řetězci: <*název serveru*> .database. **zabezpečené**. windows.net

Obsahuje seznam "Starších klientů":

- .NET 4.0 a níže,
- Rozhraní ODBC 10.0 a dole.
- JDBC (během JDBC podporuje Neuvedeno 7.4, funkce přesměrování Neuvedeno není plně podporované)
- Únavné (pro Node.JS)

**Poznámka:** Výše uvedené serveru FDQN změny může být užitečné pro použití zásad auditování úroveň SQL Server bez potřeba krok konfigurace na každou databázi (dočasné řešení).

##<a id="subheading-2"></a>Koncový bod IP se změní při aktivaci auditování

Upozorňujeme, že pokud povolíte auditování, dojde ke změně koncovém databáze. Pokud máte nastavení striktně brány firewall, aktualizujte tyto nastavení brány firewall příslušným způsobem.

Nové databáze koncovém závisí na oblast databáze:

| Oblast databáze | Možné koncové body IP |
|----------|---------------|
| Severní Číně  | 139.217.29.176, 139.217.28.254 |
| Čína východ  | 42.159.245.65, 42.159.246.245 |
| Austrálie východ  | 104.210.91.32 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Austrálie jihovýchodní | 191.239.184.223 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Brazílie jih  | 104.41.44.161 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| Centrální USA  | 104.43.255.70 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Východní Asie   | 23.99.125.133 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| Východní USA 2 | 104.209.141.31 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Východní USA   | 23.96.107.223 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Centrální Indie  | 104.211.98.219, 104.211.103.71 |
| Jižní Indie   | 104.211.227.102, 104.211.225.157 |
| Západní Indie  | 104.211.161.152, 104.211.162.21 |
| Japonsko východ   | 104.41.179.1 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Japonsko západní    | 104.214.140.140 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Severní centrální USA  | 191.236.155.178 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Severní Evropě  | 104.41.209.221 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Jižní centrální USA  | 191.238.184.128 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Jihovýchodní Asie  | 104.215.198.156 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Západní Evropě  | 104.40.230.120 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| Západ USA  | 191.236.123.146 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Centrální Kanada  | 13.88.248.106 |
| Kanada východ  |  40.86.227.82 |
