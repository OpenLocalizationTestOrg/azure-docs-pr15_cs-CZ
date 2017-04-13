<properties 
   pageTitle="Správa záznamů kontrol přístup pro pole virtuální StorSimple | Microsoft Azure"
   description="Popisuje, jak spravovat záznamy řízení přístupu (ACRs) a zjistit, které hosts se můžete připojit k hlasitost virtuální matici StorSimple."
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
   ms.date="05/03/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>Ke správě záznamů kontrol přístup pro pole virtuální StorSimple použít službu StorSimple správce 

## <a name="overview"></a>Základní informace

Ovládací prvek záznamů z aplikace Access (ACRs) umožňují určit, které hosts se můžete připojit k hlasitosti na StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení). ACRs jsou nastavená na konkrétní hlasitost a obsahují kvalifikované názvy iSCSI (IQN) hostitelů. Pokud hostitele pokusí o připojení ke svazku, zařízení zkontroluje ACR přidružené svazku pro název IQN, a pokud existuje shoda, klikněte připojení. V části **záznamy řízení přístupu** na stránce **Konfigurovat** zobrazí všechny záznamy ovládací prvek přístup s odpovídajícím IQN hostitele.

Tento kurz vysvětluje následující běžné úkoly týkající se ACR:

- Získání IQN
- Přidání záznamu řízení přístupu 
- Upravit záznam řízení přístupu 
- Odstranění záznamu řízení přístupu 

> [AZURE.IMPORTANT] 
> 
> - Po přiřazení ACR svazku, starat, že hlasitost není přístup současně víc než jednoho hostitele neseskupené vzhledem k tomu, že to může způsobit poškození hlasitost. 
> - Při odstraňování ACR ze svazku, ujistěte se, že hostitele nepřistupuje hlasitost, protože odstranění může vést k přerušení pro čtení i zápis.

## <a name="get-the-iqn"></a>Získání IQN

Takto zobrazíte IQN hostitelem systému Windows, na kterém běží Windows Server 2012.

[AZURE.INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Přidání ACR

Na stránce **Konfigurace** služby StorSimple správce můžete přidat ACRs. Obvykle budete přidružovat jeden ACR jeden hlasitost.

Informace o připojení ACR s objemem popisuje článek [Přidání svazku](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).

>[AZURE.IMPORTANT] 
> 
>Po přiřazení ACR svazku, starat, že hlasitost není přístup současně víc než jednoho hostitele neseskupené vzhledem k tomu, že to může způsobit poškození hlasitost.
 
Takto přidáte ACR.

#### <a name="to-add-an-acr"></a>Chcete-li přidat ACR

1. Na cílovou stránku služby vyberte služby, poklikejte na název služby a potom klikněte na kartu **Konfigurace** .

    ![Karta Konfigurace](./media/storsimple-ova-manage-acrs/acr1.png)

2. V tabulkovém výpisu v části **záznamy řízení přístupu**zadejte **název** pro váš ACR.

3. V části **iSCSI vyzývající název**zadejte název IQN hostitele Windows. 

4. Klepnutím na tlačítko **Uložit** v dolní části stránky uložte nově vytvořený ACR. Zobrazí se tato zpráva potvrzení.

    ![potvrzovací zprávy](./media/storsimple-ova-manage-acrs/acr2.png)

5. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-ova-manage-acrs/check-icon.png). Tabulkovém výpisu bude aktualizována toto přidání.

## <a name="edit-an-acr"></a>Úprava ACR

Na stránce **Konfigurace** Azure klasické portálu můžete upravit ACRs. 

> [AZURE.NOTE] Upravte pouze ACRs, které nejsou aktuálně používaných. Upravit ACR přidružené svazku, který právě používá, by měl hlasitost byl nejdřív offline.

Provedení následujících kroků můžete upravit ACR.

#### <a name="to-edit-an-acr"></a>Chcete-li upravit ACR

1. Na cílovou stránku služby vyberte služby, poklikejte na název služby a potom klikněte na kartu **Konfigurace** .

2. V tabulkovém výpisu záznamů kontrol přístup myší ACR, který chcete upravit.

3. Zadejte nový název a/nebo IQN pro ACR.

4. Klepnutím na tlačítko **Uložit** v dolní části stránky uložte změny ACR. Zobrazí se potvrzovací zpráva. 

5. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-ova-manage-acrs/check-icon.png). Tabulkovém výpisu bude aktualizován této změně.

## <a name="delete-an-access-control-record"></a>Odstranění záznamu řízení přístupu

Na stránce **Konfigurace** Azure klasické portálu můžete odstranit ACRs. 

> [AZURE.NOTE] 
> 
> - Odstraníte pouze ACRs, které nejsou aktuálně používaných. Odstranění ACR přidružené svazku, který právě používá, by měl hlasitost byl nejdřív offline.
> - Při odstraňování ACR ze svazku, ujistěte se, že hostitele nepřistupuje hlasitost, protože odstranění může vést k přerušení pro čtení i zápis.

Proveďte následující kroky pro odstranění záznamu řízení přístupu.

#### <a name="to-delete-an-access-control-record"></a>Chcete-li odstranit záznam řízení přístupu

1. Na cílovou stránku služby vyberte služby, poklikejte na název služby a potom klikněte na kartu **Konfigurace** .

2. V tabulkovém výpisu záznamů kontrol přístup (ACRs) najeďte myší na ACR, který chcete odstranit.

3. Ikona odstranit (**x**) uvidí na pravý krajní sloupec ACR, kterou jste vybrali. Klikněte na ikonu **x** a odstranit ACR. Zobrazí se tato zpráva potvrzení.

    ![potvrzovací zprávy](./media/storsimple-ova-manage-acrs/acr3.png)

5. Klikněte na ikonu zaškrtnutí ![Zaškrtněte ikony](./media/storsimple-ova-manage-acrs/check-icon.png). Tabulkovém výpisu bude aktualizována odstranění.

## <a name="next-steps"></a>Další kroky

- Další informace o [přidávání svazky a konfiguraci ACRs](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).
