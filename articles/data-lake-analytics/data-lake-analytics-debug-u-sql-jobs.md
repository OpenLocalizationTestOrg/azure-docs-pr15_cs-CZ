<properties 
   pageTitle="Ladění U SQL úlohy | Microsoft Azure" 
   description="Naučte se ladění selhalo vrchol U SQL pomocí aplikace Visual Studio. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>Ladění C# kód v jazyku SQL U úlohy jezera analýzy dat 

Naučte se používat Azure dat jezera Visual Studio nástroje ladění úlohy s chybami U SQL kvůli chyby uvnitř kódu uživatele. 

Nástroj Visual Studio umožňuje stahování zkompilovaný kód a potřebné vrchol data z obrázku a ladění neúspěšné úlohy.

Systémy velký dat obvykle obsahují rozšíření modelu pomocí jazycích, třeba Java C#, Python, atd. Mnoho tyto systémy poskytují omezené runtime ladění informace, díky kterému je ladění chyb runtime ve vlastní kód složité. Nejnovější nástrojů Visual Studia získáváte funkci "Se nepodařilo vrchol ladění". Pomocí této funkce můžete stáhnout runtime dat z Azure místní stanici tak, aby ladění nezdařeném uložení vlastní kód C# pomocí stejného runtime a přesné zadávání dat z cloudu.  Po opravě problémy opětovným spuštěním revidovaný kód v Azure nástrojích.

Prezentaci videa této funkce najdete v článku [ladění vlastního kódu v Azure dat jezera analýzy](https://mix.office.com/watch/1bt17ibztohcb).

>[AZURE.NOTE] Visual Studio může přestat reagovat nebo selhat Pokud nemáte následující dvě okna upgrady: [Microsoft Visual C++ 2015 Redistributable aktualizace 2](https://www.microsoft.com/download/details.aspx?id=51682), [Univerzální C Runtime pro systém Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##<a name="prerequisites"></a>Zjistit předpoklady pro
-   Šlo až v článku [Začínáme](data-lake-analytics-data-lake-tools-get-started.md) .

## <a name="create-and-configure-debug-projects"></a>Vytváření a konfigurace ladění projekty

Při otevření selhalo úlohy ve Visual Studiu dat jezera nástroji, zobrazí se upozornění. Informace o chybách podrobné zobrazí karta chybové a na žlutém panelu upozornění horní části okna. 

![Azure analýzy jezera dat U-SQL ladění visual studio stažení vrchol obrazce](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**Ke stažení vrchol a vytváření řešení ladění**

1.  Ve Visual Studiu otevřete selhalo úlohu U SQL.
2.  Klikněte na **Stáhnout** si chcete stáhnout všechny požadované zdroje a zadávání datových proudů. Pokud stahování selže, klepněte na tlačítko **Opakovat** .
3.  Po dokončení stahování vytvořit projekt místní ladění, klikněte na **Otevřít** . Vytvoří se nový řešení Visual Studio s názvem **VertexDebug** se prázdný projekt s názvem **LocalVertexHost** .

Použijete-li operátory definované uživatelem v U SQL s kódem (Script.usql.cs), musíte vytvořit projekt knihovny C přednášky s kódem operátory definované uživatelem a zahrnout projektu VertexDebug řešení.

Pokud DLL sestavení zaregistrovali k databázi jezera analýzy dat, třeba přidání zdrojového kódu sestavení VertexDebug řešení.
 
Pokud jste vytvořili a samostatné C# knihovna tříd kód U SQL a registrovaných DLL sestavení k databázi jezera analýzy dat, potřebujete přidat C# projektu zdrojového sestavení VertexDebug řešení.

V některých případech méně častých uživatelem definované operátory slouží v U SQL soubor s kódem (Script.usql.cs) v původní řešení. Pokud budete chtít usnadňují práci, potřebujete vytvoření C# knihovny obsahující zdrojového kódu a změnit název sestavení registrován clusteru. Můžete získat název sestavení registrovaných clusteru kontrola skript, který máte spuštěné v clusteru. Můžete to udělat tak, že otevřete projekt U SQL a klikněte na "skript" v panelu úloh. 

**Konfigurace řešení**

1.  V Průzkumníku klikněte pravým tlačítkem myši na C# projekt, který jste právě vytvořili a potom klikněte na **Vlastnosti**.
2.  Výstupní cestu nastavte jako LocalVertexHost projektového plánu práce adresář. Můžete získat LocalVertexHost projektu práce adresář prostřednictvím LocalVertexHost vlastnosti.
3.  Vytvoření projektu C# k umístění souboru .pdb do projectu LocalVertexHost pracovní adresář nebo soubor .pdb můžete zkopírovat do této složky ručně.
4.  Ve skupinovém rámečku **Nastavení výjimky**zaškrtněte výjimky modulu CLR:

![Azure nastavením aplikace visual studio ladění analýzy jezera dat U jazyka SQL](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>Ladění projektu

Po vytvoření řešení ladění stažením na vrchol obrazce a nakonfigurovali prostředí, můžete začít ladění kódu U SQL.

1.  V Průzkumníku klikněte pravým tlačítkem myši na **LocalVertexHost** projekt, který jste právě vytvořili, přejděte na **ladění**a potom klikněte na **spuštění nové instance**. LocalVertexHost musí být nastavena jako projekt při spuštění. Může se objevit následující zpráva poprvé, které můžete ignorovat. Může to trvat až jednu minutu do dostali na obrazovku ladění.
 
    ![Azure upozornění visual studio ladění analýzy jezera dat U jazyka SQL](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  Použití aplikace Visual Studio na základě ladění prostředí (podívejte se na video, proměnné atd.) k řešení potíží. 
5.  Teď, když jste určili problému, opravit kód a pak znovu vytvořte projekt C# před testování znovu tak, aby byly všechny problémy vyřešit. Po ladění byla úspěšně dokončena, v okně výstupu zobrazující tato zpráva 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>Opětovné odeslání projektu

Po dokončení ladění kódu U SQL při novém odeslání neúspěšná úloha.

1. Registrace nového DLL sestavení ADLA databáze.

    1.  Z Průzkumníka/cloudu Průzkumníku serveru dat jezera Visual Studio pomocí nástroje pro rozbalte uzel **databáze** 
    2.  Sestavení registr pravým tlačítkem myši sestavení. 
    3.  Zaregistrujte svůj nový sestavení DLL k databázi ADLA.
 
2.  Nebo zkopírujte kód C# script.usql.cs - C# soubor s kódem.
3.  Opětovné odeslání práce.

##<a name="next-steps"></a>Další kroky

- [Kurz: Začínáme s jazykem Azure dat jezera analýzy U SQL](data-lake-analytics-u-sql-get-started.md)
- [Kurz: vývoji U SQL skriptů pomocí Data jezera Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Můžete vyvíjet operátory definované uživatelem U SQL Azure dat jezera analýzy úloh](data-lake-analytics-u-sql-develop-user-defined-operators.md)

