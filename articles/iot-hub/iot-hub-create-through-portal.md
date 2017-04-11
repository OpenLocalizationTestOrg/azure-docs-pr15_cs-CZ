<properties
     pageTitle="Vytvoření rozbočovači IoT pomocí portálu Azure | Microsoft Azure"
     description="Základní informace o tom, jak vytvářet a spravovat Azure IoT rozbočovače portálu Azure"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Vytvoření rozbočovači IoT pomocí portálu Azure

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Úvod

Tento článek popisuje, jak vyhledat službu IoT rozbočovači Azure portálu a jak vytvářet a spravovat IoT rozbočovače.

## <a name="where-to-find-iot-hubs"></a>Kde lze najít IoT rozbočovače

Existují různé místa, kde můžete najít IoT rozbočovače.

1. **+ Nový**: **Azure IoT centrální** je služba IoT a najdete v kategorii **Internet věci**v dialogovém okně **+ Nový**, podobně jako další služby.

2. IoT rozbočovače můžete také k nim získat přístup prostřednictvím Tržiště jako obrázek služba ve skupinovém rámečku **Internetový věci**.

## <a name="create-an-iot-hub"></a>Vytvoření rozbočovači IoT

Můžete vytvořit rozbočovači IoT následujícími způsoby:

- Vytváření rozbočovači IoT pomocí příkazu **+ Nový** vede k zásuvné vidět na následující obrazovce snímek. Postup pro vytvoření centra IoT prostřednictvím tato metoda a tržiště jsou stejné.

- Vytváření rozbočovači IoT prostřednictvím Tržiště: Kliknutím na tlačítko **vytvořit** otevřete zásuvné, který je stejný jako předchozí zásuvné na **+ Nový** prostředí. V následujících částech jsou uvedeny několika kroky při vytváření rozbočovači IoT.

### <a name="choose-the-name-of-the-iot-hub"></a>Vyberte název centru IoT

Pokud chcete vytvořit rozbočovači IoT, je nutné pojmenovat centru. Tento název musí být jedinečný přes rozbočovače. Bez duplikace rozbočovače je povolena u back-end, proto se doporučuje jako jedinečně názvem tento rozbočovač.

### <a name="choose-the-pricing-tier"></a>Zvolte ceny osy

Můžete si vybrat z čtyři úrovně: **Uvolnit**, **Standardní 1** a **Standardní 2**a **Standardní S3**. Bezplatné osy umožňuje pouze 500 zařízení připojené k rozbočovači IoT a až 8000 zpráv denně.

**Standardní S1**: IoT rozbočovače S1 edition je určený pro IoT řešení, které mají velkého počtu zařízení generování relativně malým objemy dat na zařízení. Každý jednotky vydání S1 umožňuje až 400 000 zpráv denně na všech zařízeních připojení.

**Standardní S2**: edition IoT centrální S2 je určený pro IoT řešení, které zařízení generovat velké objemy dat. Každý jednotky vydání S2 umožňuje až 6 miliónů zpráv denně mezi všechna připojení zařízení.

**Standardní S3**: IoT rozbočovači S3 edition je určený pro IoT řešení, které mohou generovat velké objemy dat. Každý jednotky vydání S3 umožňuje až 300 milionů zpráv denně mezi všechna připojení zařízení.

![][4]

> [AZURE.NOTE] Rozbočovač IoT pouze umožňuje středovém bezplatné jedno předplatné Azure.

### <a name="iot-hub-units"></a>IoT rozbočovači jednotky

Jednotka centrální IoT obsahuje počet zpráv denně. Celkový počet podporované pro tento Centrum zpráv je počet jednotek vynásobené počet zpráv denně této osy. Například pokud chcete centru IoT podporuje průniku 700,000 zpráv, vyberte dvě S1 osy jednotky.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Zařízení do cloudu oddíly a pole Skupina zdroje

Můžete změnit počet oddílů pro rozbočovači IoT. Výchozí oddíly jsou nastavené 4; Můžete však rozdílný počet oddílů z rozevíracího seznamu.

Pro skupiny prostředků nepotřebujete explicitně vytvořit skupinu prázdných zdroje. Při vytváření zdroje, můžete k vytvoření nové skupiny prostředků nebo použít existující skupinu zdroje.

![][5]

### <a name="choose-subscriptions"></a>Zvolte předplatné

Azure centrální IoT automaticky zobrazí seznam Azure předplatné, ke kterým je propojený uživatelský účet. Můžete vybrat jednu z možností tady centru IoT přidružit k této Azure předplatného.

### <a name="choose-the-location"></a>Vyberte umístění

Možnost umístění obsahuje seznam oblastí, ve kterých IoT centrální nabízených. Rozbočovač IoT neexistuje nasadit do těchto umístění: Austrálie východ, Austrálie jihovýchodní, východní Asie, jihovýchodní Asie, Severní Evropa, Europe západní, Japonsko východ Japonsko Západ, nám východ, západní US.

### <a name="create-the-iot-hub"></a>Vytvořit centrum IoT

Po dokončení všech předchozích kroků rozbočovači IoT je připravená k nevytvoří. Klikněte na **vytvořit** začít back-end proces vytváření tento IoT rozbočovač s konkrétní možnosti a ho nasadit do zadaného umístění.

Může trvat několik minut centru IoT vytvořit jako trvá, než nasazení back-end problémy na serverech správného umístění.

## <a name="change-the-settings-of-the-iot-hub"></a>Změna nastavení centra IoT

Když už je vytvořená z centrální IoT zásuvné můžete změnit nastavení existující rozbočovače IoT.

![][8]

**Sdílené zásady přístupu**: tyto zásady definovat oprávnění pro zařízení a služby pro připojení k centrální IoT. Tyto zásady můžete přejít kliknutím **Sdílené zásady přístupu** v části **Obecné**. V tomto zásuvné můžete upravit existující zásady nebo přidat nové.

### <a name="create-a-policy"></a>Vytvoření zásad

- Klikněte na **Přidat** zásuvné otevřete. Tady můžete zadat nový název zásady a oprávnění, která chcete přidružit k této zásady, jak je znázorněno na následujícím obrázku.

    Existuje několik oprávnění, které jde přidružit tyto sdílené zásady. První dvě zásady, **registru číst** a **psát registru**, grant (udělit) čtení a zápis přístupová práva k úložišti identity zařízení nebo identity registru. Výběr možnosti zápisu automaticky vybere i další možnosti.

    Zásady **připojení služby** udělí oprávnění k přístupu ke cloudu straně koncové body například skupině zákaznické služby připojení k centru IoT. Zásady **zařízení připojit** udělí oprávnění pro odesílání a přijímání zpráv na straně zařízení koncové body centru IoT.

- Klikněte na **vytvořit** to nově vytvořený zásad do existujícího seznamu přidat.

![][10]

## <a name="messaging"></a>Zasílání zpráv

Klikněte na **zprávy** zobrazte seznam vlastností pro centrální IoT upravované zasílání zpráv. Existují dva hlavní typy vlastností, kterými se můžete upravit nebo kopírování: **Obláčkem, která zařízení** a **zařízení do cloudu**.

- Nastavení **obláčkem, která zařízení** : Toto nastavení má dvě subsettings: **Obláčkem, která zařízení TTL** (time-to-live) a **doba uchovávání informací** u zprávy. Při prvním vytvoření centra IoT obě tato nastavení se vytvářejí výchozí hodnotu 1 hodina. Chcete-li upravit tyto hodnoty, pomocí jezdců nebo zadejte hodnoty.

- Nastavení **zařízení do cloudu** : Toto nastavení má několik subsettings některé z nich jsou s názvem/přiřazeny při centru IoT se vytvoří a je možné jenom zkopírovat do jiných subsettings, které lze přizpůsobit. Tato nastavení jsou uvedené v následující části.

**Oddíly**: když centru IoT se vytvoří a bude možné měnit pomocí tohoto nastavení je tato hodnota nastavená.

**Kompatibilní s prohlížečem události centrální název a koncového bodu**: se vytvoří při IoT centrální, rozbočovači události se vytvoří interně, že budete potřebovat přístup k za určitých okolností. Tento název události kompatibilního centrální a koncový bod nejde přizpůsobit, ale je k dispozici pro použití pomocí tlačítka **Kopírovat** .

**Doba uchovávání informací**: ve výchozím nastavení jeden den, ale můžete přizpůsobit a dalších hodnot pomocí rozevíracího seznamu. Tato hodnota je ve dnech pro zařízení do cloudu a ne v hodinách, jako je podobný nastavení Obláčkem, která zařízení.

**Skupiny příjemce**: spotř skupiny jsou nastavení podobně jako jiné systémy zasílání zpráv, které můžete použít pro získání dat konkrétní způsoby připojení k centrální IoT jiných aplikací nebo služeb. Každý IoT centrální se vytvoří pomocí výchozí skupiny příjemce. Však můžete přidat nebo odstranit skupiny spotř IoT rozbočovače.

> [AZURE.NOTE] Výchozí skupiny spotř nelze upravovat ani odstranit.

![][11]

## <a name="pricing-and-scale"></a>Ceny a měřítka

Ceny existující centrální IoT bude možné měnit nastaveních **ceny** s následujícími výjimkami:

- V aktuálním implementaci rozbočovači IoT s bezplatné SKU nelze nahraďte úrovní placené skladové jednotky, nebo naopak.
- V Azure předplatné může být pouze středovém IoT bezplatné osy.

![][12]

Přesunutí z vyšší úroveň (S2 nebo S3) na dolní osy (S1 nebo S2) jsou povolené pouze v případě počet zpráv odeslaných pro daný den není v konfliktu. Například pokud počet zpráv denně překročí 400,000, pak osy rozbočovače IoT lze změnit. Ale pokud přejděte vrstva S1 potom centru je omezena pro daný den.

## <a name="delete-the-iot-hub"></a>Odstranění rozbočovači IoT

Můžete procházet k rozbočovači IoT, který chcete odstranit tak, že klepnutím na tlačítko **Procházet**a výběrem příslušných centrální odstranit. Kliknutím na tlačítko **Odstranit** pod názvem centrální odstranit centru.

## <a name="next-steps"></a>Další kroky

Tyto odkazy vedou na další informace o správě Azure IoT centrální:

- [Hromadné Správa IoT zařízení][lnk-bulk]
- [Použití metriky][lnk-metrics]
- [Operace sledování][lnk-monitor]

Prozkoumat další možnosti IoT centrální, najdete tady:

- [Příručka pro vývojáře][lnk-devguide]
- [Napodobení zařízení s SDK IoT brány][lnk-gateway]
- [Zabezpečené IoT řešení od základu nahoru][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md