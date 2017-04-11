<properties
    pageTitle="Aktivního stavu replikace adresáře řešení v protokolu analýzy | Microsoft Azure"
    description="Sady řešení stav replikace Active Directory pravidelně sleduje prostředí služby Active Directory pro všechny chyby, replikace sestavách a výsledky na řídicím panelu OMS."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Aktivního stavu replikace adresáře řešení v protokolu analýzy

Služba Active Directory je hlavní součástí organizace prostředí. Zajistit dostupnost a vysoký výkon každého řadiče domény obsahuje vlastní kopii databáze služby Active Directory. Řadiče domény replikovat vzájemně k šíření změn v organizaci. K chybám v tomto procesu replikace může docházet k problémů v organizaci.

Sady řešení stav replikace AD pravidelně sleduje prostředí služby Active Directory pro všechny chyby, replikace sestavách a výsledky na řídicím panelu OMS.

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Instalace a konfigurace řešení použijte následující informace.

- Řadiče domény, které jsou součástí domény má být vyhodnocen, nebo na serverech člena nakonfigurované tak, aby odešlete data replikace AD OMS musí být nainstalovaná agenti. Jak se připojit k OMS počítače se systémem Windows, najdete v tématu [připojení Windows počítačů protokolu analýzy](log-analytics-windows-agents.md). U řadiče domény se už součástí existující prostředí System Center Operations Manager, který chcete připojit k OMS najdete v článku [Připojení Operations Manager do protokolu analýzy](log-analytics-om-agents.md).
- Přidání řešení stav replikace Active Directory do pracovního prostoru OMS pomocí proces popsaný v [řešení přidat analýzy protokolu z Galerie řešení](log-analytics-add-solutions.md).  Není nutná žádná další konfigurace.


## <a name="ad-replication-status-data-collection-details"></a>Podrobnosti o kolekce dat AD replikace stavu

V následující tabulce jsou uvedeny metody shromažďování dat a další podrobnosti o tom, jak se údaje pro stav replikace AD.

| platformy | Přímé Agent | SCOM agent | Azure úložiště | SCOM povinné? | Data agent SCOM odeslaná pomocí správy skupiny | kolekce četnosti |
|---|---|---|---|---|---|---|
|Windows|![Ano](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Ano](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Ne](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Ne](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Ano](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| každých 5 dní.|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>V případě potřeby povolte bez domény řadiče odešlete AD data OMS
Pokud nechcete některou z vaší domény řadiče přímé připojení k OMS, můžete použít jakýkoli jiný počítač připojen OMS ve vaší doméně shromáždit data pro stav replikace AD sady řešení a mít ji odešlete data.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Chcete-li povolit bez domény řadiče odešlete AD data OMS
1.  Ověřte, zda počítač je členem domény, kterou chcete sledovat pomocí stav replikace AD řešení.
2.  [Připojit k OMS počítač se systémem Windows](log-analytics-windows-agents.md) nebo [pomocí prostředí Operations Manager existující k OMS připojení](log-analytics-om-agents.md), pokud již není připojený.
3.  Na tomto počítači nastavte následujícímu klíči registru:
    - Klíč: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management skupiny\<ManagementGroupName > \Solutions\ADReplication**
    - Hodnota: **IsTarge**
    - Údaj hodnoty: **PRAVDA**

    >[AZURE.NOTE]Tyto změny se projeví až vaše restart Microsoft sledování agentem (HealthService.exe).

## <a name="understanding-replication-errors"></a>Principy replikace chyb
Až budete mít posílat OMS údaje o stavu AD replikace, zobrazí se dlaždice podobně jako tento na řídicím panelu OMS označující počet chyb replikace Teď máte.  
![Stav replikace AD dlaždice](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritické chyby replikace** jsou ty, které jsou nebo nad 75 % [zneplatnění životnost](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) pro doménové služby Active Directory.

Když kliknete na dlaždici, zobrazí se další informace o chybách.
![Řídicí panel stavu replikace AD](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>Cíl serveru stav a stav zdroje serveru
Tyto listy zobrazit stav cíl servery a zdroje, které jsou dochází k chybám replikace. Číslo za každý název domény řadiče domény označuje počet chyb replikace u této domény řadiče domény.

Chyby pro cílové servery a servery zdroje se zobrazí, protože některé problémy se snáze řešení potíží s z perspektivu serveru zdroj a některé z hlediska serveru cíl.

V tomto příkladu zobrazí se mnoha cílové servery mají přibližně stejné číslo chyby, že je jeden zdroj server (ADDC35), které má mnoho dalších chyb než všechny ostatní. Je pravděpodobné, že je na ADDC35, který způsobuje ho selhání odeslání dat do partnerům replikace nějaký problém. Řešení problémů na ADDC35 pravděpodobně vyřeší spoustu chyby, které se zobrazí v zásuvné serveru cíl.

### <a name="replication-error-types"></a>Typy chyb replikace
Tento zásuvné poskytuje informace o uvedených typech zjištěny v celé organizaci chyby. Každá chyba má jedinečný číselný kód a zprávu, která vám pomůže určit příčina chyby do schránky.

Prstencový nahoře vám přehled o které se informace a méně často ve vašem prostředí.

Tím se zobrazí, když více řadiče domény prostředí stejné Chyba replikace. V tomto případě se můžou objevit identifikovat řešení řadiči jednu doménu a potom na další řadiče domény ovlivňují stejná chyba opakovat.

### <a name="tombstone-lifetime"></a>Životnost zneplatnění
Životnost zneplatnění Určuje, jak dlouho odstraněného objektu, uvedené jako nepotřebná data, zůstane v databázi služby Active Directory. Když odstraněného objektu projde životnost zneplatnění, procesu uvolnění shromažďování ji automaticky odstraní z databáze služby Active Directory.

Životnost zneplatnění výchozí je 180 dnů nejnovější verze systému Windows, ale bylo 60 dní ve starších verzích a ho můžete měnit explicitně Správce služby Active Directory.

Je důležité vědět, pokud máte replikace chyby, které se blíží nebo po termínu zneplatnění životnost. Dva řadiče domény možnosti replikace chyby, které ukládá dřívější životnost zneplatnění, replikace mezi tyto dva řadiče domény, zakázat, i když je základní replikace chybu opravit.

Životnost zneplatnění zásuvné budete moci určit místa, kde to je nebezpečí později. Každou chybu v **víc než 100 % TSL** kategorie znázorňuje oddíl, který není replikovat mezi jeho zdrojové a cílové server pro spolupráci prostřednictvím aspoň životnost zneplatnění pro struktuře.

V takovém případě jednoduše oprava chyby replikace nebude stačit. Minimálně budete muset ručně zjistit, jak identifikovat a čištění mít objekty před můžete restartovat replikace. Můžete dokonce vyřadit řadiče domény.

Kromě identifikační chybách replikace, ke kterým máte zachován dřívější životnost zneplatnění, budete taky chcete věnovat všechny chyby, které spadají do kategorie **TSL 50 75 %** nebo **TSL 75 100 %** .

Toto jsou chyby, které se jasně mít, ne přechodná, proto pravděpodobně potřeba zásahu řešení. Dobrá zpráva je, že budou zatím nenastal zneplatnění životnost. Pokud neprodleně řešení těchto problémů a *před* dosáhla životnost zneplatnění, můžete restartovat replikace s minimálními ruční zásah.

Uvedených dříve, dlaždice řídicího panelu stavu replikace AD řešení zobrazuje počet *kritické* chyby replikace v prostředí, které je definována následujícím chyby, které jsou delší než 75 % zneplatnění životnost (včetně chyb, které jsou delší než 100 % TSL). Snažte se navázat uchovávat toto číslo 0.

>[AZURE.NOTE] Všechny výpočty procento zneplatnění životnost vycházejí životnost skutečné zneplatnění pro vaše domény Active Directory, kterým můžete důvěřovat tyto procent správnost, i když máte nastavenou hodnotu vlastní zneplatnění životnost.

### <a name="ad-replication-status-details"></a>Podrobnosti o stavu replikace AD
Když kliknete na tlačítko všechny položky v jednom ze seznamů, zobrazí se další podrobnosti o pomocí protokolu hledání. Výsledky jsou filtrované pouze chyb souvisejících s danou položku. Například, pokud kliknete na první doménovém uvedené v části **Stav serveru cíl (ADDC02)**, uvidíte výsledky hledání filtrované zobrazit chyby pomocí této domény řadiče vedená jako cílovém serveru:

![AD replikace stav chyby ve výsledcích hledání](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Tady můžete dál filtrovat, upravte dotaz hledání a podobně. Další informace o použití funkce vyhledávání protokolu najdete v článku [hledání protokolu](log-analytics-log-searches.md).

Pole **HelpLink** zobrazuje adresu URL webu TechNet stránky Další podrobné informace o této konkrétní chyby. Můžete zkopírovat a vložit odkaz do okna prohlížeče informace o řešení problémů a řešení chyby do schránky.

Můžete taky kliknout, **Exportovat** do exportovat výsledky do Excelu. Umožňuje vizualizovat data chyby replikace v tak, jak chcete.

![Exportovaná AD replikace stav chyb v aplikaci Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>Stav replikace AD časté otázky
**Otázka: jak často se aktualizace údaje o stavu AD replikace?**
Odpověď: informace se aktualizuje každých 5 dní.

**Otázka: je možné konfigurovat frekvenci aktualizací Tato data?**
Odpověď: v tuto chvíli ne.

**Otázka: je potřeba přidat všechny mé domény řadiče do pracovního prostoru Moje OMS abyste mohli vidět replikace stavu?**
A: Ne, musí být přidán pouze jednoho řadiče domény. Pokud máte víc řadiče domény v pracovním prostoru OMS data z nich jsou odeslány OMS

**Otázka: můžu nechcete přidat všechny řadiče domény do mé OMS pracovního prostoru. Použití stavu replikace AD řešení**
Odpověď: Ano. Můžete nastavit hodnoty klíče registru aby tato. V tématu [povolení není domény řadiče odešlete AD data OMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**Otázka: Jaký je název proces, který nemá shromažďování dat?**
Odpověď: AdvisorAssessment.exe

**Otázka: jak dlouho trvá pro se shromažďují?**
Odpověď: data kolekce čas závisí na velikosti prostředí služby Active Directory, ale obvykle trvá méně než 15 minut.

**Otázka: Jaký typ dat se shromažďují?**
Odpověď: replikace informace se shromažďují prostřednictvím protokolu LDAP.

**Otázka: je možné nastavit v případě se shromažďují data?**
Odpověď: v tuto chvíli ne.

**Otázka: jaká oprávnění potřebuji sběr dat?**
Odpověď: normální uživatelská oprávnění ke službě Active Directory jsou obvykle dostatečné.

## <a name="troubleshoot-data-collection-problems"></a>Poradce při potížích shromažďování dat
Abyste mohli shromažďovat data o pack řešení stav replikace AD vyžaduje aspoň jeden řadiče domény k připojení k OMS pracovního prostoru. Až to uděláte, zobrazí se zpráva s upozorněním, že **se stále probíhá údaje**.

Pokud potřebujete pomoc připojení mezi řadiče domény, můžete si přečtěte následující dokumentaci zobrazit ve [počítače se systémem Windows připojit k protokolu analýzy](log-analytics-windows-agents.md). Řadiče domény se už připojení k existující System Center Operations Manager prostředí, můžete můžete zobrazit si přečtěte následující dokumentaci při [Připojení operace správce pro System Center k protokolu analýzy](log-analytics-om-agents.md).

Pokud nechcete přímo OMS nebo SCOM jednotlivých řadiče domény připojit, najdete v článku [povolení není domény řadiče odešlete AD data OMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Další kroky

- Pomocí [protokolu vyhledávání v protokolu analýzy](log-analytics-log-searches.md) zobrazíte podrobné údaje o stavu replikace služby Active Directory.
