<properties 
   pageTitle="Poradce při potížích s Průvodce - služby Azure mobilní zapojení" 
   description="Poradce při potížích s příručky pro Azure mobilní zasunutí" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="troubleshooting-guide-for-service-issues"></a>Příručka pro řešení potíží pro problémy se službou

Následují možné problémy, které se můžete setkat při spuštění Azure Mobile Engagement.

## <a name="service-outages"></a>Služba výpadků

### <a name="issue"></a>Problém
- Problémy, které zdá být způsobené Azure Mobile zapojení služby výpadků.

### <a name="causes"></a>Příčiny
- Problémy, které se zdá být způsobené Azure Mobile zapojení služby výpadků může být způsobeno několik různých věcí:
    - Izolace problémy zobrazené původně systémové u všech přijetí Mobile Azure
    - Známé problémy způsobená výpadků serveru (ne vždy zobrazuje stavu serveru):
    - Plánování zpoždění cílení chyby, Odznáček aktualizace, Statistika zastavit shromažďujete, nabízených přestane fungovat, rozhraní API přestanou nových aplikací a uživatelé nelze vytvořit, chyby DNS a chyby vypršení časového limitu v uživatelském rozhraní, rozhraní API nebo aplikací na zařízení.
    - Shluk závislost výpadků [Stav služby Azure](http://status.azure.com/)
    - Nabízená oznámení služby (PNS) závislost výpadků
    - Výpadků App Storu

1) Chcete-li otestovat zobrazíte, když je systémové můžete otestovat stejnou funkci z jiného
   
   - Azure zapojení Mobile integrované aplikace
   - Test zařízení
   - Zařízení s operačním systémem zkušební verze
   - Kampaň
   - Uživatelský účet správce
   - Prohlížeče (IE, Firefox, Chrome, atd.)
   - Počítač

2) Chcete-li otestovat, pokud problém se týká pouze uživatelské rozhraní nebo rozhraní API:

   - Otestujte stejnou funkci z uživatelského rozhraní Azure Mobile zapojení i Azure Mobile zapojení rozhraní API.

3) Chcete-li otestovat, pokud k potížím dochází k síti mobilní telefon:

   - Otestujte při připojení k Internetu přes Wi-Fi a při připojení prostřednictvím sítě 3G mobilního telefonu.
   - Potvrďte, že brány firewall neblokuje některou z Azure Mobile zapojení IP adres a porty.

4) Chcete-li otestovat problém se zařízení:

   - Vyzkoušejte, pokud vaše zařízení je možné se připojit k zapojení Mobile Azure pomocí jiného Azure Mobile zapojení integrovanou aplikaci.
   - Otestujte, generovat události, projekty a selhání z telefonu, zobrazená v uživatelském rozhraní Azure Mobile Engagement. 
   - Test při odesílání nabízená oznámení v uživatelském rozhraní Azure Mobile zapojení do zařízení podle ID své zařízení. 

5) Chcete-li otestovat, pokud k potížím dochází s aplikací:

   - Nainstalovat a testovat aplikace emulátoru ne z fyzického zařízení:
   
6) Chcete-li otestovat problém se OS inovace koncový uživatel zařízení, která vyžaduje upgrade SDK vyřešit:

   - Vyzkoušejte aplikaci na různých zařízeních s různými verzemi operačního systému.
   - Potvrďte, že používáte nejnovější verzi sady SDK.
 
## <a name="connectivity-and-incorrect-information-issues"></a>Připojení a problémy nějaké informace nejsou správné

### <a name="issue"></a>Problém
- Problémy s přihlášením do uživatelského rozhraní Azure Mobile zapojení.
- Připojení chyby pomocí Azure Mobile zapojení rozhraní API společnosti.
- Problémů s nahráváním značky informace o aplikaci prostřednictvím rozhraní API zařízení.
- Problémy s stažením protokoly nebo exportovaná data z Azure Mobile Engagement.
- Nějaké informace nejsou správné zobrazují v uživatelském rozhraní Azure Mobile Engagement.
- Nesprávný údaji zobrazenému v Azure Mobile zapojení protokoly.

### <a name="causes"></a>Příčiny
* Potvrďte, že má váš uživatelský účet dostatečná oprávnění k provedení úkolu.
* Potvrďte, že problém není izolovaná na jednom počítači nebo v místní síti.
* Potvrďte, že služby Azure Mobile zapojení bez vykázal výpadků.
* Potvrďte, že soubory značku informace o aplikaci dodržujte všechny jednotlivá pravidla:
    - Používáte pouze UTF8 znakové sady (nepodporuje znakové sady ANSI).
    - Čárkami "," jako oddělovač (otevíráte službu požadavek na požadavek na změnu znak oddělovače .csv z čárku "," jiný znak například středníkem ";").
    - Používejte všechna malá písmena pro logické hodnoty "true" a "false".
    - Pomocí souboru, který je menší než maximální velikost souboru 35 MB.
 
