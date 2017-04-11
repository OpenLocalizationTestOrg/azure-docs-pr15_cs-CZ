<properties 
   pageTitle="Možnosti podpory a vyřazování webů zásad průvodce pro hosta OS Azure | Microsoft Azure" 
   description="Informace o co podpory společnosti Microsoft se jde do OS hosta Azure používají cloudové služby." 
   services="cloud-services" 
   documentationCenter="na" 
   authors="raiye" 
   manager="timlt" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd" 
   ms.date="10/24/2016"
   ms.author="raiye"/>

# <a name="azure-guest-os-supportability-and-retirement-policy"></a>Azure zásad hostovaného OS podpory a vyřazování webů
Informace o této stránky se týká Azure hosta operačního systému ([S operačním systémem hosta](cloud-services-guestos-update-matrix.md)) pracovníka Cloudovým službám a role web (PaaS). Nevztahuje se na virtuálních počítačích (IaaS). 

Společnost Microsoft nemá publikované [zásady pro hosta OS podpory](http://support.microsoft.com/gp/azure-cloud-lifecycle-faq). Na stránku, kterou čtete teď Popisuje implementaci zásadu.

Zásady 

1. Společnost Microsoft bude podporovat **aspoň nejnovější dvěma druhy hostovaného OS**. Po ukončení rodiny zákazníci měli 12 měsíců k datu oficiální vyřazování webů provést aktualizaci na novější podporované hosta operačních systémů.
2. Bude podpory společnosti Microsoft **aspoň poslední dvě verze podporované rodiny hostovaného OS**. 
3. Bude podpory společnosti Microsoft na **nejnižších poslední dvě verze Azure SDK**. Po ukončení verzi sady SDK zákazníci budou mít 12 měsíců k datu oficiální vyřazování webů aktualizovat na novější verzi. 

V některých případech více než dvě verzemi nebo rodiny může podporované. Úřední informací na podporu s operačním systémem hosta se zobrazí na [Azure hosta OS verzí a SDK kompatibility matici](cloud-services-guestos-update-matrix.md).


## <a name="when-a-guest-os-family-or-version-is-retired"></a>Pokud je už nepoužívá hosta operačních systémů nebo verzi 


Nová hostovaného OS **řady** je někdy vyšších nové oficiální verze operačního systému Windows Server. Pokaždé, když zavádí nové hosta operačních systémů Microsoft vyřadit nejstarší hosta operačních systémů. 

Nové s operačním systémem Host **verzích** přináší o každý měsíc na zahrnutí nejnovějších aktualizacích pro Nejčastější. Z důvodu pravidelné měsíční aktualizace hosta operační systém verze je obvykle zakázané 60 dní po jeho verzi. To bude nejméně dvě verze hostovaného OS pro každou skupinu k dispozici. 

### <a name="process-during-a-guest-os-family-retirement"></a>Proces během hostovaného OS řady vyřazování webů 


Po ukončení se ohlásí, mají zákazníci tečku 12měsíční "převod" před starší řady úředně odebrali služby. Tentokrát přechodu prodloužit na rozhodnutí Microsoft. Aktualizace bude účtován na [Azure hosta OS verzí a SDK kompatibility matici](cloud-services-guestos-update-matrix.md).

Postupné vyřazování webů proces se spustí 6 měsíců v období placení přechod. Během tohoto období:

1. Microsoft upozorníte zákazníků vyřazování webů. 
2. Novější verzi Azure SDK nebude podporovat důchodu hostovaného OS řady.
3. Nové nasazení a redeployments Cloudovým službám nebude moct v důchodu řady

Microsoft zůstanou Představte nové verze s operačním systémem hosta zahrnutí nejnovější aktualizace Nejčastější až do poslední den doby přechodu jmenoval "datum vypršení platnosti". V té době budou všechny cloudových služeb spuštěné nepodporované v části Azure SLA. Společnost Microsoft nemá volnost vynutit upgradu, odstranit nebo zastavení těchto služeb po tomto datu.



### <a name="process-during-a-guest-os-version-retirement"></a>Proces během hosta operační systém verze vyřazování webů 
Pokud zákazníci nastavit jejich hostovaného OS automaticky aktualizovat, mají ukládání nemusíte vůbec starat o nakládání s operačním systémem Host verzích. Budou bude vždy používat nejnovější verze s operačním systémem Host.

Verze operačního systému hosta jsou vydána každý měsíc. Z důvodu sazba běžná vydáních každá verze má pevné životnost.

60 dní do životnost je verze "*Zakázané*". "Zakázané" znamená, že verze se odebere z portálu Microsoft Azure klasické. Je taky můžete už nastavit z CSCFG konfiguračního souboru. Existující nasazení jsou spuštěny, ale nebude moct nové nasazení a kód a konfigurace aktualizací existujících nasazení. 

Spuštění tuto verzi na později hostovaného OS verze "*vyprší jejich platnost*" a všechny instalace se platnost upgradu a nastavit na stav automaticky aktualizovat hostovaného OS v budoucnu. Vypršení platnosti probíhá v dávkách tak, aby se může lišit období doba přenosu mezi postižení a vypršení platnosti. 

Tyto období podat delší na rozhodnutí společnosti Microsoft pro usnadnění přechody zákazníka. Změny se oznamují na [Azure hosta OS vydáních a SDK kompatibility matice](cloud-services-guestos-update-matrix.md).



### <a name="notifications-during-retirement"></a>Oznámení během vyřazování webů 

* **Rodinný vyřazování webů** <br>Použijeme příspěvků na blogu a Azure klasické portálu oznámení. Zákazníci, kteří stále používáte vyřazené hosta operačních systémů se oznámení pomocí přímé komunikace (e-mailu, portálu zprávy, telefonní hovor) správcům přiřazené služby. Všechny změny bude účtován na tuto stránku a informačního kanálu RSS uvedených na začátku tuto stránku. 


* **Verze vyřazování webů** <br>Všechny změny bude účtován na tuto stránku a informačního kanálu RSS uvedené na začátek této stránky, včetně vydání, zakázáno a data vypršení platnosti. Správce služeb, dostanou e-mailů, pokud mají nasazení spuštěna zakázané hosta operační systém verze nebo členům rodiny. Časování tyto e-mailů se může lišit. Obecně jsou aspoň jeden měsíc před postižení, i když tento časování není oficiální SLA. 


## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Jak lze zmírnit důsledky migrace?**

Nejnovější hosta operačních systémů byste měli použít pro navrhování Cloudovým službám. 

1. Začněte plánovat migraci na novější skupinu brzy. 
2. Nastavte si dočasné zkušební nasazení otestovat, vaše cloudové služby na novou skupinu. 
3. Nastavení verze s operačním systémem hosta **automaticky** (osVersion = * v souboru [.cscfg](cloud-services-model-and-package.md#cscfg) ) tak, aby automaticky objeví migrace pro nové verze s operačním systémem Host.

**Co když Moje webové aplikace vyžadují hlubší integrace s operačním systémem?**

Pokud architektuře webové aplikace vyžaduje hlubší závislost v podkladovém operačním systému, podporované platformy použití funkce, například [při spuštění úkoly](cloud-services-startup-tasks.md) nebo jiné rozšiřitelnost mechanismy, které může existovat v budoucnu. Můžete taky můžete také [Azure virtuálních počítačích](https://azure.microsoft.com/documentation/scenarios/virtual-machines/) (IaaS – infrastruktury služby), kde jste zodpovědní správy operačního systému.
 
## <a name="next-steps"></a>Další kroky
Prohlédněte si nejnovější [vydání s operačním systémem Host](cloud-services-guestos-update-matrix.md).
