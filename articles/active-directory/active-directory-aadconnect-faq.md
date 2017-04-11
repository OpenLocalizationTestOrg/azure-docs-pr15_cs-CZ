<properties
    pageTitle="Azure AD Connect: Nejčastější dotazy týkající se | Microsoft Azure"
    description="Tato stránka obsahuje nejčastější dotazy k Azure AD Connect."
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
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure AD Connect časté otázky

## <a name="general-installation"></a>Obecné instalace
**Otázka: instalace fungovalo Pokud Azure AD globální správce má 2FA povolené?**  
Pomocí sestavení z února 2016 to je podporovaná.

**Otázka: existuje způsob, jak nainstalovat Azure AD Connect bezobslužného?**  
Je podporována pouze nainstalovat Azure AD Connect pomocí Průvodce instalací. Instalace bezobslužného a pasivní není podporována.

**Otázka: Můžu mít strukturu kde nelze kontaktovat jednu doménu. Jak můžu nainstalovat Azure AD Connect?**  
Pomocí sestavení z února 2016 to je podporovaná.

**Otázka: agent stavu služby AD DS funguje v server core?**  
Ano. Po instalaci je agent, můžete dokončení procesu registrace pomocí následující prostředí PowerShell.: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Sítě
**Otázka: Můžu mít bránu firewall a síťových zařízení nebo něco jiného, která omezuje maximální dobu připojení můžete pořád otevřený v síti. Jak dlouho má Moje klienta straně vypršení časového limitu mezní být při použití Azure AD Connect?**  
Všechny sociální sítě software fyzických zařízení nebo něco jiného, která omezuje maximální dobu, kterou můžete zůstat připojení otevřeny používejte mezní alespoň na úrovni 5 minut (300 sekund) propojení mezi serveru s nainstalovanou Azure AD Connect klienta a Azure Active Directory. To platí pro všechny dřívější nástroje synchronizace Microsoft Identity.

**Otázka: jsou domény SLD (jediný štítek domény) podporované?**  
Ne, Azure AD Connect nepodporuje místní strukturami/domény pomocí domény SLD.

**Otázka: jsou "tečkované" NetBios s názvem podporované?**  
Ne, Azure AD připojit nepodporuje místní strukturami/domény, kde název NetBios obsahuje dobu "." v názvu.

## <a name="federation"></a>Federace
**Otázka: Co mám dělat, když mě budou zanechány e-mailu, který zobrazí výzvu k obnovení certifikát Office 365**  
Použijte pokyny jsou uvedeny informace v tématu [obnovení certifikátů](active-directory-aadconnect-o365-certs.md) o tom, jak prodloužit certifikát.

**Otázka: Můžu mít "Automaticky aktualizovat řídicí strany" nastavení pro O365 řídicí strany. Je potřeba provádět žádnou akci, když Moje token podpisový certifikát automaticky vrátí?**  
Použijte pokyny, které je uvedené v článku [obnovení certifikátů](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Prostředí
**Otázka: jsou podporovány přejmenovat server Azure AD Connect po instalaci?**  
Ne. Změna názvu serveru způsobí modul synchronizace nebude moct připojit k databázi SQL a služby nebudou moct uživatelé zahajovat.

## <a name="identity-data"></a>Identity data
**Otázka: Přípony (userPrincipalName) atribut v Azure AD se neshoduje s místní UPN - proč?**  
Podívejte se na tyto články:

- [Uživatelská jména v Office 365, Azure nebo Intune se neshodují místních hlavních názvů uživatelů nebo alternativní přihlašovací ID](https://support.microsoft.com/en-us/kb/2523192)
- [Změny nejsou synchronizovány nástrojem Azure Active Directory Sync po změně UPN uživatelský účet použít jiné federované domény](https://support.microsoft.com/en-us/kb/2669550)

Můžete taky nakonfigurovat Azure AD umožňuje modul Synchronizace aktualizovat userPrincipalName podle popisu v [Azure AD Connect synchronizační služba funkce](active-directory-aadconnectsyncservice-features.md).

## <a name="custom-configuration"></a>Vlastní konfigurace
**Otázka: kde jsou popsány rutiny prostředí PowerShell pro Azure AD Connect?**  
S výjimkou rutiny uvedených na tomto webu jiné rutinách Powershellu najdete v Azure AD Connect nepodporuje zákazníkům.

* *Otázka: můžu používat "serveru export a import" součástí *Správce služby synchronizace* se můžete pohybovat konfigurace servers? **  
Ne. Tato možnost nebude načíst všechna nastavení konfigurace a není určená k použití. Místo toho měli pomocí průvodce k vytvoření základní konfiguraci na druhém serveru a použití editoru pravidlo synchronizace generovat skriptů Powershellu přesunout všechny vlastní pravidla mezi servery. Najdete v článku [Přesunutí vlastní konfigurace z aktivní pracovní server](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Řešení potíží
**Otázka: Jak můžu získat nápovědu ze služby Azure AD Connect?**

[Znalostní bázi Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Hledání databáze Microsoft Knowledge Base (KB) pro technické řešení běžných problémů konec oprava o podpoře ovládacího Azure AD Connect.

[Fóra pro Microsoft Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- Hledat a vyhledejte pro technické otázky a odpovědi od komunity nebo zeptejte se vlastní kliknutím [sem](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Zákaznická podpora Azure AD Connect](https://manage.windowsazure.com/?getsupport=true)

- Pomocí tohoto odkazu můžete získat prostřednictvím portálu Azure podporu.
