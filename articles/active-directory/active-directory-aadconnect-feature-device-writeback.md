<properties
    pageTitle="Azure AD Connect: Povolení zpětného zápisu zařízení | Microsoft Azure"
    description="Tento dokument podrobnosti o tom, jak povolit zpětného zápisu zařízení pomocí Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Povolení zpětného zápisu zařízení

>[AZURE.NOTE] Předplatné Azure AD Premium je potřebný pro zpětného zápisu zařízení.

Následující dokumentaci obsahuje informace o tom, jak povolit funkci zpětného zápisu zařízení v Azure AD Connect. Používá se zpětného zápisu zařízení v následujících situacích:

- Povolení podmíněné přístupu podle zařízení a služby AD FS (2012 R2 nebo vyšší) chráněné aplikací (předávající stran vztahy důvěryhodnosti služby).

To umožňuje větší zabezpečení a zaručit, že udělil přístup k aplikacím pouze důvěryhodných zařízení. Další informace o podmíněném přístupu najdete v tématu [Správa rizika pomocí podmíněného přístupu](active-directory-conditional-access.md) a [Nastavení místní podmíněné připojení přes Azure Active Directory registrace zařízení](https://msdn.microsoft.com/library/azure/dn788908.aspx).

>[AZURE.IMPORTANT]
<li>Zařízení musí být umístěné ve struktuře stejný jako uživatele. Protože zařízení musí být zpět došlo k zápisu jedné stromové struktury, tato funkce aktuálně nepodporuje nasazení s více strukturami uživatele.</li>
<li>Jedinou zařízení registrace konfigurace objektu můžete přidat struktuře místní služby Active Directory. Tato funkce není kompatibilní s topologie místo, kam se synchronizuje na adresářová služba Active Directory s více Azure AD adresářů.</li>

## <a name="part-1-install-azure-ad-connect"></a>Část 1: Instalace Azure AD připojení
1. Instalace Azure AD Connect pomocí vlastní nebo expresní nastavení. Microsoft doporučuje začínat všem uživatelům a skupinám úspěšně synchronizovat dříve než povolíte zpětného zápisu zařízení.

## <a name="part-2-prepare-active-directory"></a>Část 2: Příprava služby Active Directory
Příprava na používání zpětného zápisu zařízení pomocí následujících kroků.

1.  V počítači nainstalovanou Azure AD Connect spusťte v režimu Powershellu.

2.  Pokud není nainstalovaná modulu Active Directory PowerShell a nainstalujte ho pomocí následujícího příkazu:

    `Install-WindowsFeature –Name AD-Domain-Services –IncludeManagementTools`

3. NENÍ-li modul Azure Active Directory PowerShell nainstalováno, pak budou moct stáhnout a nainstalovat z [Modul Azure Active Directory pro Windows PowerShell (64bitová verze)](http://go.microsoft.com/fwlink/p/?linkid=236297). Tato součást obsahuje závislost na Pomocník pro přihlášení, který se instaluje s Azure AD Connect.

4.  Pomocí přihlašovacích údajů správce organizace spusťte následující příkazy a pak ukončete Powershellu.

    `Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}`

Protože jsou potřebné změny názvů konfigurace přihlašovacích údajů správce organizace podporují. Jako správce domény nebude mít dostatečná oprávnění.

![Prostředí PowerShell pro povolení zpětného zápisu zařízení](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)

Popis:

- Pokud už neexistuje, vytvoří a konfiguruje nových kontejnerů a objekty ve skupinovém rámečku CN = konfigurace registrace zařízení, CN = Services, CN = konfigurace, [doménové dn].
- Pokud už neexistuje, vytvoří a konfiguruje nových kontejnerů a objekty ve skupinovém rámečku CN = RegisteredDevices, [domény dn]. V tomto kontejneru se vytvoří objekty zařízení.
- Nastaví potřebná oprávnění Azure AD spojnice účet, pokud chcete spravovat zařízení na služby Active Directory.
- Jenom je potřeba spustit v jedné doménové i v případě, že Azure AD Connect instaluje do více strukturami.

Parametry:

- DomainName: Active Directory Domain místo, kam se bude vytvořena objekty zařízení. Poznámka: Všechny zařízení pro dané doménové služby Active Directory se vytvoří v jedné doméně.
- AdConnectorAccount: Účet služby Active Directory, který bude Azure AD Connect slouží ke správě objekty v adresáři. Toto je účet používaný Azure AD Connect synchronizace pro připojení ke službě AD. Pokud jste si nainstalovali pomocí Expresní nastavení, je účet s předponou MSOL_.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Část 3: Povolit zařízení zpětným v Azure AD Connect
Chcete-li povolit zpětného zápisu zařízení v Azure AD Connect pomocí následujícího postupu.

1.  Znova spusťte Průvodce instalací. **Přizpůsobení možností synchronizace** ze stránky Další úkoly a na tlačítko **Další.**
![Vlastní instalace přizpůsobení možností synchronizace](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2.  Na stránce volitelné funkce zpětného zápisu zařízení se už zobrazit zašedle. Zadejte poznámku, že pokud Azure AD Connect přípravu nejsou kroky zpětným dokončené zařízení k dispozici Oddálit na stránce volitelné funkce. Zaškrtněte políčko u zpětného zápisu zařízení a klikněte na tlačítko **Další**. Pokud na zaškrtávací políčko je vypnutá, přečtěte si článek [Poradce při potížích s oddíl](#the-writeback-checkbox-is-still-disabled).
![Vlastní instalaci volitelných funkcí zpětného zápisu zařízení](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3.  Na stránce zpětným zobrazí zadaný domény jako struktuře zpětným výchozí zařízení.
![Vlastní instalace zařízení zpětným cílové struktury](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4.  Dokončete instalaci průvodce se žádné další změny konfigurace. V případě potřeby odkazují [Vlastní instalace Azure AD Connect.](./connect/active-directory-aadconnect-get-started-custom.md)

## <a name="enable-conditional-access"></a>Povolení podmíněné přístupu
Podrobné pokyny, jak povolit tento scénář jsou k dispozici v rámci [Nastavení místní podmíněné připojení přes Azure Active Directory registrace zařízení](https://msdn.microsoft.com/library/azure/dn788908.aspx).

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Ověření, že zařízení jsou synchronizovány ke službě Active Directory
Zpětného zápisu zařízení by nyní funguje správně. Mějte na paměti, že to trvat až 3 hodin objektů zařízení zapsat zpět AD.  Abyste ověřili, že zařízení jsou synchronizované správně, postupujte takto po dokončení synchronizace pravidla:

1.  Spusťte Centrum pro správu služby Active Directory.
2.  Rozbalte RegisteredDevices v doméně, která je právě federované.
![Centrum pro správu Active Directory registrované zařízení](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)
3.  Aktuální registrovaných zařízení se zobrazí tam.
![Centrum pro správu Active Directory registrované seznam zařízení](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="troubleshooting"></a>Řešení potíží

### <a name="the-writeback-checkbox-is-still-disabled"></a>Zaškrtávací políčko zpětným je pořád zakázaný
Pokud na zaškrtávací políčko pro zpětného zápisu zařízení není povoleno, i když jste postupovali podle výše popsaných kroků, podle těchto kroků vás provede ověřením Průvodce instalací před povolením pole.

Co je první první:

- Ujistěte se, že máte alespoň jeden doménové 2012R2 systému Windows Server. Typ objektu zařízení musí nacházet.
- Pokud už je spuštěn Průvodce instalací, nebude zjištěny žádné změny. V tomto případě dokončete Průvodce instalací a znovu ho spusťte.
- Ujistěte se, že účet, který zadáte v skript inicializace skutečně správné uživatelské používaný konektor služby Active Directory. K ověření, postupujte takto:
    - Z nabídky start otevřete **Služby synchronizace**.
    - Otevřete kartu **spojnice** .
    - Najděte spojnice typu Active Directory Domain Services a vyberte ho.
    - V části **Akce**vyberte **Vlastnosti**.
    - Přejděte na **připojení k doménové Active Directory**. Ověřte, zda domény a uživatelské jméno zadán v tomto Přizpůsobit obrazovce účtu uvedeného skriptu.
![Spojnice účtu ve Správci služby synchronizace](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Ověřte konfiguraci ve službě Active Directory:
- Ověřte, že služba registrace zařízení je umístěn v následujícím umístění (CN = DeviceRegistrationService, CN = služby registrace zařízení, CN konfigurace registrace zařízení, CN = = Services, CN = konfigurace) naming kontextu konfigurace.

![Řešení potíží s DeviceRegistrationService v konfiguraci názvů](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

- Ověřte, že existuje pouze jeden objekt konfigurace tak, že oboru konfigurace vyhledávání. Pokud existuje více než jedno, odstraňte duplicitní.

![Poradce při potížích s, vyhledat duplicitní objekty](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

- Na objekt Služba registrace zařízení zaškrtněte políčko atribut msDS-DeviceLocation je k dispozici a má hodnotu. Vyhledávání toto umístění a ujistěte se, že je k dispozici s msDS-DeviceContainer vztah mezi typem objektu.

![Řešení potíží s msDS DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Poradce při potížích s, RegisteredDevices objekt třídy](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

- Ověření, že účet používaný službou Active Directory Connector má požadovaná oprávnění v kontejneru registrovaná zařízení nalezenou v předchozím kroku. Toto je očekávaná oprávnění pro tento kontejner:

![Poradce při potížích s, ověřte oprávnění v kontejneru](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

- Ověření účtu služby Active Directory má oprávnění k CN = konfigurace registrace zařízení, CN = Services, CN = objekt konfigurace.

![Poradce při potížích s, ověřte oprávnění ke konfiguraci registrace zařízení](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Další informace
- [Správa rizika pomocí podmíněného přístupu](active-directory-conditional-access.md)
- [Nastavovat místní podmíněné připojení přes Azure Active Directory zařízení registrace](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
