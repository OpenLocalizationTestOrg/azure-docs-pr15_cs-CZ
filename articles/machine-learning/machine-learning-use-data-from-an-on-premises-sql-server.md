<properties
pageTitle="Pomocí dat z databáze SQL serveru místní počítač přečíst | Azure"
description="Pomocí dat z databáze SQL serveru místní provádět pokročilé technologie pro analýzu s Azure počítače učení."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Provádět pokročilé technologie pro analýzu s Azure počítače učení pomocí dat z databáze SQL serveru místní


Často podniky, které spolupracují s místních dat chcete využívat měřítka a používání cloudu pro jejich počítači výukové úloh. Ale nechce přerušit jejich aktuální obchodní procesy a pracovní postupy přesunutím jejich místních dat do cloudu. Azure výukové počítače nyní podporuje čtení dat z databáze SQL serveru místní a potom školení a bodování modelu s těmito daty. Už máte ruční kopírování a synchronizace dat mezi cloudu a místní server. Místo toho modulu **Importovat Data** v Azure počítače výukové Studio můžete nyní přečíst přímo z místní databázi SQL serveru pro vaše školení a bodování úlohy. 

Tento článek obsahuje přehled o tom, jak průniku místní SQL server data do výukové Azure počítače. Předpokládá, že znáte Azure počítače výukové koncepty třeba pracovní prostory, moduly, datové sady, pokusy nebo *atd*.

> [AZURE.NOTE] Tato funkce není k dispozici pro bezplatná pracovních prostorech. Další informace o počítači výukové ceny a úrovní, najdete v tématu [Azure počítače studijní ocenění](https://azure.microsoft.com/pricing/details/machine-learning/).

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>Instalace Brána pro správu dat Microsoft

Přístup k databázi serveru SQL Server místní budete muset stáhnout a nainstalovat bránu pro správu dat Microsoft Azure počítač přečíst. Při konfiguraci brány připojení ve počítače výukové studiu budete mít možnost stažení a instalace brány pomocí dialogového okna **Stáhnout a brána dat registru** píše níže.

Taky můžete nainstalovat bránu pro správu dat předem stahování a spuštění instalačního balíčku MSI z [Webu služby Stažení softwaru](https://www.microsoft.com/download/details.aspx?id=39717). Zvolte nejnovější verzi, vyberte 32bitovou nebo 64bitovou v závislosti na vašem počítači. Instalační službě MSI lze také upgradu existující Brána pro správu dat na nejnovější verzi, se zachovají všechna nastavení.

Brány má následující požadavky:

- Podporované operační systém verze Windows jsou Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.
- Doporučená konfigurace na počítači brány je alespoň na úrovni 2 GHz 4 jádra, 8 GB paměti RAM a 80 GB disku.
- Pokud spánku na hostitelském počítači brány nebude možné odpověď na žádosti o data. Před instalací brány proto nakonfigurujte odpovídající napájecí plán v počítači. Instalace brány zobrazí zpráva, pokud počítač nakonfigurovaný tak, aby počítač.
- Protože kopírovat aktivity výskytu konkrétní frekvencí používání zdrojů (procesoru, paměti) v počítači také následující stejné s Špička a nedělá časy. Využití prostředků také velkém závisí na tom množství dat, která je přesouvána. Když úlohy více kopií se v průběhu budete sledovat přejít o špičce používání zdrojů. Sice doslova dostatečné minimální konfigurace výše uvedené, je vhodné mít konfiguraci s více zdrojů než minimální konfigurace v závislosti na určitých zatížení pro přesun dat.

Měli byste zvážit následující při nastavení a používání brány pro správu dat:

- Jenom jedna instance Brána pro správu dat můžete nainstalovat na jednom počítači.
- Jedna brána můžete použít pro více zdrojů dat místní.
- Několik bran na různých počítačích můžete připojit k stejné místního zdroje dat.
- Konfigurace brány pro pracovní prostor pouze jeden po druhém. Bran nemůže být sdíleny pracovních prostorů v současné době.
- Můžete nakonfigurovat několik bran do jedné pracovního prostoru. Chcete například provádějte brány, který je spojený s zdrojů dat test průběhu vývoje a bránu výrobní budete chtít umožňují.
- Brány nemusí být na stejném počítači jako zdroj dat, ale zachování blíž ke zdroji dat zkracuje dobu bránu pro připojení ke zdroji dat pro. Doporučujeme nainstalovat bránu na počítač, který se liší od ten, který hostuje místního zdroje dat, aby brány a zdroj dat není soutěží pro zdroje.
- Pokud už máte nainstalované v počítači podávání Power BI nebo Azure Data Factory scénáře brány instalace samostatné brány Azure počítače výukové na jiném počítači. 

    > [AZURE.NOTE] Brána pro správu dat a Power BI brány se nedá spustit na stejném počítači.

- Je potřeba použít Brána pro správu dat pro vzdělávání Azure počítače i v případě, že používáte Azure ExpressRoute pro jiná data. Zdroj dat by měly považovat jako místní zdroj dat (to znamená za bránou firewall) i když používáte ExpressRoute a použít Brána pro správu dat k vytvoření připojení mezi počítači učení s pomocí zdroje dat. 

Podrobné informace o zjistit předpoklady pro instalaci, pokynů k instalaci a tipy pro jejich odstranění najdete v článku [Přesun dat mezi zdrojů na pracovišti a cloud se Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway), počínaje části [Důležité informace týkající se používání Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Průniku data z databáze SQL serveru místní do výukové počítače Azure


V tomto návodu nastavení brány pro správu dat v pracovním prostoru Azure počítače výukové, ji nakonfigurovat a pak si projděte témata dat z databáze SQL serveru místní.

> [AZURE.TIP] Než začnete, zakázat prohlížeče blokování automaticky otevíraných oken pro `studio.azureml.net`. Pokud používáte prohlížeč Google Chrome, stáhněte a nainstalujte jeden z několika moduly plug-in jsou dostupné na webový Google Chrome [Klikněte jednou aplikace rozšíření](https://chrome.google.com/webstore/search/clickonce?_category=extensions).

### <a name="step-1-create-a-gateway"></a>Krok 1: Vytvoření brány

Cílem prvního kroku je vytvořit a nastavit brány získat přístup k databázi SQL místní.

1. Přihlaste se ke [Azure počítače výukové Studio](https://studio.azureml.net/Home/) a vyberte, kterou chcete pracovat v pracovním prostoru.

2. Klikněte na zásuvné **Nastavení** na levé straně a potom klikněte na kartu **Brány dat** nahoře.

3. Klikněte na položku **Nová brána dat** v dolní části obrazovky.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. V dialogovém okně **Nová brána dat** zadejte **Název brány** a pokud chcete přidat **Popis**. Klikněte na šipku v pravém dolním rohu přejděte k dalšímu kroku konfigurace.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. V stáhnout a přihlásit brány dialogové okno data zkopírujte do schránky registračního klíče brány.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Pokud ještě nejste jste stáhli a nainstalovaná Brána pro správu dat Microsoft, klepněte na tlačítko **Stáhnout Brána pro správu dat**. Tím přejdete do Microsoft Download Center kde můžete vybrat, které potřebujete, stáhněte si ho verze brány a nainstalujte ji. Podrobné informace o zjistit předpoklady pro instalaci, pokynů k instalaci a tipy pro řešení potíží najdete v částech začátku tohoto článku [Přesun dat mezi zdrojů na pracovišti a cloud se Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

7. Po instalaci používat bránu Správce konfigurace brány pro správu dat se otevře a zobrazí se dialogové okno **zaregistrovat bránu** . Vložte **Registrace klíče brány** , kterou jste zkopírovali do schránky a klikněte na **zaregistrovat**.

8. Pokud už máte nainstalovaný brány, spusťte Správce konfigurace brány pro správu dat, klikněte na **změnit klíč**, vložte  **Registrace klíče brány** , kterou jste zkopírovali do schránky a klikněte na **OK**.

9. Po dokončení instalace se zobrazí dialogové okno pro Microsoft dat správu Správce konfigurace brány pro **registraci brány** . Vložte registračního klíče brány, kterou jste zkopírovali do schránky nahoře a klikněte na **zaregistrovat**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. Konfigurace brány je dokončen, pokud jsou tyto hodnoty nastavit na kartě **Domů** v aplikaci Microsoft Management Správce konfigurace brány dat:

    - **Název brány** a **název Instance** jsou nastaveny na název brány.

    - **Registrace** je nastavený na **Registrováno**.

    - **Stav** je nastavený na **spuštěno**.

    - Stavový řádek v dolní části zobrazí **připojit k Služba cloudu brány pro správu dat** spolu s zelená značka zaškrtnutí.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure Studio výukové počítač také aktualizuje při registraci proběhne úspěšně.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. V dialogovém okně **Stáhnout a zaregistrovat bránu dat** klikněte na značku zaškrtnutí, abyste dokončili nastavení. Na stránce **Nastavení** zobrazí stav brány jako "Online". V pravém podokně najdete stav a dalšími informacemi.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. Ve Správci konfigurace brány pro správu dat Microsoft přepněte na kartu **certifikát** . Certifikát uvedený na této kartě slouží k šifrování/dešifrování přihlašovací údaje pro místní úložiště dat, které zadáte na portálu. Toto je výchozí certifikát generovaný. Microsoft doporučuje Změna vlastní certifikát, který obecnějším údajům v systému správy certifikát. Klikněte na **změnit** a použijte vlastní certifikát.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (volitelné) Pokud chcete povolit podrobné protokolování pro řešení problémů s bránou, v Microsoft správy Správce konfigurace brány dat přejděte na kartu **diagnostických nástrojů** a zaškrtnete políčko **Povolit podrobné protokolování pro řešení potíží** . Informace zaznamenané do protokolu najdete v Prohlížeč událostí v rámci **protokoly aplikací a služeb**  - &gt; **Brána pro správu dat** uzel. Můžete taky použít kartu **diagnostiky** otestovat připojení ke zdroji dat místní pomocí brány.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

To provede brány nastavení proces Azure počítač přečíst.
Teď budete chtít použít místní data.

Můžete vytvořit a nastavit několik bran do Studio pro jednotlivých pracovních prostorech. Například pravděpodobně brány, který chcete připojit ke zdrojům dat test průběhu vývoje a jiné bránu pro zdroje dat výroby. Azure výukové počítače vám umožní nastavit několik bran v závislosti na podnikovém prostředí. Aktuálně nemůžou sdílet bránu mezi pracovních prostorů a pouze jednu bránu možné nainstalovat na jednom počítači. Další informace o instalaci brány najdete v článku [co zvážit při použití Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) v následujícím článku [Přesun dat mezi zdrojů na pracovišti a cloud se Brána pro správu dat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Krok 2: Umožňuje číst data z místního zdroje dat brány

Po nastavení brány můžete přidat modul **Importovat Data** do pokus vstupů data z místní databázi SQL serveru.

1.  Ve počítače výukové studiu vyberte kartu **POKUSY** , klikněte na **+ Nový** v levém dolním rohu a vyberte **Prázdný Experiment** (nebo vyberte některou z několika pokusech ukázkové k dispozici).

2.  Najděte a přetáhněte modulu **Importovat Data** do plátno testu.

3.  Pod plátna, klikněte na **Uložit jako** . Zadejte "Azure počítače výukové místní SQL Server kurz" název testu, vyberte příslušný pracovní prostor a zaškrtněte **OK** .

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  Klikněte na modul **Importovat Data** vyberte a potom v podokně **Vlastnosti** napravo od plátno vyberte "Místní databázi SQL" v rozevíracím seznamu **zdroj dat** .

5.  Vyberte **Data brány** instalaci a registraci. Nastavení jiného brány tak, že vyberete "(Přidat novou bránu dat...)".

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Zadejte **název databázového serveru** SQL a **název databáze**, spolu s SQL **databázových dotazů** chcete provést.

7.  Klepněte na **Enter hodnoty** v polích **uživatelské jméno a heslo** a zadejte svoje přihlašovací údaje databáze. Můžete použít ověřování systému Windows nebo ověřování serveru SQL Server v závislosti na konfiguraci místního serveru SQL.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Zpráva "hodnoty požadované" se změní na "sadu hodnot" se zelenou značkou zaškrtnutí. Potřebujete jenom jednou zadejte přihlašovací údaje, dokud se nezmění informací z databáze nebo heslo. Azure učení počítač používá certifikát, který jste zadali při instalaci brány k šifrování přihlašovacích údajů v cloudu. Azure nikdy obsahují pověření místního bez šifrování.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Klikněte na tlačítko **Spustit** spusťte testu.

Po dokončení testu se systémem můžete vizualizovat data, která jste importovali z databáze kliknutím na výstupní port modulu **Importovat Data** a výběrem **vizualizace**.

Po dokončení vývoji vaší experiment můžete nasazení a umožňují modelu. Pomocí služby spuštění dávky, data z místní databázi serveru SQL Server nakonfigurovaný v modulu **Importovat Data** bude číst a používá pro hodnocení. Když používáte službu odpověď žádost o hodnocení místních dat Microsoft doporučuje používat modul [doplňku aplikace Excel](machine-learning-excel-add-in-for-web-services.md) místo. V současné době zápisu do místního serveru SQL databáze prostřednictvím **Exportovat Data** není podporována v pokusy nebo publikované webové služby.

