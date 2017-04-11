<properties
    pageTitle="Nastavení a data cestovní nejčastější dotazy týkající se | Microsoft Azure"
    description="Poskytuje odpovědi na některé otázky správce IT může obsahovat informace o nastavení a synchronizace dat aplikace."
    services="active-directory"
    keywords="pole organizace uveďte cestovní nastavení windows cloudu, nejčastější dotazy na cestovní stavu enterprise"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="settings-and-data-roaming-faq"></a>Nastavení a data cestovních časté otázky

V tomto tématu najdete odpovědi na některé dotazy, které může být správce IT a obsahuje informace o nastavení a synchronizace dat aplikace.

## <a name="what-data-roams"></a>Jaká data přesunuje?
**Nastavení systému Windows**: nastavení počítače, které jsou součástí operačního systému Windows. Obvykle to je nastavení, které přizpůsobení počítače a obsahují následující kategorií:

- *Motiv*, který obsahuje funkce, například nastavení motivu a na hlavním panelu na ploše.
- *Nastavení Internet Exploreru*, včetně naposledy otevřené karty a Oblíbené položky.
- *Nastavení okrajů prohlížeče*, například oblíbené položky a seznamy čtení.
- *Hesla*, včetně hesla Internet profily Wi-Fi a ostatní.
- *Jazykové předvolby systému*, který obsahuje nastavení pro rozložení klávesnice, jazyk systému, datum a čas a další.
- *Snadnější přístup k funkcím*, například motivu Vysoký kontrast, Narrator a zapnout funkci Lupa.
- *Další nastavení*, například příkazový řádek nastavení a seznam aplikací.


**Data aplikace**: aplikací univerzální Windows můžete zapisovat nastavení dat cestovní složky a všechna data napsali do této složky budou se automaticky synchronizovat. Je to na jednotlivé aplikace vývojář k návrhu aplikace využít tuto možnost. Další informace o tom, jak vyvíjet univerzální Windows aplikace, která používá cestovní, najdete v článku [appdata úložiště rozhraní API](https://msdn.microsoft.com/library/windows/apps/mt299098.aspx) a [Windows 8 appdata cestovní vývojář blogu](http://blogs.msdn.com/b/windowsappdev/archive/2012/07/17/roaming-your-app-data.aspx).

## <a name="what-account-is-used-for-settings-sync"></a>Jaký účet se používá k nastavení synchronizace?
Ve Windows 8 a Windows 8.1 používat nastavení synchronizace účtů Microsoft příjemce. Podnikoví uživatelé měli možnost připojení účtu Microsoft ke svým účtem služby Active Directory domain získat přístup k nastavení synchronizace. Ve Windows 10 toto připojení účtu Microsoft, že funkce se má nahradit s rámcem primární a sekundární účtu.

Primární účet je definován jako účet použitá k přihlášení k Windows. To může být svým účtem Microsoft, účet služby Azure Active Directory (Azure AD), účet místní služby Active Directory nebo místního účtu. Kromě primární účet uživatelé Windows 10 můžete přiřadit jeden nebo více účtů sekundární cloudu svého zařízení. Sekundární účet je obecně svým účtem Microsoft, účet Azure AD nebo jiný účet například Gmail nebo Facebooku. Tyto sekundární účty umožňují přístup k další služeb, jako je jednotného přihlašování a web Windows Store, ale nejsou může napájení nastavení synchronizace.

Ve Windows 10 se dá použít jenom primární účet pro zařízení pro nastavení synchronizace (viz [Jak můžu upgradovat z synchronizace nastavení účtu Microsoft ve Windows 8 Azure AD sync nastavení ve Windows 10?](active-directory-windows-enterprise-state-roaming-faqs.md#How-do-I-upgrade-from-Microsoft-account-settings-sync-in-Windows-8-to-Azure-AD-settings-sync-in Windows-10?)).

Data je nikdy smíšený mezi různými uživatelskými účty v zařízení. Existují dvě pravidla pro nastavení synchronizace:

- Nastavení Windows bude vždy spolu s primární účet.
- Data aplikace bude označené účet použitý k získání aplikace. Pouze aplikace označený primární účet bude synchronizovat. Přiřazování značek k aplikaci vlastnictví určí se po aplikace, straně načtené přes Windows Store nebo Správa mobilních zařízení (MDM).

Pokud aplikaci pro vlastníka nemohou být identifikovány, bude spolu s primární účet. Pokud zařízení upgradu z Windows 8 nebo Windows 8.1 na Windows 10, budou všechny aplikace příznakem získané pomocí účtu Microsoft. Je to proto většina uživatelů získávat aplikace Windows storu a došlo nepodporuje úložiště Windows Azure AD účty před Windows 10. Pokud aplikace je nainstalovaná prostřednictvím licenci offline, aplikace příznakem pomocí primární účtu na zařízení.

>[AZURE.NOTE]  
> Zařízení Windows 10, které jsou vlastní pole organizace a jste připojení k Azure AD připojovat svých účtů Microsoft už doménovým účtem. Možnost připojení účtu Microsoft k doménovým účtem a máte všechny uživatele synchronizace dat s účtem Microsoft (to znamená účet Microsoft cestovních prostřednictvím připojeného účtu Microsoft a funkce služby Active Directory) se odebere z Windows 10 zařízení připojených k připojení služby Active Directory nebo Azure AD prostředí.

## <a name="how-do-i-upgrade-from-microsoft-account-settings-sync-in-windows-8-to-azure-ad-settings-sync-in-windows-10"></a>Jak můžu upgradovat z synchronizace nastavení účtu Microsoft ve Windows 8 Azure AD sync nastavení ve Windows 10?
Pokud jsou spojeny do domény Active Directory systémem Windows 8 nebo Windows 8.1 s připojeného účtu Microsoft, kterým chcete projekt synchronizovat nastavení prostřednictvím svého účtu Microsoft. Po upgradu na Windows 10, budou dál synchronizovat nastavení uživatele pomocí účtu Microsoft, dokud se jedná o doméně uživatele a domény Active Directory nepřipojí s Azure AD.

Pokud místní domény Active Directory spojit se Azure AD, zařízení se pokusí synchronizovat nastavení pomocí připojený účet Azure AD. Pokud správce Azure AD neumožňuje Enterprise stavu cestovních, vaše připojeného účtu Azure AD bude přestat synchronizovat nastavení. Pokud jste uživatelem Windows 10 a přihlaste identitu Azure AD, začnou synchronizovat nastavení systému windows hned správce umožňuje nastavení synchronizace přes Azure AD.

Pokud uložená osobních údajů na zařízení s podnikové je třeba si uvědomit, že operačního systému Windows a aplikace data začne se synchronizací Azure AD. To má vliv na následující:

- Nastavení osobního účtu Microsoft unášena kromě nastavení na vašem čísle do zaměstnání nebo školní účty Azure AD. Důvodem je, že účet Microsoft a nastavení služby Azure AD synchronizovat teď používáte samostatné účty.
- Osobní údaje, jako jsou hesla Wi-Fi, web pověření a Oblíbené položky aplikace Internet Explorer, dříve synchronizované prostřednictvím připojeného účtu Microsoft budou synchronizovat přes Azure AD.


## <a name="how-do-microsoft-account-and-azure-ad-enterprise-state-roaming-interoperability-work"></a>Jak se účet Microsoft a práce interoperability roamingu stavu Azure AD Enterprise?
V listopadu 2015 nebo novější verze Windows 10 cestovních stavu organizace je podporována pouze pro jeden účet po druhém. Pokud jste přihlášení k Windows pomocí pracovní nebo školní účet Azure AD, všechna data bude synchronizovat přes Azure AD. Pokud jste k Windows přihlášení pomocí osobního účtu Microsoft, bude všechna data synchronizovat přes účet Microsoft. Univerzální appdata bude přecházet pomocí jenom primární přihlašovací účet ze zařízení a bude přecházet jenom v případě, že v aplikaci licenci vlastněná primární účet. Univerzální appdata aplikací vlastněná sekundární účty nebude synchronizovat.

## <a name="do-settings-sync-for-azure-ad-accounts-from-multiple-tenants"></a>Nastavení synchronizace pro účty Azure AD z víc klientů?
Při více Azure AD účtů z různých klientů Azure AD na stejné zařízení, musíte aktualizovat na zařízení registru komunikovat s Azure Rights Management (Azure RMS) pro každého klienta Azure AD.  

1. Vyhledání GUID pro každého klienta Azure AD. Otevřete portál Azure klasické a vyberte Azure AD klienta. Identifikátor GUID klienta je do pole Adresa URL v adresním řádku prohlížeče. Příklad: `https://manage.windowsazure.com/YourAccount.onmicrosoft.com#Workspaces/ActiveDirectoryExtension/Directory/Tenant GUID/directoryQuickStart`
2. Až budete mít identifikátor GUID, bude potřebujete přidat klíče registru **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\SettingSync\WinMSIPC\<klient Identifikátor GUID >**.
Klávesu **klienta Identifikátor GUID** vytvořte nové více hodnotu řetězce (REG SZ více) s názvem **AllowedRMSServerUrls**. Pro její data zadejte licencování rozdělení čárky adresy URL Azure klienti, které má přístup k zařízení.
3. Adresy URL bodu licencování rozdělení můžete najít spuštěním rutiny **Get-AadrmConfiguration** . Pokud hodnoty **LicensingIntranetDistributionPointUrl** a **LicensingExtranetDistributionPointUrl** se liší, zadejte hodnoty. Pokud hodnoty jsou stejné, zadejte hodnotu jenom jednou.


## <a name="what-are-the-roaming-settings-options-for-existing-windows-desktop-applications"></a>Jaké cestovní možnosti nastavení pro existující desktopové aplikace Windows?
Cestovní jde použít pouze pro aplikací univerzální Windows. Jsou dostupné pro povolení roamingu na existující Windows desktopovou aplikaci dvě možnosti:

- [Most plochy](http://aka.ms/desktopbridge) umožňuje přenesení existujících desktopových aplikací Windows univerzální platformu Windows už. Tady budete vyzváni k využít Azure AD aplikace dat cestovní změny minimální kódu. Most plochy poskytuje aplikace k identitě aplikace, které je potřeba povolit aplikaci dat cestovních pro existující aplikace klasické pracovní plochy.
- [Uživatelské prostředí virtualizace (UE V)](https://technet.microsoft.com/library/dn458947.aspx) vám pomůže vytvořit vlastní nastavení šablony pro existující aplikace klasické pracovní plochy Windows a povolení cestovní aplikace typu Win32. Tato možnost není nutné zadávat vývojář aplikace změňte kód aplikace. UE V se omezí na místní služby Active Directory cestovní pro zákazníky, kteří si zakoupili Microsoft Desktop optimalizace Pack.

Mohou správci konfigurovat UE-V přecházet dat desktopové aplikace Windows změnou cestovní nastavení operačního systému Windows a univerzální aplikace dat prostřednictvím [zásad skupiny UE V](https://technet.microsoft.com/itpro/mdop/uev-v2/configuring-ue-v-2x-with-group-policy-objects-both-uevv2), včetně:

- Přecházet zásad skupiny nastavení systému Windows
- Nesynchronizují zásady skupiny aplikace pro Windows
- Cestovní v části aplikace Internet Explorer

Společnost Microsoft může v budoucnosti prošetřit způsoby volání UE V integrováno k Windows a rozšíření UE-V přecházet nastavení prostřednictvím cloudu Azure AD.


## <a name="can-i-store-synced-settings-and-data-on-premises"></a>Můžete ukládat synchronizované nastavení a data místně?
Cestovní stavu Enterprise ukládá všechny synchronizované data v Azure cloudu. UE V nabízí místního cestovní řešení.

## <a name="who-owns-the-data-thats-being-roamed"></a>Kdo vlastní data, která je právě počet?
Vlastní podniky data počet prostřednictvím cestovní stavu organizace. Data se ukládají v Azure datacentra. Musí být zašifrovaný všechna uživatelská data při přenosu šifrovaná i u ostatních v cloudu pomocí službou Azure RMS. Toto je zlepšení ve srovnání s synchronizace nastavení účtu Microsoft, který šifruje jenom některých citlivá data například přihlašovací údaje uživatele před ponechá zařízení.

Společnost Microsoft se zavázala s ochranou data o zákaznících. Uživatel organizaci nastavení dat automaticky tak, že službou Azure RMS před zašifrován ponechá zařízení s Windows 10, takže žádný jiný uživatel může číst tato data. Pokud má vaše organizace na placené předplatné pro službu Azure RMS, můžete pomocí dalších funkcí službou Azure RMS, například sledování a odvolání dokumenty, automaticky ochrana e-mailů, které obsahují citlivé informace a spravovat vlastní klíče ("přenést vlastní klíč" řešení, nazývaný také BYOK). Další informace o těchto funkcích a fungování službou Azure RMS najdete v tématu [Co je Azure Rights Management](https://technet.microsoft.com/jj585026.aspx).

## <a name="can-i-manage-sync-for-a-specific-app-or-setting"></a>Můžete spravovat pro konkrétní aplikaci nebo nastavení synchronizace?
Ve Windows 10 je žádné MDM nebo zásadu skupin nastavení Zakázat cestovní pro jednotlivé aplikace. Správci klientů můžete zakázat synchronizace appdata pro všechny aplikace na spravované zařízení, ale není žádný jemnější řízení na úrovni za aplikace nebo v rámci aplikace.

## <a name="how-can-i-enable-or-disable-roaming"></a>Jak můžete povolit nebo zakázat cestovní?
V **Nastavení** aplikace, přejděte na **účty** > **synchronizovat nastavení**. Na této stránce najdete v článku účtu, který používá přecházet nastavení a můžete povolit nebo zakázat jednotlivé skupiny nastavení, aby se počet.

## <a name="what-is-microsofts-recommendation-for-enabling-roaming-in-windows-10"></a>Co je doporučení společnosti Microsoft pro povolení cestovních ve Windows 10?
Společnost Microsoft nemá několik různých nastavení cestovních k dispozici, včetně cestovních profilů uživatelů, UE V a Enterprise stavu cestovní řešení.  Společnost Microsoft se zavázala vytvářet investice Enterprise uveďte cestovní v budoucích verzích Windows. Pokud vaše organizace používá není připraveno nebo pohodlnému s přesunutí dat do cloudu, pak doporučujeme použít UE V jako primární cestovní technologii. Pokud vaše organizace vyžaduje cestovní podpory pro existující desktopové aplikace Windows, ale je přes přesunutí do cloudu, doporučujeme používat Enterprise stavu cestovní i UE V. I když jsou velmi podobné technologie UE V a Enterprise stavu cestovní, nejsou vzájemně vylučují. Budou se vzájemně doplňují zajistit, že vaše organizace poskytuje roamingové služby, které uživatelé potřebují.  

Pokud chcete použít cestovní stavu organizace a UE V, platí následující pravidla:

- Cestovní stavu organizace je primární cestovní agent na zařízení. UE V používá doplnit "Win32 mezeru."
- Cestovní nastavení systému Windows a moderní aplikace data UWP UE V by měl vypnutá, když skupině UE-V použití zásad. Tyto jsou už pokrývá cestovní stavu organizace.

## <a name="how-does-enterprise-state-roaming-support-virtual-desktop-infrastructure-vdi"></a>Jak cestovních stavu organizace podporuje Infrastruktura virtuálních (klientských počítačů VDI)?
Cestovní stavu organizace je podporována na Windows 10 klienta skladové jednotky, ale ne na serveru skladové jednotky. Pokud klienta OM je hostovaný ve počítače hypervisoru a vzdáleně přihlásit do virtuálního počítače, bude přecházet vaše data. Pokud více uživatelů sdílet stejnou OS a uživatelé vzdáleně přihlásit k serveru pro úplné počítačem, cestovní nemusí fungovat. Tento scénář na základě relace nepodporuje úředně.


## <a name="what-happens-when-my-organization-purchases-azure-rms-after-using-roaming"></a>Co se stane, když Moje organizace nákupy službou Azure RMS po použití roamingová?
Pokud vaše organizace používá předávání ve Windows 10 se službou Azure RMS omezené použití zkušebního předplatného nákupu na placené předplatné službou Azure RMS nemá žádný vliv na funkce cestovní funkce a změny konfigurace bude třeba váš správce IT.

## <a name="known-issues"></a>Známé problémy

- Pokud zkusíte se přihlásit do zařízení s Windows pomocí čipové karty nebo virtuální čipové karty, nastavení synchronizace přestane fungovat. Budoucí aktualizace Windows 10 může tento problém vyřešit.
- Budete potřebovat červenec kumulativní aktualizace pro Windows 10 (build 10586.494 nebo vyšší) pro synchronizaci pro práci Oblíbené položky aplikace Internet Explorer.
- Data, která se po zamknutí s ochranou informace Windows se nesynchronizuje prostřednictvím cestovní stavu organizace. Kromě toho strojů, které mají ochrana informací systému Windows povolena nebude zaznamenat synchronizace motiv. 
- Za určitých podmínek cestovních stavu organizace může docházet k chybám synchronizace dat případě nakonfigurování Azure Vícefaktorové ověřování.
    - Pokud vaše zařízení konfigurovaná požadovat, aby [Vícefaktorové ověřování](multi-factor-authentication.md) na portálu Azure Active Directory, může selhat synchronizace nastavení při přihlášení k zařízení s Windows 10 pomocí hesla. Tento typ Vícefaktorové ověřování konfigurace je určená pro ochranu účet Azure správce. Správce uživatelé dál můžou synchronizovat tak, že přihlásíte k jejich zařízení Windows 10 s jejich [Microsoft Passport souvislosti s prací,](active-directory-azureadjoin-passport.md) PIN kód nebo vyplněním Vícefaktorové ověřování při přístupu k jiné Azure služby, jako třeba Office 365.
    - Pokud správce nakonfiguruje Active Directory Federation Services Vícefaktorové ověřování podmíněné přístupu a vyprší jejich platnost přístupový token na zařízení, může docházet chybám synchronizace.  Zajištění přihlášení a odhlášení pomocí PIN kódu [Microsoft Passport souvislosti s prací](active-directory-azureadjoin-passport.md) nebo dokončit Vícefaktorové ověřování při přístupu k jiné Azure služby, jako třeba Office 365.

- -Li do počítače doméně s automatické registrace zařízení Azure Active Directory, může dojít k synchronizaci selhalo: Pokud je počítač mimo dlouhou dobu, a nelze dokončit ověření domény. Tento problém vyřešíte připojení počítače k podnikové síti, tak, že můžete pokračovat v synchronizaci.


## <a name="related-topics"></a>Příbuzná témata
- [Přehled cestovní stavu Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
- [Povolit enterprise stav roamingové služby Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
- [Skupina zásady a nastavení MDM pro nastavení synchronizace](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Odkaz na cestovní nastavení Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
