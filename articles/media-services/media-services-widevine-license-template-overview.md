<properties 
    pageTitle="Přehled šablon licence Widevine | Microsoft Azure" 
    description="Toto téma obsahuje přehled Widevine licenci šablonu, která používá ke konfiguraci Widevine licence." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="widevine-license-template-overview"></a>Přehled šablon Widevine licence

##<a name="overview"></a>Základní informace

Azure Media Services teď umožňují konfigurovat a požádat o Widevine licence. Pokud přehrávačem koncový uživatel se pokusí přehrávání obsahu Widevine chráněné, žádost o jsou odeslány služba pro doručování licence k získání licence Pokud schválí službu licenci žádost, problémy licenci, která se pošle klienta a mohou sloužit k dešifrování a přehrání zadaného obsahu.

Požadavek na licence Widevine formátu JSON zprávy.  

Všimněte si, že můžete zvolit vytvořit zprávu prázdné neobsahuje žádnou hodnotu jenom "{}" a vytvoří šablony licenci s všechny výchozí hodnoty.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON zprávy

Jméno | Hodnota | Popis
---|---|---
datové části |Řetězec kódovaný ve formátu Base 64 |Požadavek na licence odesílaná klientem. 
content_id | Řetězec kódovaný ve formátu Base 64|Identifikátor používaný k odvozena KeyId(s) a obsahu klíčů pro každou content_key_specs.track_type.
Zprostředkovatel |řetězec |Slouží k vyhledání obsahu klíče a zásady. Povinné.
název_zásad | řetězec |Název zásady dříve registrované. Volitelné
allowed_track_types | výčet  | SD_ONLY nebo SD_HD. Ovládací prvky, které obsah klíče měl by být součástí licence
content_key_specs | pole JSON struktury, najdete v článku **Specifikace obsahu klíč** dole | Lepší nebo barev ovládacího prvku obsahu klávesy se šipkami se vrátíte. Další informace najdete v článku obsahu specifikace klíč pod.  Můžete zadat jenom jednu allowed_track_types a content_key_specs. 
use_policy_overrides_exclusively | Logická hodnota. True nebo false | Použít atributy zásad nastavil policy_overrides a vynechat všechny dříve uložené zásad.
policy_overrides | JSON struktury najdete v tématu **Zásady přepíše** dole | Nastavení zásad pro tuto licenci.  V případě, že tento materiálů má předdefinovaných zásad, použijí se tyto zadané hodnoty. 
session_init | JSON struktury najdete v článku **Inicializace relace** dole | Volitelné data předána licenci.
parse_only | Logická hodnota. True nebo false | Analyzovat požadavek licence, ale Vystavitel žádná licence. Formulář žádosti o licenci vracejí v odpovědi však hodnoty.  

##<a name="content-key-specs"></a>Specifikace obsahu klíče 

Pokud existují stávajících zásad, není potřeba k zadání hodnot do obsahu specifikace klíče.  Stávajících zásady přidružené tento obsah se použijí k určení ochrany výstupu například HDCP a CGMS.  Pokud zásadu stávajících není registrovaný u nepodporovanou Widevine, poskytovatel obsahu vloží hodnoty do žádosti o licenci.   


Každý content_key_specs musí být zadán pro všechny stopy, bez ohledu na možnost use_policy_overrides_exclusively. 


Jméno | Hodnota | Popis
---|---|---
content_key_specs. track_type | řetězec | S názvem typu sledování. Je-li content_key_specs v žádosti o licence, nezapomeňte zadat, že všechny typy sledovat explicitně. To neuděláte bude výsledkem chyba při přehrávání posledních 10 sekund. 
content_key_specs  <br/> security_level | UInt32 | Definuje klienta odolnosti požadavky pro přehrávání. <br/> 1 – softwarové whitebox požadavkům požaduje. <br/> 2 – software požadavkům a zastaralý dekódovací požaduje. <br/> 3 – klíče a šifrování operace je nutné provést zálohované důvěryhodné spouštění prostředí hardwaru. <br/> 4 – požadavkům a dekódování obsahu musí provést zálohované důvěryhodné spouštění prostředí hardwaru.  <br/> 5 – šifrování, dekódovací a všechny manipulace médií (komprimované i nekomprimované) musí být zpracována zálohované důvěryhodné spouštění prostředí hardwaru.  
content_key_specs <br/> required_output_protection.hDC | řetězec - nějaká: HDCP_V2 HDCP_NONE, HDCP_V1, | Označuje, zda je vyžadují HDCP
content_key_specs <br/>klíč | Ve formátu Base 64 <br/>zakódovaný řetězec|Klíč obsahu pro účely tohoto sledování. Pokud je zadaná, je třeba track_type nebo key_id.  Tato možnost umožňuje obsahu poskytovateli vloží klíč obsahu pro tento sledování místo nechat Widevine nepodporovanou Generovat nebo vyhledat klíče.
content_key_specs.key_id| Kódováním Base 64 řetězec binární 16 bajtů | Jedinečný identifikátor klíče. 


##<a name="policy-overrides"></a>Přepsání zásad 

Jméno | Hodnota | Popis
---|---|---
policy_overrides. can_play | Logická hodnota. True nebo false | Určuje povolené přehrávání obsahu. Výchozí hodnota je false.
policy_overrides. can_persist | Logická hodnota. True nebo false |Označuje, že licence může zachován k základnímu úložišti stálé pro použití v offline režimu. Výchozí hodnota je false.
policy_overrides. can_renew | logické hodnoty PRAVDA nebo NEPRAVDA |Označuje, zda je povolen obnovení licence. Pokud je hodnota true, dobu trvání licence můžete prodloužit prezenční signál. Výchozí hodnota je false. 
policy_overrides. license_duration_seconds | Int64 | Označuje časového intervalu pro konkrétní licence. Hodnota 0 označuje, že je neomezené doby trvání. Výchozí hodnota je 0. 
policy_overrides. rental_duration_seconds | Int64 | Během přehrávání je povolen označuje časového intervalu. Hodnota 0 označuje, že je neomezené doby trvání. Výchozí hodnota je 0. 
policy_overrides. playback_duration_seconds | Int64 | Zobrazení okna počet minut po spuštění přehrávání v rámci doby trvání licenci. Hodnota 0 označuje, že je neomezené doby trvání. Výchozí hodnota je 0. 
policy_overrides. renewal_server_url |řetězec | Všechny žádosti o prezenční signál (obnovení) pro tuto licenci se zaměřuje na zadané adrese URL. Toto pole se používá jenom při splnění can_renew.
policy_overrides. renewal_delay_seconds |Int64 |Pokus o kolik sekund po license_start_time, než se pro něj poprvé prodloužení. Toto pole se používá jenom při splnění can_renew. Výchozí hodnota je 0 
policy_overrides. renewal_retry_interval_seconds | Int64 | Určuje zpoždění v sekundách mezi žádostí o prodloužení další licence, když nepovede. Toto pole se používá jenom při splnění can_renew. 
policy_overrides. renewal_recovery_duration_seconds | Int64 | Pokus o, ještě neúspěšné kvůli problémům s nepodporovanou back-end je okno dobu, ve kterém bude moct pokračovat při obnovení přehrávání. Hodnota 0 označuje, že je neomezené doby trvání. Toto pole se používá jenom při splnění can_renew.
policy_overrides. renew_with_usage | logické hodnoty PRAVDA nebo NEPRAVDA |Označuje, že licence se k prodloužení při zahájení používání. Toto pole se používá jenom při splnění can_renew. 

##<a name="session-initialization"></a>Inicializace relace

Jméno | Hodnota | Popis
---|---|---
provider_session_token | Řetězec kódovaný ve formátu Base 64 |Tento token relace předaná zpět licence a bude existovat v dalších prodloužení.  Token relace nebude přetrvávají po uplynutí relace. 
provider_client_token | Řetězec kódovaný ve formátu Base 64 | Klient token odeslání zpět v odpovědi na licence.  Obsahuje-li žádost o licenci token klienta, je tato hodnota ignorována. Token klient bude trvat za licenci relace.
override_provider_client_token | Logická hodnota. True nebo false |Obsahuje-li false a žádost o licenci token klienta, použijte token ze žádosti o i v případě token klienta byla uvedená na tuto strukturu.  Pokud je hodnota true, vždy používejte token podle tuto strukturu.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Konfigurace Widevine licence pomocí typy .NET

Media Services poskytuje rozhraní API .NET, které umožňují konfigurovat Widevine licence. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Třídy podle Media Services .NET SDK

Následují definice typů.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Příklad

Následující příklad ukazuje, jak pomocí rozhraní API .NET pro nastavení jednoduché Widevine licence.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Viz taky

[Pomocí PlayReady a/nebo Widevine dynamické běžné šifrování](media-services-protect-with-drm.md)
