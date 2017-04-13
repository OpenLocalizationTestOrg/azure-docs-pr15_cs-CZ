<properties 
   pageTitle="Deaktivujte a odstranění zařízení StorSimple | Microsoft Azure"
   description="Popisuje, jak odebrat StorSimple zařízení ze služby tak, že nejdřív deaktivace a potom jej odstraní."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="anoobbacker" />

# <a name="deactivate-and-delete-a-storsimple-device"></a>Deaktivujte a odstranění zařízení StorSimple

## <a name="overview"></a>Základní informace

Můžete chtít udělat StorSimple zařízení mimo služby (například pokud nahrazení nebo upgradu zařízení nebo pokud už používáte StorSimple). Pokud jde o případ, budete muset deaktivovat zařízení před odstraněním ji. Deaktivace přeruší připojení mezi zařízení a odpovídající StorSimple Správce služby. Tento kurz vysvětluje, jak odebrat StorSimple zařízení ze služby tak, že první deaktivací a potom jej odstraní. 

Po deaktivaci zařízení všechna data, která byla uložená místně na zařízení již nebude přístupných osobám s postižením. Lze obnovit pouze data spojená se zařízení, které jste uložili v cloudu.  

>[AZURE.WARNING] Deaktivace je trvalé a nelze je vrátit zpět. Deaktivovaný zařízení nelze registrovaný u službu StorSimple správce, pokud je první obnovit po výrobce. 
>
>U výrobce obnovit proces odstraní všechna data, která byla uložená místně na vašem zařízení. Proto je důležité, udělejte si snímek cloudu všechna data před deaktivovat zařízení. Bude možné obnovit všechna data na pozdější.

Tento kurz vysvětluje postup:

- Deaktivace zařízení a odstraňte data
- Deaktivace zařízení a zachovat data

Také vysvětluje, jak deaktivace a odstranění označené jako pracuje na StorSimple virtuální zařízení.

>[AZURE.NOTE] Než deaktivovat StorSimple fyzické nebo virtuální zařízení měli zastavit nebo odstranění klienty a hosts, které jsou závislé na toto zařízení.

## <a name="deactivate-and-delete-data"></a>Deaktivace a odstraňují data

Pokud zajímají úplně odstranění zařízení a nechcete, aby uchovávání dat na zařízeních, proveďte následující kroky.

#### <a name="to-deactivate-the-device-and-delete-the-data"></a>Deaktivace zařízení a odstranit data  

1. Před deaktivace zařízení, je potřeba odstranit všechny hlasitost kontejnery (a svazky) přidružený k zařízení. Kontejnery hlasitost můžete odstranit pouze po odstranění přidružené zálohy.

2. Deaktivace zařízení následujícím způsobem:

    1. Na stránce Správce StorSimple služby **zařízení** vyberte zařízení, které chcete deaktivovat a v dolní části stránky klikněte na tlačítko **deaktivovat**.

    2. Zobrazí se potvrzovací zpráva. Kliknutím na tlačítko **Ano** pokračovat. Proces deaktivovat spustí a trvat několik minut.

3. Po deaktivaci můžete úplně odstranit zařízení. Odstranění zařízení odebere ze seznamu připojení ke službě zařízení. Služba potom můžete nadále spravovat odstraněné zařízení. Odstranění zařízení pomocí následujících kroků:

    1. Na stránce Správce StorSimple služby **zařízení** vyberte deaktivovaný zařízení, které chcete odstranit.

    2. V dolní části na stránce klikněte na **Odstranit**.

    3. Zobrazí se výzva k potvrzení. Kliknutím na tlačítko **Ano** pokračovat.

    Může trvat několik minut, než zařízení, aby se odeberou.

## <a name="deactivate-and-retain-data"></a>Deaktivujte a uchovávání dat

Pokud je máte zájem o odstranění zařízení, ale chcete zachovat data, proveďte následující kroky.

####<a name="to-deactivate-a-device-and-retain-the-data"></a>Deaktivace zařízení a zachovat data 

1. Deaktivace zařízení. Hlasitost kontejnery a snímky zařízení bude platit.

    1. Na stránce Správce StorSimple služby **zařízení** vyberte zařízení, které chcete deaktivovat a v dolní části stránky klikněte na tlačítko **deaktivovat**.

    2. Zobrazí se potvrzovací zpráva. Kliknutím na tlačítko **Ano** pokračovat. Proces deaktivovat spustí a trvat několik minut.

2. Teď můžete převzít kontejnerů hlasitost a související snímky. Postupy přejděte na [převzetí a havárie obnovení StorSimple zařízení](storsimple-device-failover-disaster-recovery.md).

3. Po deaktivaci a překlopení můžete úplně odstranit zařízení. Odstranění zařízení odebere ze seznamu připojení ke službě zařízení. Služba potom můžete nadále spravovat odstraněné zařízení. Proveďte následující kroky pro odstranění zařízení:
 
    1. Na stránce Správce StorSimple služby **zařízení** vyberte deaktivovaný zařízení, které chcete odstranit.

    2. V dolní části na stránce klikněte na **Odstranit**.

    3. Zobrazí se výzva k potvrzení. Kliknutím na tlačítko **Ano** pokračovat.

    Může trvat několik minut, než zařízení, aby se odeberou.

## <a name="deactivate-and-delete-a-virtual-device"></a>Deaktivujte a odstraňte virtuální zařízení

Virtuální zařízení StorSimple zruší deaktivaci přidělení virtuální počítač. Pak můžete odstranit virtuální počítač a zdroje vytvořen, pokud byla zřízení. Po deaktivaci virtuální zařízení nejde obnovit původní stav. 

Deaktivace výsledky v následujících akcí:

- Virtuální zařízení StorSimple se odebere.

- OSDisk a Data disků vytvořené pro zařízení virtuální StorSimple se odeberou.

- Služby hostované a virtuální síť vytvořená během vytváření se zachovají. Pokud nepoužíváte tyto entity, je nutné je odstranit ručně.

- Snímky cloudu vytvořil virtuální zařízení StorSimple se zachovají.

## <a name="next-steps"></a>Další kroky
- Obnovit výchozí nastavení deaktivovaný zařízení, přejděte na [resetujte tovární výchozí nastavení](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

- Technická pomoc, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md).

- Další informace o tom, jak používat službu StorSimple správce, přejděte na [službu StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md). 
