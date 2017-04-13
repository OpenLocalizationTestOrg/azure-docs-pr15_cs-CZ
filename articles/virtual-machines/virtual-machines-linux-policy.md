<properties
    pageTitle="Používání zásad pro Azure správce prostředků virtuálních počítačích | Microsoft Azure"
    description="Způsob použití zásad do Azure správce prostředků Linux virtuálního počítače"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/13/2016"
    ms.author="singhkay"/>

# <a name="apply-policies-to-azure-resource-manager-virtual-machines"></a>Používání zásad pro správce prostředků virtuálních počítačích Azure

Pomocí zásady organizace vynutit různých konvence a pravidla v rámci organizace. Vynucení požadované chování můžou pomoct zmírnění rizik při které přispívají k úspěchu organizace. V tomto článku se popisu použití zásad správce prostředků Azure k definování požadované chování virtuálních počítačích vaší organizace.

Osnovy pokyny k tomu je jako pod

1. Správce prostředků zásad Azure 101
2. Definování zásad pro virtuálního počítače
3. Vytvoření zásad
4. Použití zásad

## <a name="azure-resource-manager-policy-101"></a>Správce prostředků zásad Azure 101

Začínáme s správce prostředků Azure zásad, doporučujeme z níže uvedených článků pro čtení a potom dalším pomocí postupu v tomto článku. Z níže uvedených článků popisuje základní definici a struktury zásadu, jak Zásady vyhodnocena a příklady různých definice zásad.

* [Přidávání a používání zdrojů a řízení přístupu pomocí zásad](../resource-manager-policy.md)

## <a name="define-a-policy-for-your-virtual-machine"></a>Definování zásad pro virtuálního počítače

Jednu obvyklé scénáře pro svou společnost může být jenom uživatelům jejich vytváření virtuálních počítačích z konkrétní operačních systémů, které byly otestovat kompatibilní s aplikací LOB. Použití zásad správce prostředků Azure tento úkol lze provést v několika krocích. V tomto příkladu zásad nyní klikněte na tlačítko Povolit pouze se systémem Ubuntu 14.04.2-LTS virtuálních počítačích vytvořit. Definice zásad vypadá pod

```
"if": {
  "allOf": [
    {
      "field": "type",
      "equals": "Microsoft.Compute/virtualMachines"
    },
    {
      "not": {
        "allOf": [
          {
            "field": "Microsoft.Compute/virtualMachines/imagePublisher",
            "equals": "Canonical"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageOffer",
            "equals": "UbuntuServer"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/imageSku",
            "equals": "14.04.2-LTS"
          }
        ]
      }
    }
  ]
},
"then": {
  "effect": "deny"
}
```

Výše uvedené zásady můžete snadno upravovat scénáře, které můžete chtít povolit nějaký obrázek se systémem Ubuntu l pro nasazení virtuálního počítače s pod změnit

```
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

#### <a name="virtual-machine-property-fields"></a>Pole vlastnosti virtuálního počítače

Následující tabulka popisuje vlastnosti virtuálního počítače, které mohou sloužit jako pole zásad definition. Další informace o pole zásad naleznete v článku níže:

* [Pole a zdrojů](../resource-manager-policy.md#fields-and-sources)


| Název pole     | Popis                                        |
|----------------|----------------------------------------------------|
| imagePublisher | Určuje vydavatele obrázku               |
| imageOffer     | Určuje nabídky pro vybraný obrázek aplikace publisher |
| imageSku       | Určuje SKU pro vybrané nabídky             |
| imageVersion   | Určuje verzi obrázku pro zvolené SKU     |

## <a name="create-the-policy"></a>Vytvoření zásad

Zásady můžete snadno vytvořit pomocí rozhraní REST API přímo nebo rutiny prostředí PowerShell. Vytváření zásad, najdete v článku níže:

* [Vytvoření zásad](../resource-manager-policy.md#creating-a-policy)


## <a name="apply-the-policy"></a>Použití zásad

Po vytvoření zásady musíte použít na definovaný rozsah. Obor může být předplatné, skupina zdroje nebo dokonce zdroje. Použití zásady, najdete v článku níže:

* [Vytvoření zásad](../resource-manager-policy.md#applying-a-policy)
