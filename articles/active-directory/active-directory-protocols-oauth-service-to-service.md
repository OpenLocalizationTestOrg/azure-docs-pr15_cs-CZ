<properties
    pageTitle="Azure AD služeb Auth pomocí OAuth2.0 | Microsoft Azure"
    description="Tento článek popisuje způsob použití zpráv HTTP implementovat služeb ověřování pomocí udělit toku OAuth2.0 klienta přihlašovací údaje."
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

# <a name="service-to-service-calls-using-client-credentials"></a>Služba pro volání služeb pomocí přihlašovacích údajů klienta

Části OAuth 2.0 klienta pověření udělit Flow umožňuje webové služby ( *důvěrné klienta*) používat vlastní přihlašovací údaje k ověření při volání jiné webové služby, místo zosobnění uživatele. V tomto scénáři klienta je obvykle vícevrstvé webové služby, démon služby nebo webu.

## <a name="client-credentials-grant-flow-diagram"></a>Pověření klienta udělit vývojový diagram

Následující diagram vysvětluje, jak pověření klienta udělit toku funguje v Azure Active Directory (Azure AD).

![OAuth2.0 klienta pověření udělit toku](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. Klientská aplikace ověří koncový bod vydání tokenu Azure AD a žádosti přístupový token.
2. Koncový bod vydání tokenu Azure AD problémy přístupový token.
3. Přístupový token slouží k ověření zabezpečené zdroji.
4. Vrátí data z zabezpečené zdroje webové aplikace.

## <a name="register-the-services-in-azure-ad"></a>Registrace služby v Azure AD

Registrace službu volání a přijímání služby v Azure Active Directory (Azure AD). Podrobné pokyny najdete v tématu [přidávání, aktualizace a odstraňování aplikace](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Žádost o přístupový Token

Požádat o přístupový token, použijte HTTP POST klienta konkrétní Azure AD koncového bodu.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Tokenu žádosti o přístup k služba

Žádost o tokenu služba služby access obsahuje následující parametry.

| Parametr | | Popis |
|-----------|------|------------|
| response_type | povinné | Určuje typ požadovanou odpověď. Hodnota musí být v toku klienta pověření udělit **client_credentials**.|
| client_id | povinné | Určuje id klienta Azure AD volání webové služby. ID volajícího aplikace klienta, na portálu Správa Azure najdete klikněte **Služby Active Directory**, klikněte na adresář, klikněte na aplikace a pak klikněte na **Konfigurovat**.|
| client_secret | povinné |  Zadejte kód registrované pro volání webové služby v Azure AD. Při vytváření klíče, na portálu Správa Azure, klikněte na **Služby Active Directory**, klikněte na adresář, klepněte na aplikaci a klikněte na **Konfigurovat**. |
| zdroje | povinné | Zadejte ID URI aplikace přijímání webové služby. Identifikátor URI ID aplikace na portálu Správa Azure najdete klikněte **Služby Active Directory**, klikněte na adresář, klikněte na aplikace a potom klikněte na **Konfigurovat**. |

## <a name="example"></a>Příklad

Následující HTTP POST žádosti přístupový token https://service.contoso.com/ webové služby. `client_id` Identifikuje webová služba, která vyžádá přístupový token.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Přístup k služba tokenu odpověď

Odpověď na úspěšné obsahuje JSON OAuth 2.0 odpověď s následujícími parametry.

| Parametr   | Popis |
|-------------|-------------|
|access_token |Požadovaná přístupový token. Volání webové služby můžete použít tento token ověření přijímání webové služby. |
|access_type  | Určuje typ tokenu hodnotu. Pouze typ, který podporuje Azure AD je **nosný**. Další informace o nosný tokeny najdete v tématu [OAuth 2.0 se tak mohli ověřovat Framework: nosný Token použití (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Jak dlouho přístupový token je platný (v sekundách).|
|expires_on   |Čas, kdy vyprší platnost přístupový token. Datum představuje počet sekund, než z 1970-01-01T0:0:0Z UTC až do doby vypršení platnosti. Tato hodnota slouží k určení dobu trvání mezipaměti tokeny. |
|zdroje     | Identifikátor URI aplikace ID přijímání webové služby. |

## <a name="example"></a>Příklad

Následující příklad ukazuje úspěch odpověď na žádost o přístupový token webové služby.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Viz taky

* [OAuth 2.0 v Azure AD](active-directory-protocols-oauth-code.md)
