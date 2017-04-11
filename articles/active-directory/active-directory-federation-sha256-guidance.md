<properties
    pageTitle="Změna podpisu algoritmus hash pro odpovídání stran zabezpečení služeb Office 365 | Microsoft Azure"
    description="Tato stránka obsahuje pokyny ke změně SHA algoritmus zabezpečení federaci s Office 365"
    keywords="SHA1, SHA256, O365, federování aadconnect, služby AD FS, služby ad fs, změnit sha federování zabezpečení, může stran zabezpečení"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Změna algoritmus hash podpisu pro odpovědi stran zabezpečení služeb Office 365

## <a name="overview"></a>Základní informace

Azure Active Directory Federation Services (AD FS) přihlásí jeho tokeny služby Microsoft Azure Active Directory zajistit, aby se nemůže být změněno. Tenhle podpis může být na základě SHA1 nebo SHA256. Azure Active Directory nyní podporuje tokeny podepsané algoritmus SHA256 a doporučujeme nastavení algoritmu podepisování token SHA256 pro nejvyšší úroveň zabezpečení. Tento článek popisuje kroky potřebné k nastavení algoritmu podepisování token bezpečnější SHA256 úrovně.

## <a name="change-the-token-signing-algorithm"></a>Změna algoritmu token podpisu

Po nastavení algoritmus podpisu s dvěma způsoby dole AD FS přihlásí tokeny pro Office 365 může stran zabezpečení s SHA256. Nemusíte proveďte požadované změny navíc konfigurace a tato změna nemá žádný vliv na možnost pro přístup k Office 365 nebo jinými aplikacemi Azure AD.

### <a name="ad-fs-management-console"></a>Konzola pro správu AD FS

1. Spusťte konzolu Správa služby AD FS na primární server služby AD FS.
2. Rozbalte položku službou AD FS a klikněte na **Může stran považuje za důvěryhodnou**.
3. Klikněte pravým tlačítkem myši vaší Office 365/Azure předávající stran zabezpečení a vyberte **Vlastnosti**.
4. Vyberte kartu **Upřesnit** a vyberte zabezpečený algoritmus hash SHA256.
5. Klikněte na **OK**.

![Algoritmus podepisování SHA256 – konzoly MMC](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS Powershellu rutiny

1. Na libovolné server služby AD FS otevřete Powershellu v části oprávnění správce.
2. Nastavte zabezpečený algoritmus hash pomocí rutinu **Set-AdfsRelyingPartyTrust** .

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Přečtěte si taky

* [Oprava zabezpečení služeb Office 365 s Azure AD Connect](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
