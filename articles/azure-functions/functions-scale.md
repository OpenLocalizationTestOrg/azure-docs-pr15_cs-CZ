<properties
   pageTitle="Jak zobrazit Azure funkce | Microsoft Azure"
   description="Vysvětlení, jak přizpůsobit funkce Azure podle potřeb vaší pracovního vytížení řízeného událostmi."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure funkcí, funkce, zpracování události, webhooks, dynamické výpočetním, bez serveru architektura"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Jak zobrazit Azure funkcí

## <a name="introduction"></a>Úvod

Výhodou Azure funkce je pro využití prostředků jsou v případě potřeby pouze spotřebované. To znamená, že není zaplatit nedělá VMs nebo muset rezervovat kapacitu při může ho potřebujete. Místo toho platformu přidělí výpočetního výkonu při kódu běží škálování podle potřeby zpracovávání načítání a potom přizpůsobí, když neběží kód.

Mechanismus při nová funkce je plán dynamické služeb.  

Tento článek obsahuje přehled o fungování plán dynamické služeb a jak platformu upraví jako služba spustit kód.

Pokud ještě nejste seznámíte s funkcemi Azure, zkontrolujte, že zaškrtněte v článku [Přehled funkcí Azure](functions-overview.md) a snadněji pochopit jeho možnosti.

## <a name="configure-azure-functions"></a>Konfigurace funkce Azure

Dva hlavní nastavení se týkají měřítko:

* [Plán služeb aplikací Azure](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) nebo dynamické plánu služeb
* Velikost paměti pro spuštění prostředí

Náklady na funkce změny v závislosti na plán služeb, kterou jste vybrali. Dynamické služby plánu náklady jsou funkci času spuštění, velikosti paměti a počet spuštění. Náklady pouze pokud kódu skutečně běží metodu nabíhání nákladů.

Plán služeb aplikací hostitelem vašich funkcí na existující VMs, které mohou také použít ke spuštění jiné kódy. Po zaplacená za těchto VMs každý měsíc, je bez dalších poplatků pro provádění funkce na nich.

## <a name="choose-a-service-plan"></a>Zvolte plán služeb

Při vytváření funkce můžete spustit na plán dynamické služeb nebo [plán služeb aplikací](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
V aplikaci služby plán funkce spustí na vyhrazené OM, stejně jako webové aplikace prací dnes Basic, standardní nebo Premium skladové jednotky.
Tento vyhrazené OM přiřazen k aplikací a funkcí a neexistuje vždy zda kód aktivně probíhá nebo ne. Toto je je vhodný, pokud máte existující, v části využít VMs, ve kterých jsou už nepoužívá jiný kód nebo pokud budete chtít spustit funkce nepřetržitě nebo téměř nepřetržitě. Virtuálního počítače zruší párování náklady od runtime a paměť o velikosti. V důsledku toho můžete nastavit omezení nákladů mnoho funkcí dlouho probíhajících nákladů jeden nebo více VMs spuštěné v operačním systému.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
