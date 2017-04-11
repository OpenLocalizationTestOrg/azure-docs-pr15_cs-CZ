<properties
    pageTitle="Azure jednoho odhlásit SAML protokol | Microsoft Azure"
    description="Tento článek popisuje jeden protokol SAML Sign-Out v Azure Active Directory"
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


# <a name="single-sign-out-saml-protocol"></a>Jeden odhlašovací SAML Protocol (protokol)

Azure Active Directory (Azure AD) podporuje SAML 2.0 webové prohlížeče jednoho odhlašovací profilu. Pro jeden odhlašovací pracovat správně, Azure AD zaregistrujte jeho adresu URL metadat při registraci aplikace. Azure AD získá adresu URL odhlásit a podpisový klíč cloudovou službu z metadat. Azure AD používá podpisový klíč pro ověření podpis na příchozí LogoutRequest, a LogoutURL přesměrovat uživatelé podepsaném.

Pokud cloudovou službu nepodporuje metadat koncový bod, po registraci aplikace, musí vývojář kontaktujte podporu od Microsoftu kvůli URL odhlásit a podpisového klíče.

Tento diagram znázorňuje pracovního postupu na Azure AD samostatný odhlašovací proces.

![Jeden odhlásit pracovního postupu](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

Odešle cloudové služby `LogoutRequest` zprávu Azure AD vyznačení, že byl ukončen relaci. Následující úryvek ukazuje výběru `LogoutRequest` prvek.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

`LogoutRequest` Prvek poslané na Azure AD vyžaduje následujícími atributy:

- `ID`: Tento označuje odhlašovací žádost. Hodnota `ID` nesmí být číslice. Typický postup je ke znázornění řetězec identifikátor GUID připojit **id** .

- `Version`: Nastavte hodnotu: Tento element **2.0**. Tato hodnota je povinný.

- `IssueInstant`: Jedná se `DateTime` řetězec s koordinaci univerzálním časem (UTC) hodnotu a [přenosu formát ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD očekává hodnotu tohoto typu, ale ne vynutit.

- `Consent`, `Destination`, `NotOnOrAfter` a `Reason` atributy se ignorují, pokud jsou součástí `LogoutRequest` prvek.

### <a name="issuer"></a>Vydavatel

`Issuer` Prvek `LogoutRequest` se musí přesně shodovat jednu **ServicePrincipalNames** ve službě cloud v Azure AD. Obvykle to nastavenou **URI ID aplikace** zadané při registraci aplikace.

### <a name="nameid"></a>NameID

Hodnota `NameID` prvek se musí přesně shodovat `NameID` uživatele, který je právě odhlášeni.
## <a name="logoutresponse"></a>LogoutResponse

Azure AD odešle `LogoutResponse` v odpovědi `LogoutRequest` prvek. Následující úryvek ukazuje výběru `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure AD sady `ID`, `Version` a `IssueInstant` hodnoty v `LogoutResponse` prvek. Nastaví také `InResponseTo` elementu na hodnotu `ID` atribut `LogoutRequest` , jejich odpověď.

### <a name="issuer"></a>Vydavatel

Azure AD Nastaví tuto hodnotu na `https://login.microsoftonline.com/<TenantIdGUID>/` kde <TenantIdGUID> je ID klienta Azure AD klienta.

Chcete zjistit hodnotu `Issuer` prvek, použijte hodnotu z **App Identifikátor URI** uvedenou při registraci aplikace.

### <a name="status"></a>Stav

Použití Azure AD `StatusCode` prvek `Status` prvek, který chcete označit úspěšně nebo neúspěšně odhlašovací. Když odhlašovací nezdaří, `StatusCode` prvek mohou také obsahovat vlastní chybové zprávy.
