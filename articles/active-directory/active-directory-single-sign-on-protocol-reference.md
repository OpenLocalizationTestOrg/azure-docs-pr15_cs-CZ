<properties
    pageTitle="Azure jednotné přihlašování SAML protokol | Microsoft Azure"
    description="Tento článek popisuje protokol jeden znak na SAML v Azure Active Directory"
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

# <a name="single-sign-on-saml-protocol"></a>Jeden protokol SAML přihlašování

Tento článek popisuje požadavky na ověření SAML 2.0 a odpovědi, které podporuje Azure Active Directory (Azure AD) pro jednotné přihlašování.

Protokol diagramu pod popisuje posloupnost přihlašování. Cloudové služby (poskytovatel služeb) používá přesměrování HTTP vazbu k předání `AuthnRequest` (ověřování) prvek Azure AD (zprostředkovatele identit). Potom Azure AD používá HTTP post vazba publikovat `Response` element ke cloudové služby.

![Jednotné přihlašování pracovního postupu](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Požádat o ověřování uživatelů, cloud services odeslat `AuthnRequest` prvek Azure AD. Ukázka SAML 2.0 `AuthnRequest` může vypadat takto:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Parametr | | Popis |
| ----------------------- | ------------------------------- | --------------- |
| ID | povinné | Azure AD používá k naplnění Tenhle atribut `InResponseTo` atribut vrácené odpověď. ID nesmí být číslice, tak společné strategie se připojte řetězci jako "identifikátor" na řetězcovou reprezentaci identifikátor GUID. Například `id6c1c178c166d486687be4aaf5e482730` je platný ID. |
| Verze | povinné | Je vhodné **2.0**.|
| IssueInstant | povinné | Toto je řetězec data a času UTC hodnotu a [přenosu formátu ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD očekává hodnoty DateTime tohoto typu, ale není vyhodnocení nebo použijte hodnotu z pole. |
| AssertionConsumerServiceUrl | volitelné | Pokud je určené tento název musí odpovídat `RedirectUri` služby cloudu v Azure AD. |
| ForceAuthn | volitelné | Pokud je určené to by měl být false. Všechny ostatní hodnoty způsobí chybu.|
| IsPassive | volitelné | Pokud je určené to by měl být false. Všechny ostatní hodnoty způsobí chybu. |  

Všechny ostatní `AuthnRequest` atributy, jako je souhlas, cíle, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex a ProviderName jsou **ignorovány**.

Azure AD taky ignoruje `Conditions` prvek `AuthnRequest`.

### <a name="issuer"></a>Vydavatel

`Issuer` Prvek `AuthnRequest` se musí přesně shodovat jednu **ServicePrincipalNames** ve službě cloud v Azure AD. Obvykle to nastavenou **Aplikace Identifikátor URI** zadané při registraci aplikace.

Ukázkový SAML úryvek, který obsahuje `Issuer` prvek vypadá nějak takto:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

: Tento element požaduje formátu určitý název ID v odpovědi a vynechán v `AuthnRequest` prvky posílat Azure AD.

Ukázka `NameIdPolicy` elementu vypadá nějak takto:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Pokud `NameIDPolicy` je za předpokladu, že můžete zahrnout jeho volitelné `Format` atribut. `Format` Atribut může mít jenom jeden z následujících hodnot; všechny ostatní hodnoty výsledkem chyba.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory problémy deklaraci NameID jako párový identifikátor.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory problémy deklaraci NameID ve formátu e-mailovou adresu.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Tuto hodnotu umožňuje Azure Active Directory vyberte formát deklarace. Azure Active Directory problémy NameID jako párový identifikátor.

Nezahrnujte `SPNameQualifer` atribut. Azure AD ignoruje `AllowCreate` atribut.

### <a name="requestauthncontext"></a>RequestAuthnContext

`RequestedAuthnContext` Element určuje metody ověřování požadované. Je nepovinný krok v `AuthnRequest` prvky posílat Azure AD. Azure AD podporuje pouze jeden `AuthnContextClassRef` hodnota: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Definování rozsahu

`Scoping` Prvek, který obsahuje seznam Zprostředkovatelé identit jiní, je nepovinný krok v `AuthnRequest` prvky posílat Azure AD.

Pokud je určené nezahrnujte `ProxyCount` atribut, `IDPListOption` nebo `RequesterID` elementu jako nejsou podporované.

### <a name="signature"></a>Podpis

Nezahrnujte `Signature` prvek `AuthnRequest` prvky, protože Azure AD nepodporuje podepsané požadavky na ověření.

### <a name="subject"></a>Předmět

Azure AD ignoruje `Subject` prvek `AuthnRequest` prvky.

## <a name="response"></a>Odpověď

Pokud požadované přihlašování úspěšném dokončení Azure AD příspěvky odpověď cloudové služby. Ukázka odpověď úspěšný pokus přihlašování vypadat takto:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Odpověď

`Response` Prvek obsahuje výsledek žádost o ověření. Azure AD sady `ID`, `Version` a `IssueInstant` hodnoty v `Response` prvek. Nastaví i následujícími atributy:

- `Destination`: Při přihlašování úspěšném dokončení toto nastavení `RedirectUri` poskytovatele služeb (cloudové služby).
- `InResponseTo`: To je nastavený na `ID` atribut `AuthnRequest` prvek, který spustil odpověď.

### <a name="issuer"></a>Vydavatel

Azure AD sady `Issuer` prvek `https://login.microsoftonline.com/<TenantIDGUID>/` kde <TenantIDGUID> je ID klienta Azure AD klienta.

Například odpověď ukázkové elementem Vystavitel může vypadat takto:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Stav

`Status` Vyjadřuje prvek úspěšně nebo neúspěšně přihlašování. Obsahuje `StatusCode` prvek, který obsahuje kód nebo sadu vnořené kódy představující stav. Tento článek obsahuje také `StatusMessage` prvek, který obsahuje vlastní chybové zprávy, které se vytvářejí během přihlašování.

<!-- TODO: Add a authentication protocol error reference -->

Následujícím obrázku je SAML odpověď neúspěšné pokus o přihlášení.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Výraz

Kromě `ID`, `IssueInstant` a `Version`, Azure AD sad následující prvky v `Assertion` prvek odpověď.

#### <a name="issuer"></a>Vydavatel

To je nastavený na `https://sts.windows.net/<TenantIDGUID>/`kde <TenantIDGUID> je ID klienta Azure AD klienta.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Podpis

Azure AD přihlásí výraz v odpovědi na úspěšné přihlašování. `Signature` Prvek obsahuje digitální podpis využívající cloudové služby ověření pramene k ověření integrity výraz.

Získat digitální podpis Azure AD používá podpisový klíč v `IDPSSODescriptor` prvek jeho metadata dokumentu.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Předmět

Určuje objekt zabezpečení, který je předmětem příkazy v výraz. V ní `NameID` prvek, který představuje ověřeného uživatele. `NameID` Hodnotu identifikátor cílových přesměrován pouze, která je na cílovou skupinu pro token poskytovatele služeb. Je trvalý – můžete zrušit, ale nikdy znovu. Je taky neprůhledné, v tomto nezjistí jakékoli okolnosti související uživatele a není možné použít jako identifikátor pro atribut dotazů.

`Method` Atribut `SubjectConfirmation` prvek vždy nastavena na hodnotu `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Podmínky

: Tento element určuje podmínky, které definují přijatelného použití SAML výrazy.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

`NotBefore` a `NotOnOrAfter` atributy určit interval, ve kterém je platný výraz.

- Hodnota `NotBefore` na nebo mírně rovná atribut (menší než druhý) vyšší než hodnota `IssueInstant` atribut `Assertion` prvek. Azure AD není účet pro všechny časový rozdíl mezi sebe sama a cloudovou službu (poskytovatel služeb) a tentokrát nepřidá všechny rezervy.
- Hodnota `NotOnOrAfter` atribut je pozdější než hodnota 70 minut `NotBefore` atribut.

#### <a name="audience"></a>Cílové skupiny

Tato stránka obsahuje identifikátor URI, který identifikuje cílová skupina. Azure AD Nastaví hodnotu: Tento element přínosu `Issuer` element `AuthnRequest` , které iniciuje přihlášení. Chcete zjistit `Audience` hodnoty, použijte hodnotu `App ID URI` , který byl zadán při registraci aplikace.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Podobně jako `Issuer` hodnotu `Audience` hodnota se musí přesně shodovat jednu hlavní názvy služeb, které představuje cloudovou službu v Azure AD. Ale pokud přínosu `Issuer` prvek URI zadaná hodnota není, `Audience` je hodnota v odpovědi `Issuer` hodnotu s předponou `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Tato stránka obsahuje tvrzení o předmětu nebo uživatele. Následující úryvek obsahuje výběru `AttributeStatement` prvek. Na tři tečky označuje, že element může zahrnovat více atributy a hodnoty atributu.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Deklarace názvu** : přínosu `Name` atribut (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`), jako je hlavní jméno uživatele ověřeného uživatele `testuser@managedtenant.com`.
- **Deklarace ObjectIdentifier** : přínosu `ObjectIdentifier` atribut (`http://schemas.microsoft.com/identity/claims/objectidentifier`) je `ObjectId` objektu adresář, který představuje ověřeného uživatele v Azure AD. `ObjectId`je neměnný, jedinečný a opakované použití bezpečí identifikátor ověřeného uživatele.

#### <a name="authnstatement"></a>AuthnStatement

: Tento element uplatňuje, že předmět výraz ověření konkrétní prostředky na určitou dobu.

- `AuthnInstant` Atribut určuje čas, pro niž ověření uživatele s Azure AD.
- `AuthnContext` Element určuje místní ověřování používá k ověření uživatele.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
