<properties 
    pageTitle="Technologie pro analýzu toku: Otočení přihlašovací pro ni přihlašovací údaje vstupů a výstupů | Microsoft Azure" 
    description="Naučte se aktualizovat přihlašovací údaje pro analýzy toku vstupů a výstupů."
    keywords="přihlašovací údaje"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Otočení přihlašovací údaje pro zadávání a výstupy v toku analýzy úlohy

##<a name="abstract"></a>Shrnutí
Azure analýzy toku dnes neumožňuje nahrazení přihlašovací údaje na vstupní a výstupní je spuštěná úloha.

Během analýzy toku Azure podporuje obnovení úlohy z posledního výstupu, chtěli jsme sdílet celý proces pro minimalizaci prodlevy mezi zastavení a od úlohy a otočení přihlašovací údaje.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>Část 1: Příprava novou sadu přihlašovacích údajů:
Tato část je k dispozici následující vstupy/výstupy:

* Úložiště objektů BLOB
* Rozbočovače události
* Databáze SQL
* Úložiště tabulek

Pro ostatní vstupy/výstupy pokračujte část 2.

###<a name="blob-storagetable-storage"></a>Úložiště objektů BLOB úložiště a tabulek
1.  Přejděte na příponu úložiště na portálu Správa Azure:  
![graphic1][graphic1]
2.  Najděte úložiště používaný práce a přejděte do něho:  
![graphic2][graphic2]
3.  Klikněte na příkaz Správa přístupových kláves z verze:  
![graphic3][graphic3]
4.  Mezi primární klíč přístup a vedlejší přístupová klávesa, **vyberte požadovanou nevyužitá práce**.
5.  Chcete-li znovu přístupů vygenerovat:  
![graphic4][graphic4]
6.  Kopírování nově generovaného klíče:  
![graphic5][graphic5]
7.  Dál část 2.

###<a name="event-hubs"></a>Rozbočovače události
1.  Přejděte na koncovku Bus služby na portálu Správa Azure:  
![graphic6][graphic6]
2.  Najděte Namespace Bus Service používané práce a přejděte do něho:  
![graphic7][graphic7]
3.  Pokud vaše úloha používá sdílené přístupu na Namespace Bus služby, přeskočíte ke kroku 6  
4.  Přejděte na kartu rozbočovače událostí:  
![graphic8][graphic8]
5.  Najděte centru události používaný práce a přejděte do něho:  
![graphic9][graphic9]
6.  Přejděte na kartu konfigurovat:  
![graphic10][graphic10]
7.  V rozevíracím seznamu název zásady vyhledejte sdílené přístupu používaný práce:  
![graphic11][graphic11]
8.  Mezi primární klíč a vedlejší klíč, **vyberte požadovanou nevyužitá práce**.  
9.  Chcete-li znovu přístupů vygenerovat:  
![graphic12][graphic12]
10. Kopírování nově generovaného klíče:  
![graphic13][graphic13]
11. Dál část 2.  

###<a name="sql-database"></a>Databáze SQL

>[AZURE.NOTE] Poznámka: musíte se připojit ke službě SQL databáze. Budeme zobrazení, jak lze provést pomocí možností správy na portálu Správa Azure, ale můžete použít některé klientské nástroje třeba SQL Server Management Studio stejně.

1.  Přejděte na koncovku SQL databáze na portálu Správa Azure:  
![graphic14][graphic14]
2.  Vyhledejte SQL databázi používá váš projekt a **klikněte na serveru** odkaz na stejném řádku:  
![graphic15][graphic15]
3.  Klikněte na příkaz Spravovat:  
![graphic16][graphic16]
4.  Typ hlavní databáze:  
![graphic17][graphic17]
5.  Zadejte uživatelské jméno a heslo a na tlačítko přihlásit:  
![graphic18][graphic18]
6.  Klikněte na nový dotaz:  
![graphic19][graphic19]
7.  Zadejte následující dotaz nahrazení < login_name > své uživatelské jméno a nahrazování <enterStrongPasswordHere> pomocí nového hesla:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Klikněte na spustit:  
![graphic20][graphic20]
9.  Přejděte zpátky na krok 2 a tento klikněte databáze:  
![graphic21][graphic21]
10. Klikněte na příkaz Spravovat:  
![graphic22][graphic22]
11. Zadejte své uživatelské jméno, heslo a klepněte na položku přihlášení:  
![graphic23][graphic23]
12. Klikněte na nový dotaz:  
![graphic24][graphic24]
13. Zadejte následující dotaz nahrazení < uživatelské_jméno > název, podle kterého chcete určit toto přihlášení v rámci této databáze (můžete zadat stejnou hodnotu, kterou jste zadali například pro < login_name >) a nahrazení < login_name > nové uživatelské jméno:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Klikněte na spustit:  
![graphic25][graphic25]
15. Teď by měl obsahovat nového uživatele se stejným role a oprávnění má původní uživatel.
16. Dál část 2.

##<a name="part-2-stopping-the-stream-analytics-job"></a>Část 2: Zastavení toku analýzy úlohy
1.  Přejděte na toku analýzy příponu na portálu Správa Azure:  
![graphic26][graphic26]
2.  Najděte svoji práci a přejděte do něho:  
![graphic27][graphic27]
3.  Přejděte na kartu vstupů nebo výstupy založené na tom, jestli jsou otočení přihlašovací údaje na vstup nebo výstup.  
![graphic28][graphic28]
4.  Klikněte na tlačítko Zastavit a potvrďte, že přestal úlohy:  
![graphic29][graphic29] čekat pro danou úlohu ukončit.
5.  Vyhledejte vstupní a výstupní, který chcete otočit přihlašovací údaje na a přejděte do něho:  
![graphic30][graphic30]
6.  Přejděte k části 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Část 3: Úprava přihlašovací údaje na úlohy analýzy toku

###<a name="blob-storagetable-storage"></a>Úložiště objektů BLOB úložiště a tabulek
1.  Vyhledejte pole klíč účtu úložiště a vložte nově generovaného klíče:  
![graphic31][graphic31]
2.  Klikněte na příkaz Uložit a potvrďte uložení změn:  
![graphic32][graphic32]
3.  Test připojení bude automaticky vytvořena, když uložíte provedené změny, ujistěte se tedy úspěšně termínu.
4.  Přejděte k části 4.

###<a name="event-hubs"></a>Rozbočovače události
1.  Vyhledejte pole klíč zásad centrální události a vložte nově generovaného klíče:  
![graphic33][graphic33]
2.  Klikněte na příkaz Uložit a potvrďte uložení změn:  
![graphic34][graphic34]
3.  Test připojení bude automaticky vytvořena, když uložíte provedené změny, ujistěte se, že byl úspěšně předaný.
4.  Přejděte k části 4.

###<a name="power-bi"></a>Power BI
1.  Klikněte na povolení obnovení:  
* ![graphic35][graphic35]
* Zobrazí se následující potvrzení:  
* ![graphic36][graphic36]
2.  Klikněte na příkaz Uložit a potvrďte uložení změn:  
![graphic37][graphic37]
3.  Test připojení bude automaticky vytvořena, když jste neuložili svoje změny, ujistěte se, že ho úspěšně termínu.
4.  Přejděte k části 4.

###<a name="sql-database"></a>Databáze SQL
1.  Najít pole uživatelské jméno a heslo a vložte je nově vytvořený sadu přihlašovacích údajů:  
![graphic38][graphic38]
2.  Klikněte na příkaz Uložit a potvrďte uložení změn:  
![graphic39][graphic39]
3.  Test připojení bude automaticky vytvořena, když uložíte provedené změny, ujistěte se, že byl úspěšně předaný.  
4.  Přejděte k části 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>Část 4: Od posledního přestal práce
1.  Přejděte směrem od vstupní a výstupní:  
![graphic40][graphic40]
2.  Klikněte na tlačítko Start:  
![graphic41][graphic41]
3.  Vyberte posledního zastaven a klikněte na OK:  
 ![graphic42][graphic42]
4.  Přejděte k části 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>Část 5: Odstranění staré sadu přihlašovacích údajů
Tato část je k dispozici následující vstupy/výstupy:
* Úložiště objektů BLOB
* Rozbočovače události
* Databáze SQL
* Úložiště tabulek

###<a name="blob-storagetable-storage"></a>Úložiště objektů BLOB úložiště a tabulek
Opakujte část 1 pro přístup k klávesy, která byla dříve používal práce k obnovení teď nepoužitý přístupové klávesy.

###<a name="event-hubs"></a>Rozbočovače události
Opakujte část 1 pro klávesy, která byla dříve používal práce obnovíte teď nepoužitý klíč.

###<a name="sql-database"></a>Databáze SQL
1.  Přejděte zpátky do okna dotazu z část 1 kroku 7, zadejte následující dotaz nahrazení < previous_login_name > uživatelské jméno, které byly dříve používal práce:  
`DROP LOGIN <previous_login_name>`  
2.  Klikněte na spustit:  
    ![graphic43][graphic43]  

Dostanete potvrzení následující: 

    Command(s) completed successfully.

## <a name="get-help"></a>Získání nápovědy
Další pomoc Vyzkoušejte naše [Fórum komunity analýzy toku Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
