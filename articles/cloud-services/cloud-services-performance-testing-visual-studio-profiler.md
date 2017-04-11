<properties 
    pageTitle="Vytvoření profilu do cloudové služby místně ve výpočetním emulátoru | Microsoft Azure" 
    services="cloud-services"
    description="Prozkoumejte problémy s výkonem ve službě cloud services pomocí nástroje Visual Studio" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Testování výkonu do cloudové služby místně v Azure výpočetním emulátoru pomocí nástroje Visual Studio

Celou řadu nástrojů a postupy jsou k dispozici pro účely testování výkonu cloudovým službám.
Když budete publikovat do cloudové služby Azure, můžete mít Visual Studio shromažďovat data o vytváření profilů a potom ji místně, analyzovat jako pokynů v tématu [vytváření profilů aplikace Azure][1].
Můžete také diagnostiky můžete sledovat různé výkonnosti, popsanou v [používání výkonnosti v Azure][2].
Můžete taky profilu aplikace místně ve výpočetním emulátoru před nasazením do cloudu.

Tento článek popisuje procesoru odběr způsob vytvoření profilu, který se dá udělat místně emulátor. Analytický nástroj vzorkování procesoru je způsob, jak vytváření profilů, který není velmi rušivá. V intervalu odběru určenou přenese nástroje snímek zásobníku volání. Data se shromažďují průběhu určitého časového období a zobrazené v sestavě. Tento způsob použití profilů obvykle vyznačte, kde nelze náročné aplikace většinu procesoru práce právě probíhá.  To vám dá příležitosti zaměřit se na "žádanou cestu" kde aplikace tráví nejvíce času.



## <a name="1-configure-visual-studio-for-profiling"></a>1: Konfigurace aplikace Visual Studio pro vytváření profilů

Nejdřív způsoby několik Visual Studio konfigurace, které můžou být užitečné při vytváření profilu. Abyste měli představu o vytváření profilů sestavy, musíte mít symboly (soubory PDB) pro aplikace a také symboly pro knihovny systému. Je vhodné, abyste měli jistotu, odkazovat na servery k dispozici symbol. Uděláte to takto: v nabídce **Nástroje** ve Visual Studiu, zvolte **Možnosti**a pak zvolte **ladění**, pak **symboly**. Ujistěte se, že je Symbol servery Microsoftu uvedené v části **Symbol umístění souborů (.pdb)**.  Můžete taky odkazovat http://referencesource.microsoft.com/symbols, které můžou mít další symbol soubory.

![Symbol možnosti][4]

Pokud budete chtít, můžete zjednodušit sestavy, které nástroje vygeneruje nastavením pouze můj kód. Pouze můj kód povoleno jsou zjednodušenými štosech volání funkce tak, aby hovory úplně interní pro knihovny a .NET Framework jsou skryté ze sestavy. V nabídce **Nástroje** vyberte **Možnosti**. Potom rozbalte položku **Výkonu nástroje** a zvolte **Obecné**. Zaškrtněte políčko **Povolit pouze můj kód pro sestavy nástroje**.

![Jenom možnosti můj kód][17]

Tyto pokyny slouží s existujícím projektu nebo nový projekt.  Pokud vytvoříte nový projekt zkusit postupy popsané níže, zvolte projekt C# **Cloudové služby Azure** a vyberte **Web rolí** a **Pracovní Role**.

![Azure role projektu cloudové služby][5]

Účely, například přidání kódu pro některé projektu, který má mnoho času a předvádí nějaký problém zjevných výkonu. Například přidáte následující kód do pracovních role projektu:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Volat tento kód z metodu RunAsync ve třídě odvozeno RoleEntryPoint roli kolegy. (Upozornění o způsobu spuštění synchronní ignorovat).

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Vytvořit a spustit cloudové služby místně bez ladění (Ctrl + F5) s konfigurace řešení nastavit **verzi**. To zajišťuje, aby všechny soubory a složky vytvořené pro spuštění aplikace místně a spuštění všech emulátorů. Spusťte uživatelského rozhraní emulátoru vypočítat z hlavního panelu pro ověření, jestli je spuštěný pracovní role.

## <a name="2-attach-to-a-process"></a>2: připojení k proces

Místo použití profilů aplikace spuštěním z Visual STUDIU 2010, je nutné připojit nástroje pracovního procesu. 

Pokud chcete připojit nástroje obrázku v nabídce **analyzovat** zvolte **Nástroje** a **Připojit nebo odpojit**.

![Připojit možnosti profilu][6]

Najděte proces WaWorkerHost.exe pro roli kolegy.

![Proces WaWorkerHost][7]

Pokud je složka projektu do síťové složky, nástroje vás vyzve k zadání jiného umístění pro uložení vytváření profilů sestavy.

 Můžete taky připojit k roli web připojením ke WaIISHost.exe.
Pokud existuje více pracovních role procesů v aplikaci, musíte jsou odlišeny pomocí ID procesu. Můžete dotaz ID procesu programově přístupem objekt procesu. Třeba když naopak přidáte tento kód metodu spustit třídy odvozeno RoleEntryPoint v roli, můžete si může samostatně prohlížet protokol v uživatelském rozhraní emulátoru výpočet vědět, jaké proces se připojit k.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Protokol mohou prohlížet, začněte uživatelského rozhraní emulátoru výpočet.

![Zahájení emulátoru výpočetním uživatelského rozhraní][8]

Otevřete okno pracovníka role protokol konzoly v uživatelském rozhraní výpočet emulátoru kliknutím na záhlaví okna konzoly. Zobrazí se proces ID v protokolu.

![ID zobrazení obrázku][9]

Jiný, který jste připojili, proveďte kroky v uživatelském rozhraní aplikace (v případě potřeby) reprodukovat scénáře.

Když budete chtít přestat vytváření profilů, zvolte odkaz **Zastavení vytváření profilu** .

![Zastavit vytváření profilů možnost][10]

## <a name="3-view-performance-reports"></a>3: zobrazení sestav výkonu

Zobrazí se zpráva o výsledku aplikace.

V tomto okamžiku nástroje ukončen uloží data v souboru .vsp a zobrazí zprávu, která zobrazuje analýzu tato data.

![Nástroje sestavy][11]


Pokud se zobrazí String.wstrcpy v kritická cesta, klikněte na pouze můj kód změnit zobrazení: Zobrazí jenom kód uživatele.  Pokud se zobrazí String.Concat, pokuste se stisknutím klávesy na tlačítko Zobrazit všechny kód.

Měli byste vidět Concatenate metoda a String.Concat zabírá velkou část čas spuštění.

![Analýza sestavy][12]

Pokud jste přidali kód zřetězení řetězců v tomto článku, byste měli vidět upozornění v seznamu úkolů pro tento. Může taky zobrazit upozornění, že je nadbytečné počtu uvolnění paměti, který je kvůli počet řetězce, které jsou vytvořené a odstraněny.

![Upozornění na výkon][14]

## <a name="4-make-changes-and-compare-performance"></a>4: proveďte změny a porovnat

Můžete taky porovnat výkon před a po změně kódu.  Zastavit spuštěný proces a upravit kód, který chcete nahradit operaci zřetězení řetězců pomocí StringBuilder:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Provedení další výkonu a potom porovnat. V podokně výkonu-li pracuje v jedné relace můžete jenom vybrat obě sestavy, otevřete místní nabídku a klikněte na **Porovnat sestavy výkonu**. Pokud chcete porovnat s spustit v jiné relaci výkonu, otevřete nabídku **analyzovat** a zvolte **Porovnání sestav výkonu**. V dialogovém okně, které se zobrazí určete oba soubory.

![Porovnání možnost sestavy výkonu][15]

V sestavách se zvýraznění rozdíly mezi dvěma pracuje.

![Porovnání sestavy][16]

Blahopřejeme! Jste získali spustili s nástroje.

## <a name="troubleshooting"></a>Řešení potíží

- Zkontrolujte, jestli jsou vytváření profilů vydání vytvořit a spustit bez ladění.

- Pokud možnost připojit nebo odpojit není povolený v nabídce Nástroje, spusťte Průvodce výkonu.

- Uživatelské rozhraní emulátoru výpočet slouží k zobrazení stavu aplikace. 

- Pokud máte problémy se spuštěním aplikace v emulátor nebo připojení nástroje, ukončete dolů emulátoru výpočetním a restartujte ji. Pokud, problém nevyřeší, zkuste restartovat. Tomuto problému může dojít, pokud používáte emulátoru výpočet pozastavení a odebrání pracovního nasazení.

- Pokud jste použili vytváření profilů příkazů z příkazového řádku, zejména globálního nastavení, ujistěte se, že VSPerfClrEnv /globaloff zavolání a, aby byl vypnout VsPerfMon.exe.

- Pokud při odběru, se zobrazí zpráva "PRF0025: žádná data shromažďuje," Ověřte, zda procesu připojené k procesoru aktivity. Aplikace, které nejsou činnost všechny výpočetní nemusí plodiny žádná data odběr.  Je také možné, že před provedením jakýkoli odběr k ukončení procesu. Zkontrolujte, že neskončí spustit metody roli, která jsou vytváření profilu.

## <a name="next-steps"></a>Další kroky

Nastavení Azure binární soubory v emulátor není podporována ve Visual Studiu nástroje, ale pokud chcete testovat přidělování paměti, můžete tuto možnost při vytváření profilu. Můžete taky zvolit souběžné vytváření profilu, který umožňuje určit, zda jsou vláknech zbytečně používány čas soutěží o zámků webů nebo vrstvy interakce vytváření profilů, která slouží k řešení potíží výkon při interakce mezi úrovní aplikace modus-nejčastěji mezi osy dat a roli pracovní.  Můžete zobrazit databázových dotazů, které generuje aplikace a používat data vytváření profilů zlepšit použití databáze. Informace o vytváření profilů interakce osy, najdete v blogovém příspěvku [Návod: pomocí nástroje interakce osy ve Visual Studiu týmu systém 2010][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 