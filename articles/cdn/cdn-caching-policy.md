<properties
    pageTitle="CDN ukládání do mezipaměti zásad v rozšíření služby médií"
    description="Toto téma obsahuje přehled CDN ukládání do mezipaměti zásad v rozšíření Media Services."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN ukládání do mezipaměti zásad v rozšíření služby médií

Azure Media Services poskytuje že HTTP na základě adaptivní přenos a postupné stahování. Nastavit informace HTTP na základě datových proudů je vysoce scalable s výhodami ukládání do mezipaměti proxy a CDN vrstvy stejně jako v mezipaměť klienta. Přenos koncové body obsahuje obecné možnosti datových proudů i konfiguraci záhlaví mezipaměti HTTP. Přenos koncové body nastaví mezipaměti ovládací prvek HTTP: max věku a platnost vyprší záhlaví. Získat další informace o záhlaví mezipaměti HTTP z [adrese W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Výchozí mezipaměť záhlaví

Ve výchozím nastavení platí streamování koncové body 3 den mezipaměti záhlaví na vyžádání streamování data (neúplné skutečné mediální bloky dat) a manifest(playlist). Živé datového proudu streamování koncové body použít 3 den záhlaví mezipaměti dat (neúplné skutečné mediální bloky dat) a 2 sekund mezipaměti záhlaví manifest(playlist) žádosti. Když živou program přejde na vyžádání (živou archiv) streamování mezipaměti záhlaví na vyžádání se použijí.

##<a name="azure-cdn-integration"></a>Azure CDN integrace

Azure Media Services obsahuje [integrované CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) pro přenos koncové body. Záhlaví mezipaměti ovládacích prvků platí stejným způsobem jako datových proudů koncových bodů CDN povolená streamování koncové body. Azure CDN používá streamování koncový bod nakonfigurované mezipaměti hodnot a ty pak definovat životnosti interně mezipaměti objektů a taky používá tuto hodnotu k nastavení doručení mezipaměti záhlaví. Při použití CDN povolit streamování koncové body se nedoporučuje nastavení hodnot malé mezipaměti. Nastavení malé hodnot snížení výkonu a sníží výhodou CDN. Není povoleno nastavení mezipaměti záhlaví menší než 600 sekund CDN povolit streamování koncové body.

>[AZURE.IMPORTANT] Azure Media Services integrace se službou Azure CDN implementovaná v **Azure CDN z Verizon**.  Pokud chcete použít **Azure CDN z Akamai** pro službu Azure Media Services, musíte [nakonfigurovat koncový bod ručně](cdn-create-new-endpoint.md).  Další informace o funkcích Azure CDN najdete v článku [Přehled CDN](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Konfigurace mezipaměti záhlaví s Azure Media Services

Můžete portálu Správa Azure nebo Azure Media Services rozhraní API pro nastavení mezipaměti záhlaví hodnoty.

1. Pro nastavení mezipaměti záhlaví Správa portálu získáte část [jak spravovat Streaming koncové body](../media-services/media-services-portal-manage-streaming-endpoints.md) konfigurace Streaming koncového bodu.
2. Mediální služby Azure rozhraní REST API [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Mediální služby Azure .NET SDK [StreamingEndpointCacheControl vlastnosti](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Mezipaměť konfigurace Priorita pořadí

1. Hodnotou mezipaměť Azure Media Services nakonfigurované přepíše výchozí hodnota.
2. Pokud tam je ruční konfigurace, platí výchozí hodnoty.
3. Výchozí 2 sekund mezipamětí záhlaví slouží k použití s dynamickými streamování manifest(playlist) bez ohledu na Azure Media nebo úložišti Azure konfigurace a přepsání tato hodnota není k dispozici.
