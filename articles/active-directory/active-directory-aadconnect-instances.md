<properties
    pageTitle="Azure AD Connect: Synchronizace instancí služby | Microsoft Azure"
    description="Tato stránka dokumenty důležité pro Azure AD instance."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Důležité informace pro případy
Azure AD Connect se výskyty world wide Azure AD nejčastěji používají a Office 365. Ale existuje taky další instance a mají různé hodnoty adresy URL a další zvláštní aspekty.

## <a name="microsoft-cloud-germany"></a>Německo cloudu společnosti Microsoft
[Německo cloudu společnosti Microsoft](http://www.microsoft.de/cloud-deutschland) se svrchovaných obláčkem provozuje správců německé data.

Tento cloudu momentálně v náhledu. V mnoha scénáře, které můžete obvykle to sami, jako je třeba ověření domény, musíte udělat pomocí operátoru. Další informace o tom, jak účast v náhledu obraťte se prosím místní zástupce Microsoftu.

Adresy URL otevřete na proxy serveru |
--- |
\*. microsoftonline.de |
\*. windows.net |
+ Seznamy odvolaných certifikátů |

Při přihlašování do adresáře Azure AD je v doméně onmicrosoft.de použít účet.

Funkce momentálně není k dispozici v Německo cloudu společnosti Microsoft:

- Stav připojení Azure AD není k dispozici.
- Automatické aktualizace není k dispozici.
- Heslo zpětným není k dispozici.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure Government cloud
[Microsoft Azure Government cloud](https://azure.microsoft.com/features/gov/) je cloudu pro vládní organizace.

Tento cloudu už nepodporují všesměrové different releases of DirSync. Z sestavení 1.1.180 Azure AD Connect dalšího generování cloudu podporováno. Tento nástroj Generátor používá jen USA základě koncové body a mají různé seznam adres URL otevřete v proxy serveru.

Adresy URL otevřete na proxy serveru |
--- |
\*. microsoftonline.com |
\*. gov.us.microsoftonline.com |
+ Seznamy odvolaných certifikátů |

Azure AD Connect nebude moct automaticky zjišťovat, že adresáři Azure AD umístěné v cloudu pro státní správu. Místo toho musíte provést tyto akce při instalaci Azure AD Connect.

1. Spusťte instalaci Azure AD Connect.
2. Jakmile se zobrazí na první stránku, kde se má přijmout smlouvu EULA, a pokračujte ale ponechání Průvodce instalací spuštěný.
3. Spusťte příkaz regedit a změna klíče registru `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` hodnotě `2`.
4. Přejděte zpátky do Průvodce instalací Azure AD Connect přijmout smlouvu EULA a pokračovat. Během instalace zkontrolujte pomocí **Vlastní konfigurace** Instalační cesta (ne ztvárnění a instalace). Potom pokračujte v instalaci jako obvykle.

Funkce momentálně není k dispozici v cloudu společnosti Microsoft Azure Government:

- Stav připojení Azure AD není k dispozici.
- Automatické aktualizace není k dispozici.
- Heslo zpětným není k dispozici.

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
