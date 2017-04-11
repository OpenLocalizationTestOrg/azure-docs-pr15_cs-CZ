<properties
    pageTitle="# Konfigurace registrace automatické zařízení pro zařízení s Windows 7 domény připojen | Microsoft Azure"
    description="Postup pro nastavení vaší domény Windows 7 připojen zařízení se automaticky zaregistrovat s Azure AD. a kroky k nasazení balíčku software registrace zařízení na vaši doménu Windows 7 zařízeních pomocí systém distribuce softwaru například Centrum Správce konfigurace pro System."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="MarkVi"/>

# <a name="configure-automatic-device-registration-for-windows-7-domain-joined-devices"></a>Konfigurace registrace automatické zařízení pro zařízení s Windows 7 domény připojen

Správce IT může konfigurovat zařízení s Windows 7 domény připojen automaticky zaregistrovat s Azure AD. Tak třeba nasazení balíčku software registrace zařízení na vaši doménu Windows 7 připojen zařízeních pomocí systém distribuce softwaru například Centrum Správce konfigurace pro System. Nezapomeňte přečetli a dokončete požadavky uvedené v automatické registrace zařízení s Azure Active Directory pro Windows domény připojen zařízení.

>[AZURE.NOTE]
 Nejnovější pokyny k nastavení registrace automatické zařízení najdete v článku, [jak nastavit automatické registrace Windows domény připojen zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

##<a name="installing-the-device-registration-software-package-on-windows-7-domain-joined-devices"></a>Instalace softwaru balíček registrace zařízení ve Windows 7 domény připojen zařízení

Registrace zařízení ve Windows 7 je k dispozici [ke stažení balíček Instalační službu MSI](https://connect.microsoft.com/site1164). Balíček musí být nainstalovaný na počítačích Windows 7, připojených k doméně služby Active Directory. Nasadíte balíček pomocí systém distribuce softwaru například Centrum Správce konfigurace pro System. Balíček Instalační službu MSI podporuje možnosti standardní pasivní instalace pomocí/quiet parametr.
Balíček softwaru je k dispozici ke stažení na [webu Microsoft připojit](https://connect.microsoft.com/site1164). Tady můžete vybrat a pak budou moct stáhnout pracovišti připojit se pro systém Windows 7.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## <a name="workplace-join-with-azure-active-directory"></a>Pracovišti spojení se službou Azure Active Directory
Registrace zařízení pro zařízení s Windows 7 domény připojen nevyžaduje ani zahrnout uživatelského rozhraní. Po instalaci na počítači všichni uživatelé domény, který zaznamenává do tiskárny bude automaticky a tiše registrován s objektem zařízení v Azure AD. V Azure AD pro každého uživatele registrovaných fyzické zařízení bude jeden objekt zařízení.

Instalační program vytvoří úkol naplánované na systému, který spustí v kontextu uživatele který se spustí při přihlášení uživatele. Úkol tiše zaregistruje uživatele a zařízení s Azure AD poté, co uživatel znaménka se změnami dokončení.
Plánovaných úkolů najdete v knihovně Plánovač úloh v části **Microsoft** > **Připojení pracovišti**.
Úkol spustí a zaregistrovat všechny služby Active Directory uživatele, kteří Přihlaste se k počítači.
Na následujícím obrázku jsou uvedeny Podrobný proces registrace automatické zařízení.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. (Pracovník) přihlášení do systému Windows 7 klientského počítače pomocí přihlašovacích údajů domény Active Directory.
1. Připojit se ke pracovišti naplánovaný úkol probíhá.
1. Uživatel je tiše ověřen se službou AD FS pomocí ověřování systému Windows.
1. Windows 7 PC registraci uživateli v Azure AD.
1. Objekt zařízení a certifikát se vytvoří v Azure AD. Objekt představuje user@device.
1. Certifikát pracovišti připojení uložený v počítači.

## <a name="unregistering-your-windows-7-domain-joined-devices"></a>Registrace domény Windows 7 připojen zařízení

Můžete zvolit unregister zařízení s Windows 7 domény připojen takto: Odinstalace pracovišti připojení balíček z vaší domény Windows 7 připojen zařízeních pomocí systém distribuce softwaru například Centrum Správce konfigurace pro System.

Potom otevřete okno příkazového řádku v počítači Windows 7 a spusťte následující příkaz registraci zařízení:

    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>Tento příkaz musí být v kontextu jednotlivých uživatelů domény, který byl podepsán nastat v počítači.
Prohlížeč událostí a chyb pro systém Windows 7 domény připojen zařízení.

Protokol událostí systému Windows ve Windows 7 počítači se zobrazí zprávy související ke pracovišti. Vyhledání zpráv pro úspěšné a neúspěšné připojení pracovišti zvláštní události. Protokol událostí najdete událostí v prohlížeči protokoly aplikací a služeb > připojit Microsoft pracovišti.

## <a name="additional-topics"></a>Další témata

- [Přehled Azure Active Directory zařízení registrace](active-directory-conditional-access-device-registration-overview.md)
- [Registrace automatické zařízení s Azure Active Directory for Windows Domain-Joined zařízení](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurace registrace automatické zařízení pro zařízení s Windows 8.1 domény připojen](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Registrace automatické zařízení se zařízeními doméně Azure Active Directory pro Windows 10](active-directory-azureadjoin-devices-group-policy.md)
