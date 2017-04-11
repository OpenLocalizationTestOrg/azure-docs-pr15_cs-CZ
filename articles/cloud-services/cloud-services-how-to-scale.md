<properties
    pageTitle="Automatické měřítko do cloudové služby na portálu | Microsoft Azure"
    description="(klasický) Naučte se používat portál klasické pro nastavení automatického měřítko pravidla pro cloudové služby webu nebo pracovního role v Azure."
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
- [Azure klasický portálu](cloud-services-how-to-scale.md)

Na stránce měřítko portálu Azure klasické ručně měřítko vašeho webu nebo pracovního role nebo můžete povolit automatické měřítko na základě zatížení procesoru nebo fronta.

>[AZURE.NOTE] Tento článek se zaměřuje na web a pracovníka role cloudové služby. Při vytváření virtuálních počítače (klasický) přímo, je hostovaný ve službě cloud. Některé z těchto informací se vztahuje k těmto typům virtuálních počítačích. Změna velikosti sady dostupnost virtuálních počítačích pouze probíhá je zapínat a vypínat na základě pravidel měřítko, které jste nakonfigurovali. Další informace o virtuálních počítačích a sady dostupnost přečtěte si téma [Správa dostupnosti virtuálních počítačích](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

Před zahájením konfigurace měřítko pro aplikaci byste měli zvážit následující informace:

- Změna měřítka je ovlivněna základní použití. Větší instance role používají další jádra. Můžete změnit měřítko aplikace pouze v rámci celého aplikaci jádra pro vaše předplatné. Pokud vaše předplatné může obsahovat maximálně 20 jádra a spuštění aplikace se dvěma střední velikostí cloud services (celkem čtyři jádra), je možné jenom přizpůsobit si ostatní nasazení služby cloudu ve vašem předplatném tak, že 16 jádra. Další informace o velikosti najdete [Cloudové služby velikosti](cloud-services-sizes-specs.md) .

- Musíte vytvořit fronty a přidružení k roli předtím, než je možné přizpůsobit aplikace založené na prahovou hodnotu zprávy. Další informace najdete v tématu [jak používat službu fronty úložiště](../storage/storage-dotnet-how-to-use-queues.md).

- Je možné přizpůsobit zdroje, které jsou propojené cloudové služby. Další informace o propojení materiály, najdete v článku [jak: propojení zdroje do cloudové služby](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Povolit dostupnost aplikace, ujistěte se, nasazení s dvěma nebo víc instancí role. Další informace najdete v tématu [Smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).



## <a name="schedule-scaling"></a>Změna měřítka plánu

Ve výchozím nastavení všech rolí neprovádějte konkrétní plán. Proto změnit nastavení platí pro všechny časy a všechny dny celý rok. Pokud chcete, můžete nastavit ručního nebo automatické změny velikosti pro:

- Dny v týdnu
- Víkendy
- Noci týden
- Ráno týden
- Konkrétní datum
- Rozsahy konkrétní den

Toto je conigured [Azure klasické portál](https://manage.windowsazure.com/) na  
**Cloudových služeb** > **\[cloudové služby\]** > **Měřítko** > **\[výrobní nebo pracovní\] ** stránky.

Kliknutím na tlačítko **Nastavení plánování doby** pro každou roli, kterou chcete změnit.

![Automatické měřítko cloudové služby založené na plán][scale_schedules]



## <a name="manual-scale"></a>Ruční měřítko

Na stránce **Měřítko** můžete ručně nezvětšíte nebo nezmenšíte počet spuštěných instancí do cloudové služby. Pokud jste nevytvořili plán to je nakonfigurovaný pro každý plán, který jste vytvořili nebo všechny času.

1. [Azure klasické portál](https://manage.windowsazure.com/)klikněte na **Cloudovým službám**a potom klikněte na název cloudovou službu otevřete řídicího panelu.

    > [AZURE.TIP] Pokud nevidíte cloudové služby, budete muset změnit z **výrobní** **pracovní** nebo naopak.

2. Klikněte na **velikost**.

3. Vyberte kalendář, který chcete změnit možnosti měřítka pro. Ve výchozím nastavení *bez naplánovaný čas* Pokud máte žádné plány definované.

4. Najděte v části **měřítko tak, že metriky** a vyberte **žádný**. Toto je výchozí nastavení pro všechny role.

5. Jednotlivé role ve službě cloud má posuvník pro změnu počtu instance, které chcete použít.

    ![Ruční změna velikosti rolí služby cloudu][manual_scale]

    Pokud potřebujete další instance, budete muset změnit [velikost virtuálního počítače služby cloudu](cloud-services-sizes-specs.md).

6. Klikněte na **Uložit**.  
Role instance bude přidat či odebrat podle svého výběru.

>[AZURE.TIP] Pokud se zobrazí ![][tip_icon] přesunutím ukazatele myši na něj a můžete ji získat o jaké konkrétní nastavení.


## <a name="automatic-scale---cpu"></a>Automatické měřítko - procesoru

Tím změníte měřítko, pokud průměrné procento využití procesoru, půjde nad nebo pod zadaných prahových hodnot; role instance jsou vytvořené nebo odstranit.

1. [Azure klasické portál](https://manage.windowsazure.com/)klikněte na **Cloudovým službám**a potom klikněte na název cloudovou službu otevřete řídicího panelu.

    > [AZURE.TIP] Pokud nevidíte cloudové služby, budete muset změnit z **výrobní** **pracovní** nebo naopak.

2. Klikněte na **velikost**.

3. Vyberte kalendář, který chcete změnit možnosti měřítka pro. Ve výchozím nastavení *bez plánované doby* Pokud máte žádné plány definované.

4. Najděte v části **měřítko tak, že metrické** a vyberte **procesoru**.

5. Teď můžete nakonfigurovat minimální a maximální oblasti instancí role, cílové využití procesoru (Aktivace měřítkem nahoru) a kolik instancí zobrazit jako nahoru a dolů.

![Zobrazit rolí služby cloudu jako zatížení procesoru][cpu_scale]

>[AZURE.TIP] Pokud se zobrazí ![][tip_icon] přesunutím ukazatele myši na něj a můžete ji získat o jaké konkrétní nastavení.





## <a name="automatic-scale---queue"></a>Automatické měřítko - fronty

To automaticky přizpůsobí Pokud počet zpráv ve frontě přejde nad nebo pod zadaný prahové hodnoty. role instance jsou vytvořené nebo odstranit.

1. [Azure klasické portál](https://manage.windowsazure.com/)klikněte na **Cloudovým službám**a potom klikněte na název cloudovou službu otevřete řídicího panelu.

    > [AZURE.TIP] Pokud nevidíte cloudové služby, budete muset změnit z **výrobní** **pracovní** nebo naopak.

2. Klikněte na **velikost**.

3. Najděte v části **měřítko tak, že metrické** a vyberte **procesoru**.

4. Teď můžete nakonfigurovat minimální a maximální oblasti Role instance, fronty a počtu zpráv pro zpracování pro jednotlivé instance a kolik instancí nahoru a dolů zobrazit jako.

![Zobrazit rolí služby cloudu jako fronty zpráv][queue_scale]

>[AZURE.TIP] Pokud se zobrazí ![][tip_icon] přesunutím ukazatele myši na něj a můžete ji získat o jaké konkrétní nastavení.


## <a name="scale-linked-resources"></a>Změna velikosti propojených zdrojů

Často při změně měřítka roli, je vhodné zobrazit databáze, který taky používá aplikaci. Pokud vytváříte odkaz databázi cloudové služby, můžete přejít měřítka nastavení pro daný zdroj kliknutím na příslušný odkaz.

1. [Azure klasické portál](https://manage.windowsazure.com/)klikněte na **Cloudovým službám**a potom klikněte na název cloudovou službu otevřete řídicího panelu.

    > [AZURE.TIP] Pokud nevidíte cloudové služby, budete muset změnit z **výrobní** **pracovní** nebo naopak.

2. Klikněte na **velikost**.

3. Najděte část **propojených zdrojů** a kliknuli na **Spravovat měřítko pro tuto databázi**.

    > [AZURE.NOTE] Pokud nevidíte oddíl **propojených zdrojů** , nejspíš nemáte propojeného zdroje.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
