<properties
    pageTitle="Služba Azure aplikací a jejich dopad na existující služby Azure"
    description="Vysvětluje, jak novou aplikaci služby Azure a jeho funkce mít vliv na existující služby Azure."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure aplikace existující Azure služby a

Tento článek shrnuje změny existující služby Azure jako součást změnit sloučit několik Azure služeb do [Aplikace služby Azure](https://azure.microsoft.com/services/app-service/)nové integrované nabídky.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Základní informace

[Aplikace služby Azure](https://azure.microsoft.com/services/app-service/) je novou a jedinečnou cloudové služby, který umožňuje vývojářům vytvořit web a mobilní aplikace pro jakoukoli platformu a libovolném zařízení. Aplikaci služby je navržen tak, aby zjednodušení opakujících kódovací funkcí, integrace s enterprise a SaaS systémy a automatizace firemních procesů při splnění vašim požadavkům na zabezpečení, spolehlivost a škálovatelnost integrované řešení.

Aplikace služby spojuje následující existující Azure služby – [weby](https://azure.microsoft.com/services/websites/), [Mobilní služby](https://azure.microsoft.com/services/mobile-services/)a [Služby Biztalk](https://azure.microsoft.com/services/biztalk-services/) do jednoho kombinované služby při přidávání výkonné nové funkce.  Služba aplikace umožňuje hostovat těmihle aplikace:

-   Web Apps
-   Mobilní aplikace
-   Rozhraní API aplikace
-   Použití logických operátorů aplikace

Následující tabulka uvádí, jak existující mapování Azure services a aplikaci služby a typy aplikací dostupné v něm obsažené.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Stávající služba Azure</th>
<th align="left", style="width:10%">Azure aplikace služby</th>
<th align="left", style="width:80%">Co se změnilo</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure weby</td>
<td align="left">Web Apps</td>
<td align="left"><li>Weby Azure aplikaci služby je sporná, i když změnit název weby na Web Apps.
<p><li>Všechny existující instance webů, jsou teď Web Apps v aplikaci služby.</p>
<p><li>Přístup k existující weby prostřednictvím <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Portálu Azure</a>, kde bude hledat všechny existující weby v části <em>Web Apps</em>.</p>
<p><li><em>Web, který je hostitelem plánování</em> odteď umožňuje zadávání <em>Plán služeb aplikací</em>. <em>Plánování služby aplikace</em> můžete hostovat libovolný typ aplikace aplikace služby, jako je Web, mobilní telefon, použití logických operátorů nebo rozhraní API aplikace.</p>
<p><li>Azure aplikace služby webových aplikací Web Apps se obecně dostupnosti.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Další informace o webových aplikacích</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure mobilní služby</td>
<td align="left">Mobilní aplikace</td>
<td align="left"><p><li>Mobilní služba dál dostupný jako samostatnou službu a ponechání kurzoru plně podporované.</p>
<p><li>Mobilní aplikace je typ aplikace v aplikaci služby, které integruje všechny funkce služby Mobile a další.</p>
<p><li>Není těžké si <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">migrovat z mobilních služeb aplikací Mobile</a>.</p>
<p><li>Mobilní aplikace získat jako součást aplikace služby nové funkce za mobilní služby, například integrace s místním prostředím a SaaS systémy přípravu sloty WebJobs, možnosti lépe měřítka a další.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Další informace o mobilní aplikace</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">Rozhraní API aplikace</td>
<td align="left">
<p><li>Rozhraní API aplikace je nový typ aplikace v aplikaci služby, které můžete snadno vytvářet a používat rozhraní API v cloudu.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Další informace o rozhraní API aplikace</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Použití logických operátorů aplikace</td>
<td align="left">
<p><li>Logika aplikace je nový typ aplikace v aplikaci služby, které můžete snadno automatizaci obchodních procesů.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Další informace o použití logických operátorů aplikace</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Služby Azure BizTalk</td>
<td align="left">BizTalk rozhraní API aplikace</td>
<td align="left">
<li><p>Služby BizTalk dál dostupný jako samostatnou službu a ponechání kurzoru plně podporované.</p>
<li><p>Všechny funkce služby BizTalk je integrovaný v aplikaci služby jako rozhraní API aplikace umožnit tak uživatelům umožňuje provádět integraci podnikových aplikací a scénáře integrace B2B pomocí kterékoli z app typy v aplikaci služby</p>
<li><p>S aplikacemi jiných logiky můžete nyní automatizace firemních procesů pomocí prostředí vizuálním můžete vytvořit pracovní postupy</p></td>
</tr>
</tbody>
</table>

Další informace, navštivte [si přečtěte následující dokumentaci aplikaci služby](https://azure.microsoft.com/documentation/services/app-service/).
