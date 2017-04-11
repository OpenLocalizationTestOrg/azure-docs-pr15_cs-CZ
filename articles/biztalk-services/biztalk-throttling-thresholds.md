<properties 
    pageTitle="Další informace o omezení služby BizTalk | Microsoft Azure" 
    description="Informace o omezení mezní hodnoty a výsledná chování runtime služby BizTalk. Omezení vychází spotřebu paměti a počet zpráv. MABS WABS" 
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
    ms.date="08/15/2016" 
    ms.author="mandia"/>





# <a name="biztalk-services-throttling"></a>Služby BizTalk: omezení

Služby Azure BizTalk implementuje služby omezení na základě dvou podmínek: spotřebu paměti a počet souběžné zpracování zpráv. Toto téma obsahuje seznam omezení mezní hodnoty a popisuje chování běhu dojde omezující podmínku.

## <a name="throttling-thresholds"></a>Omezení mezní hodnoty

V následující tabulce jsou uvedeny zdroj omezení a limity:

||Popis|Mezní hodnotu|Vysoký mezní hodnota|
|---|---|---|---|
|Paměti|% celkového systém paměti dostupné/PageFileBytes. <p><p>Celkové dostupné PageFileBytes je přibližně 2 krát RAM systému.|60 %|70 %|
|Zpracování zpráv|Počet současně zpracování zpráv|40 * počet vzorky|100 * počet vzorky|

Při dosažení vysoké prahové hodnoty, spustí se služby Azure BizTalk omezení. Omezení zarážkami, u kterých při dosažení dolní prahové hodnoty. Například služby používá pamětí 65 %. V takovém případě nejsou omezení služby. Služby spuštění jsou použity pamětí 70 %. V takovém případě službu omezení a nadále omezení, dokud služba používá pamětí 60 % (mezní hodnotu).

Služby Azure BizTalk sleduje omezení stavu (normální state porovnání omezené stavu) a omezení doby trvání.


## <a name="runtime-behavior"></a>Chování běhu

Když služby Azure BizTalk přejde omezení stavu, mít tyto důsledky:

- Omezení je za instanci rolí. Příklad:<br/>
RoleInstanceA omezení. RoleInstanceB není omezení. V takovém případě zpracování zpráv v RoleInstanceB podle očekávání. Zprávy v RoleInstanceA ignorovány a nepovede se tato chyba:<br/><br/>
**Server teď na něčem pracuje. Zkuste to znovu.**<br/><br/>
- Všechny zdroje vyžádané hlasování ani stahovat zprávy. Příklad:<br/>
Potrubí použije zprávy z externího zdroje FTP. Role instance dělají vyžádané získá do omezení stavu. V takovém případě se zastaví kanálu stahování další zprávy, dokud instanci rolí přestane omezení.
- Odpověď jsou odeslány klienta tak klienta můžete znovu odeslat zprávu
- Můžete třeba počkejte omezení vyřeší. Konkrétně musí Počkejte, dokud dosažení dolní prahové hodnoty.

## <a name="important-notes"></a>Důležité poznámky
- Omezení se nedá zakázat.
- Omezení mezní hodnoty nelze změnit.
- Omezení prováděna systémové.
- Databázový Server SQL Azure má také vestavěné omezení.

## <a name="additional-azure-biztalk-services-topics"></a>Další témata BizTalk služby Azure

-  [Instalace služby Azure BizTalk SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Výukové programy pro: Služby Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Jak můžu začít používat služby SDK BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Služby Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Viz taky
- [BizTalk služby: Vývojář Basic, Standard a Premium edice grafu](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk služby: Zřizování klasické portál pomocí Azure](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk služby: Diagram stavu zřizování](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk služby: Řídicí panel, sledování a měřítko karty](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk služby: Zálohování a obnovení](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk služby: Název vydavatele a Vystavitel klíč](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
