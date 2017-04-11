<properties
    pageTitle="Správa a sledování spojnic a rozhraní API aplikace v aplikaci služby | Microsoft Azure"
    description="Zobrazení výkonu spojnic a rozhraní API aplikace v aplikacích pro použití logických operátorů; microservices architektura"
    services="app-service\logic"
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger"
    manager="anneta"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="mandia"/>

# <a name="manage-and-monitor-your-built-in-api-apps-and-connectors"></a>Správa a sledování předdefinované rozhraní API aplikace a spojnic

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2014 12 01 náhled schématu verze.

Jste vytvořili předdefinované rozhraní API aplikace. Co dál?

V Azure je každý rozhraní API aplikace samostatném webu umístěném na Azure. V důsledku toho můžete snadno zobrazit kolik požadavků větších změn a najdete v článku množství zpracovávaných dat je používán spojnice. Můžete taky zálohování rozhraní API aplikace, vytvořit oznámení, povolení Tinfoil zabezpečení a přidání uživatelů a rolí.

Toto téma popisuje různé ke správě rozhraní API aplikace.

Tyto předdefinované funkce zobrazíte otevřete aplikaci rozhraní API v [Azure portálu](http://go.microsoft.com/fwlink/p/?LinkID=525040). Pokud rozhraní API aplikace je na vaše startboard, vyberte ji otevřete dialogové okno Vlastnosti. Můžete taky vybrat **Procházet**, vyberte **Rozhraní API aplikace**a pak vyberte rozhraní API aplikace:

![][browse]

## <a name="see-the-properties-you-entered"></a>Zobrazit vlastnosti, které jste zadali

Při otevření aplikace rozhraní API mívají několik funkcí úkoly k dispozici:

![][settings]

Můžeš:

- **Nastavení** zobrazuje konkrétní informace o aplikaci rozhraní API, včetně svého Podrobnosti předplatného a seznam uživatelů, kteří mají přístup k rozhraní API aplikace. Můžete taky zvětšit nebo zmenšit počet výskytů rozhraní API aplikace pomocí funkce měřítko; mezi další funkce.
- Pomocí tlačítek **Spustit** a **Zastavit** řídit rozhraní API aplikace.
- Aktualizace produktů jsou určené pro základní soubory používané aplikace rozhraní API, můžete kliknutím **aktualizaci** a získejte nejnovější verze. Třeba při opravě nebo aktualizace zabezpečení uvolnění společností Microsoft, kliknutím na **Aktualizovat** automaticky aktualizuje rozhraní API aplikace zahrnout tento opravný nástroj.
- Vyberte **Plán změnit** po upgradu nebo přechodu na základě dat použití rozhraní API aplikace. Můžete taky pomocí této funkce můžete najdete v článku použití data.
- Při vytváření spojnice s tabulkami, jako je konektoru SQL můžete volitelně zadejte název tabulky se připojit k. Schéma založený na tabulce je automaticky vytvořené a dostupné po klepnutí na tlačítko **Stáhnout schémat**. Pak můžete toto stažený schéma vytvořit transformace nebo mapy.

## <a name="change-your-connector-or-api-configuration-values-you-entered"></a>Změna spojnice nebo rozhraní API konfigurace zadaných

Po nakonfigurovali nebo vytvořili na integrované spojnici, můžete změnit hodnoty, které jste zadali. Například pokud jste nakonfigurovali konektoru SQL a chcete změnit název serveru SQL Server nebo název tabulky, můžete to uděláte v rozhraní API aplikace zásuvné spojnice.

Zahrnuje následující kroky:

1. Otevřete spojnice nebo rozhraní API aplikace. Když provedete, otevře se zásuvné rozhraní API aplikace.
2. V **Essentials**klikněte na hypertextový odkaz v části Vlastnosti Host (hostitel). Hypertextový odkaz je vypadat jako *slackconnector* nebo *microsoftsqlconnector123*:

    ![][apiapphost]

3. V rozhraní API aplikace hostitele zásuvné vyberte **Nastavení**. V zásuvné nastavení vyberte **Nastavení aplikace**. Konfigurace hodnoty jsou uvedené v části **Nastavení aplikace**:

    ![][hostsettings]

4. Klikněte na nastavení, které chcete změnit, zadejte novou hodnotu a **uložte** provedené změny.


## <a name="install-the-hybrid-connection-manager---optional"></a>Instalace hybridní Connection Manager – volitelné

![][hcsetup]

Správce hybridní připojení umožňuje připojení k místní systém, třeba SQL Server nebo SAP. Toto hybridní připojení používá Bus služby Azure připojení a řídit zabezpečení mezi vaší Azure a místních zdroje.

V tématu [použití hybridního Connection Manager služby Azure aplikace](app-service-logic-hybrid-connection-manager.md).

> [AZURE.NOTE] Hybridní Connection Manager se jenom když se připojujete k místní zdroje za brány firewall. Pokud se nepřipojujete k místní systém, nemusí být Connection Manager hybridní uvedeny na zásuvné spojnice.

## <a name="monitor-the-performance"></a>Sledování výkonu
Měřítka jsou integrované funkce a zahrnutý v sadě každé rozhraní API aplikace vytvoříte. Tyto metriky jsou specifické pro vaše rozhraní API aplikace hostovaná v Azure. Ukázka metriky:

![][monitoring]

Můžeš:

- Vyberte **požadavky a chyby** přidat různé měřítka včetně kódy chyb HTTP běžně známé, jako je 200, 400 nebo 500 HTTP stavů. Můžete taky uvidíte doby odezvy, najdete v článku požadavky na kolik jsou určené pro rozhraní API aplikace a najdete v článku množství zpracovávaných dat je a množství dat, půjde. E-mailových upozornění můžete vytvořit na základě výkonu metrik, pokud metriky větší než mezní hodnota e.
- V části **použití**, zobrazí se **procesoru** používá aplikaci rozhraní API, zkontrolujte aktuální **Kvóta využití prostředků** v Megabajtech a najdete v článku používání maximální data podle náklady osy. **Předpokládané výdaje** vám pomůže určit, potenciálních nákladů spuštění rozhraní API aplikace.
- Vyberte **procesy** , spusťte Průzkumníka obrázku. Zobrazí instance webu a jejich vlastnosti, včetně použití vlákna počet a paměti.

Pomocí těchto nástrojích, můžete určit, pokud plán služeb aplikací má být zachován nebo diagramů s měřítky dolů, podle potřeb uživatele. Tyto funkce jsou integrované k portálu s žádné další nástroje povinné.

## <a name="control-the-security"></a>Ovládací prvek zabezpečení

Rozhraní API aplikace pomocí na základě rolí zabezpečení. Tyto role platí pro celou Azure experience, včetně rozhraní API aplikace a další Azure zdroje. Role patří:

Role | Popis
--- | ---
Vlastník | Máte úplný přístup k možnostem správy a udělení přístupu k další uživatele nebo skupiny.
Skupiny přispěvatelů | Mají úplný přístup k možnostem správy. Nelze udělení přístupu k další uživatele nebo skupiny.
Čtečky | Můžete zobrazit všechny zdroje s výjimkou tajemství.
Správce přístup uživatelů | Můžete zobrazit všechny zdroje, vytvořit a spravovat role a vytvořit a spravovat požadavky podpory.

V tématu [řízení přístupu na základě rolí v portálu Microsoft Azure](../active-directory/role-based-access-control-configure.md).

Můžete snadno přidávat uživatele a přiřadit konkrétní rolí k rozhraní API aplikace. Na portálu zobrazuje uživatelů, kteří mají přístup a jejich přiřazenou roli:

![][access]  

- Vyberte **Uživatelé** přidat uživatele, přiřadit roli a odebrat uživatele.
- Výběr **role** zobrazíte všechny uživatele v konkrétní rolí, přidat uživatele k roli a odebrání uživatele z role.


## <a name="more-good-stuff"></a>Další dobrý obsahu
- Vyberte **definice rozhraní API** , která automaticky vytvoří Swagger soubor otevřen pro konkrétní rozhraní API aplikace.
- Vyberte **závislosti** zobrazíte soubory vyžadované rozhraní API aplikace. Pokud používáte konektoru SAP, například nainstalujete některé další soubory na místní Connection Manager hybridní. Tyto závislosti jsou zobrazeny ve vaší zásuvné rozhraní API aplikace.

>[AZURE.IMPORTANT] Při otevírání vlastnostech rozhraní API aplikace a se podívejte do části **Základy**, existují **Host (hostitel)** a **branou** odkazy, které otevírají novou listy:
>
> ![][host]
>
>Tyto vlastnosti jsou specifické pro web, který je hostitelem rozhraní API aplikace. Pokud chcete použít předdefinované rozhraní API aplikace nebo spojnici, většina těchto vlastností nevztahují skutečně a doporučujeme, aby neaktualizovali jste tyto vlastnosti. Pokud jste vytvořili vlastní rozhraní API aplikace ve Visual Studiu a používaný k předplatnému Azure, můžete použít listy Host (hostitel) a branou. <br/><br/>


>[AZURE.NOTE] Začínáme s aplikacemi jiných logiky před registrací účet Azure, přejděte na [Akci logiky aplikace](https://tryappservice.azure.com/?appservice=logic). Můžete vytvořit aplikaci logiky krátkodobý starter. Žádné povinné kreditní karty a bez závazky.

## <a name="read-more"></a>Další informace

[Sledování aplikací logiky](app-service-logic-monitor-your-logic-apps.md)<br/>
[Spojnice a rozhraní API aplikace seznamu v aplikaci služby](app-service-logic-connectors-list.md)<br/>
[Řízení přístupu na základě rolí v portálu Microsoft Azure](../active-directory/role-based-access-control-configure.md)<br/>
[Pomocí Connection Manager hybridní Azure aplikace služby](app-service-logic-hybrid-connection-manager.md)


<!--Image references-->
[browse]: ./media/app-service-logic-monitor-your-connectors/browse.png
[settings]: ./media/app-service-logic-monitor-your-connectors/settings.png
[hcsetup]: ./media/app-service-logic-monitor-your-connectors/hcsetup.png
[monitoring]: ./media/app-service-logic-monitor-your-connectors/monitoring.png
[access]: ./media/app-service-logic-monitor-your-connectors/access.png
[host]: ./media/app-service-logic-monitor-your-connectors/host.png
[hostsettings]: ./media/app-service-logic-monitor-your-connectors/hostsettings.png
[apiapphost]: ./media/app-service-logic-monitor-your-connectors/apiapphost.png
