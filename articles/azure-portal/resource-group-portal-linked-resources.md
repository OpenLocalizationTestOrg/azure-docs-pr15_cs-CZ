<properties 
    pageTitle="Související a propojených zdrojů v galerii vedle sebe" 
    description="Informace o souvisejících a propojených zdrojů, které se zobrazí v galerii dlaždice portálu Azure náhled." 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>Související a propojených zdrojů v galerii vedle sebe

Galerie dlaždice umožňuje najít dlaždice pro určitého zdroje a přetáhněte je na aktuální zásuvné. Pomocí Galerie dlaždice, můžete vytvořit zobrazení pro správu, které zahrnují zdroje. Související materiály pro daný zdroj, zahrnout všechny zdroje, které mají stejné skupiny prostředků jako zdroje a zdroje, které odkazují na nebo ze zdroje.

## <a name="linked-resources-in-azure-resource-manager"></a>Propojených zdrojů v správce prostředků Azure

Propojení je funkce správce Azure prostředků.  Umožňuje deklarovat vztahy mezi zdroji i v případě, že není uložena ve stejné skupině zdroje. Propojení nemá žádný vliv na modul runtime zdrojů, žádný vliv na fakturace a žádný vliv na přístupu na základě rolí.  Je jednoduše mechanismus použitelných znázornit vztahy tak, aby nástroje jako galerii dlaždice managementu formátovaným dojít.  Vaše nástroje můžete prozkoumat odkazy pomocí odkazů rozhraní API a poskytují správce vlastní vztahů dojde i. 

## <a name="how-do-i-link-my-resources"></a>Propojení zdrojů

Při vytváření zdrojů prostřednictvím portálu nebo nasazení šablony pomocí prostředí PowerShell Azure nebo rozhraní příkazového řádku Azure spojení jsou vytvořena automaticky pro některé závislá zdroje. Můžete také programově propojíte zdrojů pomocí [Propojených zdrojů rozhraní REST API](https://msdn.microsoft.com/library/azure/mt238499.aspx) nebo deklarování vztahy v šabloně. Úplné informace o práci s propojených zdrojů najdete v článku [propojení zdroje v Azure zdroje správce](../resource-group-link-resources.md).

## <a name="next-steps"></a>Další kroky

- V případě potřeby Úvod k psaní správce prostředků Azure šablon najdete v článku [vytváření šablon](../resource-group-authoring-templates.md).
- Se ponoříte hlouběji do větší podrobnosti o vytváření propojení mezi materiály, najdete v článku [propojení zdroje v Azure zdroje správce](../resource-group-link-resources.md).
- Další informace o práci s skupiny zdrojů prostřednictvím portálu náhled, najdete v tématu [na portálu Azure náhled ke správě Azure prostředků](resource-group-portal.md).
