<properties 
   pageTitle="Změna hesla správce StorSimple virtuální zařízení | Microsoft Azure"
   description="Popisuje, jak pomocí portálu klasické Azure nebo webové StorSimple virtuální pole uživatelského rozhraní můžete změnit heslo správce zařízení."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>Změna hesla správce zařízení StorSimple virtuální matice

## <a name="overview"></a>Základní informace

Při použití rozhraní prostředí Windows PowerShell pro přístup k virtuální zařízení StorSimple, budete vyzváni k zadání hesla správce zařízení. Při StorSimple zařízení je poprvé spuštěna, je výchozí heslo *hesla1*. Zabezpečení data vypršení platnosti hesla výchozí při prvním přihlášení a je třeba změnit heslo.

Taky můžete místní web uživatelské rozhraní nebo Azure klasické portál kdykoli po nasazení zařízení v provozním prostředí změnit heslo správce zařízení. Každá z těchto postupů je popsán v tomto článku.

## <a name="use-the-azure-classic-portal-to-change-the-password"></a>Pomocí portálu Azure klasické můžete změnit heslo

Provedení následujících kroků můžete změnit heslo správce zařízení portálu Azure klasické.

#### <a name="to-change-the-device-administrator-password-via-the-azure-classic-portal"></a>Chcete-li změnit heslo správce zařízení prostřednictvím portálu Azure klasické

1. Na portálu, klikněte na **zařízení** > **Konfigurace** pro svoje zařízení.

2. Přejděte dolů do části **Heslo správce zařízení** . Zadání hesla správce, který obsahuje z 8 15 znaky. Heslo musí být kombinací velká, malá písmena, číselné a speciální znaky.

3. Potvrďte heslo.

4. Klepněte na tlačítko **Uložit** v dolní části stránky.

Teď je třeba aktualizovat heslo správce zařízení. Použijte tento změněné heslo pro přístup k zařízení místně.

## <a name="use-the-storsimple-virtual-array-web-ui-to-change-the-password"></a>Použití webu virtuální pole StorSimple uživatelského rozhraní pro Změna hesla

Provedení následujících kroků můžete změnit heslo správce zařízení prostřednictvím místní web uživatelského rozhraní.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Chcete-li změnit heslo správce zařízení přes místní web uživatelského rozhraní

1. V místní web uživatelského rozhraní klikněte na **údržbu** > **změnit heslo** pro zařízení.

    ![Změna hesla1](./media/storsimple-ova-change-device-admin-password/image40.png)

2. Zadejte **aktuální heslo**.

3. Zadejte **nové heslo**. Heslo musí obsahovat alespoň na úrovni 8 znaků. Soubor musí obsahovat 3 / 4 z těchto věcí: velká, malá písmena, číselné a speciální znaky.

    Všimněte si, že hesla nemůžou být stejné jako poslední 24 hesla.

3. Zadejte znovu heslo pro potvrzení.

    ![Změna Heslo2](./media/storsimple-ova-change-device-admin-password/image41.png)

4. V dolní části stránky klikněte na **použít**. Potom se použije nové heslo. Změna hesla se nezdaří, zobrazí se tato chyba.

    ![Chyba hesla](./media/storsimple-ova-change-device-admin-password/image42.png)

    Po úspěšném aktualizaci heslo budete upozorněni. Pak můžete toto změněné heslo pro přístup k zařízení místně.

## <a name="next-steps"></a>Další kroky

Další informace o [správě své StorSimple virtuální pole](storsimple-ova-web-ui-admin.md).
