<properties
    pageTitle="Odkaz protokol Azure AD SAML | Microsoft Azure"
    description="Tento článek obsahuje přehled jednotného přihlašování a jeden SAML Sign-Out profilů v Azure Active Directory."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/23/2016"
    ms.author="priyamo"/>


# <a name="how-azure-active-directory-uses-the-saml-protocol"></a>Použití protokolu SAML Azure Active Directory

Azure Active Directory (Azure AD) použití SAML 2.0 protokol sloužící k povolit aplikacím svým uživatelům poskytovat jednoho prostředí přihlašování. [Jednotné přihlašování](active-directory-single-sign-on-protocol-reference.md) a [jeden odhlašovací](active-directory-single-sign-out-protocol-reference.md) SAML profil Azure AD vysvětlit, použití SAML výrazy protokoly a vazby ve službě zprostředkovatele identit.

Protokol SAML vyžaduje zprostředkovatele identit (Azure AD) a poskytovatele služeb (aplikace) pro výměnu informací o sami.

Při registraci aplikace s Azure AD vývojář aplikace zaregistruje federace obsahují informace o úkolech s Azure AD. Platí to i **Přesměrovat URI** a **Metadata URI** rohu aplikace.

Azure AD používá **Metadat URI** cloudovou službu k načtení podpisového klíče a odhlášení URI cloudovou službu. Pokud aplikace nepodporuje metadat URI, vývojář musíte kontaktovat podporu Microsoftu kvůli odhlášení URI a podepisování klíče.

Azure Active Directory zpřístupňuje specifické pro klienta a běžných (klienta nezávislé) jeden přihlašování a jeden odhlašovací koncové body. Tyto adresy URL představují s možností zadání umístění – nejsou jenom identifikátory – takže můžete přejít na koncový bod číst metadata.

 - Koncový bod specifické pro klienta je umístěn v `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  <TenantDomainName> Zastupuje registrovaný název domény nebo TenantID GUID Azure AD klienta. Například je federace metadat na klientovi contoso.com: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

- Koncový bod nezávislým klienta je umístěn v `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. V tomto adresa koncového bodu **běžné** místo se zobrazí, název domény klienta nebo ID.

Informace o dokumentech federace Metadata, které publikuje Azure AD najdete v tématu [Federace Metadata](active-directory-federation-metadata.md).
