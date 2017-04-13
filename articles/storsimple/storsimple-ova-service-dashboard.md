<properties 
   pageTitle="Řídicí panel služeb StorSimple správce – virtuální pole | Microsoft Azure"
   description="Popisuje řídicí panel služeb StorSimple správce a vysvětluje, jak se používá ke sledování stavu své StorSimple virtuální pole."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>Pro pole virtuální StorSimple použít řídicí panel StorSimple Správce služeb

## <a name="overview"></a>Základní informace

Stránky řídicího panelu služeb StorSimple správce obsahuje souhrnné zobrazení StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení nebo virtuální zařízení), který je spojený s službu StorSimple správce zvýraznění ty, které vyžadují pozornost správce systému. Tento kurz představuje stránky řídicího panelu, vysvětluje obsahu řídicího panelu a (funkce) a popisuje úkoly, které můžete provádět na této stránce.

![Řídicí panel služeb](./media/storsimple-ova-service-dashboard/dashboard1.png)

Řídicí panel služeb StorSimple správce zobrazí následující informace:

- V oblasti **grafu** v horní části stránky uvidíte relevantní metriky virtuální zařízení. Můžete zobrazit použité na všech zařízeních virtuální primární úložiště, jakož i cloudového úložiště využívané virtuální zařízení průběhu určitého časového období. Pomocí ovládacích prvků v pravém horním rohu grafu můžete určit absolutní nebo relativní použití a zobrazíte 1 týden, 1 měsíc, 3 měsíce nebo roku 1 časové měřítko. Použití ovládacího prvku aktualizace ![nastavení aktualizace](./media/storsimple-ova-service-dashboard/refresh-control.png) aktualizovat graf.

- **Přehled použití** zobrazuje primární úložiště, která je zřízení využívané všechny virtuální zařízení relativní celkové dostupné úložiště na všech zařízeních virtuální. **Provisioned** označuje velikost úložiště, které byly shromážděny a přidělit pro použití, **použité** odkazuje na používání sdílené složky nebo svazky jako jistotu, že iniciátory připojených k virtuální zařízení a **Kapacita maximální** zobrazuje maximální kapacitu všech virtuální zařízení.

- Oblast **upozornění** obsahuje snímek aktivní upozornění na všech zařízeních virtuální, seskupené podle upozornění závažnosti. Kliknutím na tlačítko úroveň závažnosti otevřete stránku **oznámení** s rozsahem zobrazit tyto upozornění. Na stránce **upozornění** můžete kliknete na jednotlivé upozornění zobrazíte další informace o oznámení, včetně akcích doporučené. Pokud odstranění problému můžete také vymazat upozornění.

- Oblast **projekty** poskytuje snímek poslední práce na všech zařízeních virtuální, které jsou připojeny ke službě. Jsou odkazy, které můžete použít k zobrazení úloh, které jsou v současnosti v průběhu a ty, které úspěšný nebo za posledních 24 hodin. 

- Užitečné informace například stav služby, celkový počet virtuální zařízení připojených k služby, umístění služby a podrobnosti předplatného, který je spojený se službou obsahuje oblasti **můžete rychle podívat** na pravé straně stránky. Je taky odkaz protokol operace. Klikněte na odkaz zobrazíte seznam všech StorSimple Správce služby operace. 

Na stránce Správce StorSimple služby řídicího panelu zahajte tyto věci:

- Pokud potřebujete klíč registrace služby.
- Zobrazení protokolů operace.

## <a name="get-the-service-registration-key"></a>Získání klíč registrace služby

Služba registrace používá se při registraci virtuální zařízení StorSimple službu StorSimple správce tak, aby zařízení se zobrazí na portálu Azure klasické pro další činnost správy. Klíč vytvořen na prvním zařízení, virtuální a sdíleny s zbývající virtuální zařízení. 

Kliknutím na tlačítko **Registrační klíč** (v dolní části stránky) otevřete **Klíč registrace služby** dialogovým oknem, kde je můžete zkopírovat aktuální klíč registrace služby do schránky nebo obnovit klíč registrace služby.

![registrační klíč](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

Obnovení klávesu nemá vliv na dříve registrované virtuální zařízení: ovlivní pouze virtuální zařízení, které jsou registrované se službou po obnovených klávesu.

Další informace o získání klíč registrace služby přejděte na [získat klíč registrace služby](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>Zobrazení protokolů operace

Protokoly operací můžete zobrazit kliknutím na odkaz protokoly operace dostupné v podokně **rychle podívat** na řídicí panel. Tím přejdete na stránku služby správy, kde můžete filtrovat a zobrazit konkrétní protokoly StorSimple Správce služby.

![Operace protokolu](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Další kroky

Zjistěte, jak [používat místní web uživatelského rozhraní pro spravovat své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md).