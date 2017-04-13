<properties
    pageTitle="Nasazení aplikace na virtuální počítač měřítko sady | Microsoft Azure"
    description="Nasazení aplikace na sady měřítko virtuálního počítače"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Nasazení aplikace na sady měřítko virtuálního počítače

Spuštění aplikace v sadě měřítko OM obvykle nasazení jedním ze tří způsobů:

- Instalace nové softwaru nad snímkem platformy při nasazení. Obrázek platformy v tomto kontextu je operační systém obrázek z webu Azure Marketplace, jako se systémem Ubuntu 16.04, Windows Server 2012 R2 atd.

Nové software můžete nainstalovat platformu obrázek [OM rozšíření](../virtual-machines/virtual-machines-windows-extensions-features.md). Rozšíření OM je software, které se spouští při nasazení virtuálního počítače. Můžete použít jakýkoli kód, který chcete v době nasazení s příponou vlastní skript. [Tady](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) je příklad Azure správce prostředků šablony se dvěma příponami OM: Linux vlastní skript rozšíření k instalaci Apache a PHP a diagnostiky rozšíření posílat data o výkonu používá automatické Azure měřítko.

Výhodou tento přístup je máte úrovni oddělení mezi kód aplikace a operačního systému a samostatně spravovat aplikace. Samozřejmě označuje se taky další proměnné části a OM nasazení čas může být delší, pokud existuje mnohem skriptu stáhnout a nakonfigurovat.

**Pokud předáváte citlivé informace ve vlastní skript rozšíření příkaz (například hesla), je nutné zadat `commandToExecute` v `protectedSettings` atribut vlastní skript rozšíření místo `settings` atribut.**

- Vytvoření vlastního obrázku OM, která obsahuje operačního systému a aplikace v jedné virtuální pevný disk. Sada měřítko zde se skládá ze sady VMs zkopírovány z obrázku, který vytvoříte, které je potřeba spravovat. Tento postup vyžaduje žádnou další konfiguraci při nasazení OM. Však v `2016-03-30` verze OM měřítko sad (a starší verze), jsou omezené k účtu jednoho úložiště disků OS pro VMs v sadě měřítko. Tak můžete mít maximálně 40 VMs v sadě měřítko, namísto 100 OM měřítka nastavit omezení platformy obrázky. Další informace naleznete v tématu [Přehled návrh nastavte měřítko](./virtual-machine-scale-sets-design-overview.md) .

- Nasazení platformu nebo vlastní obrázek, který je v podstatě hostitel kontejner a nainstalujte aplikaci jako jednu nebo více kontejnery, které lze spravovat pomocí nástroje pro správu orchestrator nebo konfigurace. Hodní věc o tento přístup je jsou vyjádřeny infrastruktury cloudu z aplikace vrstvy a můžete udržovat je samostatně.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Co se stane, když nastavte měřítko OM zmenší se?

Když přidáte jeden nebo více VMs měřítko nastavil zvýšení kapacity – ať už ručně nebo prostřednictvím automatické měřítko – automaticky aplikaci. Příklad Pokud nastavené měřítko má rozšíření definované, spouštějí na nové OM pokaždé, když už je vytvořená. Pokud měřítko je založena na vlastní obrázek, je nové OM kopii vlastní obrázek zdroje. Pokud nastavené měřítko VMs jsou kontejneru hosts, pak může obsahovat kód při spuštění načíst kontejnery rozšíření vlastní skript nebo rozšíření instalují agenta registruje orchestrator obrázku (například služba Azure kontejneru).

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Jak spravujete aktualizace aplikací v sadách měřítko OM

Aktualizace aplikace v sadách měřítko OM tři hlavní způsoby postupujte podle ze tří způsobů předchozí nasazení aplikace:

* Aktualizace OM rozšíření. Všechny OM rozšíření definované pro sadu měřítko OM zpracují pokaždé, když zavedení nové OM existující OM reimaged nebo příponu OM se aktualizuje. Pokud potřebujete aktualizovat aplikace, přímo aktualizaci aplikace prostřednictvím rozšíření je životaschopné přístup – jednoduše aktualizujte definici rozšíření. Jedním jednoduchých způsobů, jak to udělat je změnou fileUris tak, aby ukazovaly na nové software.

* Přístup neměnný vlastní obrázek. Když cukrárna aplikace (nebo součásti aplikace) do obrázku OM můžete zaměřit na budování spolehlivé kanálu k automatizaci Tvůrce dotazů, testování a nasazení obrázky. Můžete navrhovat architektura usnadnit rychlé výměna množiny fázované měřítko do výroby. Příkladem tento přístup je [Azure Spinnaker ovladač](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Správce prostředků Azure podpora také balírna a Terraform, tak taky můžete definovat obrázků "jako kód" a jejich vytváření v Azure, použijte v sadě měřítko virtuální pevný disk. Však provedete tak by se stanou problematické pro Tržiště obrázky, skripty rozšíření/vlastní stanou místo, kam důležitější, protože nemáte ovládáte přímo bitů z Marketplace.

* Aktualizujte kontejnery. Abstraktní Správa životního cyklu aplikací na úrovni nad cloudové infrastruktury, například podle encapsulating aplikací a součástí aplikace do kontejnerů a spravovat tyto kontejnery prostřednictvím kontejneru orchestrators a Správce konfigurace jako Chef/Puppet.

Měřítka nastavit VMs pak stát se jejím stabilní podklad pro kontejnery a pouze vyžadují občas zabezpečení a aktualizace související s operačním systémem. Jak je uvedeno, je kontejner služby Azure příkladem používají tento přístup a vytváření službu kolem něj.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Jak se zavedením aktualizaci OS mezi aktualizace doménami?

Předpokládejme, že chcete aktualizovat obrázek s operačním systémem zachováním spuštění OM měřítko vybrána položka. Jedním ze způsobů tak je aktualizovat OM obrázky OM jeden po druhém. Lze provést pomocí prostředí PowerShell nebo Azure rozhraní příkazového řádku. Existují různé příkazy aktualizovat model OM měřítko vybrána položka (jak je definován konfigurace) a o vystavení "ruční upgrade" volání na jednotlivé VMs.

[Tady](https://github.com/gbowerman/vmsstools) je příklad Python skript, který umožňuje automatické aktualizace jedna aktualizace doménu nastavte měřítko OM najednou. (Výstrahou: je větší ověření koncepce než zesílený řešení připravené na výrobní – můžete chtít přidat některé chybové kontroly atd.).
