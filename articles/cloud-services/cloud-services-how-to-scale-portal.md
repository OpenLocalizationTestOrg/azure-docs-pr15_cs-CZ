<properties
    pageTitle="Automatické měřítko do cloudové služby na portálu | Microsoft Azure"
    description="Naučte se používat portál pro nastavení automatického měřítko pravidla pro cloudové služby webu nebo pracovního role v Azure."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Jak automatické měřítko do cloudové služby

> [AZURE.SELECTOR]
- [Azure portálu](cloud-services-how-to-scale-portal.md)
- [Azure klasické portálu](cloud-services-how-to-scale.md)

Podmínky můžete nastavit pro roli cloudové služby pracovníka, která aktivace měřítko či oddálení operace. Podmínky rolí můžete založeny na využití procesoru, disku nebo zatížení sítě role. Můžete také nastavit conditation na základě fronta nebo míru některé Azure zdroje přidružený k předplatnému.

>[AZURE.NOTE] Tento článek se zaměřuje na web a pracovní role cloudové služby. Při vytváření virtuálních počítače (klasický) přímo, je hostovaný ve službě cloud. Můžete zobrazit standardní virtuálního počítače jako přiřadí se mu [Nastavení dostupnost](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) a ručně je zapnout nebo vypnout.

## <a name="considerations"></a>Co byste měli zvážit

Než nastavíte měřítko pro aplikaci byste měli zvážit následující informace:

- Změna měřítka je ovlivněna základní použití. Větší instance role používají další jádra. Můžete změnit měřítko aplikace pouze v rámci celého aplikaci jádra pro vaše předplatné. Pokud vaše předplatné může obsahovat maximálně 20 jádra a spuštění aplikace se dvěma střední velikostí cloud services (celkem čtyři jádra), je možné jenom přizpůsobit si ostatní nasazení služby cloudu ve vašem předplatném tak, že 16 jádra. Další informace o velikosti najdete [Cloudové služby velikosti](cloud-services-sizes-specs.md) .

- Je možné přizpůsobit podle prahovou hodnotu fronty zprávy. Další informace o tom, jak používat fronty, najdete v tématu [používání fronty úložiště služby](../storage/storage-dotnet-how-to-use-queues.md).

- Můžete také změnit velikost dalších zdrojů přidružený k předplatnému.

- Povolit dostupnost aplikace, ujistěte se, nasazení s dvěma nebo víc instancí role. Další informace najdete v tématu [Smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

## <a name="where-scale-is-located"></a>Kde je uložena měřítko

Když vyberete cloudové služby, měli byste mít zásuvné služby cloudu viditelné.

1. Na zásuvné cloudové služby, na dlaždici **rolí a instance** vyberte název cloudovou službu.   
**Důležité**: nezapomeňte klikněte na roli služby cloudu není role instanci systému, který je pod roli.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Vyberte dlaždici **Měřítko** .

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatické měřítko

Konfigurace nastavení pro roli s dva způsoby **ručního** nebo **Automatické**. Ruční tak, jak byste chtěli, nastavte absolutní počet instance. Automatické umožňuje však můžete nastavit pravidla, které určují způsob jak a v jaké míry můžete by měl zobrazit.

Nastavte **měřítko tak, že** možnost **plán a výkonu**pravidel.

![Nastavení služby cloudu s profilem a pravidla](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Existující profil.
2. Přidání pravidla nadřazené profilu.
3. Přidání jiného profilu.

Vyberte možnost **Přidat profilu**. Profil určuje režimu, který chcete použít pro měřítko: **vždy**, **opakování**, **pevné datum**.

Po dokončení konfigurace profilu a pravidla, vyberte ikonu **Uložit** nahoře.

#### <a name="profile"></a>Profil

Profil Nastaví minimální a maximální instancí měřítka, a taky tento měřítka rozsah po aktivní.

* **Vždy**

    Vždy zachovat tato oblast instancí k dispozici.  

    ![Cloudové služby, který vždy přizpůsobit](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Opakování**

    Klikněte na nastavit dny v týdnu zobrazit.

    ![Cloudové služby měřítko s plánu opakování](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Pevné datum**

    Pevné rozsah zobrazit roli.

    ![Cloudové služby měřítko s pevnou datum](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Po dokončení konfigurace profilu, klikněte na tlačítko **OK** v dolní části zásuvné profilu.

#### <a name="rule"></a>Pravidlo

Pravidla se přidají do profilu a představují podmínce, která bude spouštět měřítka. 

Aktivační událost pravidlo je založena na míru cloudovou službu (využití procesoru, diskem nebo činnosti sítě) do kterého můžete přidávat podmíněné hodnoty. Dále můžete nechat aktivační události na základě fronta nebo míru některé Azure zdroje přidružený k předplatnému.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Poté, co jste nakonfigurovali pravidlo, klikněte na tlačítko **OK** v dolní části zásuvné pravidlo.

## <a name="back-to-manual-scale"></a>Zpět na ruční měřítko

Přejděte na [Nastavení](#where-scale-is-located) a nastavte **měřítko tak, že** na **počet instancí, které mám zadat ručně**.

![Nastavení služby cloudu s profilem a pravidla](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Tato akce odebere automatické změny velikosti z role a pak můžete nastavit počet instancí přímo. 

1. Stupnice (ručního nebo automatické) možnost.
2. Posuvník instance role pro nastavení instance, které chcete přizpůsobit.
3. Výskyty roli chcete přizpůsobit.

Po dokončení konfigurace nastavení měřítka, vyberte ikonu **Uložit** nahoře.

