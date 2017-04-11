
<properties
    pageTitle="Použití Azure AD stavu připojení se službou AD FS | Microsoft Azure"
    description="Tohle je stránka stavu připojení Azure AD sledování vaší místní infrastruktury služby AD FS."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-with-ad-fs"></a>Použití Azure AD stavu připojení se službou AD FS
Následující dokumentaci si specifické pro sledování infrastruktury služby AD FS s Azure AD stavu připojení. Informace o sledování Azure AD Connect (synchronizace) s stavu připojení Azure AD najdete v článku [Použití Azure AD připojení stav synchronizace](active-directory-aadconnect-health-sync.md). Kromě toho informace týkající se sledování Active Directory Domain Services s stavu připojení Azure AD najdete v tématu [Stavu připojení pomocí Azure AD pomocí služby AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>Upozornění na AD FS
V části Azure AD připojení upozornění stavu obsahuje seznam aktivní upozornění. Každý upozornění obsahuje příslušné informace, rozlišení kroky a odkazy na související si přečtěte následující dokumentaci.

Poklikejte na aktivní nebo přeložena upozornění, otevřete nový zásuvné s dalšími informacemi kroky, kterými můžete přeložit upozornění a odkazy na příslušné si přečtěte následující dokumentaci. Můžete taky zobrazit historických dat na upozornění, které byly přeložit v minulosti.

![Azure AD Connect portál stavu](./media/active-directory-aadconnect-health/alert2.png)



## <a name="usage-analytics-for-ad-fs"></a>Analýz použití pro službu AD FS
Azure AD připojení stavu analýzy využití analyzuje ověřování provoz federační servery. Poklikejte na pole analýzy využití a otevřete zásuvné technologie pro analýzu použití, které vidíte několika metrik a seskupení.

>[AZURE.NOTE] Použití analýzy použití se službou AD FS, musíte se ujistit, že je povolená auditování AD FS. Další informace najdete v tématu [Povolení auditování pro službu AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).

![Azure AD Connect portál stavu](./media/active-directory-aadconnect-health/report1.png)

Vyberte další nastavení, zadejte časový rozsah a změna seskupení, klikněte pravým tlačítkem na graf analýzy využití a vyberte upravit graf. Pak můžete zadat časový rozsah, vybrat jiné metriky a změna seskupení. Můžete zobrazit výskyt komunikace ověřování na základě jiné "metriky" a seskupení každý metriky s parametry relevantní "Seskupit podle" popsané v následující tabulce:

| Metriky | Seskupit podle | Jaké seskupení a proč je užitečné? |
| ------ | -------- | -------------------------------------------- |
| Součet žádosti: Celkový počet žádosti o zpracovány službou federace | Všechny | Zobrazení počtu celkový počet požadavků bez seskupování. |
|  | Aplikace | Celkový počet požadavků podle cílové řídicí strany skupin. Toto seskupení je užitečné, pokud chcete zjistit, kterou aplikaci dostává informace o kolik procento celkového přenos. |
|  | Server | Seskupení celkový počet požadavků založený na serveru, který zpracovány žádosti. Toto seskupení je užitečná pochopit distribuce zatížení celkový provoz. |
|  | Připojit se ke pracovišti | Seskupení celkový počet požadavků založené na tom, jestli jsou pocházejících z zařízení, které je připojen pracovišti (nazývané). Toto seskupení je užitečná pochopit, pokud jsou dostupné zdroje pomocí zařízení, které Neznámý infrastruktury identity. |
|  | Metody ověřování | Seskupení celkový počet požadavků založené na metodě ověřování použitého pro ověření. Toto seskupení je užitečná pochopit běžné metody ověřování, který se používá k ověření. Tady jsou metody možné ověřování <ol> <li>Ověřování systému Windows (Windows)</li> <li>Ověřování na základě ověřování (formuláře)</li> <li>Jednotné přihlašování (jednotné přihlašování)</li> <li>X509 certifikát ověřování (certifikát)</li> <br>Pokud na federační servery obdrží žádost o s soubor Cookie jednotné přihlašování, žádosti se počítá jako jednotné přihlašování (jednotného přihlašování). V těchto případech pokud platí soubor cookie uživatel není požádáni o zadání přihlašovacích údajů a získá Bezproblémová přístup k aplikaci. Toto chování se běžné, pokud máte víc předávající strany chráněné federačními servery. |
|  | Umístění v síti | Seskupení celkový počet požadavků na základě síťové umístění uživatele. Může být buď intranetové nebo extranetové. Toto seskupení je užitečné znát procento přenos pocházejících z intranetové versus extranetové. |
| : Celkový neúspěšný celkový počet Nezdařené požadavky Žádosti o zpracovány službou federace. <br> (Tento míru je k dispozici pouze na AD FS pro Windows Server 2012 R2)| Chyba typu | Zobrazí počet chyb založené na typech předdefinovanou chybovou. Toto seskupení je užitečná rozlišovat běžných chyb. <ul><li>Nesprávné uživatelské jméno a heslo: chyby kvůli nesprávné uživatelské jméno a heslo.</li> <li>"Extranetové uzamčení": k chybám kvůli přijaté od uživatele, který byl uzamčen z extranetové požadavky </li><li> "Platnost hesla": k chybám kvůli uživatelé protokolování pomocí vypršela platnost hesla.</li><li>"Zakázané účtu": k chybám kvůli uživatelé protokolování pod svým účtem zakázané.</li><li>"Zařízení ověřování": k chybám kvůli uživatelé selhání ověření pomocí ověřování zařízení.</li><li>"Ověřovací certifikát uživatel": k chybám kvůli uživatelé neúspěšný ověření z důvodu neplatný certifikát.</li><li>"MFA": k chybám kvůli neúspěšný ověření pomocí Vícefaktorové ověřování uživatele.</li><li>"Jiné přihlašovací údaje": "Vydávání se tak mohli ověřovat": k chybám kvůli k chybám ověření.</li><li>"Vydávání delegování": k chybám důsledku vydávání delegování chyb.</li><li>"Token přijetí": k chybám kvůli služby AD FS odmítnutí token z zprostředkovatele identit třetích stran.</li><li>"Protokol": Chyba důsledku protokol chyb.</li><li>"Neznámý stav": zachycení všechny. Jiné chyby, které nezapadají do definovaných kategorií.</li> |
|  | Server | Chyby založené na serveru. Toto seskupení je užitečná pochopit rozdělení chyby na serverech. Lichý rozdělení může být indikátor serveru ve vadná stavu. |
|  | Umístění v síti | Chyby založené na umístění v síti požadavků (intranet a extranetové). Toto seskupení je užitečná pochopit typ požadavky, které se nedaří. |
|  | Aplikace | Seskupení podle cílové aplikace (řídicí strany) k chybám. Toto seskupení využijete porozumět tomu, jaký cílovou aplikaci vidí většina počet chyb. |
| Průměrný počet jedinečných uživatelů v systému aktivní počet uživatelů: | Všechny | Tento míru obsahuje počet průměrný počet uživatelů pomocí služby federace ve vybrané časový úsek. Uživatelé nebudou seskupeny. <br>Průměr, závisí na časový úsek vybraná. |
|  | Aplikace | Seskupení průměrný počet uživatelů podle cílové aplikace (řídicí strany). Toto seskupení je užitečné, pokud chcete zjistit, kolik uživatelů používáte aplikaci, pro kterou. |


## <a name="performance-monitoring-for-ad-fs"></a>Sledování výkonu pro službu AD FS
Azure AD připojení stavu sledování výkonu týkající se sledování informací metriky. Podrobné informace o metriky zaškrtnete políčko sledování otevře nový zásuvné.


![Azure AD Connect portál stavu](./media/active-directory-aadconnect-health/perf1.png)


Výběrem možnosti filtr v horní části zásuvné můžete filtrovat podle serveru zobrazíte metriky jednotlivé serveru. Pokud chcete změnit nastavení, klikněte pravým tlačítkem myši na sledování grafu v části Sledování zásuvné a vyberte upravit graf. Potom můžete z nových zásuvné, které se objeví, vyberte další metriky z rozevíracího seznamu a zadejte časový rozsah zobrazení data o výkonu.

## <a name="reports-for-ad-fs"></a>Sestavy služby AD FS
Stav připojení Azure AD poskytuje sestav o zjišťování aktivity a výkonu služby AD FS. Tyto sestavy správcům získat přehled o aktivit na jejich servery služby AD FS.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>Prvních 50 uživatelů s selhalo přihlášení uživatelského jména a hesla

Jednou z běžné důvody pro požadavek neúspěšné ověření na server služby AD FS je žádost o s neplatné údaje, tedy nesprávné uživatelské jméno a heslo. Obvykle se stane s uživatelů z důvodu složitá hesla, zapomenutého hesla nebo překlepy.

Ale existují dalších důvodů, které mohou vést k neočekávané počet požadavků právě uskutečněných jednotlivými servery služby AD FS, jako například: aplikace, která přihlašovací údaje uživatele pro mezipaměti a přihlašovací údaje platnost nebo se zlými úmysly pokoušející se přihlaste se k účtu řadou známý hesla. Tyto dva příklady důvodů platné, které může vést k nárůstu v žádosti o.

Azure AD připojení stavu pro služby AD FS obsahuje zprávu o horní 50 uživatelů s neúspěšný pokus o přihlášení kvůli neplatné uživatelské jméno a heslo. Tuhle sestavu nedosáhnete zpracováním auditování události generované všechny servery služby AD FS v farmy

![Azure AD Connect portál stavu](./media/active-directory-aadconnect-health-adfs/report1a.png)

V této sestavě máte snadný přístup k následující části informací:

- Celkový počet neúspěšných požadavků s nesprávné uživatelské jméno a heslo v posledních 30 dní
- Průměrný počet uživatelů, které se nepodařilo s přihlášením špatné uživatelské jméno a heslo denně.


Kliknutím na tuto část přejdete na zásuvné hlavní sestavu, která poskytují další informace. Tento zásuvné obsahuje graf s trendu informací o kontaktech, vytvořte směrný plán o požadavcích s nesprávné uživatelské jméno a heslo. Dále poskytuje seznamu prvních 50 uživatelů počet neúspěšných pokusů.

Zobrazí se v grafu poskytuje následující údaje:

- Celkový počet neúspěšných přihlášení kvůli špatné uživatelské jméno a heslo na základě jednoho dne.
- Celkový počet jedinečných uživatelů, které se nepodařilo přihlášení na základě jednoho dne.
- Klient IP adresu pro poslední požadavek

![Azure AD Connect portál stavu](./media/active-directory-aadconnect-health-adfs/report3a.png)

Sestava obsahuje následující informace:

| Položky sestavy | Popis
| ------ | -------- |
|ID uživatele| Zobrazuje ID uživatele, který byl použit. Tato hodnota je, co uživatel zadal, které v některých případech je nesprávné uživatelské ID, který se používá.|
|Neúspěšné pokusy| Zobrazuje celkový počet neúspěšných pokusů ID tohoto určitého uživatele. Je tabulka seřazena s největší počet neúspěšných pokusů v sestupném pořadí.|
|Poslední chyba| Zobrazuje časové razítko poslední k chybě.
|Naposledy selhání IP | Zobrazuje klienta IP adresu z nejnovější chybný požadavek.|



>[AZURE.NOTE] Tuhle sestavu automaticky aktualizován po každé dvě hodiny nové informace shromažďované v té době. Pokus o přihlášení během posledních dvou hodin za následek, nemusí být součástí sestavy.



## <a name="related-links"></a>Související odkazy

* [Azure AD Connect stavu](active-directory-aadconnect-health.md)
* [Azure AD Connect instalaci agenta stavu](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect stavu operace](active-directory-aadconnect-health-operations.md)
* [Použití stavu připojení Azure AD pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Použití Azure AD stavu připojení se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect stavu časté otázky](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect historie verzí stavu](active-directory-aadconnect-health-version-history.md)
