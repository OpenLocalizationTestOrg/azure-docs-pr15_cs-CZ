<properties 
   pageTitle="Zabezpečení StorSimple | Microsoft Azure" 
   description="Popisuje funkce zabezpečení a ochrana osobních údajů, které chránit služby StorSimple, zařízení a data místně a v cloudu." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="SharS" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="05/03/2016"
   ms.author="v-sharos"/>

# <a name="storsimple-security-and-data-protection"></a>StorSimple zabezpečení a ochranu dat

## <a name="overview"></a>Základní informace

Pro každý, kdo přijala novou technologii, zejména při použití technologie se důvěrné nebo vlastních dat je hlavní zabezpečení. Při vyhodnocení různých technologií je třeba uvážit lepší rizika a náklady pro ochranu dat. Microsoft Azure StorSimple poskytuje zabezpečení a ochrany osobních údajů řešení pro ochranu dat pomáhají zajistit: 

- **Důvěrnost informací** – jen oprávněnými entity můžete zobrazit data. 
- **Integrita** – pouze oprávnění entity můžete změnit nebo odstranit data.

Řešení Microsoft Azure StorSimple se skládá ze čtyř hlavní součástí, které spolupracují s překrývajícími:

- **Služba StorSimple správce hostované v Microsoft Azure** – službu pro správu, které umožňují konfigurovat a zřízení StorSimple zařízení.
- **Zařízení StorSimple** – fyzické zařízení nainstalovaný ve vaší datacentra. Zařízení StorSimple tabulkami hosts a klienti, které mohou generovat dat připojit, a zařízení spravuje data a přesune ji do cloudu Azure podle potřeby.
- **Klienti/hosts připojené k zařízení** – klientech v infrastrukturu připojení k zařízení StorSimple a generovat data, která musí být chráněny.
- **Cloudového úložiště** – umístění v Azure cloudu, kam se ukládají data.

Následující oddíly popisují funkce StorSimple zabezpečení, které pomáhají chránit každý z těchto složek a dat uložených na nich. Obsahuje taky seznam otázek, které byste mohli Microsoft Azure StorSimple zabezpečení a odpovídající odpovědi.

## <a name="storsimple-manager-service-protection"></a>Ochrana StorSimple Správce služeb

Služba StorSimple správce je služba správy hostované v Microsoft Azure a sloužící ke správě všechna StorSimple zařízení, která obsahuje získané vaší organizace. Máte přístup ke službě StorSimple správce pomocí přihlašovacích údajů organizace pro přihlášení k portálu Azure klasické prostřednictvím webového prohlížeče. 

Přístup ke službě StorSimple správce vyžaduje vaše organizace Azure předplatné, které obsahuje StorSimple. Vaše předplatné řídí funkce, které se dostanete na portálu Azure klasické. Pokud vaše organizace nemá Azure předplatné a chcete se dozvědět víc o nich znáte, najdete v článku [registraci Azure jako organizace](../active-directory/sign-up-organization.md). 

Protože službu StorSimple správce je umístěn v Azure, je chráněn funkce Azure zabezpečení. Další informace o funkcích zabezpečení poskytovanou Microsoft Azure přejděte na [Centrum zabezpečení aplikace Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>Ochrana StorSimple zařízení

Zařízení StorSimple je místní hybridní paměťové zařízení, které obsahuje plné stavu jednotkách (disky SSD) a pevného disku (pevných disků), spolu s nadbytečné řadiče a automatické převzetí. Řadiče spravovat úložiště vrstvení, umístěním aktuálně používaných (nebo žádanou) dat v místním úložišti (v StorSimple zařízení nebo místních serverů), při přesunování méně častých dat do cloudu.

Pouze oprávnění StorSimple zařízení jsou povoleny k připojení ke službě StorSimple správce, který jste vytvořili v Azure předplatné. Abyste autorizovali zařízení, je nutné zaregistrovat ke službě StorSimple správce zadáním klíč registrace služby. Registrační klíč služby je 128bitové náhodné klíč generovaný v Azure klasické portálu. 

![Klíč registrace služby](./media/storsimple-security/ServiceRegistrationKey.png)

Se dozvíte, jak získat služby registračního klíče, přejděte na [Krok 2: získání klíč registrace služby](storsimple-deployment-walkthrough.md#step-2-get-the-service-registration-key).

Klíč registrace služby je dlouhý klíč, který obsahuje 100 + znaky. Můžete zkopírovat klávesu a uložit v textovém souboru na zabezpečeném místě, takže můžete povolit další zařízení podle potřeby. Pokud klíč registrace služby dojde ke ztrátě po registraci prvním zařízení, si můžete vygenerovat nový klíč z StorSimple Správce služby. Neovlivní to operace existující zařízení. 

Po registraci zařízení používá tokeny pro komunikaci s Microsoft Azure. Služba registrace není používá po zápisu zařízení.

> [AZURE.NOTE] Doporučujeme obnovit klíč registrace služby po každé použití.

## <a name="protect-your-storsimple-solution-via-passwords"></a>Ochrana řešení StorSimple pomocí hesla

Hesla jsou důležitým aspektem zabezpečení počítače a jsou používány v řešení StorSimple aby zajistil, že vaše data přístupný oprávněnými pouze na uživatele. StorSimple můžete nakonfigurovat následující hesla:

- Heslo správce zařízení StorSimple
- Ověřovací hesla vyzývající a cílové ověřování CHAP (Handshake Protocol)
- StorSimple snímek správce hesel

### <a name="windows-powershell-for-storsimple-and-the-storsimple-device-administrator-password"></a>Prostředí Windows PowerShell pro StorSimple a hesla správce zařízení StorSimple

Prostředí Windows PowerShell pro StorSimple je rozhraní příkazového řádku, který slouží ke správě StorSimple zařízení. Prostředí Windows PowerShell pro StorSimple obsahuje funkce, které umožňují zaregistrovat zařízení rozhraní network na svém zařízení nastavit, nainstalujte určitých typů aktualizace, Poradce při potížích s zařízení s přístupem k relaci podpory a změnit stav zařízení. Dostanete se k prostředí Windows PowerShell pro StorSimple připojením ke konzole sériové na zařízení nebo pomocí vzdálené prostředí Windows PowerShell.

Vzdálený PowerShell se teď dá přes HTTPS nebo HTTP. Pokud je Vzdálená správa přes protokol HTTPS zapnuto, bude potřebujete stáhnout certifikát Vzdálená správa ze zařízení a nainstalujte ji ve vzdáleném klientovi. Další informace o vzdálené Powershellu přejděte na [připojení vzdáleně do StorSimple zařízení](storsimple-remote-connect.md).

Po připojení k zařízení pomocí Windows Powershellu pro StorSimple, musíte heslo účtu správce zařízení se přihlaste k zařízení.

![Heslo správce zařízení](./media/storsimple-security/DeviceAdminPW.png)

Mějte na paměti následující doporučené postupy:

- Vzdálená správa je ve výchozím nastavení vypnuté. K tomu můžete službu StorSimple správce. Zabezpečení se doporučuje používat vzdálený přístup jenom během časové období, kdy je potřeba skutečně.
- Pokud změníte heslo, je potřeba informujte všichni uživatelé vzdáleného přístupu, aby nedochází ztrátou neočekávané připojení.
- Služba StorSimple správce nelze načíst existující hesla: ji můžete jenom obnovit. Doporučujeme, abyste tak, že nemáte k resetování hesla, pokud je zapomněli ukládat všechny hesla bezpečné místo. Pokud potřebujete k resetování hesla, ujistěte se, všem uživatelům oznamovat předtím, než ho obnovte. 

Máte přístup k rozhraní prostředí Windows PowerShell pomocí sériové připojení k zařízení. Můžete taky přistupovat ho vzdáleně pomocí protokolu HTTP nebo HTTPS, které umožňují větší zabezpečení. HTTPS představuje vyšší úroveň zabezpečení než sériové nebo připojení HTTP. Však použijete HTTPS, nejprve nainstalujte certifikát v klientském počítači, který bude přistupovat k zařízení. Na stránce konfigurace zařízení na službu StorSimple správce můžete stáhnout certifikát vzdáleného přístupu. Pokud dojde ke ztrátě certifikát pro vzdáleného přístupu, je nutné stáhnout nový certifikát a šířit na všechny klienty, kteří jsou oprávněni používat vzdálená správa.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Ověřovací hesla vyzývající a cílové ověřování CHAP (Handshake Protocol)

CHAP je režimu ověřování používané zařízení StorSimple k ověření identity vzdálené klienty. Ověření se podle sdílené heslo. CHAP může být jednosměrné (Jednosměrný) nebo vzájemné (obousměrné). S jednosměrné protokol ověří cílové (zařízení StorSimple) Autor (hostitel). Vzájemné nebo zpětného CHAP vyžaduje, aby cílové ověření vyzývající a poté vyzývající ověřte cíl. Vaše StorSimple je možné konfigurovat používat obě metody.

Uvědomte si následující při konfiguraci CHAP:

- Uživatelské jméno CHAP musí obsahovat méně než 233 znaků.
- Heslo CHAP musí být 12 až 16 znaků. Pokusíte použít delší uživatelské jméno nebo heslo způsobí selhání ověření na hostiteli Windows.
- Nelze použít stejné heslo pro vyzývající CHAP a cílové CHAP.
- Po nastavení hesla bude možné měnit, ale nebude načíst. Pokud se změní heslo, ujistěte se, všechny vzdálený přístup uživatelům oznamovat tak, aby se mohli úspěšně přihlásit k StorSimple zařízení.

Další informace o CHAP a jak ji nakonfigurovat pro vaše StorSimple řešení přejděte na [Konfigurovat CHAP StorSimple zařízení](storsimple-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>StorSimple snímek správce hesel

Správce snímek StorSimple je snap-in Microsoft Management Console (MMC), který používá ke generování konzistenci aplikací zálohování hlasitost skupiny a službě Windows hlasitost stín kopie. Kromě toho můžete StorSimple snímek správce vytvořit záložní plány a klonovat nebo obnovit svazky.

Když nakonfigurujete zařízení na používání StorSimple snímek správce, budete požádáni k zadání hesla správce StorSimple snímek. Toto je nejdřív nastavit heslo v prostředí Windows PowerShell pro StorSimple při registraci. Hesla můžete taky nastavit a změnil z službu StorSimple správce. Toto heslo ověří zařízení pomocí StorSimple snímek správce.

![StorSimple snímek správce hesel](./media/storsimple-security/SnapshotMgrPassword.png)

Heslo správce snímek StorSimple musí být 14 až 15 znaků a musí obsahovat 3 nebo vyšší kombinace velká, malá písmena, číselné a speciální znaky. Po nastavení hesla StorSimple snímek správce bude možné měnit, ale nelze načíst. Pokud změníte heslo, ujistěte se, všechny vzdálené uživatelům oznamovat.

Další informace o StorSimple snímek správce, přejděte na [Co je správce snímek StorSimple?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Doporučené postupy heslo

Doporučujeme, aby zajistil StorSimple heslech se pevné a dobře chráněn řiďte těmito pravidly:

- Změna hesla za tři měsíce. Změna hesla je nevynucují archivaci.
- Použití silných hesel. Další informace klikněte na [vytvořit silnější heslo a chránit](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
- Vždy používejte různá hesla pro různé přístup mechanismy; Každý hesel, které zadáte musí být v jedinečný.
- Nesdílet hesla s kýmkoli, kdo nemá oprávnění pro přístup k StorSimple zařízení.
- Prodiskutovat hesla před ostatními uživateli nebo pokyn v části formát hesla.
- Pokud máte podezření, že účet nebo heslo už ohroženo, Nahlaste incident se vaše oddělení zabezpečení informací.
- Všechna hesla považovat za citlivé důvěrné informace. 

## <a name="storsimple-data-protection"></a>Ochrana dat StorSimple

Tato část popisuje funkce zabezpečení StorSimple chránit data při přenosu šifrovaná a uložená data.

Způsobem popsaným v dalších částech hesla se používají k autorizace a ověření uživatele před můžete získat přístup k StorSimple řešení. Zabezpečení uvážit chrání data z neoprávněným uživatelům zatímco ho přenášena mezi systémy úložiště a zatímco ho uložena. Funkce pro ochranu dat s StorSimple k dispozici v následujících částech.

> [AZURE.NOTE] Deduplication poskytuje další ochranu dat uložených na zařízení StorSimple a v úložišti Microsoft Azure. Když deduplicated dat datové objekty, které se ukládají odděleně od metadatech, které slouží k mapování nebo k nim: je není k dispozici kontext úroveň úložiště obnovit dat na základě struktura svazku, systému souborů nebo název souboru.

## <a name="protect-data-flowing-through-the-service"></a>Ochrana dat předávaných prostřednictvím služby

Hlavním účelem StorSimple Správce služby je Správa a konfigurace StorSimple zařízení. Služba StorSimple správce spustí v Microsoft Azure. Pomocí portálu Azure klasické zadávání dat konfigurace zařízení a potom Microsoft Azure pomocí služby StorSimple správce odešlete data do zařízení. StorSimple používá systém dvojic asymetrické klíčů zajistit, že k vyzrazení službu Azure nemá za následek k vyzrazení ukládané informace. 

![Šifrování dat v letech](./media/storsimple-security/DataEncryption.png)

Asymetrické klíčové systémové pomáhá chránit data, která prochází službu následujícím způsobem:

1. Certifikát šifrování dat, který používá asymetrické veřejných a privátních klávesu se pár generováno na zařízení a slouží k ochraně data. Vygenerování klíčů při registraci první zařízení. 
2. Klíče dat šifrovacím certifikátu jsou exportovat do souboru osobních informací Exchange (.pfx), který je chráněn šifrovací klíč dat služby, což je klíči silných 128bitové, vytvořený náhodně první zařízení při registraci.
3. Veřejný klíč certifikátu bezpečně je k dispozici ve službě StorSimple správce a privátním klíčem zůstane se zařízením.
4. Data zadání službu je zašifrovat pomocí veřejného klíče a dešifrována pomocí privátním klíčem uložené v zařízení, zajistit, že služba Azure nemůže dešifrovat dat předávaných zařízení.

Služba dat šifrovací klíč generováno na pouze první zařízení registrovaný u služby. Všechny následující zařízení, které jsou registrované se službou používají stejný šifrovací klíč dat služby. 

> [AZURE.IMPORTANT] 
> 
> Je velmi důležité vytvořit kopii šifrovací klíč služby dat a jejich ukládání na zabezpečeném místě. Kopii služby dat šifrovací klíč by měl uložené v tak, že je přístupný oprávněnými uživatelem a můžete snadno sdělují Správce zařízení.
>
> Pokud dojde ke ztrátě šifrovací klíč data service, pracovníci technické podpory společnosti Microsoft můžete obnovit za předpokladu, že máte alespoň jeden zařízení v režimu online. Doporučujeme po načtením změnit šifrovací klíč dat služby. Pokyny přejděte k části [Změna šifrovací klíč dat služby](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Můžete změnit šifrovací klíč dat služby a certifikát pro šifrování odpovídající data tak, že vyberete možnost **změnit služby dat šifrovací klíč** na řídicí panel služeb. Abyste měli jistotu, že není ohroženo zabezpečení dat, musí používáte fyzickou StorSimple zařízení změnit šifrovací klíč dat služby. Změna šifrovacího klíče vyžaduje aktualizovat všechna zařízení s novým klíčem. Proto doporučujeme Změna klíče při online všech zařízeních. Pokud je zařízení offline, legendou lze změnit na jiný čas. Zařízení s zastaralé klávesami nebude moct spouštět zálohování, ale nebude je možné obnovit data, dokud se aktualizuje klávesu. Další informace přejděte na článek [použití řídicí panel služeb StorSimple správce](storsimple-service-dashboard.md).

Služba dat šifrovacího klíče a certifikát šifrování dat nevyprší. Doporučujeme ale změnit šifrovací klíč data service ročně pomáhající při ochraně narušení klíče zabezpečení.

## <a name="protect-data-at-rest"></a>Chránit data na ostatních

Zařízení StorSimple slouží ke správě dat ukládáním do úrovní místně a v cloudu, podle toho, četnost použití. Všechny počítače Host (hostitel), která jsou připojená k zařízení odeslání dat do zařízení, které pak slouží k přesunutí dat do cloudu, podle potřeby. Data jsou přenášena ze zařízení do cloudu bezpečně na Internetu. Každé zařízení má jeden cíl iSCSI, která vyvolá všechny sdílené svazky na toto zařízení. Všechna data musí být zašifrovaný před odesláním do úložiště v cloudu. 

![Cloudové úložiště šifrovacího klíče](./media/storsimple-security/CloudStorageEncryption.png)

Pokud chcete, aby o zabezpečení a integrity dat přesunout do cloudu, StorSimple lze definovat cloudové úložiště šifrovací klíč následujícím způsobem:

- Při vytváření kontejneru hlasitost zadat cloudové úložiště šifrovacího klíče. Klíč nelze upravit nebo přidat později. 
- Všechny svazky v kontejneru hlasitost sdílet stejný šifrovací klíč. Pokud budete potřebovat jiný tvar šifrování určitého svazku, doporučujeme vytvořit nové kontejner hlasitost hostovat svazku.
- Když zadáte šifrovací klíč cloudové úložiště ve službě StorSimple správce, musí být zašifrovaný klávesu pomocí veřejné části šifrovací klíč dat služby a potom odesílá do zařízení.
- Cloudové úložiště šifrovací klíč nebude uloženo kdekoli ve službě a se používá pouze k zařízení.
- Určení šifrovací klíč cloudové úložiště je nepovinný krok. Můžete odeslat data, která obsahuje zašifrovaný u hostitele zařízení.

### <a name="additional-security-best-practices"></a>Doporučené postupy pro větší zabezpečení

- Rozdělení přenosy: izolovat iSCSI SAN z provozu v podnikové síti LAN nasazení úplně samostatnému sítě a použitím VLAN kterou fyzické izolace není jednu z možností. Vyhrazené síť pro úložiště iSCSI bude zajistit bezpečnost a výkonu kritický pro obchodní data. Míchání úložiště a uživatel přenosy přes podnikové sítě LAN se nedoporučuje, můžete zvětšit latence a vést k chybám sítě.

- Zabezpečení straně hostitele sítě používejte sítě, které podporují TCP/IP převzít Engine (TOE). TOE zmenší zatížení procesoru zpracování TCP na síťový adaptér.

## <a name="protect-data-via-storage-accounts"></a>Ochrana dat pomocí úložiště účty

Každého předplatného Microsoft Azure můžete vytvořit jednu nebo více účtů úložiště. (Účet úložiště poskytuje jedinečných názvů pro práci s daty uloženými v Azure cloudu.) Přístup k účtu úložiště řídí předplatné a přístup klávesy přidruženým k tomuto účtu úložiště. 

Při vytváření účtu úložiště Microsoft Azure vygeneruje dva 512-bit úložiště přístupové klávesy, jedna z nich se používá k ověření StorSimple zařízení k účtu úložiště. Všimněte si, že pouze jeden z těchto klíčů používá. Další klávesy směřuje v rezervovat, umožňuje pravidelně otočení klíče. Chcete-li otočit klíčů, aktivovat sekundárního klíče a odstraňte primární klíč. Můžete vytvořit nový klíč pro použití při další otočení. (Z bezpečnostních důvodů mnoho datacentrech vyžadují střídání.) 

Doporučujeme, použijte tyto doporučené postupy pro střídání klíče:

- Otočíte úložiště účtu klávesy se šipkami pravidelně pomáhají zajistit, že není účtu úložiště používána neoprávněným uživatelům.
- Správce Azure měli pravidelně, změnit nebo obnovit klávesu primární a sekundární pomocí části úložiště portálu Azure klasické přímo přístup k účtu úložiště.


## <a name="protect-data-via-encryption"></a>Ochrana dat pomocí šifrování

StorSimple používá následující algoritmy šifrování chránit data uložená v nebo cestování mezi složkami StorSimple řešení.

| Algoritmus | Délka klíče | Protokoly/aplikace/komentáře |
| --------- | ---------- | ------------------------------- |
| RSA       | 2048       | Verze 1.5 RSA PKCS 1 používá Azure klasické portálu šifrování konfigurační data, která se pošle zařízení: například úložiště zadání přihlašovacích údajů, konfiguraci zařízení StorSimple účet a cloudového úložiště šifrovacího klíče. |
| AES       | 256        | AES s CBC slouží k šifrování veřejné části šifrovací klíč služby data před odesláním k portálu Azure klasické z StorSimple zařízení. Taky se používá zařízením StorSimple šifrování dat ke cloudové úložiště účtu se odesílá data. |


## <a name="storsimple-virtual-device-security"></a>StorSimple virtuální zařízení zabezpečení

[AZURE.INCLUDE [storsimple virtual device security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Nejčastější dotazy

Následuje několik otázky a odpovědi týkající se zabezpečení a Microsoft Azure StorSimple.

**Q:** Služba ohroženo. Co by měly být další kroky?

**A:** Měli byste okamžitě změnit šifrovací klíč dat služby a klávesy účtu úložiště pro úložiště účet, který se používá pro datové vrstvení. Pokyny najdete v tématu: 

- [Změna šifrovací klíč datové služby](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Klíčové otočení v prostoru úložiště účtů](storsimple-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**Q:** Mám nové StorSimple zařízení, které se s žádostí o klíči registrace služby. Jak ho můžu získat?

**A:** Tento klíč byla vytvořená při prvním vytvoření službu StorSimple správce. Pokud používáte službu StorSimple správce pro připojení k zařízení, můžete zobrazit nebo obnovit klíč registrace služby na úvodní stránku služby. Vytváření nových klíče registrace služby nemá vliv na existující registrovaných zařízení. Pokyny najdete v tématu:

- [Zobrazení nebo obnovit klíč registrace služby](storsimple-service-dashboard.md#view-or-regenerate-the-service-registration-key)

**Q:** Můžu ztratilo Moje služby dat šifrovacího klíče. Co mám dělat?

**A:** Kontaktujte podporu od Microsoftu. Můžete přihlásí k relaci podpory na zařízení a nápovědy načíst klíč (za předpokladu, že je online alespoň jeden zařízení). Ihned po získání šifrovací klíč dat služby změňte ji zajistit, aby nový klíč se používá jenom pro vás. Pokyny najdete v tématu:

- [Změna šifrovací klíč datové služby](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**Q:**  Můžu oprávnění zařízení pro změnu klíčové šifrování data service, ale nespustili procesu změnu klíče. Co mám dělat?

**A:** Pokud má vypršel časový limit, budou muset autorizovat zařízení pro změnu klíče šifrování dat služby a znovu spustíte proces.

**Q:**  Změnit šifrovací klíč data service, ale mám se nepodařilo aktualizovat jiných zařízeních v rámci 4 hodiny. Mám a začít znovu?

**A:** 4 hodiny časové období je pouze pro zahájení změny. Po zahájení procesu aktualizace na oprávněnými zařízení StorSimple povolení platí, dokud se automaticky aktualizují všech zařízeních.

**Q:** Náš StorSimple správce opustil společnosti. Co mám dělat?

**A:** Změna a resetování hesel, které umožňují přístup k zařízení StorSimple a změnit služby dat šifrovací klíč, který zajistí, že nové informace nezná neoprávněným. Pokyny najdete v tématu:

- [Změna hesla storsimple pomocí služby StorSimple správce](storsimple-change-passwords.md)
- [Změna šifrovací klíč datové služby](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
- [Konfigurace CHAP pro zařízení s StorSimple](storsimple-configure-chap.md)

**Q:** Chcete zadat heslo správce snímek StorSimple hostiteli, který se připojuje k zařízení StorSimple, ale heslo není k dispozici. Co mám dělat?

**A:** Pokud jste heslo zapomněli, byste měli vytvořit novou. Potom je potřeba informovat všechny existující uživatele změnu hesla a aby měli aktualizovat svoje klienty pomocí nového hesla. Pokyny najdete v tématu:

- [Změna hesla správce StorSimple snímku](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password)
- [Ověření zařízení](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**Q:** Certifikát pro vzdálený přístup k prostředí Windows PowerShell pro StorSimple změnil na zařízení. Jak aktualizovat Můj vzdálených klientů?

**A:** Můžete stáhnout nový certifikát z StorSimple Správce služby a zadejte ho do úložiště certifikátů svoje klienty vzdálený přístup je třeba nainstalovat. Pokyny najdete v tématu:

- [Rutina Import certifikátu](https://technet.microsoft.com/library/hh848630.aspx)

**Q:** Je chráněné když Moje datové StorSimple Správce služby ohroženo?

**A:** Konfigurace data služby vždy zašifrován veřejným klíčem při zobrazení ve webovém prohlížeči. Protože službu nebude mít přístup k privátním klíčem, nepůjde službu zobrazíte všechna data. Službu StorSimple správce ohroženo, dojde žádný vliv, protože neexistují žádné klíče uložené ve službě StorSimple správce.

**Q:** Pokud někdo získá přístup k šifrování certifikát dat, je bude Moje data být ohroženo?

**A:** Microsoft Azure ukládá zákazníka dat šifrovací klíč (soubor .pfx) ve formátu zašifrované. Protože soubor .pfx zašifrován a StorSimple služba nemá šifrovací klíč data service dešifrovat soubor .pfx, jednoduše získání přístupu k soubor .pfx nebudou žádné tajemství vystavit.

**Q:** Co se stane, když vládními entity zeptá Microsoft moje data?

**A:** Vzhledem k tomu všechna data, musí být zašifrovaný na službě a bude k dispozici privátním klíčem se zařízením, vládními entity musíte zeptat zákazníka data. 

## <a name="next-steps"></a>Další kroky

[Nasazení StorSimple zařízení](storsimple-deployment-walkthrough.md).
 
