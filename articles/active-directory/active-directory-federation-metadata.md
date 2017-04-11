<properties
    pageTitle="Metadata federace Azure AD | Microsoft Azure"
    description="Tento článek popisuje federace metadat dokument, který Azure Active Directory publikuje služby, které podporují tokeny Azure Active Directory."
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
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="federation-metadata"></a>Federace metadat

Azure Active Directory (Azure AD) publikuje dokument metadat federace o službách, které nakonfigurován pro tokenů zabezpečení, které Azure AD problémy. Formát federace metadat dokument je popsán v [Web Services federace Language (WS federace) verze 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), které slouží k rozšíření [metadat pro verze 2.0 OASIS zabezpečení výraz SAML (Markup Language)](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Specifické pro klienta a koncové body metadat nezávislé na klienta

Azure AD publikuje specifické pro klienta a nezávislým klienta koncové body.

Koncové body specifické pro klienta jsou sice pro konkrétní klienta. Specifické pro klienta federace metadata obsahují informace o klienta, včetně informací specifických pro klienta Vystavitel a koncový bod. Aplikace, které omezit přístup do jednoho klienta používat koncové body specifické pro klienta.

Koncové body nezávislým klienta zadejte informace, které jsou společná pro všechny Azure AD klienti. Tyto informace se vztahuje k tenantům hostovaná u *login.microsoftonline.com* a sdíleny klienti. Koncové body klienta nezávislým jsou vám doporučené více klienta aplikace, protože nejsou přidružené k určité klienta.

## <a name="federation-metadata-endpoints"></a>Koncové body metadat federace

Azure AD publikuje federace metadata na `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Pro **koncové body specifické pro klienta** `TenantDomainName` může být jeden z následujících typů:

- Název registrované domény z Azure AD klienta, jako například: `contoso.onmicrosoft.com`.
- Neměnný klient ID domény, například `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Pro **koncové body klienta nezávislým** `TenantDomainName` je `common`. Tento dokument obsahuje seznam jenom ty prvky federace Metadata, která jsou společná pro všechny Azure AD klienti, které jsou hostované na login.microsoftonline.com.

Koncový bod specifické pro klienta může být například `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Koncový bod nezávislým klienta je [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Dokument metadat federace můžete zobrazit tak, že zadáte tato adresa URL v prohlížeči.

## <a name="contents-of-federation-metadata"></a>Obsah federace metadat

Následující části jsou uvedeny informace potřebné službami, které používání tokeny vydán Azure AD.

### <a name="entity-id"></a>ID entity.

`EntityDescriptor` Obsahuje prvek `EntityID` atribut. Hodnota `EntityID` atribut představuje vystavitel, tedy služba tokenů zabezpečení (STS), jehož tokenu. Je třeba ověřit vystavitel, když dostanete token.

Následující metadata zobrazí ukázku specifické pro klienta `EntityDescriptor` elementem `EntityID` prvek.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
ID klienta v klientovi nezávislým koncového bodu můžete nahradit svoje ID klienta k vytvoření určitou klienta `EntityID` hodnotu. Výsledné hodnoty budou stejné jako vydavatele tokenu. Strategie umožňuje aplikace více klientovi ověřte Vystavitel u daného klienta.

Následující metadata zobrazí ukázku klienta nezávislým `EntityID` prvek. Dejte pozor, který `{tenant}` je literál, ne zástupný symbol.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Token podepisování certifikátů

Přijaté službu token, který vystavil Azure AD klienta, musí být podpis tokenu ověřeny podpisový klíčem, který je publikovaných v galerii dokument metadat federace. Federace metadata obsahuje veřejné části certifikáty, které klienti použít token podpisu. Jako nezpracovaná bajtů certifikát se zobrazí v `KeyDescriptor` prvek. Podpisový certifikát tokenů je platný pro podepisování pouze v případě hodnoty `use` atribut je `signing`.

Dokument metadat federace zveřejněné Azure AD může obsahovat více podpisový klíčů, například při přípravě Azure AD aktualizovat podpisového certifikátu. Když dokument metadat federace obsahuje více než jeden certifikát, má služba, která je ověření tokeny podporovat certifikátů v dokumentu.

Následující metadata zobrazí ukázku `KeyDescriptor` elementem podpisového klíče.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

`KeyDescriptor` Prvek se zobrazuje na dvou místech v dokumentu federace metadata. v části WS federace konkrétní a SAML specifické oddílu. Certifikáty publikovaných v obou částech budou stejné.

V části WS federace konkrétní by čtečka metadat WS Federation načte certifikáty z `RoleDescriptor` elementem `SecurityTokenServiceType` typu.

Následující metadata zobrazí ukázku `RoleDescriptor` prvek.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

V části SAML specifické by čtečka metadat WS Federation načte osvědčení `IDPSSODescriptor` prvek.

Následující metadata zobrazí ukázku `IDPSSODescriptor` prvek.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Nejsou žádné rozdíly ve formátu specifické pro klienta a nezávislým klienta certifikátů.

### <a name="ws-federation-endpoint-url"></a>Adresa URL koncového bodu WS federace

Federace metadata obsahuje adresu URL, která je Azure AD použití jednotného přihlašování a jeden odhlašovací v protokolu WS Federation. Tento koncový bod se zobrazí v `PassiveRequestorEndpoint` prvek.

Následující metadata zobrazí ukázku `PassiveRequestorEndpoint` prvku specifické pro klienta koncového bodu.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Nezávislé na klienta koncového bodu adresu URL WS Federation se zobrazí v koncového bodu WS Federation v následujícím příkladu.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>Adresa URL koncového bodu SAML Protocol (protokol)

Federace metadata obsahuje adresa URL, která používá Azure AD pro jednotného přihlašování a jeden odhlašovací v protokolu SAML 2.0. Tyto koncové body zobrazují v `IDPSSODescriptor` prvek.

Přihlášení a odhlašovací URL se zobrazí v `SingleSignOnService` a `SingleLogoutService` prvky.

Následující metadata zobrazí ukázku `PassiveResistorEndpoint` koncového bodu specifické pro klienta.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Podobně koncové body pro běžné koncové body protokolů SAML 2.0 jsou publikovaných v galerii metadata federace nezávislým klienta, jak je vidět v následujícím příkladu.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
