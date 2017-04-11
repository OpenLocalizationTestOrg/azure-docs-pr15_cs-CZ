<properties 
    pageTitle="Základní informace o události rozbočovače ověřování a zabezpečení modelu | Microsoft Azure"
    description="Událost rozbočovače ověřování a zabezpečení přehled modelu."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Událost rozbočovače ověřování a zabezpečení přehled modelu

Model zabezpečení události rozbočovače splňuje následující požadavky:

- Pouze zařízení, které vedou k platných přihlašovacích údajů můžete odeslat data k rozbočovači události.
- Zařízení nelze zosobnění jiné zařízení.
- Zařízení s škodlivý můžete zablokováno odesílání dat k rozbočovači události.

## <a name="device-authentication"></a>Ověřování zařízení

Model zabezpečení události rozbočovače vychází z kombinací tokeny [Přidružení sdílené podpisu aplikace Access (zabezpečení)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) a *Vydavatelé události*. Vydavatele události definuje virtuální koncový bod pro rozbočovači události. Vydavatele lze použít pouze k odesílání zpráv rozbočovači události. Není možné přijímat zprávy od vydavatele.

Obvykle rozbočovači události využívá jeden Publisheru na zařízení. Všechny zprávy, které jsou odeslané na libovolný vydavatelů rozbočovači událost jsou byla zařazena do fronty v rámci této události centrální. Vydavatelé: umožňuje povolit řízení přístupu jemně odstupňovaná a omezení.

Každý zařízení se přiřadí jedinečné token, který je nahráli zařízení. Tokeny je vyrobeno tak, aby každý jedinečný token povolí přístup do jiné aplikace publisher jedinečný. Zařízení, které má token můžete odeslat pouze jeden Publisheru, ale žádné jiné aplikace publisher. Pokud několika zařízeních sdílet stejný token, každý z těchto zařízení sdílí vydavatel.

Se nedoporučuje, je možné zařízení zařízení s klíčovými udělit přímý přístup k rozbočovači události. Libovolné zařízení, které slouží k přidržení to tokenu začnete posílat zprávy přímo do této události centrální. Tato zařízení nebudou vyměřené poplatky za jeho omezení. Kromě toho zařízení nelze zakázáno odesílání k této události rozbočovači.

Všechny tokeny přihlášeni klíčem přidružení zabezpečení. Všechny tokeny jsou obvykle podepsané se stejným klíčem. Zařízení nejsou vědět klíče; manufacturing tokeny nebude zařízení.

### <a name="create-the-sas-key"></a>Vytvoření přidružení zabezpečení klíče

Při vytváření názvů rozbočovače události, Azure události rozbočovače vygeneruje klíč přidružení zabezpečení 256 bitů s názvem **RootManageSharedAccessKey**. Tento klíčové podpory poslat, poslech a Správa přístupových práv k oboru. Můžete vytvořit další klávesy. Je vhodné plodiny klíč, že podpory posílat oprávnění k centru zvláštní událost. Pro zbytek v tomto tématu, předpokládá se tento klíč s názvem `EventHubSendKey`.

Následující příklad vytvoří jen Odeslat klíč při vytváření Centru událostí:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Generování tokenů

Si můžete vygenerovat tokeny pomocí klávesy přidružení zabezpečení. Můžete třeba vytvářet jedinou token na zařízení. Tokeny potom bylo možno pomocí následujícího postupu. Všechny tokeny se vytvářejí pomocí klávesy **EventHubSendKey** . Každý token se přiřadí jedinečný identifikátor URI.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Při volání tento způsob, identifikátor URI by měl být zadán jako `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Pro všechny tokeny identifikátor URI je stejné, s výjimkou produktů `PUBLISHER_NAME`, která by měla být odlišná na každý token. V ideálním případě `PUBLISHER_NAME` představuje ID zařízení, která přijímá token.

Tento způsob generuje token následující strukturu:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

Doby vypršení platnosti tokenu je uvedená v sekundách od 1. ledna 1970. Následující obrázek je příkladem token:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Tokeny mají obvykle, životnost, který je podobný nebo překračuje životnost zařízení. Pokud zařízení možnost získat nový token, lze použít tokeny s kratší životnost.

### <a name="devices-sending-data"></a>Zařízení odesílání dat

Po vytvoření tokeny jednotlivých zařízeních zřízení s vlastním jedinečné token.

Pokud zařízení odešle data do rozbočovači události, značky zařízení jeho token s žádostí o odeslat. Nechcete, aby mohl odposlechu a před odcizením tokenu, musí spadat komunikace mezi zařízení a centru události šifrované kanálem.

### <a name="blacklisting-devices"></a>Blokované zařízení

Pokud token krádeže zlými úmysly, mohl zosobnění zařízení, jehož token má krádeže. Adaptivně zařízení vykresluje zařízení nelze použít dokud zařízení je uveden nový token využívající jiné aplikace publisher.

## <a name="authentication-of-back-end-applications"></a>Ověřování aplikace back-end

Ověření back-end aplikace, které spotřebovávat data generovaná zařízení, událostí rozbočovače využívá model zabezpečení, který je podobný modelu, který se používá pro službu Bus témata. Skupina spotř rozbočovače události odpovídá předplatné téma Bus služby. Klient můžete vytvořit skupinu příjemce, pokud žádost o vytvoření skupiny příjemce je spolu s token, že podpory spravovat oprávnění pro centrální události nebo pro názvů, ke kterému patří centru události. Klient bude moct používat data ze skupiny spotř Pokud chcete žádost přijmout se spolu token udělí přijímání práva na danou skupinu spotřebitele, centrální událost nebo názvů, ke kterému patří centru události.

Aktuální verze služby Bus nepodporuje přidružení zabezpečení pravidla pro jednotlivé předplatné. Platí to i pro událost rozbočovače spotř skupiny. Podpora přidružení zabezpečení se přidá obou funkcí v budoucnu.

Při nepřítomnosti ověřování přidružení zabezpečení pro jednotlivé spotř skupiny můžete pomocí kláves přidružení zabezpečení zabezpečit všechny skupiny spotř běžné klíčem. Tento přístup umožňuje aplikaci používat data ze skupiny spotř rozbočovači události.

## <a name="next-steps"></a>Další kroky

Další informace o události rozbočovače, naleznete v následujících tématech:

- [Přehled rozbočovače událostí]
- [Ve frontě zpráv řešení] pomocí služby Bus fronty.
- Kompletní [Ukázková aplikace, který využívá rozbočovače události].

[Přehled rozbočovače událostí]: event-hubs-overview.md
[Ukázka aplikace, která používá rozbočovače události]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[ve frontě zpráv řešení]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
