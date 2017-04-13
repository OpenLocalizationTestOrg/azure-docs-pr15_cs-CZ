<properties 
   pageTitle="Testování výkonu do cloudové služby | Microsoft Azure"
   description="Testování výkonu do cloudové služby pomocí nástroje Visual Studio"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="testing-the-performance-of-a-cloud-service"></a>Testování výkonu do cloudové služby 

##<a name="overview"></a>Základní informace

Testování výkonu do cloudové služby následujícími způsoby:

- Pomocí diagnostických nástrojů Azure shromažďování informací o připojení a žádosti a chcete-li zobrazit statistiku webu znázorňující, jak službu provádí z hlediska zákazníka. Začínáme s, najdete v článku [Konfigurace Diagnostika pro Azure cloudovými službami a virtuálních počítačích]( http://go.microsoft.com/fwlink/p/?LinkId=623009).

- Pomocí nástroje Visual Studio hloubkovou analýzu výpočetní aspekty spuštění služby. Jak toto téma popisuje, můžete nástroje měření výkonu při spuštění služby v Azure. Informace o tom, jak pomocí nástroje měření výkonu při službu běží místně ve výpočetním emulátoru najdete v tématu [testování plnění Azure cloudové služby místně do výpočtu emulátoru pomocí aplikace Visual Studio nástroje](http://go.microsoft.com/fwlink/p/?LinkId=262845).



## <a name="choosing-a-performance-testing-method"></a>Výběr testování metoda výkonu

###<a name="use-azure-diagnostics-to-collect"></a>Umožňuje shromáždit Azure diagnostiky:###

- Statistik webové stránky nebo služby, jako je požadavky a připojení.

- Statistiky podle rolí, například jak často se nerestartuje roli.

- Informace o využití paměti, například procentuální poměr čas, který má kolekcí uvolnění nebo paměť celkové sada pracovního roli.

###<a name="use-the-visual-studio-profiler-to"></a>Pomocí nástroje Visual Studia do:###

- Zjistěte, které funkce nejvíce dobu trvat.

- Změřte jak dlouho trvá každou část nelze náročné programu.

- Porovnání sestavy podrobné výkonu pro dvě verze služby.

- Analýza přidělování paměti podrobnější než úroveň přidělení jednotlivé paměti.

- Analýza souběžné problémů v podprocesy kódu.

Při použití nástroje můžete shromažďovat data o spuštění do cloudové služby místně, nebo v Azure.

###<a name="collect-profiling-data-locally-to"></a>Shromáždit data vytváření profilů místně do:###

- Otestujte výkon součástí cloudové služby, jako je plnění jednotlivých pracovních role, který nevyžaduje reálné simulovaný načíst.

- Otestujte výkon do cloudové služby samostatně řízené podmínek.

- Otestujte výkon do cloudové služby, před nasazením Azure.

- Otestujte výkon do cloudové služby soukromě, aniž by existující nasazení.

- Otestujte výkon služby bez nabíhání poplatků za ke spouštění v Azure.

###<a name="collect-profiling-data-in-azure-to"></a>Shromáždit data v Azure pro vytváření profilů:###

- Otestujte výkon do cloudové služby Simulovaný nebo skutečnou zatížení.

- Použijte metodu přístrojového vybavení shromažďování vytváření profilů dat jako Toto téma popisuje později.

- Otestujte výkon služby v prostředí stejné jako při spuštění služby v výroby.

Obvykle simulovat načíst do test cloudovým službám v části normální nebo namáhání podmínky.

## <a name="profiling-a-cloud-service-in-azure"></a>Vytvoření profilu do cloudové služby Azure

Při publikování služby cloudu z aplikace Visual Studio můžete profilu služby a zadejte vytváření profilů nastavení, které vám informace, které chcete. Vytváření profilů relace je spuštěn pro všechny výskyty roli. Další informace o tom, jak publikovat službu z aplikace Visual Studio najdete v článku [publikování do cloudové služby Azure z aplikace Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

Další informace o vytváření profilů výkonu ve Visual Studiu, najdete v tématu [Začátečníky Příručka pro vytváření profilů výkonu](https://msdn.microsoft.com/library/azure/ms182372.aspx) a [Analýza výkonu aplikace tak, že pomocí nástroje pro vytváření profilů](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] Můžete povolit IntelliTrace nebo vytváření profilů při publikování cloudové služby. Není možné povolit obojí.

###<a name="profiler-collection-methods"></a>Nástroje kolekce metody

Můžete použít jiné kolekce metody pro vytváření profilů, založené na problémy s výkonem:

- **Analytický nástroj vzorkování procesoru** – tato metoda shromažďuje statistiky aplikace, které jsou užitečné pro počáteční analýzu problémů využití procesoru. Analytický nástroj vzorkování procesoru je navrhované způsob spuštění většina výkonu vyšetřování. V aplikaci, která jsou vytváření profilů při shromažďování dat odběr procesoru je malý dopad.

- **Přístrojového vybavení** – tato metoda shromažďuje podrobné časování data, která je užitečné pro zaměřený analýzy a analýzy problémy s výkonem vstupní a výstupní. Metoda přístrojového vybavení záznamů jednotlivé položky, konec a volání funkce funkcí v modulu během vytváření profilů spustit. Tento způsob je užitečný pro shromažďování časování podrobné informace o část kódu a pochopit dopad na výkon aplikace vstupní a výstupní samostatných. Tento způsob není k dispozici pro počítač s operačním systémem 32bitová verze. Tato možnost je dostupná jenom při spuštění cloudovou službu v Azure, místně ve výpočetním emulátoru.

- **Přidělování paměti .NET** – tato metoda shromažďuje dat přidělování paměti .NET Framework pomocí odběru vytváření profilů metody. Sběr dat obsahuje číslo a velikost přidělená objektů.

- **Souběžné** – tato metoda shromažďuje zdroje konflikty a proces a vlákna spuštění data, která je užitečné při analýze přepočet ve více vláknech a více procesy aplikací. Metoda souběžné shromažďovat data pro každou událost bloky provádění kódu, například kdy podproces čeká uzamčené přístup k prostředku aplikace na uvolnění. Tato metoda slouží k analýze přepočet ve více vláknech aplikací.

- Můžete taky povolit **Vytváření profilů interakce osy**, který poskytuje další informace o čas spuštění synchronní ADO.NET volá ve funkcích vícevrstevného aplikace komunikovat s jednu nebo více databází. Shromáždit vrstva interakce dat pomocí kterékoli metody vytváření profilů. Další informace o vytváření profilů interakce osy zobrazení [osy interakce](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Konfigurace nastavení pro vytváření profilů

Následující obrázek ukazuje, jak konfigurovat nastavení vytváření profilů v dialogovém okně Publikovat Azure aplikace.

![Konfigurace vytváření profilů nastavení](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] Chcete-li zaškrtnutí políčka **Povolit vytváření profilu** , musíte mít nástroje nainstalované v počítači použitém publikovat cloudové služby. Ve výchozím nastavení je nainstalovaná nástroje při instalaci aplikace Visual Studio.

### <a name="to-configure-profiling-settings"></a>Konfigurace nastavení pro vytváření profilů

1. V Průzkumníku otevřete místní nabídky pro Azure projektu a potom vyberte **Publikovat**. Podrobnosti o tom, jak publikovat do cloudové služby najdete v tématu [publikování cloudové služby Azure nástroje](http://go.microsoft.com/fwlink/p?LinkId=623012).

1. V dialogovém okně **Publikovat Azure aplikace** vyberte kartu **Upřesnit nastavení** .

1. Povolit vytváření profilů, zaškrtněte políčko **Povolit vytváření profilu** .

1. Konfigurace vytváření profilů nastavení, zvolte **Nastavení** hypertextový odkaz. Zobrazí se dialogové okno Nastavení vytváření profilu.

1. Z přepínačů na **Jaké způsob použití profilů byste chtěli použít** zvolte typ vytváření profilu, které potřebujete.

1. Sběr dat vytváření profilů interakce vrstvy, zaškrtněte políčko **Povolit vytváření profilů interakce osy** .

1. Pokud chcete uložit nastavení, klikněte na tlačítko **OK** .

    Při publikování této aplikace toto nastavení slouží k vytvoření vytváření profilů relaci pro každou roli.

## <a name="viewing-profiling-reports"></a>Zobrazení sestav použití profilů

Vytváření profilů relace se vytvoří každý výskyt role v cloudové službě. Vytváření profilů sestavách každé relace z aplikace Visual Studio zobrazíte můžete zobrazení okna Průzkumník serveru a klikněte na uzel Azure výpočet vyberte instanci roli. Můžete pak zobrazení vytváření profilů sestavy, jak je znázorněno na následujícím obrázku.

![Zobrazení vytváření profilů sestavy z Azure](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Chcete-li zobrazit vytváření profilů sestavy

1. Zobrazení okna Průzkumník serveru ve Visual Studiu, vyberte na řádku nabídek zobrazení, Průzkumník serveru.

1. Vyberte uzel výpočet Azure a klikněte na uzel Azure nasazení služby cloudu, který jste vybrali profilu, když jste publikovali z aplikace Visual Studio.

1. Vytváření profilů sestav pro vytvoření instance zobrazíte zvolte role ve službě, otevřete místní nabídky pro konkrétní instanci a pak zvolte **Zobrazit vytváření profilů zprávu**.

    Sestava souboru .vsp se teď stahují z Azure a stav stahování se zobrazí v protokolu činnosti Azure. Po dokončení stahování vytváření profilů sestava se zobrazí na kartě v editoru Visual Studio s názvem <Role name> _<Instance Number>_ <identifier>.vsp. Zobrazí se souhrnná data pro sestavu.

1. K zobrazení různých sestavy, v seznamu aktuální zobrazení, vyberte typ zobrazení, které chcete. Další informace najdete v tématu [Vytváření profilů zobrazení sestav nástroje](https://msdn.microsoft.com/library/azure/bb385755.aspx).

## <a name="next-steps"></a>Další kroky

[Ladění Cloudovým službám](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[Publikování na služby Azure cloudu z aplikace Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

