<properties 
   pageTitle="Sledování StorSimple zařízení | Microsoft Azure"
   description="Popisuje, jak používat službu StorSimple správce ke sledování výkonu vstupu a výstupu, využití kapacity, výkon sítě a výkonu zařízení."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>Sledování StorSimple zařízení pomocí služby StorSimple správce 

## <a name="overview"></a>Základní informace

Služba StorSimple správce slouží ke sledování určitá zařízení v rámci vašeho StorSimple řešení. Můžete vytvořit vlastní grafy založené na vstupu a výstupu výkonu využití kapacity, výkon sítě a měřítka zařízení. 

Zobrazit v portálu Azure klasické sledování informace pro konkrétní zařízení, vyberte službu StorSimple správce. Klikněte na kartě **Monitor** a vyberte ze seznamu zařízení. Stránka **Sledování** obsahuje následující informace.

## <a name="io-performance"></a>Výkon vstupu a výstupu 

**Výkon vstupu a výstupu** skladeb metriky související s počet pro čtení a zápis mezi buď rozhraní vyzývající iSCSI na hostitelském serveru a zařízení nebo zařízení a cloudu. Tento výkon můžete měřit pro konkrétní svazku, kontejneru konkrétní hlasitost nebo všechny kontejnery hlasitost.

Následující diagram znázorňuje vstup/výstup pro vyzývající do zařízení pro všechny svazky výrobních zařízení. Metriky vykreslí se budou číst a psát bajtů za sekundu, číst a psát operací v/v sekundu a čtení a zápis čekacích dob.

![ZVYŠUJÍ výkon z vyzývající do zařízení](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Stejné zařízení jsou zobrazeny operací pro data ze zařízení v cloudu pro všechny kontejnery hlasitost. Na tomto zařízení data jsou pouze v lineární osy a má nic uniknout do cloudu. Neexistují žádné pro čtení i zápis, ke kterým došlo ze zařízení v cloudu. Píků v grafu, proto jsou v intervalu 5 minut, který odpovídá počet_plateb niž prezenční signál zaškrtnuté mezi zařízení a službách. 

![ZVYŠUJÍ výkon ze zařízení do cloudu](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


Stejné zařízení snímek cloudu pořízení pro data hlasitost počínaje 2:00 odp. Výsledkem dat předávaných ze zařízení do cloudu. Čtení a zápis bylo podáno do cloudu v této doby trvání Graf vstupu a výstupu zobrazuje vrcholu různých metriky odpovídající čas, kdy byla přijatá snímek. 

![ZVYŠUJÍ výkon zařízení do cloudu po cloudu snímku](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## <a name="capacity-utilization"></a>Využití kapacity 

**Využití kapacity** sleduje metriky související s prostor úložiště dat používanou svazky, kontejnerů hlasitost nebo zařízení. Můžete vytvořit sestavy založené na využití kapacity primární úložiště, cloudového úložiště nebo úložišti zařízení. Kapacita využití můžete měřit na konkrétní svazku, kontejneru konkrétní hlasitost nebo všechny kontejnery hlasitost.


Primární cloudu a zařízení kapacitou lze popsat následujícím způsobem:

###<a name="primary-storage-capacity-utilization"></a>Využití kapacity primární úložiště
 
Tyto grafy zobrazují množství dat, aby došlo k zápisu StorSimple objemů dat je deduplicated a komprimovány. Využití primární úložiště můžete zobrazit tak, že všechny svazky nebo pro jeden hlasitost.

Při zobrazení primární hlasitost kapacita využití grafy pro všechny svazky porovnání všech jednotlivé svazky a sečíst primární data v obou těchto případech může být neshodu mezi těmihle dvěma čísly. Celkové primární data na všechny svazky nemusí sečtěte časy celkový součet primární data jednotlivých svazky. Může být příčinou jedna z následujících akcí:

- **Data snímku zahrnuté pro všechny svazky**: Toto chování je vidět jenom v případě, že máte verzi starší než aktualizace 3. Primární dat zobrazených pro všechny svazky představuje součet primární dat pro každou hlasitost a data snímku. Primární dat zobrazených pro dané hlasitost odpovídá pouze množství dat na hlasitost přiděleno (a neobsahuje odpovídající data snímku hlasitost).

    Můžete vysvětlení taky následující rovnice:

    *Primární dat (všechny svazky) = SUMA (primární dat (objem i) + velikost snímku dat (objem i))*
    
    *Pokud primární dat (objem i) = velikost primární dat přidělit hlasitost i*
 
    Pokud snímky se odstraní prostřednictvím služby, odstranění asynchronní dokončení na pozadí. Může trvat delší dobu velikost dat hlasitost mají být aktualizovány po odstranění snímku. 

    Pokud aktualizace 3 nebo novější, pak data snímku není součástí hlasitost data. A primární využití se vypočítá následujícím způsobem:

    * Primární dat (všechny svazky) = SUMA (primární dat (objem i)
    
    *Pokud primární dat (objem i) = velikost primární dat přidělit hlasitost i*
 
- **Svazky se sledováním zakázané součástí všech svazky**: Pokud máte svazky na vašem zařízení, pro kterou sledování je vypnutá, sledování data pro tyto jednotlivé svazky nebudou k dispozici v grafech. Data pro všechny svazky v grafu však obsahuje svazky, u kterých sledování je vypnuté. 
 
- **Odstranit svazky s živou přidružené zálohy zahrnuté pro všechny svazky**: Pokud svazky obsahující data snímku se odstraní, ale pořád existují související snímky a pak můžete vidět neshodu.

- **Odstranění svazky zahrnuté pro všechny svazky**: V některých případech může existovat staré svazky, i když tyto odstraněných. Efekt odstranění se nezobrazuje a zařízení může zobrazit nižší kapacitu k dispozici. Budete muset kontaktovat Microsoft Support odeberte tyto svazky.

Následující grafy zobrazují využití kapacity primární paměťové zařízení StorSimple před a po snímku cloudu byla přijata. Je to jenom data svazku, snímek cloudu neměňte primární úložiště. Jak vidíte, graf zobrazuje žádný rozdíl využití primární kapacity důsledku pořizování cloudu snímku. Snímek cloudu spustit asi 2:00 odp na toto zařízení.

![Využití primární kapacity před cloudu snímku](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Využití primární kapacity po cloudu snímku](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Pokud používáte aktualizace 2 nebo vyšší, je můžete rozdělit využití kapacity primární úložiště podle svazku, všechny svazky, všechny vrstvené a všechny místně připnuté svazky jak je ukázáno v následujícím příkladu. Rozdělení podle všechny místně připnuté svazky vám umožní rychle zjistit, kolik místních osy se používá.

![Primární využití kapacity pro všechny místně připnuté svazky](./media/storsimple-monitor-device/localvolumes.png)


###<a name="cloud-storage-capacity-utilization"></a>Využití kapacitu úložiště cloudu

Tyto grafy zobrazují množství cloudového úložiště používá. Tato data deduplicated a komprimovány. Tuto hodnotu obsahuje cloudu snímky, které mohou obsahovat data, která není projeví v svazky primární a bude k dispozici pro účely starším nebo potřeba uchovávání informací. Můžete porovnat primární a cloudu úložiště spotřebu obrázky na určitou představu o snížení sazeb dat, i když nebudou přesné číslo. Následující grafy zobrazují cloudu kapacita využití úložiště StorSimple zařízení před a po snímku cloudu byla přijata. Snímek cloudu začátek asi 2:00 odp na toto zařízení, uvidíte využití kapacity cloudu snímek ve stejnou dobu zvětšení z 5.73 MB 4.04 GB.

![Využití kapacity cloudu před cloudu snímku](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Využití kapacity po cloudu snímek v cloudu](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


###<a name="device-storage-capacity-utilization"></a>Využití kapacitu úložiště zařízení

Tyto grafy zobrazují celkové využití pro zařízení, které budou využití víc než primární úložiště, protože obsahuje SSD lineární osy. Tato úroveň obsahuje data, která existuje také na zařízení je ostatních vrstev. Kapacita ve vrstvě lineární SSD je uvedená tak, aby při příchodu nových dat na pevný disk osy (kdy je deduplicated a komprimovány) a následně do cloudu, přesune se původní data.

V průběhu času využití primární kapacity a využití kapacity zařízení pravděpodobně vzroste společně až do data zahájení vrstveny do cloudu. V tomto okamžiku využití kapacity zařízení pravděpodobně začne plateau, ale využití primární kapacity zvýší napsali další data.

Následující grafy zobrazují využití kapacity primární paměťové zařízení StorSimple před a po snímku cloudu byla přijata. Snímek cloudu začátek 2:00 odp a využití kapacity zařízení spustit zmenšení v té době. Využití kapacitu úložiště zařízení se na 11.58 GB 7.48 GB. To označuje, že největší pravděpodobností nekomprimované dat v lineární osy SSD byl deduplicated, komprimovány a přesune do osy pevný disk. Všimněte si, že pokud zařízení v SSD a pevný disk úrovní už velkého množství dat, nemusí zobrazit tento pokles. V tomto příkladu zařízení má malou část data.

![Využití kapacity zařízení před cloudu snímku](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Využití kapacity zařízení po cloudu snímku](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## <a name="network-throughput"></a>Výkon sítě

**Výkon sítě** sleduje metriky související s množství dat na hostitelském serveru a zařízení a mezi zařízení a cloudem převést z rozhraní iSCSI vyzývající sítě. Tento míru pro jednotlivá pole rozhraní sítě iSCSI můžete sledovat na svém zařízení.

Následující grafy zobrazují výkon sítě pro Data 0 a dat 4 obou 1 rozhraní sítě GbE na vašem zařízení. V této instanci Data 0 byl cloudu s podporou dat 4 byla povolena iSCSI. Zobrazí se příchozí a odchozí přenosy pro zařízení s StorSimple. Ploché řádek v grafu počínaje 3:24 hodin je díky tomu, že jsme shromažďování dat pouze každých 5 minut a by ho ignorovat. 

![Výkon sítě pro Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Výkon sítě pro Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## <a name="device-performance"></a>Zařízení výkonu 

**Výkon zařízení** sleduje metriky souvisejících s výkonem zařízení. Následující graf zobrazuje stat využití procesoru pro zařízení výroby.

![Využití procesoru pro zařízení](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [používat řídicí panel StorSimple Správce služeb zařízení](storsimple-device-dashboard.md).

- Zjistěte, jak [používat službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
