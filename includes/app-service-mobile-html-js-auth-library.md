###<a name="server-auth"></a>Postup: ověření zprostředkovateli (toku serveru)

Pokud chcete, aby aplikace Mobile spravovat proces ověřování v aplikaci, musíte zaregistrovat aplikace u poskytovatele identity. V aplikaci služby Azure musíte nakonfigurovat ID aplikace a tajná od svého poskytovatele.
Další informace najdete v tématu kurz [přidat ověřování aplikace].

Jakmile jste si zaregistrovali zprostředkovatele identit, jednoduše volejte metodu .login() s názvem svého poskytovatele. Chcete-li například přihlášení pomocí Facebooku pomocí následující kód.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Pokud používáte zprostředkovatelem identit než Facebook, změňte hodnotu předán metodu login nad na jeden z těchto věcí: `microsoftaccount`, `facebook`, `twitter`, `google`, nebo `aad`.

V tomto případě aplikaci služby Azure má na starosti ověřování toku OAuth 2.0 zobrazením na přihlašovací stránku vybraného zprostředkovatele a generování token ověřování aplikace služby po úspěšném přihlášení pomocí poskytovatele identit. Funkce přihlášení, až budete hotovi, vrátí funkce JSON objektu (uživatel), který poskytuje ID uživatele a aplikaci služby ověřovací token v polích uživatelské ID a authenticationToken. Tento token můžete do mezipaměti a znovu použít, dokud vypršením jeho platnosti.

###<a name="client-auth"></a>Postup: ověření zprostředkovateli (toku klienta)

Aplikaci můžete taky nezávisle na sobě kontaktujte poskytovatele identity a potom zadejte vrácené token aplikace služby pro ověřování. Tok tohoto klienta umožňuje uživatelům poskytnout jednoho přihlášení nebo načtení dat dalších uživatelů z zprostředkovatele identit.

#### <a name="social-authentication-basic-example"></a>Sociální příklad základní ověřování

Tento příklad používá klienta služby Facebook SDK pro ověření:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
Tento příklad předpokládá uložení token poskytnutý poskytovatelem odpovídajících SDK tokenu proměnné.

#### <a name="microsoft-account-example"></a>Příklad Account Microsoft

Následující příklad využívá Live SDK, která podporuje jednoduchým jednotného přihlašování pro aplikace pro Windows Store pomocí Account Microsoft:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

V tomto příkladu získá token z Live připojení, která nastavuje aplikace služby volání funkce přihlásit.

###<a name="auth-getinfo"></a>Postup: získání informací o ověřeného uživatele

Informace o ověřování pro aktuálního uživatele můžete načtená z kontingenčního seznamu `/.auth/me` koncový bod jakýmkoli způsobem AJAX.  Ujistěte se, je nastavena `X-ZUMO-AUTH` záhlaví ověřovací token.  Ověřovací token uložený ve `client.currentUser.mobileServiceAuthenticationToken`.  Chcete-li například použijte vzdálené použití rozhraní API:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Vzdálené použití je k dispozici jako balíček npm nebo ke stažení prohlížeče CDNJS. Můžete také použít jQuery nebo jiný AJAX rozhraní API pro načtení informací.  Data se přijatý jako objekt JSON.
