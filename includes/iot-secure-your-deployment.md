# <a name="securing-your-iot-deployment"></a>Zabezpečení nasazení IoT

Tento článek poskytuje další úroveň podrobností pro zabezpečení infrastruktury založené na platformě Azure IoT Internet z věcí (IoT). Odkazuje na úroveň podrobností implementace pro konfiguraci a nasazení každé součásti. Poskytuje také porovnání a volby mezi různými konkurenčními metodami.

Zabezpečení nasazení Azure IoT lze rozdělit do následujících oblastí tři zabezpečení:

- **Zabezpečení zařízení**: zabezpečení IoT zařízení, zatímco je nasazen ve volné přírodě.
- **Zabezpečení připojení**: zajištění všech dat přenášených mezi zařízení IoT a IoT rozbočovač je důvěrná a manipulací.
- **Zabezpečení cloudu**: poskytnutí prostředků pro zabezpečení dat, zatímco prochází a je uložena v cloudu.

![Tři oblasti zabezpečení][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Zabezpečené ověřování a zřizování zařízení

V sadě IoT Azure IoT zařízení zabezpečuje pomocí následujících dvou metod:

- Tím, že poskytuje jedinečnou identitu klíče (tokeny zabezpečení) pro každé zařízení, které umožňuje zařízením komunikovat s rozbočovačem IoT.

- V zařízení [certifikát X.509] pomocí[ lnk-x509] a soukromý klíč jako prostředek k ověření zařízení IoT rozbočovače. Tato metoda ověřování zajišťuje, že soukromý klíč na zařízení není známa mimo zařízení kdykoli, poskytuje vyšší úroveň zabezpečení.

Metoda token zabezpečení poskytuje ověření pro každé volání provedené zařízení IoT rozbočovač přidružením symetrický klíč pro každé volání. Ověřování založené na X.509 umožňuje ověřování zařízení IoT na fyzické vrstvě jako součást navazování připojení TLS. Na základě zabezpečení token metodu lze použít bez ověření X.509, který je méně bezpečný vzorek. Výběr mezi těmito dvěma metodami je určen především způsob zabezpečení zařízení ověřování musí být a dostupnost bezpečné úložiště v zařízení (k bezpečnému ukládání soukromého klíče).

## <a name="iot-hub-security-tokens"></a>Tokeny zabezpečení IoT rozbočovač

IoT rozbočovač používá tokeny zabezpečení k ověřování zařízení a služby, aby se zabránilo odesílání klíčů v síti. Navíc je omezena doba platnosti a rozsahu tokeny zabezpečení. Azure IoT rozbočovač SDK automaticky generovat tokeny bez nutnosti zvláštní konfigurace. Některé scénáře však vyžadují uživatel generování a použití tokenů zabezpečení přímo. Patří sem přímé použití ploch MQTT, AMQP a HTTP nebo implementace vzoru token služby.

Další podrobnosti o struktuře token zabezpečení a jeho použití naleznete v následujících článcích:

-   [Struktura tokenu zabezpečení][lnk-security-tokens]
-   [Pomocí přidružení zabezpečení tokeny jako zařízení][lnk-sas-tokens]

Každý rozbočovač IoT má [Registru identit zařízení] [ lnk-identity-registry] , lze vytvořit prostředky vázané na zařízení v provozu, například frontu obsahující zprávy cloudu zařízení během letu a umožnit přístup ke koncovým bodům směřující zařízení. Registru identity IoT rozbočovač poskytuje bezpečné úložiště identit zařízení a zabezpečení klíčů pro řešení. Jednotlivce nebo skupiny identit zařízení lze povolit seznam nebo seznam blokování umožňuje úplnou kontrolu nad přístupem k zařízení. Následující články poskytují další podrobnosti o struktuře registru identit zařízení a operace podporována.

[IoT rozbočovači podporuje protokoly, jako například MQTT, AMQP a HTTP][lnk-protocols]. Každý z těchto protokolů použít tokeny zabezpečení IoT zařízení IoT rozbočovače odlišně:

- AMQP: OBYČEJNÝ a na základě deklarací AMQP zabezpečení SASL ({policyName}@sas.root.{iothubName} v případě rozbočovač úroveň tokeny; {deviceId} v případě zařízení s rozsahem tokeny).

- MQTT: Připojte paket používá {deviceId} v {ClientId} {IoThubhostname} / {deviceId} v poli **uživatelské jméno** a SAS token do pole **heslo** .

- HTTP: Platný token je v hlavičce požadavku povolení.

IoT rozbočovač zařízení Identity registru lze použít ke konfiguraci pověření zabezpečení na zařízení a řízení přístupu. Však pokud jako řešení IoT již významné investice v [registru a ověřovací schéma identitu vlastní zařízení][lnk-custom-auth], lze ji integrovat do stávající infrastruktury s rozbočovačem IoT vytvořením tokenu služby.

### <a name="x509-certificate-based-device-authentication"></a>Ověřování zařízení pomocí certifikátů X.509

Použití [certifikátu X.509 na zařízení] [ lnk-use-x509] a jeho přidružené veřejné a soukromé dvojice klíčů umožňuje další ověření na fyzické vrstvě. Soukromé klíče jsou bezpečně uloženy v zařízení a není zjistitelný mimo zařízení. Certifikát X.509 obsahuje informace o zařízení, například ID zařízení a další organizační podrobnosti. Podpis certifikátu je generován pomocí soukromého klíče.

Zajišťování toku špičkových zařízení:

- Přiřaďte identifikátor fyzické zařízení – identita zařízení a/nebo certifikát X.509 přidružený k zařízení při zařízení výroby nebo uvádění do provozu.

- V IoT rozbočovač – identita zařízení a zařízení přidružené informace v registru zařízení IoT rozbočovači vytvořte odpovídající položku Identita.

- Kryptografický otisk certifikátu X.509 bezpečně uložte v registru zařízení IoT rozbočovač.

### <a name="root-certificate-on-device"></a>Kořenový certifikát v zařízení

Při vytváření zabezpečené připojení TLS s rozbočovačem IoT, ověřuje IoT zařízení IoT rozbočovači pomocí kořenový certifikát, který je součástí sady SDK zařízení. C klient SDK se certifikát nachází ve složce "\\c\\certifikáty" pod kořenem repo. I když tyto kořenové certifikáty jsou dlouhodobé, stále může nevyprší jejich platnost nebo být odvolán. Neexistuje žádný způsob aktualizace certifikátu na zařízení, zařízení nemusí být následně připojit k rozbočovači IoT (nebo jiné cloudové služby). S prostředky aktualizace kořenového certifikátu po nasazení IoT zařízení bude toto riziko účinně snížit.

## <a name="securing-the-connection"></a>Zabezpečení připojení

Připojení k Internetu zařízení IoT a IoT rozbočovač je zabezpečen pomocí standardní zabezpečení TLS (Transport Layer). Azure IoT podporuje [TLS 1.2][lnk-tls12], TLS 1.1 a TLS 1.0 v tomto pořadí. Podpora protokolu TLS 1.0 je poskytována z důvodu zpětné kompatibility pouze. Je doporučeno použít TLS 1.2, protože poskytuje nejvyšší míru zabezpečení.

Azure IoT Suite podporuje následující šifrovací sada, v tomto pořadí.

| Šifrovací sada | Délka |
|--------------|--------|
| TLS\_ECDHE\_RSA\_s\_AES\_256\_CBC\_SHA384 ECDH secp384r1 (0xc028) (ekvalizéru FS 7680 bitů RSA) | 256    |
| TLS\_ECDHE\_RSA\_s\_AES\_128\_CBC\_SHA256 ECDH secp256r1 (0xc027) (ekvalizéru FS 3072 bitů RSA) | 128    |
| TLS\_ECDHE\_RSA\_s\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (ekvalizéru FS 7680 bitů RSA)           | 256    |
| TLS\_ECDHE\_RSA\_s\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (ekvalizéru FS 3072 bitů RSA)           | 128    |
| TLS\_RSA\_s\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_s\_AES\_128\_GCM\_SHA256 (0x9c) | 128    |
| TLS\_RSA\_s\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_s\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_s\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_s\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_s\_3DES\_EDE\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>Zabezpečení v cloudu

Definice zásad [řízení přístupu] umožňuje Azure IoT rozbočovač[ lnk-protocols] pro každý klíč zabezpečení. Udělení přístupu k jednotlivé koncové body IoT rozbočovač používá následující sadu oprávnění. Oprávnění omezit přístup k rozbočovači IoT založené na funkce.

- **RegistryRead**. Uděluje přístup ke čtení registru identit zařízení. Další informace naleznete v tématu [Identity registru zařízení][lnk-identity-registry].

- **RegistryReadWrite**. Uděluje přístup Číst a zapisovat do registru identit zařízení. Další informace naleznete v tématu [Identity registru zařízení][lnk-identity-registry].

- **ServiceConnect**. Přidělí přístupová komunikace a sledování koncových bodů služby směřující v cloudu. Například udělí oprávnění služby back-end cloudu zařízení cloud zprávy, odesílání zpráv do cloudu zařízení a načíst odpovídající potvrzení o doručení.

- **DeviceConnect**. Uděluje přístup ke koncové body komunikace směřující zařízení. Například udělí oprávnění zařízení cloud zprávy odesílat a přijímat zprávy cloudu zařízení. Toto oprávnění používá zařízení.

Existují dva způsoby, jak získat oprávnění **DeviceConnect** s rozbočovačem IoT s [tokeny zabezpečení][lnk-sas-tokens]: pomocí identity zařízení klíč nebo klíče zásad sdílený přístup. Kromě toho je důležité si uvědomit, že všechny funkce, které jsou přístupné ze zařízení vystavené návrh na koncové body s předponou `/devices/{deviceId}`.

[Součásti služby můžete generovat pouze tokeny zabezpečení] [ lnk-service-tokens] použití sdílených zásad přístupu udělit příslušná oprávnění.

Azure IoT rozbočovače a další služby, které mohou být součástí řešení umožňují správu uživatele služby Azure Active Directory.

Požita rozbočovač IoT Azure data mohou být spotřebovány v různých služeb, jako je Azure Stream Analytics a úložiště objektů blob Azure. Tyto služby umožňují přístup ke správě. Další informace o těchto služeb a k dispozici následující možnosti:

- [Azure DocumentDB][lnk-docdb]: škálovatelné, plně indexované databáze služby částečně strukturovaných dat, která spravuje metadata pro zařízení je zřídit, jako jsou například atributy, konfigurace a vlastnosti zabezpečení. DocumentDB nabízí vysoký výkon a Vysoká propustnost zpracování, bez ohledu na schéma indexování dat a bohaté rozhraní SQL dotazu.

- [Azure Stream Analytics][lnk-asa]: v reálném čase proudu zpracování v cloudu, který umožňuje rychle vyvinout a nasadit řešení analytics levné odkrýt poznatky v reálném čase ze zařízení, senzorů, infrastruktury a aplikací. Data z tohoto plně spravované služby lze škálovat pro svazek při stále dosáhnout vysoké propustnosti, nízké latence a odolnost.

- [Služby Azure App][lnk-appservices]: cloud platform k vytvoření výkonné webové a mobilní aplikace, které se připojují k datům kdekoli; v cloudu nebo místně. Sestavení, vykonávající mobilní aplikací pro iOS, Android a Windows. Integraci se Software jako služba (SaaS) a aplikací v rozlehlé síti s připojením out-of-the-box na desítky cloudové služby a podnikových aplikací. Kód v Oblíbené jazyce a integrovaném vývojovém prostředí (rozhraní .NET, NodeJS, PHP, Python nebo Java) k vytvoření webové aplikace a rozhraní API rychleji než kdy dříve.

- [Logic Apps][lnk-logicapps]: funkce The Logic Apps služby Azure App Service pomáhá integrovat řešení IoT tak, aby vaše stávající řádek obchodní systémy a automatizovat pracovní postupy. Logic Apps umožňuje vývojářům pracovní postupy návrhu, které spustit pomocí aktivační události a pak provést řadu kroků – pravidla a akce, které používají výkonné konektory pro integraci s podnikovými procesy. Logic Apps nabízí připojení out-of-the-box k obrovské ekosystému SaaS, založené na cloudu a místní aplikace.

- [Úložiště objektů blob Azure][lnk-blob]: spolehlivé, hospodárné cloud úložiště pro data, která vaše zařízení odeslat do cloudu.

## <a name="conclusion"></a>Závěr

Tento článek obsahuje přehled provádění úroveň podrobností pro vytvoření a zavedení infrastruktury pomocí Azure IoT IoT. Každá komponenta je zabezpečené konfigurace je klíč zabezpečení celé infrastruktury IoT. Návrh možnosti dostupné v Azure IoT zajištěna alespoň určitá úroveň flexibilitu a volbu; každou volbu však mohou mít vliv na zabezpečení. Je doporučeno každé z těchto možností hodnotit prostřednictvím posouzení rizika/náklady.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
