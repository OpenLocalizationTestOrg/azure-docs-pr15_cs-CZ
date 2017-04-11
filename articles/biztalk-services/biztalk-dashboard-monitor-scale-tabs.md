<properties 
    pageTitle="Konfigurace řídicích panelů, Monitor, měřítko a hybridní připojení ve službách BizTalk | Microsoft Azure" 
    description="Další informace o prvcích a sledovat výkon na kartách klasická portálu služby BizTalk: řídicích panelů, Monitor, měřítko, nakonfigurovat a hybridní připojení. MABS WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="mandia"/>




# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a>Kontrola karty řídicích panelů, Monitor, měřítko, nakonfigurovat a hybridní připojení

Po vytvoření služby BizTalk a nasazení aplikace, můžete změnit některé nastavení služby BizTalk a sledovat výkon aplikace. 

Při otevření portálu Azure klasické jsou automaticky umístěny na kartě **Vše** . Služba BizTalk zobrazíte výběr služby BizTalk na kartě **Vše** nebo vyberte kartu **BIZTALK služeb** ; a pak vyberte název BizTalk služby.

Otevře se nové okno s následujících kartách. Toto téma popisuje těchto kartách.

## <a name="quick-start-quick-startquickstart"></a>Rychlý Start (![Představení aplikace][QuickStart])
V závislosti na BizTalk Services Edition nemusí být k dispozici všechny možnosti uvedené. 
<table border="1">
    <tr>
        <td><strong>Získání nástrojů</strong></td>
        <td>Stáhněte si Services SDK BizTalk nainstalovat šablony projektů Visual Studio místní vývojového počítače. Tyto šablony vytvořit <strong>BizTalk služby</strong> (Most) a nasazené ke službě BizTalk projekty Visual Studio <strong>Artefaktů služby BizTalk</strong> (transformace).
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Jak můžu začít používat SDK služby Azure BizTalk</a> a <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">instalaci SDK služby Azure BizTalk</a> jsou uvedené kroky, jak začít.
        </td>
    </tr>
    <tr>
        <td><strong>Vytvoření partnera smlouvami</strong></td>
        <td>Otevře portálu Azure BizTalk služeb hostovaných na Azure, kde se přidávají partnery a vytvoření X12 AS2 a smlouvami EDIFACT upravit.
        <br/><br/>
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv upravit na portál služeb BizTalk</a> jsou uvedené kroky, jak začít.
        </td>
    </tr>

<tr>
        <td><strong>Další informace o službách BizTalk</strong></td>
        <td>Přejděte <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">výukové centrum pro</a> Další informace o Azure BizTalk Services.</td>
</tr>
</table>


V hlavním panelu v dolní máte tyto možnosti:

<table border="1">

<tr>
<td><strong>Správa</strong> nasazení aplikace</td>
<td>Otevře portálu Azure BizTalk Services. Portál služeb BizTalk je úvodní konfigurace upravit, včetně přidání partnery a vytvoření X12 AS2 a EDIFACT smlouvy.
<br/><br/>
Toto je stejná jako <strong>smlouvami partnera vytvořit</strong> na kartě <strong>Rychlý Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv upravit na portál služeb BizTalk</a> obsahuje více informací o portálu BizTalk služby.</td>
</tr>

<tr>
<td><strong>Informace o připojení</strong> z Namespace řízení přístupu</td>
<td>Když vyberete informace o připojení, pak Namespace řízení přístupu, výchozí Vystavitel a výchozí klíč zobrazují. Můžete zkopírovat tyto hodnoty.
<br/><br/>
Můžete otevřít taky portálu řízení přístupu. <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Vytvoření ovládacího prvku přístup Namespace</a> obsahuje více informací o portálu řízení přístupu.</td>
</tr>

<tr>
<td><strong>Synchronizace klíče</strong> účtu úložiště</td>
<td>Při vytváření účtu úložiště jsou automaticky vytvoří primární klíč a vedlejší klíč. Klávesy šifrování pomocí řízení přístupu k účtu úložiště. Služba BizTalk automaticky používá primárního klíče. <strong>Synchronizace klíče</strong> umožňují uživatelům můžete přepínat mezi primární klíč a sekundárního klíče bez přerušení služby BizTalk.
<br/><br/>
Můžete například služba BizTalk pro použití nového primární klíč účtu úložiště. Akce:
<br/><br/>
<ol>
<li>Vyberte služby BizTalk a vyberte <strong>Synchronizovat klíče</strong>. Vyberte sekundární klíč. Po výběru spuštění služby BizTalk použitím sekundárního klíče.</li>
<li>Na portálu Azure klasické vyberte svůj účet úložiště a obnovit primární klíč. Nezapomeňte, že služba BizTalk používá sekundárního klíče.</li>
<li>Vyberte služby BizTalk a vyberte <strong>Synchronizovat klíče</strong>. Nyní vyberte primární klíč. Toto je nová primární klíč je obnoven.</li>
<li>Na portálu Azure klasická vyberte svůj účet úložiště a obnovit sekundárního klíče.</li>
</ol>
<br/>
Tento proces se nazývá "při přechodu myší klávesy". Slouží k umožňují uživatelům můžete přepínat mezi primární klíč a sekundárního klíče bez přerušení služby BizTalk.</td>
</tr>

<tr>
<td><strong>Odstranění</strong> aplikace</td>
<td>Po výběru odstranit, služba BizTalk a všechny položky používaný k němu budou odebrány.</td>
</tr>
</table>


## <a name="dashboard"></a>Řídicí panel
V závislosti na BizTalk Services Edition nemusí být k dispozici všechny možnosti uvedené. 

Po výběru služba BizTalk název se zobrazí karta řídicí panel. Na řídicím panelu máte tyto možnosti:

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a>Přehled použití: Zobrazuje počet použitých hybridní připojení
Použití dat zobrazí taky v GB. 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a>Metriky graf: Zobrazí pevného seznamu měřítka
Tyto metriky zadat v reálném čase hodnoty týkající se stavu služby BizTalk. Můžete taky zvolit **absolutní** nebo **relativní** hodnoty a časový rozsah **Interval** metriky, které se zobrazí v grafu. 

Popis těchto měřítka přejděte na [Metriky dostupné](#Metrics) v tomto tématu.


##### <a name="quick-glance-lists-your-biztalk-service-properties"></a>Stručný přehled: Seznamy vlastností BizTalk služby

<table border="1">

<tr>
<td><strong>Aktualizovat přihlašovací údaje pro sledování databázi</strong></td>
<td>Změní se uživatelské jméno a heslo pro přihlášení k sledování databáze.</td>
</tr>
<tr>
<td><strong>Aktualizovat certifikát SSL</strong></td>
<td>Můžete aktualizovat služba BizTalk použít jiný certifikát SSL. Certifikát podepsaný svým držitelem SSL se automaticky vytvoří při <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vytvoření služba BizTalk</a>.</td>
</tr>
<tr>
<td><strong>Stáhněte si certifikát</strong></td>
<td>Můžete si stáhnout použita službou BizTalk do místního počítače certifikát SSL.</td>
</tr>
<tr>
<td><strong>Stav</strong></td>
<td>Zobrazuje aktuální stav služby BizTalk. V tématu <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk služby: grafu stavu služby</a>. </td>
</tr>
<tr>
<td><strong>Adresy URL služby</strong></td>
<td>Adresa URL pro službu BizTalk. Toto je stejný jako <strong>Adresu URL domény</strong> zadaného při vytvoření BizTalk služby.</td>
</tr>
<tr>
<td><strong>Adresa veřejného virtuální IP (VIP)</strong></td>
<td>IP adresa přiřazená BizTalk služby. Slouží k všechny koncové body zadávání a adresu zdroje pro odchozí přenosy. Tuto IP adresu patří ke službě BizTalk, dokud se vytvoří. Pokud byste odstranili služba BizTalk, IP adresa přiřazena na jinou službu BizTalk.</td>
</tr>
<tr>
<td><strong>ACS Namespace</strong></td>
<td>Ověří ke službě BizTalk.</td>
</tr>
<tr>
<td><strong>Edition</strong></td>
<td>Seznamy edici napsána při služba BizTalk.</td>
</tr>
<tr>
<td><strong>Umístění</strong></td>
<td>Zobrazí zeměpisná oblast, který je hostitelem služby BizTalk.</td>
</tr>
<tr>
<td><strong>Vytvoření</strong></td>
<td>Zobrazí datum a čas vytvoření služba BizTalk.</td>
</tr>
<tr>
<td><strong>Sledování databáze</strong></td>
<td>Název databáze SQL Azure, který ukládá použita službou BizTalk tabulkami sledování. 
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vysvětlení požadavků</a> poskytuje podrobné informace o sledování databáze.</td>
</tr>
<tr>
<td><strong>Monitorování a archivace úložiště</strong></td>
<td>Název účtu úložiště Azure, který ukládá sledování výstup BizTalk služby.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Vysvětlení požadavků</a> poskytuje podrobné informace o účtu úložiště.</td>
</tr>
<tr>
<td><strong>Název předplatného</strong></td>
<td>Seznam, který je hostitelem služby BizTalk předplatného. Předplatné se řídí přístup na portál Azure klasické.</td>
</tr>
<tr>
<td><strong>ID předplatného</strong></td>
<td>Po vytvoření předplatného se vytvářejí automaticky ID předplatného. Při používání rozhraní REST API, budete muset zadat ID předplatného.</td>
</tr>
</table>

[BizTalk služby: zřizovací Azure pomocí klasické portál](http://go.microsoft.com/fwlink/p/?LinkID=302280) jsou uvedené kroky k vytvoření BizTalk služby.


##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a>Spravovat informace o připojení, synchronizovat klávesy a odstraňte v řádku úkolu:

<table border="1">

<tr>
<td><strong>Správa</strong> nasazení aplikace</td>
<td>Otevře se na portál služby Azure BizTalk. Portál služeb BizTalk je úvodní konfigurace upravit, včetně přidání partnery a vytvoření X12 AS2 a EDIFACT smlouvy.
<br/><br/>
Toto je stejná jako <strong>smlouvami partnera vytvořit</strong> na kartě <strong>Rychlý Start</strong> .
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurace součástí pro zasílání zpráv upravit na portál služeb BizTalk</a> obsahuje více informací o portálu BizTalk služby.</td>
</tr>
<tr>
<td><strong>Informace o připojení</strong> z Namespace řízení přístupu</td>
<td>Zobrazí Namespace řízení přístupu, výchozí Vystavitel a klíč výchozí hodnoty. které lze zkopírovat.
<br/><br/>
Můžete otevřít taky portálu řízení přístupu. Ovládací prvek portál tento přístup je stejná jako možnost služby Active Directory v levém navigačním podokně.
<br/><br/>
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Správa ACS Namespace</a> obsahuje více informací o portálu řízení přístupu.</td>
</tr>
<tr>
<td><strong>Synchronizace klíče</strong> účtu úložiště</td>
<td>Při vytváření účtu úložiště jsou automaticky vytvoří primární klíč a vedlejší klíč. Klávesy šifrování pomocí řízení přístupu k účtu úložiště. Služba BizTalk automaticky používá primárního klíče. <strong>Synchronizace klíče</strong> umožňují uživatelům můžete přepínat mezi primární klíč a sekundárního klíče bez přerušení služby BizTalk.
<br/><br/>
Můžete například služba BizTalk pro použití nového primární klíč účtu úložiště. Akce:
<br/><br/>
<ol>
<li>Vyberte služby BizTalk a vyberte <strong>Synchronizovat klíče</strong>. Vyberte sekundární klíč. Po výběru spuštění služby BizTalk použitím sekundárního klíče.</li>
<li>Na portálu Azure klasické vyberte svůj účet úložiště a obnovit primární klíč. Nezapomeňte, že služba BizTalk používá sekundárního klíče.</li>
<li>Vyberte služby BizTalk a vyberte <strong>Synchronizovat klíče</strong>. Nyní vyberte primární klíč. Toto je nová primární klíč je obnoven.</li>
<li>Na portálu Azure klasická vyberte svůj účet úložiště a obnovit sekundárního klíče.</li>
</ol>
<br/>
Tento proces se nazývá "při přechodu myší klávesy". Slouží k umožňují uživatelům můžete přepínat mezi primární klíč a sekundárního klíče bez přerušení služby BizTalk.</td>
</tr>

<tr>
<td><strong>Odstranění</strong> aplikace</td>
<td>Služba BizTalk a všechny položky používaný k němu budou odebrány.</td>
</tr>
</table>


## <a name="monitor"></a>Sledování
Nebudou mít vliv na bezplatné verzi.

Po výběru názvu služba BizTalk kartě Monitor je k dispozici a zobrazí se následující:

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a>Metriky graf: Zobrazí vybrané měřítka
Tyto metriky zadat v reálném čase hodnoty týkající se stavu služby BizTalk. Můžete se zobrazují které měřítka. Maximálně měřítka šest mohou být zobrazena současně. 

Můžete taky zvolit **absolutní** nebo **relativní** hodnoty a časový rozsah **Interval** metriky, které se zobrazí. 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a>Odebrání nebo zobrazení metriky v grafu:
1. Vyberte kartu **Monitor** .
2. Vyberte **Přidat metriky** na panelu úkolů:  
![Vyberte Přidat metriky][AddMetrics]
3. Zaškrtněte políčko měřítka, který chcete zobrazit.
4. Zaškrtněte políčko se vrátíte na kartě **Monitor** .
5. Vyberte kruh vedle míru k zobrazení, které míru hodnoty v grafu.  

    Například míru **Využití procesoru** se zobrazuje šedě; výstup není zobrazených v grafu:  
![Využití procesoru míru se zobrazuje šedě][GrayedMetric]  

    Vyberte šedě kruh povolit míru **Využití procesoru** zobrazíte jeho výstup v grafu:  
![Využití procesoru míru aktivované][EnabledMetric]

6. Pokud chcete odebrat metriky ze zobrazení graf a v seznamu, vyberte **Odstranit míru** na hlavním panelu. Přidat metrických zpět do seznamu, vyberte **Přidat metriky** na hlavním panelu, zkontrolujte metriky a vyberte zaškrtnutí se vrátíte na kartě **Monitor** . Vyberte šedě kruh povolit metriky.

## <a name="Metrics"></a>K dispozici metriky
Dostupné jsou následující čítače/metriky výkonu:

<table border="1">

<tr>
<td><strong>Latence RountdTrip</strong></td>
<td>Průměrná doba v milisekundách ([ms) zpracuje zprávu od doby, že dokud se zpráva je plně zpracovány službou BizTalk doručení zprávy se zobrazí přes všechny mosty. Jsou započteny pouze zprávy úspěšně zpracovat.<br/><br/>
Při následujících událostech, je vytvořen časové razítko:
<ul>
<li>Zpráva zadá brány</li>
<li>Zpráva je nasměrovaná do cíle</li>
<li>Určení odpověď</li>
<li>Odeslané brány pro odpovědi na potvrzení cíl</li>
</ul>
<br/>
Tento míru zobrazí výsledek tohoto vzorce:
<br/><br/>
[Cíl potvrzení odeslané odpovědi brány] - [zpráva slouží k vložení brány]</td>
</tr>
<tr>
<td><strong>K chybám na straně zdroje</strong></td>
<td>Zobrazuje celkový počet zpráv, které se nepodařilo službou BizTalk při zavedení zprávy od koncové body zdroje.</td>
</tr>
<tr>
<td><strong>Využití procesoru</strong></td>
<td>Seznam průměr % času procesoru všechny instance role.</td>
</tr>
<tr>
<td><strong>Latence zpracování</strong></td>
<td>Zobrazí Průměrná doba v milisekundách ([ms) zpracuje zprávy službou BizTalk přes všechny mosty bez čas strávený do cíle. Jsou započteny pouze zprávy úspěšně zpracovat.<br/><br/>
Při každé z následujících událostí výskytu se vytvoří časové razítko:

<ul>
<li>Zpráva zadá brány</li>
<li>Zpráva je nasměrovaná do cíle</li>
<li>Určení odpověď</li>
<li>Odeslané brány pro odpovědi na potvrzení cíl</li>
</ul>
<br/>Tento míru zobrazí výsledek tohoto vzorce:<br/><br/>
[Cíl potvrzení odeslané odpovědi na brány] - [zpráva slouží k vložení brány] - [je přijata odpověď na cíl] + [zpráva je nasměrovaná do cíle]</td>
</tr>
<tr>
<td><strong>K chybám v procesu</strong></td>
<td>Zobrazuje celkový počet zpráv, které se nepodařilo během zpracování službou BizTalk přes všechny mosty během časového intervalu.</td>
</tr>
<tr>
<td><strong>Zprávy zasílané</strong></td>
<td>Zobrazuje celkový počet zprávy posílané službou BizTalk všechny mosty během časového intervalu. Tento míru se zvyšuje zprávy odeslané z potrubí dosažení směrovat cíle. Tento míru neznamená úspěšně zpracování zprávy.<br/><br/>
Ve scénáři požadavek odpověď metriky zvýšen, pokud směrovat cíle, odešle potvrzení přijetí zpět kanálu.</td>
</tr>
<tr>
<td><strong>Přijaté zprávy</strong></td>
<td>Zobrazuje celkový počet zprávy přijaté službou BizTalk přes všechny mosty během časového intervalu. Tento míru se zvyšuje při doručení nové zprávy kanálem.</td>
</tr>
<tr>
<td><strong>Zprávy v procesu</strong></td>
<td>Zobrazuje celkový počet zpráv aktuálně zpracovávají služba BizTalk během časového intervalu.</td>
</tr>
<tr>
<td><strong>Zpracování zpráv</strong></td>
<td>Zobrazuje celkový počet zpráv úspěšně zpracovány službou BizTalk přes všechny mosty během časového intervalu. Tento míru je zvýšen, pokud zprávu úspěšně dostali kanálu a úspěšně směrovány do cíle.</td>
</tr>
</table>


## <a name="scale"></a>Stupnice
Na kartě Měřítko můžete přidat nebo odečíst počet jednotek použita službou BizTalk. Ve výchozím nastavení je jednu jednotku konfigurovat. Zobrazit služby BizTalk lze přidat další jednotky. Pokud zvětšíte rozsah, jsou zvýšit výkon. Množství prostředků taky zvyšuje, včetně nasazeném mosty, smluv, LOB připojení a zpracování power. Můžete například zvětšit měřítko 1 jednotka 2 jednotky. V takovém případě můžete nasadit dvojité počet mosty, poklikáním dohod, poklikáním LOB připojení a poklikáním výkon.

Některé edice BizTalk nenabízí možnost měřítko. V tomto případě je povolen jednu jednotku. Zjistit, kolik jednotek můžou diagramů s měřítky vaše verze, najdete pod odkazy [BizTalk služby: edice grafu](biztalk-editions-feature-chart.md).

Zvětšení počet jednotek, mohou ovlivnit ceny. Pokud zvětšíte jednotka výběr **Uložit** zobrazí zprávu, která sděluje, zda se týkají fakturace. Pak budete pokračovat. Pokud zvětšíte počet jednotek, změny stavu služby BizTalk z aktivní aktualizace. Ve stavu aktualizace nadále BizTalk služba spustit.

[BizTalk služby: edice grafu](biztalk-editions-feature-chart.md) definuje "Jednotku".


## <a name="configure"></a>Konfigurace
Nebudou mít vliv na hybridní připojení.

Zálohování stav nastaví, žádné nebo automatické. Když nastaven žádné, žádné zálohy se vytvoří automaticky. Když nastaven na automatické konfigurace umístění zálohy, počet_plateb zálohy a jak dlouho chcete-li zachovat záložních souborů. 

[BizTalk služby: zálohování a obnovení](biztalk-backup-restore.md) poskytuje údaje. 


## <a name="HybridConnections"></a>Hybridní připojení
Hybridní připojení připojit k místní zdroj, který používá statické port TCP, například SQL serveru, MySQL, rozhraní API webových HTTP a většina vlastní webové služby Azure aplikace, jako jsou webové aplikace nebo mobilní aplikace v aplikaci služby Azure. Hybridní připojení se spravují v BizTalk služby Azure klasická portálu.

Vytvoření hybridní připojení aplikace služby Azure najdete v tématu [přístup místních zdrojů pomocí hybridní připojení aplikace služby Azure](../app-service-web/web-sites-hybrid-connection-get-started.md).

Vytvořit nebo spravovat připojení hybridní BizTalk služby Azure, najdete v tématu [Hybridní spojení](integration-hybrid-connection-overview.md).



## <a name="next"></a>Další
Teď, když znáte různé karty, můžete se dozvíte víc o funkcích Azure BizTalk Services:

- [Služby BizTalk: omezení](biztalk-throttling-thresholds.md)  
- [BizTalk služby: Název vydavatele a Vystavitel klíč](biztalk-issuer-name-issuer-key.md)  
- [BizTalk služby: Zálohování a obnovení](biztalk-backup-restore.md)

## <a name="see-also"></a>Viz taky
- [Hybridní připojení](integration-hybrid-connection-overview.md)  
- [BizTalk služby: Vývojář Basic, standardní a Premium edice grafu](biztalk-editions-feature-chart.md)  
- [BizTalk služby: Zřizování klasická portál pomocí Azure](biztalk-provision-services.md)  
- [BizTalk služby: Diagram stavu služby BizTalk](biztalk-service-state-chart.md)  
- [Jak můžu začít používat služby SDK BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[QuickStart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png
 
