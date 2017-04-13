<properties 
   pageTitle="Deaktivujte a odstraňte virtuální pole StorSimple | Microsoft Azure"
   description="Popisuje, jak odebrat StorSimple zařízení ze služby tak, že nejdřív deaktivace a potom jej odstraní."
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
   ms.date="06/20/2016"
   ms.author="alkohli" />

# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Deaktivujte a odstraňte StorSimple virtuální matice

## <a name="overview"></a>Základní informace

Po deaktivaci virtuální pole StorSimple server připojení mezi zařízení a odpovídající StorSimple Správce služby. Deaktivace je trvalé a nelze je vrátit zpět. Deaktivovaný zařízení nelze registrovaný u službu StorSimple správce znovu.

Budete muset deaktivujte a odstraňte StorSimple virtuální zařízení v následujících situacích:


- Zařízení je online a máte v plánu selhání toto zařízení. Budete muset udělat, pokud se chystáte upgradovat na větší zařízení. Po převedení dat zařízení a záložní dokončení, můžete odstraňte zařízení.

- Zařízení je offline a budete chtít převzít toto zařízení. K tomu může dojít v případě havárie primárního zařízení kvůli výpadku v datacentru, kde je dolů. Plánujete selhání zařízení na vedlejší zařízení. Po převedení dat zařízení a záložní dokončení, můžete odstranit zařízení.

- Chcete vyřadit zařízení a pak ji odstraňte. 
 

Po deaktivaci zařízení všechna data, která byla uložená místně již nebude přístupných osobám s postižením. Lze obnovit pouze data uložená v cloudu. Pokud chcete zachovat data zařízení po deaktivaci, pak byste měli vzít cloudu snímek všechna data před deaktivovat zařízení. Bude možné obnovit všechna data na pozdější.


Tento kurz vysvětluje postup:

- Deaktivace zařízení 
- Odstranění deaktivovaný zařízení


## <a name="deactivate-a-device"></a>Deaktivace zařízení

Proveďte následující postup k deaktivaci zařízení.

#### <a name="to-deactivate-the-device"></a>Chcete-li deaktivovat zařízení   

1. Přejděte na stránku **zařízení** . Vyberte zařízení, které chcete deaktivovat.

    ![Vyberte zařízení deaktivovat](./media/storsimple-ova-deactivate-and-delete-device/deactivate1m.png)

3. V dolní části stránky klikněte na tlačítko **deaktivovat**.

    ![Klikněte na deaktivovat](./media/storsimple-ova-deactivate-and-delete-device/deactivate2m.png)

4. Zobrazí se potvrzovací zpráva. Kliknutím na tlačítko **Ano** pokračovat. 

    ![Potvrďte deaktivovat](./media/storsimple-ova-deactivate-and-delete-device/deactivate3m.png)

    Proces deaktivovat spustí a trvat několik minut.

    ![Deaktivace probíhá](./media/storsimple-ova-deactivate-and-delete-device/deactivate4m.png)

3. Po deaktivaci bude aktualizovat seznam zařízení. 

    ![Deaktivace dokončení](./media/storsimple-ova-deactivate-and-delete-device/deactivate5m.png)

    Nyní můžete odstranit toto zařízení. 

## <a name="delete-the-device"></a>Odstranění zařízení

Zařízení musí nejdřív deaktivovat, abyste mohli odstranit. Odstranění zařízení odebere ze seznamu připojení ke službě zařízení. Služba potom můžete nadále spravovat odstraněné zařízení. Data spojená se zařízením ale zůstanou v cloudu. Mějte na paměti, že tato data pak nabíhání nákladů. 

Proveďte následující kroky pro odstranění zařízení:

#### <a name="to-delete-the-device"></a>Chcete-li odstranit zařízení 

 1. Na stránce Správce StorSimple služby **zařízení** vyberte deaktivovaný zařízení, které chcete odstranit.

    ![Vyberte zařízení, aby se odstranit](./media/storsimple-ova-deactivate-and-delete-device/deactivate5m.png)

 2. V dolní části na stránce klikněte na **Odstranit**.
 
    ![Klikněte na příkaz Odstranit](./media/storsimple-ova-deactivate-and-delete-device/deactivate6m.png)

 3. Zobrazí se výzva k potvrzení. Zadejte název zařízení potvrďte odstranění zařízení. Všimněte si, že odstraníte zařízení se neodstraní cloudu data spojená se zařízením. Klikněte na ikonu kontrola pokračovat.
 
    ![Potvrzení odstranění](./media/storsimple-ova-deactivate-and-delete-device/deactivate7m.png) 

 5. Může trvat několik minut, než zařízení, aby se odeberou. 

    ![Probíhá odstraňování](./media/storsimple-ova-deactivate-and-delete-device/deactivate8m.png)

    Po odstranění zařízení bude aktualizovat seznam zařízení.

    ![Odstranit dokončení](./media/storsimple-ova-deactivate-and-delete-device/deactivate9m.png)


## <a name="next-steps"></a>Další kroky

- Další informace o tom, jak používat službu StorSimple správce, přejděte na [službu StorSimple správce ke správě své StorSimple virtuální pole](storsimple-ova-manager-service-administration.md). 