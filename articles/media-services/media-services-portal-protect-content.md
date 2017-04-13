<properties 
    pageTitle="Konfigurace zásad ochrana obsahu pomocí portálu Azure | Microsoft Azure" 
    description="Tento článek ukazuje, jak používat portál Azure konfigurace zásad ochranu obsahu. V článku také ukazuje, jak povolit dynamické šifrování majetku." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016"    
    ms.author="juliako"/>

# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Konfigurace zásad ochrana obsahu pomocí portálu Azure

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="overview"></a>Základní informace

Microsoft Azure Media Services (AMS) umožňuje zabezpečená médií od okamžiku, kdy ponechá počítače pomocí úložiště, zpracování a doručení. Media Services umožňuje poskytovat obsah zašifrovaných dynamicky s šifrování AES (Advanced Standard) (pomocí kláves 128bitové šifrování), běžné šifrování (CENC) pomocí PlayReady a/nebo Widevine DRM a Apple FairPlay. 

AMS poskytuje služby pro doručování DRM licence a AES zrušte klávesy se šipkami oprávněnými klienty. Portál Azure umožňuje vytvořit jednu **klávesu/licenci se tak mohli ověřovat zásady** pro všechny typy šifrování.

Tento článek ukazuje, jak nakonfigurovat zásady ochrany obsahu pomocí portálu Azure. V článku také ukazuje, jak použít na svém majetku dynamické šifrování.

> [AZURE.NOTE]  Pokud jste použili portálu Azure klasické vytvořit zásady ochrany zásady nemusí zobrazit v [Azure portálu](https://portal.azure.com/). Však všechny starého zásady stále existuje. Můžete prozkoumat je Azure Media Services .NET SDK nebo nástrojem [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) (zobrazíte zásady klikněte pravým tlačítkem myši na majetek -> Zobrazení -> informace (F4), klepněte na kartu obsahu klíče -> klepněte na klávesu). 
> 
> Pokud chcete šifrovat vaší majetku pomocí nových zásad, konfigurovat pomocí portálu Azure, klikněte na Uložit a znovu použít dynamické šifrování. 

## <a name="start-configuring-content-protection"></a>Zahájení konfigurace ochranu obsahu

Používat portál zahájíte konfigurace ochranu obsahu, globální ke svému účtu AMS, postupujte takto:

1. [Azure portál](https://portal.azure.com/)vyberte svůj účet Azure Media Services.
2. Vyberte **Nastavení** > **ochranu obsahu**.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection001.png)
 

## <a name="keylicense-authorization-policy"></a>Zásady se tak mohli ověřovat klíč/licence

AMS podporuje více možností, jak ověřování uživatelů, kteří vytvářejí klíč nebo licenční požadavky. Zásady obsahu klíčové autorizace musí být nakonfigurované tak, že jste a hradit klientem v pořadí klíč/licence a delived klientovi. Zásady obsahu klíčové autorizace může mít jeden nebo více omezení se tak mohli ověřovat: **otevření** nebo **tokenu** omezení.

Portál Azure umožňuje vytvořit jednu **klávesu/licenci se tak mohli ověřovat zásady** pro všechny typy šifrování.

###<a name="open"></a>Otevřít 

Otevřít omezení znamená, že systému dodá klávesu každému, kdo je žádost o klíčových. Toto omezení může být užitečné pro účely testování. 

### <a name="token"></a>Tokenu

Token s omezeným přístupem zásad musí opatřeny token Vystavitel tak, že Secure tokenu Service (Služba tokenů zabezpečení). Media Services podporuje tokeny jednoduché Web tokeny (SWT) a formátem JSON Web tokenu (JWT). Media Services neposkytuje zabezpečené tokenu služby. Můžete vytvořit vlastní STS nebo využít Microsoft Azure ACS tokenů problému. STS musí být nakonfigurované pro vytvoření token podepsané zadaný deklarace spolu s problém, které jste zadali v konfiguraci tokenu omezení. Služba klíčové doručení Media Services vrátí požadované klíč (nebo licenci) klienta, pokud platí tokenu a deklarací tokenu se shodují můžou být nakonfigurované pro klíč (nebo licence).

Při konfiguraci tokenu omezení zásad, třeba zadat ověřovací primární klíč, Vystavitel a parametry cílové skupiny. Ověření primární klíč obsahuje klíč, který byl podepsán tokenu, které vystavitel služba zabezpečené tokenu problémy tokenu. Cílové skupiny (někdy se jí říká obor) popisuje záměr tokenu nebo zdroje tokenu povolí přístup k. Služby klíčové doručení Media Services ověří, že tyto hodnoty v tokenu odpovídat hodnotám na šabloně.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>Šablona PlayReady práv

Podrobné informace o šabloně práva PlayReady najdete v tématu [Přehled šablony médií služby PlayReady licenci](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Bez trvalý

Pokud konfigurace licence jako dočasnou pouze udržované ve paměti při přehrávač používá licence.  

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Trvalý

Pokud konfigurace licence jako trvalý se uloží do trvalé úložiště na klienta.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Šablona Widevine práv

Podrobné informace o šabloně práva Widevine najdete v tématu [Přehled šablony licenci Widevine](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Základní

Po výběru **základní**, šablona se vytvoří hodnoty všech výchozích hodnot.

### <a name="advanced"></a>Rozšířené možnosti

Podrobný popis o upřesňující nastavení konfigurace Widevine naleznete v tématu [Toto](media-services-widevine-license-template-overview.md) .

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>Konfigurace FairPlay

Povolit FairPlay šifrování, budete muset poskytnout aplikaci certifikát a aplikace tajná klíč (ASK) pomocí příkazu FairPlay konfigurace. Podrobné informace o konfiguraci FairPlay a požadavky najdete v [tomto](media-services-protect-hls-with-fairplay.md) článku.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Použití dynamického šifrování vaší materiálů

Umožní využít výhod dynamické šifrování potřebujete postupujte takto:

- Kódování zdrojový soubor do sadu adaptivní bitrate MP4 souborů.
- Potřebujete alespoň jednu jednotku streamování na vyžádání streamování koncového bodu, ze kterého chcete dodávat obsah. Další informace najdete v tématu [jak měřítko streamování rezervovaná jednotky na vyžádání](media-services-portal-manage-streaming-endpoints.md).

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Vybrat materiály, kterou chcete šifrovat

Pokud chcete zobrazit všechny prostředky, vyberte **Nastavení** > **prostředky**.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Šifrování AES nebo DRM

Po stisknutí **zašifrovat** na aktivum, zobrazí se dvěma možnostmi: **AES** nebo **DRM**. 

#### <a name="aes"></a>AES

AES vymazat šifrovacího klíče budou povoleny ve všech datových proudů protokolů: hladký přenos HLS a MPEG-přerušované čáry.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM

Když kliknete na kartu DRM, se zobrazí různé možnosti zásad ochranu obsahu (které musí nakonfigurovali nyní) + sadu datových proudů protokoly.

- **PlayReady a Widevine pomlčkou MPEG** - dynamicky šifrování MPEG-POMLČKU toku s PlayReady a Widevine DRMs.
- **PlayReady a Widevine MPEG-POMLČKU + FairPlay s HLS** - se dynamicky šifrování se MPEG-POMLČKU toku s PlayReady a Widevine DRMs. Bude taky šifrování HLS proudy s FairPlay.
- **PlayReady jenom s hladký přenos HLS a POMLČKU MPEG** - dynamicky zašifrovat hladký přenos HLS, datové proudy MPEG-POMLČKU s PlayReady DRM.
- **Widevine jenom s pomlčkou MPEG** - dynamicky šifrování se MPEG-POMLČKU s Widevine DRM.
- **FairPlay jenom s HLS** - dynamicky šifrování HLS toku s FairPlay.

Povolit FairPlay šifrování, budete muset poskytnout aplikaci certifikát a aplikace tajná klíč (ASK) pomocí příkazu FairPlay konfigurace nastavení zásuvné ochranu obsahu.

![Ochrana obsahu](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Po výběru šifrovací klepněte na tlačítko **použít**.

##<a name="next-steps"></a>Další kroky

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





