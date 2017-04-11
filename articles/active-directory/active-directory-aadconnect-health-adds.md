
<properties
    pageTitle="Pomocí Azure AD stavu připojení se službou AD DS | Microsoft Azure"
    description="Tohle je stránka stavu připojení Azure AD, který bude diskutovat o tom, jak sledovat služby AD DS."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Pomocí Azure AD stavu připojení se službou AD DS
Následující dokumentaci si specifické pro sledování Active Directory Domain Services s Azure AD stavu připojení. Jsou podporované verze služby AD DS: Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.

Další informace o sledování AD FS stavu připojení Azure AD najdete v článku [Použití Azure AD připojení stavu se službou AD FS](active-directory-aadconnect-health-adfs.md). Navíc informace týkající se sledování Azure AD Connect (synchronizace) s stavu připojení Azure AD najdete v článku [Použití Azure AD připojení stav synchronizace](active-directory-aadconnect-health-sync.md).

![Azure AD Connect stavu služby AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Upozornění pro Azure AD připojení stavu služby AD DS
V části upozornění v Azure AD připojení stavu služby AD DS obsahuje seznam aktivní a přeložena upozornění související s řadiče domény. Výběr aktivní nebo přeložena upozornění otevře nový zásuvné s dalšími informacemi, spolu s kroků řešení a odkazy na podklady. Každého typu upozornění můžete mít nejméně jedna instance, které odpovídají všech řadiče domény ovlivňují konkrétní oznámení. V dolní části upozornění zásuvné poklepáním na příslušné domény řadiče otevřete další zásuvné s další podrobnosti o oznámení instance.

V tomto zásuvné lze povolit e-mailová oznámení pro oznámení a změna časového rozsahu viditelné. Rozbalení časový rozsah vám umožní zobrazit předchozí přeložena upozornění.

![Chyby synchronizace Azure AD Connect](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Řídicí panel řadiče domény
Tento řídicí panel nabízí topologické zobrazení prostředí spolu s klíčové provozní metriky a stav každé sledované domény řadiče. Prezentovaný metriky pomoci rychlá identifikace žádné řadiče domény, které můžou vyžadovat další vyšetřování. Ve výchozím nastavení se zobrazí pouze podmnožinu sloupce. Však můžete najít celou sadu sloupce k dispozici, dvojitým kliknutím na příkaz sloupce. Výběr sloupců, které nejčastěji zajímavého, zapne tento řídicí panel do jednoho a snadno umístit do zobrazení stavu služby AD DS prostředí.

![Řadiče domény](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Řadiče domény můžete seskupené podle jejich jednotlivé domény nebo web, který je užitečné pro Principy topologii prostředí. Nakonec pokud poklikejte na zásuvné záhlaví na řídicím panelu maximalizuje využívat nemovitostí dostupná na obrazovce. Tento větší zobrazení je užitečné, když se zobrazí víc sloupců.

## <a name="replication-status-dashboard"></a>Řídicí panel stavu replikace
Tento řídicí panel poskytuje zobrazení stavu replikace a topologie replikace řadiče domény sledované. Stav posledních replikace pokus o koncovém spolu s užitečné si přečtěte následující dokumentaci pro všechny chyby, která se nachází. Poklikejte na položku řadiče domény zobrazí se chybová zpráva, jako například otevřete nový zásuvné s informacemi: Podrobnosti o chybě, doporučeno rozlišení kroky a odkazy na následující dokumentaci pro řešení potíží.

![Stav replikace](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Sledování
Tato funkce umožňuje grafické trendů různých výkonnosti, které jsou průběžně shromážděné ze všech řadiče domény sledované. Výkon řadiče domény můžete snadno porovnat všechny ostatní řadiče domény sledované v doménové. Kromě toho uvidíte různé výkonnosti vedle sebe, což je užitečné při řešení problémů s v prostředí.

![Sledování](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Ve výchozím nastavení jsme mít předvolena výkonnosti čtyři; však ostatní mohou obsahovat tak, že kliknete na příkaz Filtr a výběr nebo zrušení libovolné požadované výkonnosti. Kromě toho můžete poklikat graf čítač výkonu otevřete nový zásuvné, kterého patří datovým bodům všech sledované domény řadiče.

## <a name="related-links"></a>Související odkazy

* [Azure AD Connect stavu](active-directory-aadconnect-health.md)
* [Azure AD Connect instalaci agenta stavu](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect stavu operace](active-directory-aadconnect-health-operations.md)
* [Použití Azure AD stavu připojení se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Použití stavu připojení Azure AD pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect stavu časté otázky](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect historie verzí stavu](active-directory-aadconnect-health-version-history.md)
