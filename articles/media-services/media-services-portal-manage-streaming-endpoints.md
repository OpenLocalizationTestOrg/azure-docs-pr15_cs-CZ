<properties 
    pageTitle="Správa datových proudů koncové body pomocí portálu Azure | Microsoft Azure" 
    description="Toto téma ukazuje, jak spravovat streamování koncové body pomocí portálu Azure." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    writer="juliako" 
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


#<a name="manage-streaming-endpoints-with-the-azure-portal"></a>Správa datových proudů koncové body pomocí portálu Azure

## <a name="overview"></a>Základní informace

> [AZURE.NOTE] Pro dokončení tohoto kurzu, třeba účet Azure. Podrobnosti najdete v tématu [Bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/). 

V aplikaci Microsoft Azure Media Services představuje **Streamování koncový bod** streamování služba, která přináší obsah přímo do aplikace přehrávače klienta nebo na doručení sítě obsahu (CDN) pro další rozdělení. Bezproblémová integrace Azure CDN obsahuje také Media Services. Odchozí proudu od služby StreamingEndpoint může být živou toku nebo videa na vyžádání materiálů ve vašem účtu Media Services.

Kromě toho můžete určit kapacitu streamování koncového bodu služby pro zpracování potřeb rostoucí šířky pásma úpravou streamování jednotky. Doporučujeme přidělit jednu nebo více jednotek měřítko pro aplikace v pracovním prostředí. Streamování jednotky umožňují vyhrazené výstupní kapacitu, můžete si koupili úsecích 200 MB / a další funkce, která zahrnuje: [dynamické balení](media-services-dynamic-packaging-overview.md), CDN integrace a Upřesnit konfiguraci.

>[AZURE.NOTE]Je pouze faktura po koncový bod datových proudů v stav po spuštění.

Toto téma obsahuje přehled hlavních funkcí, které jsou k dispozici streamování koncových bodů. Téma také ukazuje, jak používat portál Azure ke správě streamování koncové body. Další informace o tom, jak změnit velikost datových proudů koncový bod, najdete v [tomto](media-services-portal-scale-streaming-endpoints.md) tématu.

## <a name="start-managing-streaming-endpoints"></a>Zahájení Správa datových proudů koncové body

Správa datových proudů koncové body pro váš účet zahájíte takto:

1. [Azure portál](https://portal.azure.com/)vyberte svůj účet Azure Media Services.
2. V okně **Nastavení** vyberte **koncové body datových proudů**.

    ![Přenos koncový bod](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##<a name="adddelete-a-streaming-endpoint"></a>Přidání nebo odstranění streamování koncový bod

Přidání nebo odstranění streamování koncový bod pomocí portálu Azure, takto:

1. Pokud chcete přidat streamování koncový bod, klikněte **+ koncového bodu** v horní části stránky. 
2. Odstranit streamování koncový bod, na tlačítko **Odstranit** . 

    Výchozí streamování koncový bod nelze odstranit.
2. Klikněte na tlačítko **Spustit** spusťte streamování koncového bodu.

    ![Přenos koncový bod](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

Ve výchozím nastavení můžete mít až dva streamování koncové body. Pokud potřebujete požádat o další, najdete v článku [kvóty](media-services-quotas-and-limitations.md).
    
##<a id="configure_streaming_endpoints"></a>Konfigurace streamování koncový bod

Přenos koncový bod umožňuje nakonfigurovat následující vlastnosti, když budete mít minimálně 1 jednotku měřítko: 

- Řízení přístupu
- Ovládací prvek mezipaměti
- Křížové zásady přístupu webu

Podrobné informace o těchto vlastností najdete v tématu [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx).

Konfigurace streamování koncový bod následujícím způsobem:

1. Vyberte streamování koncový bod, který chcete konfigurovat.
1. Klikněte na **Nastavení**.
  
Stručný popis pole sleduje.

![Přenos koncový bod](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Zásady maximální mezipaměti: používá ke konfiguraci Životnost mezipaměti pro prostředky podávané množství prostřednictvím tento streamování koncový bod. Pokud žádná hodnota, použije se výchozí hodnota. Výchozí hodnoty můžete definovat taky přímo v Azure úložiště. Pokud Azure CDN aktivované řešení streamování koncový bod, neměli nastavte hodnotu zásad mezipaměti na menší než 600 sekund.  

2. Povolené IP adresy: slouží k určení IP adresy, které by se mohou připojit k publikované streamování koncového bodu. Pokud žádné IP adresy zadané Libovolná IP adresa bude moct připojit. IP adresy můžete zadat jako jednu IP adresu (například "10.0.0.1"), rozsahu IP pomocí IP adresy a masky podsítě CIDR (například "10.0.0.1/22") nebo rozsahu IP pomocí IP adresy a masky tečkovaným desetinné podsítě (například "10.0.0.1(255.255.255.0)').

3. Konfigurace ověřování záhlaví Akamai podpis: slouží k určení konfigurací podpis záhlaví ověřování žádost Akamai servery. Vypršení platnosti je UTC.



##<a id="enable_cdn"></a>Povolení integrace Azure CDN

Můžete zadat postup povolení integrace Azure CDN pro přenos Endpoint (to je vypnutá, ve výchozím nastavení.)

Chcete-li nastavit integrace Azure CDN true (pravda):

- Streamování koncový bod musí mít aspoň jeden streamování jednotku. Pokud chcete později jednotkách na hodnotu 0, musíte nejdřív zakázat integrace CDN. Ve výchozím nastavení při vytváření nové datové proudy jednu jednotku streamování koncový bod automaticky nastavenou.

- Streamování koncový bod musí být ve přestal stavu. Až CDN získá povolí, můžete začít streamování koncového bodu. 

Může trvat až 90 min pro integraci Azure CDN načíst aktivní.  Trvá až dva hodin změny být aktivní přes všechny CDN POP.

Integrace CDN je povolen v všem centrům pro Azure dat: nám západní nám východ, severní Europe, západní Europe, Japonsko západní, Japonsko východ, jihovýchodní Asie a jihovýchodní Asie.

Po povolení, konfigurace **Řízení přístupu** získá zakázané.

![Přenos koncový bod](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] Azure Media Services integrace se službou Azure CDN implementovaná v **Azure CDN z Verizon**.  Pokud chcete použít **Azure CDN z Akamai** pro službu Azure Media Services, musíte [nakonfigurovat koncový bod ručně](../cdn/cdn-create-new-endpoint.md).  Další informace o funkcích Azure CDN najdete v článku [Přehled CDN](../cdn/cdn-overview.md).

###<a name="additional-considerations"></a>Další informace

- Když CDN aktivované řešení streamování koncový bod, klienty nejde požádat o obsahu přímo z původ. V případě potřeby možnost otestování obsahu pomocí hesla nebo bez CDN můžete vytvořit jiné streamování koncový bod, který není CDN povolené.
- Streamování hostname koncový bod zůstane po povolení CDN. Nemusíte žádné změny pracovní postup služby médií po povolení CDN. Třeba při streamování hostname koncový bod strasbourg.streaming.mediaservices.windows.net, po povolení CDN, se používá přesné stejný název hostitele.
- Pro nové streamování koncové body můžete povolit CDN jednoduše tak, že vytvoříte nový koncový bod; existující streamování koncové body musíte nejdřív zastavte koncového bodu a poté CDN.
 

Další informace naleznete v [oznámení o Azure Media Services integrace se službou Azure CDN (obsahu síť pro doručování)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##<a name="next-steps"></a>Další kroky

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
