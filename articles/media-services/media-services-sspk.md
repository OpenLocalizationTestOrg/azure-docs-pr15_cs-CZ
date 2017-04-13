<properties 
    pageTitle="Licencování vyhlazenými streamování klienta Microsoft® přenos Kit" 
    description="Přečtěte si, jak na licence Microsoft® hladký přenos klienta přenos Kit." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Licencování vyhlazenými streamování klienta Microsoft® přenos Kit

##<a name="overview"></a>Základní informace

Microsoft hladký přenos klienta přenos Kit (**SSPK** pro krátký) je hladký přenos implementace klienta, optimalizovaného výrobci vložený zařízení, kabel a mobilní operátoři, obsahu poskytovatelů, výrobci sluchátko, nezávislí dodavatelé softwaru (tvůrce programů), a řešení poskytovatelů vytvářet produkty a služby pro proudů adaptivní datových proudů ve formátu hladký přenos. SSPK je zařízení, tak i platformu nezávislé implementace hladký přenos klienta, který můžete přenést licence k zařízení a platformu. 

Zahrnuta pod architekturu nejvyšší úrovně, IIS hladký přenos přenos Kit pole je implementace hladký přenos klienta poskytované společností Microsoft a zahrnuje všechny základní logiky pro přehrávání hladký přenos obsahu. To je pak přenést partnery pro konkrétní zařízení nebo platformu implementací odpovídající rozhraní. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Popis

SSPK je licencován podmínek, které nabízejí pracovníků obchodní hodnoty. Odvětví s obsahuje SSPK licenci:

- Hladký přenos přenos Kit zdroje v C++ 
  - implementuje funkce hladký přenos klienta
  - Přidá formát analýza heuristiku vyrovnávací paměť použití logických operátorů, atd.
- Aplikační Playeru rozhraní API 
  - programové rozhraní pro interakci s aplikací media player
- Platformu odběru vrstvy (PAL) rozhraní 
  - programové rozhraní pro interakci s operačním systémem (vláknech, sockets)
- Hardwarové odběru vrstvy HAL rozhraní 
  - programové rozhraní pro interakce s hardwarem A / V dekodéry (dekódování, vykreslování)
- Digitálních práv (DRM) správy rozhraní 
  - programové rozhraní pro zpracování DRM prostřednictvím vrstvy DRM odběru (DAL)
  - Microsoft PlayReady přenos Kit dodávána samostatně ale integruje prostřednictvím rozhraní. Podrobné informace o Microsoft PlayReady Device licencování, klikněte [sem](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- Implementace vzorky 
  - Ukázka PAL implementaci Linux
  - Ukázka HAL implementaci GStreamer

##<a name="licensing-options"></a>Význam možností licencování

Microsoft hladký přenos klienta přenos Kit je k dispozici nabyvatelů licence v části dvou různých licenční smlouvy: jedno pro vývoj hladký přenos produkty klienta ¨dočasné a jiné pro distribuci hladký přenos klienta konečný produktů koncovým uživatelům.
 
- Výrobci čipovou, doplňky systému nebo nezávislí dodavatelé softwaru (tvůrce programů), které vyžadují zdrojového kódu přenos kit se dají ¨dočasné produkty by měl provést Microsoft hladký přenos klienta přenos Kit **Licenci na produkt ¨dočasné** .
- Výrobci zařízení nebo tvůrce programů, kteří potřebují distribuční práva produktů hladký přenos klienta konečný koncovým uživatelům by měl provést Microsoft hladký přenos klienta přenos Kit **Konečný licenci na produkt** .

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft hladký přenos klienta přenos Kit ¨dočasné licence

V části licence Microsoft nabízí hladký přenos klienta přenos sadu potřebné duševního vlastnictví práva k vytvoření a distribuce hladký přenos produkty klienta ¨dočasné jiným nabyvatelům licence hladký přenos klienta přenos Kit zařízení, které distribuce hladký přenos klienta konečných produktů.

####<a name="fee-structure"></a>Struktura poplatků

Jednorázové licenci poplatek 50 000 Kč USA umožňuje přístup k hladký přenos klienta přenos sadě. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft hladký přenos klienta přenos Kit konečný licence

V části licence nabízí Microsoft všechny potřebné duševního vlastnictví a dostanete přitom zajistit hladký přenos produkty klienta ¨dočasné od jiným nabyvatelům licence hladký přenos klienta přenos Kit distribuce společnosti hladký přenos klienta konečných produktů koncovým uživatelům.

####<a name="fee-structure"></a>Struktura poplatků

Hladký přenos konečný produkt klienta nabízených v rámci modelu royalty jako v části:

- 0,10 za zařízení implementaci vyřízeny
- Licencovaní uzavřeny v 50 000 Kč, každý rok
- Není licencovaný pro prvních 10 000 zařízení implementace každý rok 

##<a name="licensing-procedure-and-sspk-access"></a>Přístup pomocí protokolu licencování postup nebo SSPK

Odešlete e-mail [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) pro všechny licencování dotazů.

[Portál SSPK rozdělení](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) bude přístupný pro registrovaných ¨dočasné nabyvatelů licence.

¨Dočasné a konečné SSPK nabyvatelů licence můžete odeslat technické dotazy k [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft hladký přenos nabyvatelů klienta dočasné produktu smlouva licence

- Adroit Business Solutions, Inc.
- Rozšířené správce systému digitální vysílání
- AirTies Kablosuz Iletism Sanayive Dis Ticaret a. s.
- Albis Technologies Ltd.
- Alticast Corporation
- Amazon digitální služeb, Inc.
- Software multimédia AVC Co., Ltd.
- Cavium, Inc.:
- Zakoupení Corporation EchoStar
- Enseo, Inc.:
- Fluendo a.
- HANDAN BroadInfoCom Co., Ltd.
- Infomir GMBH
- Irdeto USA Inc.
- Globální služby BV Svoboda
- MediaTek Inc.
- MStar Co, Ltd
- NINTENDO Co., Ltd.
- OpenTV, Inc.:
- Omezené digitální šafrán
- Elektrické Changhong Sichuan Co., Ltd.
- SoftAtHome
- Sony Corporation
- Tatung technologii Inc.
- TCL Technoly elektronických (Huizhou) Co., Ltd.
- Ve Vestel Elektronik Sanayi Ticaret a. s.
- VisualOn, Inc.:
- ZTE Corporation

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft hladký přenos nabyvatelů klienta výsledný dokument smlouva licence

- Rozšířené správce systému digitální vysílání
- AirTies Kablosuz Iletism Sanayive Dis Ticaret a. s.
- Albis Technologies Ltd.
- Amazon digitální služeb, Inc.
- Technologie AmTRAN Co., Ltd.
- Arcadyan technologii Corporation
- ATMACA ELEKTRONİK SAN. VE TİC. A.Ş
- Omezené vysílání Sky britských
- CastPal technologii Inc. Shenzhen
- Compal elektronických, Inc.
- Digitální Dongguan AV technologii Corporation, Ltd.
- Zakoupení Corporation EchoStar
- Enseo, Inc.:
- Omezené filmy Filmflex
- Fluendo a.
- Omezené inovace Gibsona
- Haier informace Applicantion S.R.L
- HANDAN BroadInfoCom Co., Ltd.
- Homecast Co., Ltd.
- HON Hai přesnost odvětví Co., Ltd.
- Infomir GMBH
- Kaonmedia Co., Ltd.
- KDDI Corporation
- NINTENDO Co., Ltd.
- Přidružení oranžová zabezpečení
- Omezené digitální šafrán
- Sagemcom širokopásmového přidružení zabezpečení
- Shenzhen Coship elektronických CO., Ltd.
- Elektrické Jiuzhou Shenzhen Co., Ltd.
- Shenzhen Skyworth digitální technologie Co., Ltd.
- Elektrické Changhong Sichuan Co., Ltd.
- Průmyslové Corporation Skardin
- SKY Deutschland Fernsehen GmbH & Co. KG
- SmarDTV a.
- SoftAtHome
- Sony Corporation
- Omezené TCL zahraničních Marketing (moři Macao obchodní)
- Technicolor doručení technologií přidružení zabezpečení
- Tongfang globální Ltd.
- Životní Toshiba styl produkty a služby společnosti
- Univerzální Corporation médií /Slovakia/ s.r.o.
- VIZIO, Inc.:
- Wistron Corporation
- ZTE Corporation

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
