<properties
    pageTitle="Nastavení virtuálního počítače jako server Poznámkový blok IPython | Microsoft Azure"
    description="Nastavte si Azure virtuálního počítače pro použití v prostředí pro výzkum dat serverem IPython pro pokročilé technologie pro analýzu."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Nastavení Azure virtuálního počítače jako serveru IPython Poznámkový blok pro pokročilé technologie pro analýzu

Toto téma ukazuje, jak můžete vytvořit a nakonfigurovat Azure virtuálního počítače pro pokročilé technologie pro analýzu, můžete se nemusí používat v rámci datového prostředí pro výzkum. Virtuální počítač Windows nastaven podpůrných nástrojů, jako například IPython Poznámkový blok, Azure úložiště Exploreru, AzCopy i další nástroje, které jsou vhodné pro pokročilé technologie pro analýzu projekty. Azure Explorer úložiště a AzCopy, například nabídněte pohodlný předávat data k úložišti objektů blob Azure ze svého místního počítače a stáhněte si ho do místního počítače z úložiště objektů blob.

## <a name="create-vm"></a>Krok 1: Vytvoření univerzální Azure virtuálního počítače

Pokud už máte Azure virtuálního počítače a chcete jenom nastavení serveru IPython poznámkového bloku na něm, můžete tento krok přeskočit a přejít k [Krok 2: Přidání koncový bod pro poznámkové bloky IPython do existující virtuálního počítače](#add-endpoint).

Dříve než spustíte proces vytváření virtuálních počítačů na Azure, musíte zjistit velikost počítač, který je potřeba zpracovat data pro svůj projekt. Menší počítačích mít méně paměti nebo méně jádra procesoru než větší počítačích, ale ty představují taky levnější. Seznam typů počítače a ceny najdete na stránce <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtuálních počítačích ceny</a>

1. Přihlaste se k <a href="https://manage.windowsazure.com" target="_blank">Portálu klasické Azure</a>a klikněte na tlačítko **Nový** v levém dolním rohu. Okno se objevovat. Vyberte **Výpočet** -> **VIRTUÁLNÍHO počítače** -> **Z GALERIE**.

    ![Vytvoření pracovního prostoru][24]

2. Vyberte jednu z následujících obrázcích:

    * Windows serveru 2012 R2 datacentru
    * Windows Server Essentials prostředí (Windows serveru 2012 R2)

    Klikněte na šipku doprava v pravém dolním přejdete na další stránku konfigurace.

    ![Vytvoření pracovního prostoru][25]

3. Zadejte název virtuálního počítače, který chcete vytvořit, vyberte požadovanou velikost v počítači (výchozí: A3) v závislosti na velikosti dat počítač bude proces a jak výkonné chcete počítače je třeba (paměť o velikosti a počet jejích výpočetním jádra), zadejte uživatelské jméno a heslo pro počítač. Klikněte na šipku doprava se přejde na další stránku konfigurace.

    ![Vytvoření pracovního prostoru][26]

4. Vybrat **Oblast/SPŘAŽENÍ skupiny/virtuální sítě** , která obsahuje **Úložiště účtu** , který se chystáte používat pro tento virtuální počítač a potom vyberte tohoto účtu úložiště. Přidání koncový bod dole v poli **koncové body** zadáním název koncového bodu ("IPython" sem). Libovolný řetězec můžete jako **název** koncového bodu a libovolné celé číslo od 0 do 65536, která je **k dispozici** jako **Veřejný PORT**. **Soukromé PORT** musí být **9999**. Uživatelé mají **Vyhněte se** použití veřejných porty, které již byly přiřazeny pro internetové služby. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Porty pro internetové služby</a> obsahuje seznam porty, které byly přiřazeny a je nutno.

    ![Vytvoření pracovního prostoru][27]

5. Klikněte na značku zaškrtnutí zahájíte virtuální počítač zřizování obrázku.

    ![Vytvoření pracovního prostoru][28]


Může trvat 15 25 minut virtuální počítač zřizování obrázku. Po vytvoření virtuálního počítače stav tento počítač má zobrazit jako **spuštěný**.

![Vytvoření pracovního prostoru][29]

## <a name="add-endpoint"></a>Krok 2: Přidání koncový bod pro poznámkové bloky IPython do existující virtuálního počítače

Pokud jste vytvořili virtuální počítač postupujte podle pokynů v kroku 1, koncový bod pro IPython Poznámkový blok již byly přidány a můžete tento krok přeskočit.

Pokud již existuje virtuální počítač a budete muset přidat koncový bod pro IPython Poznámkový blok, který nainstalujete v kroku 3 pod první protokolu do klasického portálu Azure, vyberte virtuálního počítače a přidejte koncový bod pro poznámkový blok IPython serveru. Na následujícím obrázku obsahuje snímek obrazovky s portálu po koncový bod pro poznámkový blok IPython přidal do virtuálního počítače v systému Windows.

![Vytvoření pracovního prostoru][17]

## <a name="run-commands"></a>Krok 3: Instalace IPython Poznámkový blok a dalších podpůrných nástrojů

Po vytvoření virtuálního počítače pomocí vzdálené plochy RDP (Protocol) pro přihlášení k Windows virtuálního počítače. Pokyny najdete v tématu [přihlášení k virtuálního počítače spuštění systému Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). Otevřete okno **příkazového řádku** (**nikoli okna příkazového prostředí Powershell**) jako **Správce** a spusťte tento příkaz.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Po dokončení instalace serveru IPython poznámkového bloku se spustí automaticky v *C:\\uživatelé\\\<uživatelské jméno\>\\dokumenty\\poznámkové bloky IPython* adresář.

Po zobrazení výzvy zadejte heslo pro poznámkový blok IPython a hesla správce počítače. Díky IPython Poznámkový blok jako služba spustit v počítači.

## <a name="access"></a>Krok 4: Přístup k poznámkovým blokům IPython z webového prohlížeče
Pro přístup k serveru IPython Poznámkový blok, otevřete webové prohlížeče a vstupní *https://&#60;virtual název počítače DNS >: & #60; číslo portu veřejné >* v textovém poli URL. Tady *& #60; číslo portu veřejné >* by měl být číslo portu zadané přidání koncový bod IPython Poznámkový blok.

*& #60; název DNS virtuálního počítače >* najdete na klasické portál Azure. Po klepněte na protokolování v portálu klasické **VIRTUÁLNÍCH počítačích**, vyberte počítač, který jste vytvořili a pak vyberte **řídicích panelů**, název DNS zobrazí následujícím způsobem:

![Vytvoření pracovního prostoru][19]

Setkáte upozornění informující o této _došlo k potížím s certifikátem zabezpečení tento web_ (Internet Explorer) nebo _připojení se však soukromé_ (Chrome), jak je vidět na následujících obrázcích. Klikněte na **pokračovat na tento web (nedoporučuje se)** (Internet Explorer) nebo **Upřesnit** a potom * *pokračovat k & #60;* Název DNS*> (nebezpečné) ** (Chrome) pokračovat. Potom zadejte heslo zadanému dříve přístup k poznámkovým blokům IPython.

**Internet Explorer:**
![vytvořit pracovní prostor][20]

**Chrome:**
![vytvořit pracovní prostor][21]

Po přihlášení k poznámkovému bloku IPython adresář *DataScienceSamples* se zobrazí v prohlížeči. Tento adresář obsahuje IPython poznámkové bloky vzorku, které jsou sdílené společnost Microsoft pomáhá uživatelům vedení dat pro výzkum úkoly. Tyto poznámkové bloky IPython vzorku jsou rezervovaný z [**úložiště Github**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) virtuálních počítačích během serveru IPython poznámkového bloku proces nastavit. Společnost Microsoft udržuje a často aktualizuje tohoto úložiště. Uživatelé můžou navštívit úložišti Github zobrazíte nedávno aktualizované poznámkové bloky IPython vzorku.
![Vytvoření pracovního prostoru][18]

## <a name="upload"></a>Krok 5: Nahrajte existující IPython Poznámkový blok z místního počítače k serveru IPython poznámkového bloku

Poznámkové bloky IPython poskytují snadný způsob, jak uživatelům odesílat existujícího poznámkového bloku IPython v místních počítačích na Poznámkový blok IPython server na virtuálních počítačích. Po přihlášení k poznámkovému bloku IPython ve webovém prohlížeči, klikněte na **adresář** , který Poznámkový blok IPython nahráli. Vyberte soubor Poznámkový blok IPython .ipynb k nahrání na místním počítači v **Průzkumníku souborů**a přetáhněte ho do adresáře IPython poznámkového bloku ve webovém prohlížeči. Kliknutím na tlačítko **Uložit** nahrajete soubor .ipynb server IPython Poznámkový blok. Ostatní uživatelé můžete spustit aplikaci v z webového prohlížeče.

![Vytvoření pracovního prostoru][22]

![Vytvoření pracovního prostoru][23]


##<a name="shutdown"></a>Vypnutí a zrušit přidělení virtuální počítač není při použití

Azure virtuálních počítačích jsou cenou jako **platí jenom pro můžete použít**. Abyste měli jistotu, že jste nejsou účtované bez použití virtuálního počítače, musí být ve stavu **Zastaveno (Deallocated)** není při použití.

> [AZURE.NOTE] Pokud vypnete virtuálního počítače z uvnitř OM (pomocí možností power Windows), OM zastavení ale zůstane přidělené. Abyste měli jistotu, že není nadále vám nebudou účtovat poplatky, vždy vypnout virtuálních počítačích z [Azure klasické portálu](http://manage.windowsazure.com/). Můžete taky ukončit OM pomocí prostředí Powershell tak, že zavoláte **ShutdownRoleOperation** s "PostShutdownAction" rovno "StoppedDeallocated".

Vypnutí a zrušit virtuálního počítače:

1. Přihlaste se k [Portálu klasické Azure](http://manage.windowsazure.com/) pomocí svého účtu.  

2. V levém navigačním panelu vyberte **VIRTUÁLNÍCH počítačích** .

3. V seznamu virtuálních počítačích klikněte na název virtuálního počítače a přejděte na stránku **řídicího PANELU** .

4. V dolní části stránky klepněte na možnost **vypnout**.

![Vypnutí OM][15]

Virtuální počítač budou odebrána nikoli však odstranit. Kdykoli z portálu Microsoft Azure klasické může restartujte počítač virtuální.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Azure OM je připraven k použití: Další krok?

Virtuální počítač je nyní připravena k použití v jiné vědecké cvičení vaše data. Také je připraven k použití jako server IPython Poznámkový blok pro průzkum a zpracováním dat a dalších úlohách ve spojení s Azure počítače učení s pomocí procesu týmu dat pro výzkum virtuální počítač.

Další kroky procesu týmu dat pro výzkum jsou namapované na [Cesta výuky](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může zahrnovat kroky, které přesunutí dat do HDInsight, zpracovat a přehrajte si ho tam při přípravě na výukové z dat s Azure počítače učení.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
