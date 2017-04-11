<properties
 pageTitle="Velikost cloudovým službám | Microsoft Azure"
 description="Seznam různých virtuálního počítače velikosti (a ID) pro web a pracovní role služby Azure cloudu."
 services="cloud-services"
 documentationCenter=""
 authors="Thraka"
 manager="timlt"
 editor=""/>
<tags
 ms.service="cloud-services"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="tbd"
 ms.date="10/27/2016"
 ms.author="adegeo"/>

# <a name="sizes-for-cloud-services"></a>Velikost Cloudovým službám

Toto téma popisuje dostupné velikosti a možnosti cloudové služby role instance (web rolí a pracovní). Je také důležité informace o zavádění nějaká při plánování použijte následující materiály. Každou velikost má číslo ID, které bude vložíte [Soubor definice služby](cloud-services-model-and-package.md#csdef).

Cloudové služby je jeden z několika typů zdrojů výpočetním nabízená Azure. Klikněte na [zde](cloud-services-choose-me.md) Další informace o Cloudovým službám.

> [AZURE.NOTE]Související Azure limity najdete v tématu [Azure předplatné a omezení služby, kvót a omezení](../azure-subscription-service-limits.md)

## <a name="sizes-for-web-and-worker-role-instances"></a>Formáty pro web a pracovní instance rolí

Existuje několik standardní velikosti můžete vybírat na Azure. Důležité informace týkající se některé z těchto velikostí patří:

* D řady VMs slouží ke spuštění aplikace, které požadovat vyšší výpočetního výkonu a výkonu dočasné disku. D řady VMs přidávejte k dočasné disku rychlejší procesorů, vyšší poměr paměti core a jednotce (SSD). Další informace najdete v tématu oznámení na Azure blogu [Nové velikosti virtuálního počítače D řady](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Dv2-řady, následník původní D-řada funkcemi výkonnější procesoru. Procesor Dv2 řady je o 35 % rychlejší procesor D řady. Je založená na nejnovější generování 2,4 GHz Intel Xeon® E5-2673 v3 procesor (Haswell) a s technologií 2.0 Intel Turbo zesílení můžete přejít až 3.1 GHz. Dv2 řada má stejné konfigurace paměti a disku jako D řady.

*   G řady VMs nabízí většina paměti a spustit hosts s rodinnými procesory Intel Xeon E5 V3.

*   VMs A řady můžete být nasazené na různé typy hardwaru a procesorů. Velikost je omezený na hardware nabízet výkon konzistentní procesoru spuštěné instance, bez ohledu na hardware, který je nasazený na základě. Stanovit fyzických na kterém je velikost nasazený dotaz virtuální hardware z uvnitř virtuálního počítače.

*   Velikost A0 je povolená přihlášeného na fyzické hardwaru. U této konkrétní velikosti jenom ostatní zákazníka nasazení ovlivnit výkon pracovního vytížení. Relativní výkon je podrobněji popsána níže jako očekávané základní vyměřené poplatky za jeho přibližnou variabilitě 15 procent.


Velikost virtuálního počítače ovlivňuje cen. Velikost má dopad i na zpracování paměti a úložiště objemu virtuální počítač. Úložiště počítá náklady samostatně založené na použité stránkách účtu úložiště. Podrobnosti najdete v tématu [Virtuálních počítačích ceny podrobnosti](https://azure.microsoft.com/pricing/details/virtual-machines/) a [Ceny úložiště Azure](https://azure.microsoft.com/pricing/details/storage/). 


Následující skutečnosti může můžete rozhodnout, na velikosti:


* Velikost A8 A11 a H řady jsou označovaná taky jako *náročné instance*. Navržený a optimalizované pro náročné hardwaru, které se spouští tyto formáty a aplikace náročné sítě, včetně výkonné výpočetní HPC () clusteru aplikací, modelování a simulace. Řada A8 A11 používá Xeon E5. 2670 Intel @ 2.6 GHZ a řadu H používá Xeon E5. 2667 Intel v3 @ 3,2 GHz. Podrobné informace a informace o použití těchto rozměrů, najdete v článku [o VMs A řadu H řady a náročné](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md). 

* Dv2-řady, D řady G řady, jsou ideální pro aplikace, které poptávka rychlejší procesorů, lepší místní disk výkon nebo mít vyšší požadavky paměti.  Nabízejí výkonné kombinaci pro mnoho podnikové aplikace.

*   Virtuální počítač větších, například A5 – část fyzické hosts v Azure datacentrech nemusí podporovat A11. Jako výsledek může zobrazit chybová zpráva **Konfigurace virtuálního počítače {název počítače} se nepodařilo** nebo **vytvořit virtuální počítač {název počítače}** při změně velikosti existující virtuální počítač nové velikost; Vytvoření nového virtuálního počítače v síti virtuální před 16 duben 2013; nebo přidání nového virtuálního počítače existující cloudové služby. Najdete v článku [Chyba: "Se nepodařilo konfigurace virtuálního počítače"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) ve fóru na podporu pro řešení pro každou scénáře.  

* Vaše předplatné může taky omezení počtu jádra nainstalované řady určitou velikost. Chcete-li zvýšit kvóty, kontaktujte podporu Azure.


## <a name="performance-considerations"></a>Důležité informace o výkonu

Jsme vytvořili koncept výpočet jednotky Azure (ACU) představuje způsob jejich výkonu (procesor) přes Azure skladové jednotky. To vám umožní snadno identifikovat které SKU je velmi pravděpodobné možnosti vyhovují vašim potřebám výkonu.  ACU aktuálně vztahují na malé (Standard_A1) OM, pak je 100 a všech SKU představovat asi kolik rychleji, že SKU mohlo by umožnit spuštění standardní srovnávacích. 

>[AZURE.IMPORTANT] ACU je pouze vodítko.  Výsledky pro vaše pracovní zátěž lišit. 

<br>

|SKU rodinu |ACU/základní |
|---|---|
|[Standard_A0](#a-series)   |50 |
|[Standard_A1-4 při instalaci](#a-series) |100 |
|[Standard_A5-7](#a-series) |100 |
|[A8 A11](#a-series)    |225 *|
|[D1 14](#d-series) |160 |
|[D1 15v2](#dv2-series) |210 - 250 *|
|[G1 – 5](#g-series)  |180 - 240 *|
|[H](#h-series) |290 - 300 *|

ACUs označené * pomocí technologie Intel® Turbo zvýšit četnost procesoru a poskytují zvýšení výkonu.  Částka zesílení může lišit v závislosti na velikosti OM, pracovního vytížení a jiných pracovní vytížení ve stejném hostiteli.

## <a name="size-tables"></a>Změnit velikost tabulek

V následujících tabulkách jsou zobrazeny velikosti kapacity, které poskytují.

* Kapacita se zobrazují v jednotkách OS nebo 1024 ^ 3 bajtů. Při porovnávání disků měří se v GB (1000 ^ 3 bajtů) disků měří se v OS (1 024 ^ 3) mějte na paměti, že čísel kapacity podle OS se může zobrazit menší. Například 1023 OS = 1098.4 GB

* Výkon disku měří se v vstupní a výstupní operace sekundu (procesorů) a MB / kde MB / = 10 ^ 6 bajty/s.

* Disků dat můžete pracovat v režimu cached nebo bez vyrovnávací paměti. Režim mezipaměť host operace disku data uložená v mezipaměti, je nastavena na **jen pro čtení** nebo **pouze**.  Režim mezipaměť hostitele bez vyrovnávací paměti dat operace disku, je nastavena na **žádný**.

* Maximální síťové šířky pásma je maximální agregovanou přidělit a přiřazené podle typu OM. Maximální šířky pásma obsahuje pokyny pro výběr správný typ OM zajistit odpovídající síťovou kapacitu je k dispozici. Při přecházení mezi minimum, střední, Maximum a velmi vysoké, se příslušným způsobem zvýší výkon. Výkon skutečné sítě závisí na spousta faktorů, včetně sítě a aplikace zatížení a nastavení sítě aplikace.


## <a name="a-series"></a>A řady

| Velikost        | Procesor vzorky | Paměť: OS | Místní pevný disk: OS | Max dat disků | Max dat disku výkon: procesorů | Max nic / šířku pásma sítě |
|-------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A0 | 1         | 0.768        | 20                    | 1              | 1 x 500              | 1 / Nízká                   |
| Standard_A1 | 1         | 1,75         | 70                    | 2              | 2 x 500              | 1 / střední              |
| Standard_A2 | 2         | 3,5 GB       | 135                   | 4              | 4 x 500              | 1 / střední              |
| Standard_A3 | 4         | 7            | 285                   | 8              | 8 x 500              | 2 / vysoké                  |
| Standard_A4 | 8         | 14           | 605                   | 16             | 16 × 500             | 4 / vysoké                  |
| Standard_A5 | 2         | 14           | 135                   | 4              | 4 X 500              | 1 / střední              |
| Standard_A6 | 4         | 28           | 285                   | 8              | 8 x 500              | 2 / vysoké                  |
| Standard_A7 | 8         | 56           | 605                   | 16             | 16 × 500             | 4 / vysoké                  |

## <a name="a-series---compute-intensive-instances"></a>A-řady – instance náročné

Informace a informace o použití těchto rozměrů, najdete v článku [o VMs A řadu H řady a náročné](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


| Velikost         | Procesor vzorky | Paměť: OS | Místní pevný disk: OS | Max dat disků | Max dat disku výkon: procesorů | Max nic / šířku pásma sítě |
|--------------|-----------|--------------|-----------------------|----------------|--------------------|-----------------------|
| Standard_A8 * | 8         | 56           | 382                   | 16             | 16 × 500             | 2 / vysoké                  |
| Standard_A9 * | 16        | 112          | 382                   | 16             | 16 × 500             | 4 / velmi vysoké             |
| Standard_A10 | 8         | 56           | 382                   | 16             | 16 × 500             | 2 / vysoké                  |
| Standard_A11 | 16        | 112          | 382                   | 16             | 16 × 500             | 4 / velmi vysoké             |

* RDMA může

## <a name="d-series"></a>D řady


| Velikost         | Procesor vzorky | Paměť: OS | Místní SSD: OS | Max dat disků | Max dat disku výkon: procesorů | Max nic / šířku pásma sítě |
|--------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / střední              |
| Standard_D2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / vysoké                  |
| Standard_D3  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / vysoké                  |
| Standard_D4  | 8         | 28           | 400                  | 16             | 16 × 500             | 8 / vysoké                  |
| Standard_D11 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / vysoké                  |
| Standard_D12 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / vysoké                  |
| Standard_D13 | 8         | 56           | 400                  | 16             | 16 × 500             | 8 / vysoké                  |
| Standard_D14 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / velmi vysoké             |

## <a name="dv2-series"></a>Dv2 řady

| Velikost            | Procesor vzorky | Paměť: OS | Místní SSD: OS | Max dat disků | Max dat disku výkon: procesorů | Max nic / šířku pásma sítě |
|-----------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_D1_v2  | 1         | 3.5          | 50                   | 2              | 2 x 500              | 1 / střední              |
| Standard_D2_v2  | 2         | 7            | 100                  | 4              | 4 x 500              | 2 / vysoké                  |
| Standard_D3_v2  | 4         | 14           | 200                  | 8              | 8 x 500              | 4 / vysoké                  |
| Standard_D4_v2  | 8         | 28           | 400                  | 16             | 16 × 500             | 8 / vysoké                  |
| Standard_D5_v2  | 16        | 56           | 800                  | 32             | 32 x 500             | 8 / velmi vysoké        |
| Standard_D11_v2 | 2         | 14           | 100                  | 4              | 4 x 500              | 2 / vysoké                  |
| Standard_D12_v2 | 4         | 28           | 200                  | 8              | 8 x 500              | 4 / vysoké                  |
| Standard_D13_v2 | 8         | 56           | 400                  | 16             | 16 × 500             | 8 / vysoké                  |
| Standard_D14_v2 | 16        | 112          | 800                  | 32             | 32 x 500             | 8 / velmi vysoké        |
| Standard_D15_v2 | 20        | 140          | 1 000                | 40             | 40 x 500             | 8 / velmi vysoké        |

## <a name="g-series"></a>G řady

| Velikost        | Procesor vzorky | Paměť: OS  | Místní SSD: OS  | Max dat disků | Max disku výkon: procesorů | Max nic / šířku pásma sítě |
|-------------|-----------|--------------|----------------------|----------------|--------------------|-----------------------|
| Standard_G1 | 2         | 28           | 384                  | 4              | 4 x 500            | 1 / vysoké                  |
| Standard_G2 | 4         | 56           | 768                  | 8              | 8 x 500            | 2 / vysoké                  |
| Standard_G3 | 8         | 112          | 1,536                | 16             | 16 × 500           | 4 / velmi vysoké             |
| Standard_G4 | 16        | 224          | 3,072                | 32             | 32 x 500           | 8 / velmi vysoké        |
| Standard_G5 | 32        | 448          | 6,144                | 64             | 64 x 500           | 8 / velmi vysoké        |


## <a name="h-series"></a>H řady

Azure virtuálních počítačích H řady jsou další vysoký výkon generování výpočetní že VMS zaměřené na výkonných výpočetních potřeby jako molekulární modelování a výpočetní dutá dynamics. Tyto 8 a 16 core VMs jsou vytvořené na technologii procesor Intel Haswell E5. 2667 V3 DDR4 paměti a místní úložiště SSD založený. 

Kromě který nabízí podstatně vyšší výkon procesoru nabízí řadu H různorodého možnosti Nízká latence RDMA sítí pomocí FDR InfiniBand a několik konfigurací paměti podporuje paměti náročné výpočetních požadavky.


| Velikost           | Procesor vzorky | Paměť: OS | Místní SSD: OS | Max dat disků | Max disku výkon: procesorů | Max nic / šířku pásma sítě |
|----------------|-----------|-------------|--------------------------|----------------|---------------------------|------------------------------|
| Standard_H8    | 8         | 56          | 1 000                     | 16             | 16 × 500                    | 8 / vysoké                      |
| Standard_H16   | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / velmi vysoké                  |
| Standard_H8m   | 8         | 112         | 1 000                     | 16             | 16 × 500                    | 8 / vysoké                      |
| Standard_H16m  | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / velmi vysoké                 |
| Standard_H16r * | 16        | 112         | 2000                     | 32             | 32 x 500                    | 8 / velmi vysoké                  |
| Standard_H16mr * | 16        | 224         | 2000                     | 32             | 32 x 500                    | 8 / velmi vysoké                  |


* RDMA může

## <a name="notes-standard-a0---a4-using-cli-and-powershell"></a>Poznámky: Standardní A0 - A4 pomocí rozhraní příkazového řádku a prostředí PowerShell 

Klasický nasazení modelu některé názvy velikost OM způsoby mírně odlišnou v rozhraní příkazového řádku a Powershellu:

* Standard_A0 je ExtraSmall 
* Standard_A1 je malý
* Standard_A2 je střední
* Standard_A3 je velká
* Standard_A4 je ExtraLarge

## <a name="configure-sizes-for-cloud-services"></a>Nastavení velikosti pro Cloudovým službám

Virtuální počítač velikost instanci rolí můžete zadat jako součást modelu služby popsaný v [souboru definice služby](cloud-services-model-and-package.md#csdef). Velikost roli zjistí počet procesoru jádra, kapacita paměti a místní systém velikost souboru přidělený instanci. Výběr velikosti roli podle požadavek zdrojů aplikace.

Tady je příklad pro nastavení velikosti roli být [Standard_D2](#general-purpose-d) Role webového instance:

```xml
<WorkerRole name="Worker1" vmsize="<mark>Standard_D2</mark>">
...
</WorkerRole>
```

## <a name="next-steps"></a>Další kroky

- Informace o [azure předplatné a omezení služby, kvót a omezení](../azure-subscription-service-limits.md).
- Informace [o VMs A řadu H řady a náročné](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) pro pracovního vytížení jako výkonných výpočetních HPC ().

