<properties 
   pageTitle="Změna hesla StorSimple | Microsoft Azure" 
   description="Popisuje, jak používat službu StorSimple správce Změna hesla správce StorSimple snímek správce a zařízení." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="alkohli" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>Změna hesla StorSimple pomocí služby StorSimple správce

## <a name="overview"></a>Základní informace 

Portál Azure klasické stránku **Konfigurovat** obsahuje všechny parametry zařízení, které lze překonfigurovat na StorSimple zařízení, které se spravuje StorSimple Správce služby. Tento kurz vysvětluje, jak můžete na stránce **Konfigurovat** můžete změnit heslo StorSimple snímek správce nebo Správce zařízení.

## <a name="change-the-device-administrator-password"></a>Změna hesla správce zařízení

Při použití rozhraní prostředí Windows PowerShell pro přístup k zařízení StorSimple, budete vyzváni k zadání hesla správce zařízení. Při prvním StorSimple zařízení máte zaregistrované v rámci služby, je výchozí heslo pro rozhraní *hesla1*. Zabezpečení dat je nutné změnit toto heslo na konci procesu registrace. Proces registrace nelze opuštění beze změny toto heslo. Další informace najdete v tématu [Krok 3: nakonfigurovat a zaregistrovat zařízení přes Windows PowerShell pro StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Heslo, které byl nejdřív nastavit pomocí rozhraní prostředí Windows PowerShell při registraci pak bude možné měnit prostřednictvím portálu Azure klasické. Provedení následujících kroků můžete změnit heslo správce zařízení.

#### <a name="to-change-the-device-administrator-password"></a>Chcete-li změnit heslo správce zařízení

1. Na portálu klasické klikněte na **zařízení** > **Konfigurovat** pro svoje zařízení.

2. Přejděte dolů do části **Heslo správce zařízení** . Zadání hesla správce, který obsahuje z 8 15 znaky. Heslo musí být kombinací 3 nebo vyšší velká, malá písmena, číselné a speciální znaky.

3. Potvrďte heslo.

4. Klepněte na tlačítko **Uložit** v dolní části stránky.

Teď je třeba aktualizovat heslo správce zařízení. Použijte tento změněné heslo pro přístup k rozhraní prostředí Windows PowerShell.

## <a name="change-the-storsimple-snapshot-manager-password"></a>Změna hesla správce StorSimple snímku

Software StorSimple snímek Manager je umístěn na hostitele Windows a umožňuje správcům spravovat zálohy StorSimple zařízení ve formuláři místních i cloudových snímky.

Při konfiguraci zařízení ve Správci StorSimple snímku, se výzva k poskytování zařízení IP adresy a hesla pro ověření vaší úložné zařízení. Toto heslo je nejdřív nastavit pomocí rozhraní prostředí Windows PowerShell. Další informace najdete v tématu [Krok 3: nakonfigurovat a zaregistrovat zařízení přes Windows PowerShell pro StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Heslo, které byl nejdřív nastavit pomocí rozhraní prostředí Windows PowerShell při registraci pak bude možné měnit prostřednictvím portálu klasické. Provedení následujících kroků můžete změnit heslo správce StorSimple snímek.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>Chcete-li změnit heslo správce StorSimple snímku

1. Na portálu klasické klikněte na **zařízení** > **Konfigurovat** pro svoje zařízení.

2. Přejděte dolů do části **StorSimple snímek správce** . Zadejte heslo, které je 14 nebo 15 znaků. Ujistěte se, že heslo obsahuje kombinaci 3 nebo vyšší velká, malá písmena, číselné a speciální znaky.

3. Potvrďte heslo.

4. Klepněte na tlačítko **Uložit** v dolní části stránky.

Teď je třeba aktualizovat heslo správce StorSimple snímek.
 

## <a name="next-steps"></a>Další kroky

- Další informace o [StorSimple zabezpečení](storsimple-security.md).

- Další informace o [úpravách konfiguraci zařízení](storsimple-modify-device-config.md).

- Další informace o [použití služby StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
