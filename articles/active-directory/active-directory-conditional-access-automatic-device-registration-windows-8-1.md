<properties
    pageTitle="Konfigurace registrace automatické zařízení pro zařízení s Windows 8.1 domény připojen | Microsoft Azure"
    description=" Postup pro nastavení zásad skupiny pro Windows 8.1 doméně zařízení se automaticky zaregistrovat s Azure AD. "
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
    ms.author="Markvi"/>

# <a name="configure-automatic-device-registration-for-windows-81-domain-joined-devices"></a>Konfigurace registrace automatické zařízení pro zařízení s Windows 8.1 domény připojen

Aktivní zásad skupiny adresáře můžete použít pro nastavení domény připojen zařízení Windows 8.1 automaticky zaregistrovat s Azure AD. Konfigurace zásad skupiny, musíte mít alespoň jeden domény spojených Windows serveru 2012 R2 nebo Windows 8.1 počítače s nainstalovanou funkcí Správa zásad skupiny. Po povolení zásad skupiny pro vaši doménu všichni uživatelé domény, který zaznamenává do tiskárny bude automaticky a tiše registrován s objektem zařízení v Azure AD. V Azure AD pro každého uživatele registrovaných fyzické zařízení bude jeden objekt zařízení. Nezapomeňte přečetli a dokončete požadavky z automatické registrace zařízení s Azure Active Directory for Windows Domain-Joined zařízení.

>[AZURE.NOTE]
 Nejnovější pokyny k nastavení registrace automatické zařízení najdete v článku, [jak nastavit automatické registrace Windows domény připojen zařízení s Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).



## <a name="configure-the-group-policy-for-your-windows-81-domain-joined-devices"></a>Konfigurace zásad skupiny pro zařízení s Windows 8.1 domény připojen

1. Otevřete správce serveru a přejděte na **Nástroje** > **Správa zásad skupiny**.
2. Správa zásad skupiny přejděte na uzel domény, který odpovídá domény, ve kterém chcete povolit **Automatické spojení pracovišti**.
3. Klikněte pravým tlačítkem myši **Objekty zásad skupiny** a vyberte **Nový**. Pojmenujte objekt zásad skupiny, například **Automatické připojení pracovišti**. Klikněte na **OK**.
4. Klikněte pravým tlačítkem myši na nový objekt zásad skupiny a pak vyberte **Upravit**.
5. Přejděte na **počítač konfigurace** > **zásady** > **šablony pro správu** > **součásti systému Windows** > **připojení pracovišti**.
6. Klikněte pravým tlačítkem myši automaticky pracovišti připojení ke klientské počítače a pak vyberte **Upravit**.
7. Na přepínač Povolit a potom klikněte na použít. Klikněte na **OK**.
8. Objekt zásad skupiny můžou teď odkaz do umístění podle svého výběru. Chcete-li této zásady pro všechny domény připojen zařízení Windows 8.1 ve vaší organizaci, odkaz na doménu zásad skupiny.

## <a name="unregistering-your-windows-81-domain-joined-devices"></a>Registrace domény Windows 8.1 připojen zařízení

Můžete zvolit unregister zařízení s Windows 8.1 domény připojen takto: Úprava nastavení zásad skupiny připojení pracovišti vytvořený v předchozí části. Nastavení automaticky pracovišti spojení počítačů zásada klienta zakázáno. Nová zařízení to bude zabráníte automaticky připojit na pracovišti.

Zrušení registrace existující počítačích Windows 8.1 domény připojen jedním z následujících dvou možností:

* Možnost 1: **Unregister Windows 8.1 domény připojen zařízení pomocí nastavení počítače**
  1. Zařízení Windows 8.1, přejděte na **Nastavení počítače** > **sítě** > **pracovišti**
  2. Vyberte **opustit**.
Tento proces opakuje pro každý uživatel domény, který má přihlášení do počítače a byla automaticky pracovišti připojen.

* Možnost 2: Unregister spojených zařízení s Windows 8.1 domény, pomocí skriptu
    1. Otevřete okno příkazového řádku v počítači Windows 8.1 a spusťte následující příkaz:` %SystemRoot%\System32\AutoWorkplace.exe leave`
   
Tento příkaz musí být v kontextu jednotlivých uživatelů domény, který byl podepsán nastat v počítači.

##<a name="event-viewer--errors-for-windows-81-domain-joined-devices"></a>Prohlížeč událostí a chyb pro Windows 8.1 domény připojen zařízení

Protokol událostí systému Windows na počítači Windows 8.1 slouží k zobrazení zprávy související s registrace zařízení. Pro úspěšné a neúspěšné události bude hledat zprávy. 

Protokol událostí najdete událostí v prohlížeči aplikací a služeb **protokoly** > **Microsoft** > **Windows > připojit pracovišti**.

##<a name="additional-details"></a>Další informace

Zásady skupiny umožňuje naplánovaný úkol v systému, který spustí v kontextu uživatele který se spustí při přihlášení uživatele. Úkol tiše registrovat uživatele a zařízení s Azure AD po přihlášení dokončení. Naplánovaný úkol se nachází na zařízení Windows 8.1 v knihovně Plánovač úloh v části **Microsoft** > **Windows** > **Připojení pracovišti**. Spustí a registrovat všechny uživatele služby Active Directory, které přihlásit se k úkolu. 

## <a name="additional-topics"></a>Další témata
- [Přehled Azure Active Directory zařízení registrace](active-directory-conditional-access-device-registration-overview.md)
- [Registrace automatické zařízení se zařízeními doméně Azure Active Directory pro Windows 10](active-directory-conditional-access-automatic-device-registration.md)
- [Konfigurace registrace automatické zařízení pro zařízení s Windows 7 domény připojen](active-directory-conditional-access-automatic-device-registration-windows7.md)

