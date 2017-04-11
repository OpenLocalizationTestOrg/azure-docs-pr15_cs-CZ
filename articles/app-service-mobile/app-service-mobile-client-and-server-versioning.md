<properties
  pageTitle="Klienta a serveru správy verzí SDK v mobilní aplikace a služby Mobile | Azure aplikace služby"
  description="Seznam klientských SDK a kompatibilita verzí SDK serveru pro mobilní služby a Azure mobilní aplikace"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Klienta a serveru správy verzí v mobilní aplikace a služby Mobile

Nejnovější verze služby Azure Mobile je funkce **Mobilní aplikace** služby Azure aplikace.

Mobilní aplikace klienta a serveru SDK původně založené na odkazy v mobilních služeb, ale jsou navzájem *není* kompatibilní.
To znamená je nutné použít *Mobilní aplikace* klienta SDK se serverem *Mobilní aplikace* SDK a podobně služby *Mobile*. Tato smlouva nevynucují prostřednictvím hodnotu zvláštní záhlaví používá klienta a serveru SDK, `ZUMO-API-VERSION`.

Poznámka: pokaždé, když tento dokument odkazuje na back-end *Mobilních služeb* , ho nemusí být nutně být umístěny na mobilních služeb. Nyní je možné k migraci Mobilní služba ke spuštění v aplikaci služby bez jakýchkoli změn kód, ale služba by pořád mohly používat *Mobilní služby* SDK verze.

Další informace o migrace na aplikaci služby bez jakýchkoli změn kód, najdete v článku [migrace Mobilní služba aplikace služby Azure].

## <a name="header-specification"></a>Specifikace záhlaví

Klíč `ZUMO-API-VERSION` může být zadán v záhlaví HTTP nebo řetězce dotazu. Hodnota je řetězec verze ve formuláři **x.y.z**.

Příklad:

ZÍSKÁNÍ https://service.azurewebsites.net/tables/TodoItem

ZÁHLAVÍ: ZUMO ROZHRANÍ API – VERZE: 2.0.0

PŘÍSPĚVEK https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Souhlasu s možností kontroly verze

Můžete se rozhodnout mimo kontroly nastavením vrací hodnotu **true** pro aplikaci nastavení **MS_SkipVersionCheck**verze. Uveďte to ve vaší web.config nebo v části Nastavení aplikace portál Azure.

> [AZURE.NOTE] Existuje celá řada změny v chování mezi službami Mobile a mobilní aplikace, zejména v oblasti offline synchronizace, ověřování a nabízená oznámení. Pouze by měl vyjádření výslovného nesouhlasu verze kontrola po dokončení testování zajistit, že tyto nějaké změny nejsou konce funkce vaše aplikace.

## <a name="summary-of-compatibility-for-all-versions"></a>Přehled kompatibility pro všechny verze

Následující diagram znázorňuje kompatibilita všechny typy klienta a serveru. Back-end je zařadit jako mobilních **služeb** nebo mobilní **aplikace** založené na serveru SDK, která používá.

|                           | **Mobilní služby** Node.js nebo .NET | **Mobilní aplikace** Node.js nebo .NET |
| ----------                | -----------------------             |   ----------------              |
| [Služby mobilní klienty] | Ok                                  | Chyba\*                         |
| [Aplikace mobilní klienty]     | Chyba\*                             | Ok                              |

\*Tuto funkci lze řídit zadáním **MS_SkipVersionCheck**.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobilní služby klienta a serveru

Klient SDK v následující tabulce jsou kompatibilní s **Mobilních služeb**.

Poznámka: klient služby Mobile SDK *co nedělat* odeslat hodnotu záhlaví pro `ZUMO-API-VERSION`. Pokud službu obdrží toto záhlaví nebo hodnotu řetězce dotazu, bude vrácena chyba, pokud jste se výslovně přihlásily oddálit, jak jsme je popsali výše.

### <a name="MobileServicesClients"></a>Mobilní klient *služby* SDK

| Klient platformy                   | Verze                                                                   | Hodnota záhlaví verze |
| -------------------               | ------------------------                                                  | -------------------  |
| Spravované klienta (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | není k dispozici                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | není k dispozici                  |
| Android                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | není k dispozici                  |
| VE FORMÁTU HTML                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | není k dispozici     |

### <a name="mobile-services-server-sdks"></a>Mobilní *Služba* serveru SDK

| Server platformy  | Verze                                                                                                        | Přijaté verze záhlaví |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Verze WindowsAzure.MobileServices.Backend.* 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Bez záhlaví verze** |
| Node.js          | (už brzo)                        | **Bez záhlaví verze** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Chování back-end mobilní služby

| VERZE ZUMO ROZHRANÍ API | Hodnota MS_SkipVersionCheck | Odpověď |
| ---------------- | ---------------------------- | -------- |
| Není zadán    | Všechny                          | 200 - OK |
| Libovolná hodnota        | PRAVDA                         | 200 - OK |
| Libovolná hodnota        | NEPRAVDA nebo není zadán          | 400 - Chybný požadavek |

## <a name="2.0.0"></a>Azure mobilní aplikace klienta a serveru

### <a name="MobileAppsClients"></a>Mobilní *aplikace* klienta SDK

Kontrola verze byla zavedená počínaje následujícími verzemi klienta SDK **Azure mobilních**aplikací:

| Klient platformy                   | Verze                   | Hodnota záhlaví verze |
| -------------------               | ------------------------  | -----------------    |
| Spravované klienta (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobilní *aplikace* serveru SDK

Kontrola verze je součástí sledováním verzí SDK serveru:

| Serverová platforma  | SDK                                                                                                        | Přijaté verze záhlaví |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [Azure mobilní aplikace)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Chování back-end mobilní aplikace

| VERZE ZUMO ROZHRANÍ API | Hodnota MS_SkipVersionCheck | Odpověď |
| ---------------- | ---------------------------- | -------- |
| x.y.z nebo hodnota Null    | PRAVDA                         | 200 - OK |
| Hodnota Null             | NEPRAVDA nebo není zadán          | 400 - Chybný požadavek |
| 1.x.y            | NEPRAVDA nebo není zadán          | 400 - Chybný požadavek |
| 2.0.0-2.x.y      | NEPRAVDA nebo není zadán          | 200 - OK |
| 3.0.0-3.x.y      | NEPRAVDA nebo není zadán          | 400 - Chybný požadavek |


## <a name="next-steps"></a>Další kroky

- [Migrace Mobilní služba Azure aplikace služby]


[Služby mobilní klienty]: #MobileServicesClients
[Aplikace mobilní klienty]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Migrace Mobilní služba Azure aplikace služby]: app-service-mobile-migrating-from-mobile-services.md

