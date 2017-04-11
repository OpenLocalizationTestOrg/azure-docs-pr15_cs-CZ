<properties 
    pageTitle="Úkoly v různých stavech nebo stav služby BizTalk povoleny | Microsoft Azure" 
    description="Akce či operace povolené v různých MABS stav: zastavit, spustit, restartujte, pozastavení, obnovení, odstranit, měřítko, aktualizovat konfigurace a zálohování nahoru" 
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



# <a name="biztalk-services-service-state-chart"></a>BizTalk služby: Diagram stavu služby
V závislosti na aktuální stav služby BizTalk jsou operacích, které můžete nebo nemůžete provádět BizTalk služby.

Například zřízení nového BizTalk služby Azure klasické portálu. Po úspěšném dokončení, je služba BizTalk v aktivního stavu. V aktivní stav můžete zastavit službu BizTalk. -Li zastavit úspěšné, služba BizTalk přejde do ukončení uživatelem stavu. Když zarážky nepovede, služba BizTalk půjde StopFailed stavu. Ve stavu StopFailed můžete restartovat službu BizTalk. Pokud se pokusíte operace, které nejsou povolené, třeba životopis službu BizTalk k následující chybě:

**Operace není povolena**

Zřízení služby BizTalk, přejděte na [BizTalk služby: zřizujete Azure pomocí klasické portál](http://go.microsoft.com/fwlink/p/?LinkID=302280).

Následující tabulka uvádí operace nebo akce, které lze provést po služba BizTalk konkrétní stav. ✔ znamená, že operace je povolena v tomto stavu. Prázdné položky znamená, že operaci nelze použít v této stavu.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Spustit, zastavit, restartujte, pozastavení, obnovení operace a odstranění
<table border="1">
<tr>
        <th colspan="15">Operace nebo akce</th>
</tr>

<tr>
        <th rowspan="18">Stav služby BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Zahájení</th>
        <th>Stop</th>
        <th>Restartování počítače</th>
        <th>Pozastavení</th>
        <th>Životopis</th>
        <th>Odstranění</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktivní</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Vypnutí</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Pozastavené</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Přerušili</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktualizace služby se nezdařila.</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Měřítko, konfiguraci aktualizace a operace zálohování
<table border="1">
<tr>
        <th colspan="15">Operace nebo akce</th>
</tr>

<tr>
        <th rowspan="18">Stav služby BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Stupnice</th>
        <th>Konfigurace aktualizace</th>
        <th>Zálohování</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktivní</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Vypnutí</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Pozastavené</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Přerušili</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktualizace služby se nezdařila.</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Viz taky
- [BizTalk služby: Zřizování klasické portál pomocí Azure](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk služby: Řídicí panel, sledování a měřítko karty](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk služby: Vývojář Basic, Standard a Premium edice grafu](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk služby: Zálohování a obnovení](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [Služby BizTalk: omezení](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk služby: Název vydavatele a Vystavitel klíč](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Jak můžu začít používat služby SDK BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
