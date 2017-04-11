<properties
    pageTitle="Poradce při potížích s Azure Active Directory access problémy | Microsoft Azure"
    description="Další kroky, které může trvat k řešení problémů přístup ke zdrojům online vaší organizace."
    services="active-directory"
    keywords="registrace zařízení, registrace zařízení a MDM povolíte zařízení přístupu na základě podmíněné, registrace zařízení"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Poradce při potížích s Azure Active Directory access problémy

Vyzkoušejte pro přístup k Sharepointu Online intranet vaší organizace a zobrazí se chybová zpráva "přístup odepřen". Co děláš?

Tento článek popisuje nápravné kroky, které vám mohou pomoci vyřešit problémy týkající se přístupu s online zdroje organizace.

O pomoc s vyřešením Azure Active Directory (Azure AD) potíže s přístupem k, přejděte k části v článku znalostní báze, které bude pokrývat platformu zařízení:

-   Zařízení Windows
-   zařízení s iOS (Kontrola brzy se vraťte pomoc s Iphony a Ipady.)
-   Zařízení s androidem (prohlížejte brzy bude k dispozici pro pomoc s Android telefonech a tabletech s.)

## <a name="access-from-a-windows-device"></a>Aplikace Access ze zařízení s Windows

Pokud vaše zařízení běží jednu z následujících platformách, podívejte se v dalších částech pro chybovou zprávu, která se zobrazí při pokusu o přístup k aplikaci nebo službu:

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7
- Windows Server 2016
- Windows serveru 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Zařízení není registrovaný

Pokud vaše zařízení není registrovaný u Azure AD a aplikace se po zamknutí zásadami na zařízení, může se zobrazit stránky, která obsahuje jednu z těchto chybové zprávy:

!["Můžete nemůže zobrazovat si je z tohoto umístění" zpráv pro registrace zařízení] (./media/active-directory-conditional-access-device-remediation/01.png "Scénář")

Pokud vaše zařízení není doméně služby Active Directory ve vaší organizaci, zkuste tohle:

1.  Ujistěte se, že přihlášení k Windows pomocí svého pracovního účtu (váš účet služby Active Directory).
2.  Připojení k podnikové síti prostřednictvím virtuální privátní sítě (VPN) nebo DirectAccess.
3.  Když jste připojeni, stisknutím klávesy s logem Windows + klávesy L uzamknout relaci systému Windows.
4.  Zadejte svoje přihlašovací údaje účtu práce odemknout relaci systému Windows.
5.  Počkejte chvíli a pak to zkuste znova pro přístup k aplikaci nebo službu.
6.  Pokud se zobrazí na stejné stránce, klikněte na odkaz **Další informace** a potom kontaktujte svého správce s podrobnostmi.

Pokud vaše zařízení není doméně a spuštění Windows 10, máte dvě možnosti:

- Spuštění Azure AD spojení
- Přidejte svůj pracovní nebo školní účet systému Windows

O čem se liší tyto možnosti najdete v článku [zařízení pomocí Windows 10 ve vaší firmě](active-directory-azureadjoin-windows10-devices.md).

Ke spuštění Azure AD připojení, proveďte následující kroky pro platformu zařízení běží. (Azure AD spojení není dostupná na Windows phony.)

**Windows 10 výročí Update**

1.  Otevřete aplikaci **Nastavení** .
2.  Klikněte na **účty** > **přístup k práci nebo ve škole**.
3.  Klikněte na **Připojit**.
4.  Klikněte na **Připojit se ke zařízení Azure AD**.
5.  Ověřování pro vaši organizaci, a poskytují vícefaktorové ověřování, pokud se zobrazí výzva, postupujte podle kroků, které se zobrazí.
6.  Odhlásit se a pak se přihlaste pomocí svého pracovního účtu.
7.  Akci opakujte pro přístup k aplikaci.


**Aktualizace Windows 10 listopad 2015**

1.  Otevřete aplikaci **Nastavení** .
2.  Klikněte na **systém** > **o**.
3.  Klikněte na **Připojit se ke Azure AD**.
4.  Ověřování pro vaši organizaci, a poskytují vícefaktorové ověřování, pokud se zobrazí výzva, postupujte podle kroků, které se zobrazí.
5.  Odhlásit se a pak se přihlaste pomocí svého pracovního účtu (účet Azure AD).
6.  Akci opakujte pro přístup k aplikaci.

Pokud chcete přidat svůj pracovní nebo školní účet, proveďte následující kroky:

**Windows 10 výročí Update**

1.  Otevřete aplikaci **Nastavení** .
2.  Klikněte na **účty** > **přístup k práci nebo ve škole**.
3.  Klikněte na **Připojit**.
4.  Ověřování pro vaši organizaci, a poskytují vícefaktorové ověřování, pokud se zobrazí výzva, postupujte podle kroků, které se zobrazí.
5.  Akci opakujte pro přístup k aplikaci.


**Aktualizace Windows 10 listopad 2015**

1.  Otevřete aplikaci **Nastavení** .
2.  Klikněte na **účty** > **účtů**.
3.  Klikněte na **Přidat pracovní nebo školní účet**.
4.  Ověřování pro vaši organizaci, a poskytují vícefaktorové ověřování, pokud se zobrazí výzva, postupujte podle kroků, které se zobrazí.
5.  Akci opakujte pro přístup k aplikaci.

Pokud vaše zařízení není doméně a spuštění Windows 8.1 na pracovišti připojení a zapsat v Microsoft Intune, proveďte následující kroky:

1.  Otevřete **Nastavení počítače**.
2.  Klikněte na **síť** > **pracovišti**.
3.  Klikněte na **Připojit se**.
4.  Ověřování pro vaši organizaci, a poskytují vícefaktorové ověřování, pokud se zobrazí výzva, postupujte podle kroků, které se zobrazí.
5.  Klikněte na **Zapnout**.
6.  Akci opakujte pro přístup k aplikaci.


### <a name="browser-is-not-supported"></a>Prohlížeč není podporovaný

Může být odepřen přístup Pokud chcete získat přístup k aplikaci nebo službu jedním z těchto prohlížečů:

- Chrome, Firefox nebo jiný prohlížeč než Microsoft Edge nebo Microsoft Internet Explorer ve Windows 10 nebo Windows Server 2016
- Firefox ve Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2

Zobrazí se chybová stránka, která vypadá takto:

![Zpráva "Nelze získat tam odsud" pro nepodporované prohlížeče] (./media/active-directory-conditional-access-device-remediation/02.png "Scénář")

Pouze remediation, je použít prohlížeč podporující aplikace pro svoji platformu zařízení.

## <a name="next-steps"></a>Další kroky

[Azure Active Directory podmíněného přístupu](active-directory-conditional-access.md)
