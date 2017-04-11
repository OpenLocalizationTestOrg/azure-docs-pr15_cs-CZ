<properties 
    pageTitle="Poradce při potížích s BizTalk služby, které využívají protokoly operací | Microsoft Azure" 
    description="Poradce při potížích BizTalk služby, které využívají protokoly operací. MABS WABS" 
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


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk služby: Poradce při potížích s operace protokoly

## <a name="what-are-the-operation-logs"></a>Jak fungují protokoly operací
Protokoly operací je k dispozici v Azure klasický portál, který umožňuje zobrazit historii protokoly operací týkajících se služby Azure, včetně BizTalk služby Správa přístupových funkce. Umožňuje zobrazit historii data týkající se správy operace ve vašem předplatném služby BizTalk již 180 dní.

> [AZURE.NOTE]Tato funkce pouze zaznamenává protokoly pro správu operace na BizTalk služby, například při spuštění služby, záložní až, a tak dál. Tyto operace sledují bez ohledu na to, zda jsou provedených z portálu Microsoft Azure klasické nebo pomocí [BizTalk služba REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx). Úplný seznam operacích, které sledují pomocí služby správy najdete v článku [Operace sledované pomocí Azure správu přístupových](#bizops).<br/><br/>
Toto není zachytit protokoly pro činností spojených služba BizTalk spuštěnou aplikaci (jako je zpráva zpracovat mosty atd.). Zobrazíte tyto protokoly použijte zobrazení Sledovací z portálu BizTalk služby. Další informace najdete v tématu [Sledování zpráv](http://msdn.microsoft.com/library/azure/hh949805.aspx).

## <a name="view-biztalk-services-operation-logs"></a>Zobrazení BizTalk služby operace protokoly
1. Na portálu Azure klasické vyberte **Správa přístupových**a pak vyberte kartu **Protokoly operací** .
2. Můžete filtrovat protokoly podle různých parametry jako předplatné rozsah kalendářních dat, typ služby (například BizTalk služby), název služby a stav operace (byl úspěšný, neúspěšný).
3. Zaškrtněte políčko Zobrazit filtrovaným seznamem. Následující obrázek znázorňuje činností spojených testbiztalkservice:  ![protokolech operace][ViewLogs] 
4. Zobrazíte další informace o konkrétní operace, vyberte řádek a klikněte na **Podrobnosti** do hlavního panelu na spodní.


## <a name="bizops"></a>Operace sledovat pomocí služby Azure správy
V následující tabulce jsou uvedeny operace, které sledují pomocí služby správy Azure:

Název operace | Úkol
--- | ---
CreateBizTalkService | Vytvoření nové služby BizTalk
DeleteBizTalkService | Způsob odstranění BizTalk služby
RestartBizTalkService | Operace restartování BizTalk služby
StartBizTalkService | Operace spustit službu BizTalk
StopBizTalkService | Operace zastavení služby BizTalk
DisableBizTalkService | Operace zakázání BizTalk služby
EnableBizTalkService | Operace povolení služby BizTalk
BackupBizTalkService | Operace k obecnějším údajům BizTalk služby
RestoreBizTalkService | Operace obnovit služba BizTalk ze zadané zálohy
SuspendBizTalkService | Operace za účelem pozastavení BizTalk služby
ResumeBizTalkService | Operace pokračovat BizTalk služby
ScaleBizTalkService | Operace zobrazit služba BizTalk nahoru nebo dolů
ConfigUpdateBizTalkService | Operace aktualizace konfiguraci služby BizTalk
ServiceUpdateBizTalkService | Operace aktualizace nebo omezit služby BizTalk na jinou verzi
PurgeBackupBizTalkService | Operace vyprázdnění zálohy služba BizTalk mimo doba uchovávání informací


## <a name="see-also"></a>Viz taky
- [Služba záložní BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [Obnovení služba BizTalk ze zálohy](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk služby: Vývojář Basic, Standard a Premium edice grafu](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk služby: Zřizování klasické portál pomocí Azure](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk služby: Diagram stavu zřizování](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk služby: Řídicí panel, sledování a měřítko karty](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [Služby BizTalk: omezení](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk služby: Název vydavatele a Vystavitel klíč](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Jak můžu začít používat služby SDK BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 
