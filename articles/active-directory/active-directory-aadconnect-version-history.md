<properties
   pageTitle="Azure AD Connect: Historie verzí verzi | Microsoft Azure"
   description="Toto téma obsahuje seznam všech verzích Azure AD Connect a Azure AD Sync"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Historie verzí verzi

Týmu Azure Active Directory se aktualizuje pravidelně Azure AD Connect s novými funkcemi a funkce. Ne všechny doplňky jsou k dispozici všechny cílové skupiny.

Tento článek je navržené tak, abyste mohli sledovat verze vydané a pokud chcete zjistit, jestli budete muset aktualizovat na nejnovější verzi nebo ne.

Toto je seznam souvisejících témat:

Téma |  
--------- | --------- |
Postup upgradu z Azure AD Connect | Způsoby [upgradu z předchozí verze na nejnovější](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect verzi.
Požadované oprávnění | Informace o oprávněních požadovaných pro použití aktualizace najdete v článku [účtů a oprávnění](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)
Ke stažení| [Připojení ke stažení Azure AD](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Uvolnění: srpen 2016

**Pevné problémy:**

- Změny synchronizovat interval nekoná až po dokončení další synchronizaci obrázku.
- Azure AD Connect Průvodce nepřijímá účet Azure AD jehož uživatelské jméno začíná podtržítko (\_).
- Azure AD Connect Průvodce nemůže ověřit účet Azure AD Pokud heslo účtu obsahuje moc speciální znaky. Chybová zpráva "nelze ověřit přihlašovací údaje. Neočekávané došlo k chybě." Vrátí se hodnota.
- Odinstalace přípravu serveru zakáže synchronizace hesel v Azure AD klienta a způsobí, že synchronizace hesel selhání s active server.
- Synchronizace hesel, se nezdaří v málo běžných případech je uložený na uživatele bez algoritmus hash hesla.
- Pokud server Azure AD Connect aktivní pracovní režimu, není dočasně zakázané zpětným heslo.
- Azure AD Connect průvodce nezobrazuje synchronizace aktuální heslo a heslo zpětným konfigurace při serveru v pracovní režimu. Vždy zobrazuje je jako zakázané.
- Změny konfigurace synchronizace hesel a heslo zpětným nejsou zachován průvodcem Azure AD Connect při serveru v pracovní režimu.

**Vylepšení:**

- Aktualizované Start ADSyncSyncCycle rutiny, aby byly označuje, zda je možné úspěšně spustit nový cyklus synchronizace nebo ne.
- Přidané rutina zastavit ADSyncSyncCycle ukončíte synchronizaci obrázku a operace, které právě probíhá.
- Aktualizované rutina zastavit ADSyncScheduler ukončíte synchronizaci obrázku a operace, které právě probíhá.
- Při konfiguraci [Rozšíření Directory](active-directory-aadconnectsync-feature-directory-extensions.md) v Průvodci Azure AD Connect, mohou být vybrány teď atribut Active Directory typu "Teletex řetězec".

## <a name="111890"></a>1.1.189.0
Uvolnění: 2016 dne

**Pevné problémy a vylepšení:**

- Azure AD Connect můžete teď měli mít nainstalovanou na serveru kompatibilní se standardem FIPS.
    - Synchronizace hesel najdete v článku [Synchronizace hesel a FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
- Oprava problému, které nelze přeložit název NetBIOS plně kvalifikovaný název domény v konektor služby Active Directory.

## <a name="111800"></a>1.1.180.0
Vydání: 2016 květen

**Nové funkce:**

- Zobrazí upozornění a pomůže vám ověřením domény, pokud jste se vlastně to udělat před spuštěním Azure AD Connect.
- Přidaná podpora [Německo cloudu společnosti Microsoft](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
- Přidaná podpora nejnovější [Microsoft Azure pro státní správu cloudové](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) infrastruktury požadavkům nová adresa URL.

**Pevné problémy a vylepšení:**

- Přidá filtrování do editoru pravidlo synchronizace tak, aby snadno najít synchronizace pravidla.
- Vyšší výkon při odstraňování mezerou spojnice.
- Pevná problémy při stejný objekt byl jak odstranění a přidání ve stejném spustit (označované jako odstranit/přidat).
- Zakázané pravidlo synchronizace se už znovu povolit zahrnuty objekty a atributy na upgrade nebo adresáře schématu aktualizace.

## <a name="111300"></a>1.1.130.0
Uvolnění: 2016 s dubnovou

**Nové funkce:**

- Přidaná podpora s více hodnotami atributů [Adresáře rozšíření](active-directory-aadconnectsync-feature-directory-extensions.md).
- Přidaná podpora pro další varianty konfigurace pro [Automatické upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) považuje za nárok na upgrade.
- Přidá některé rutiny pro [vlastní plánovač](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Vydání: 2016 březen

**Pevné problémy:**

- Udělali, že instalace Express nelze použít v systému Windows Server 2008 (pre R2) od synchronizaci hesel není podporována v operačním systému.
- Upgrade z DirSync s konfigurací vlastního filtru nefunguje očekávaným způsobem.
- Při upgradu na novější verzi žádné změny konfigurace, nemá naplánováno úplná import a synchronizace.

## <a name="111100"></a>1.1.110.0
Vydání: únor 2016

**Pevné problémy:**

- Instalace není ve složce výchozí **C:\Program** Files upgrade z předchozích verzí nefunguje.
- Pokud si nainstalujete a zrušte zaškrtnutí políčka **Spustit synchronizaci obrázku.** na konci Průvodce instalací opětovným spuštěním Průvodce instalací nepovolí plánovač.
- Plánovač nefunguje očekávaným na serverech, kde je formát data a času nejsou en US. Bude taky blokovat `Get-ADSyncScheduler` vrátit správné časy.
- Pokud jste nainstalovali dřívější verzi Azure AD Connect pomocí služby AD FS i na přihlášení možnost upgradu, nemůžete spustíte Průvodce instalací znovu.

## <a name="111050"></a>1.1.105.0
Vydání: únor 2016

**Nové funkce:**

- Funkce [Automatický upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) pro zákazníky nastavení Express.
- Podpora globální správce pomocí MFA a správce osobních informací v Průvodci instalací.
    - Budete muset povolit proxy taky umožňující přenosy https://secure.aadcdn.microsoftonline-p.com použijete MFA.
    - Budete muset přidat https://secure.aadcdn.microsoftonline-p.com do svého seznamu důvěryhodných webů pro MFA správně fungovat.
- Povolte změnu způsobu přihlášení uživatele po počáteční instalaci.
- Povolení [filtrování domény a OU](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) v Průvodci instalací. To umožňuje, aby připojení k strukturami, které jsou k dispozici všechny domény.
- [Plánovač](active-directory-aadconnectsync-feature-scheduler.md) je předdefinovaná modul synchronizace.

**Funkce stát z náhledu GA:**

- [Zpětného zápisu zařízení](active-directory-aadconnect-feature-device-writeback.md).
- [Rozšíření directory](active-directory-aadconnectsync-feature-directory-extensions.md).

**Nové funkce:**

- Nový výchozí synchronizovat obrázku, který interval je 30 minut. Používá 3 hodiny pro všechny dřívějších verzích. Přidá podporu postup změny chování [Plánovač](active-directory-aadconnectsync-feature-scheduler.md) .

**Pevné problémy:**

- Na stránce ověření DNS domény vždy nerozpoznala domény.
- Výzvy k zadání domény Správce pověření při konfiguraci služby AD FS.
- Místní účty AD nerozpoznala Průvodce instalací jednou členských zemí doménu s jinou strom DNS než kořenovou doménu.

## <a name="1091310"></a>1.0.9131.0
Uvolnění: 2015 prosinec

**Pevné problémy:**

- Synchronizace hesel nemusí fungovat při změně hesla ve službě AD DS, ale funguje při nastavení hesla.
- Pokud používáte proxy server, ověřování Azure AD může selhat během instalace nebo zrušením zaškrtnutí upgradu na stránce konfigurace.
- Aktualizace z předchozí verze Azure AD Connect s plné SQL Server selže, pokud nejste správce systému v jazyce SQL.
- Aktualizace z předchozí verze Azure AD Connect s vzdálené SQL serveru se zobrazí chyba "Nelze získat přístup k databázi ADSync SQL".

## <a name="1091250"></a>1.0.9125.0
Uvolnění: listopad 2015

**Nové funkce:**

- Můžete změnit konfiguraci služby AD FS na Azure AD důvěryhodnost.
- Můžete aktualizovat schématu služby Active Directory a obnovit synchronizace pravidla.
- Můžete zakázat synchronizace pravidla.
- Můžete definovat "AuthoritativeNull" jako nový literál v pravidlu synchronizace.

**Nové funkce:**

- [Azure AD připojení stav synchronizace](active-directory-aadconnect-health-sync.md).
- Podpora synchronizace hesel [Azure AD Domain Services](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) .

**Nový podporované scénář:**

- Podporuje více organizacemi místního serveru Exchange. Další informace najdete v tématu [hybridní nasazení s více struktur služby Active Directory](https://technet.microsoft.com/library/jj873754.aspx) .

**Pevné problémy:**

- Heslo synchronizací:
    - Objekt přesouvána z mimo rozsah v oboru nemá jeho heslo synchronizovat. Tento incudes OU a atribut filtrování.
    - Výběr nového OU zahrnout synchronní nevyžaduje synchronizace úplné hesel.
    - Je-li zakázané uživatelské povolena nesynchronizuje heslo.
    - Fronta Opakovat heslo je nekonečné a předchozí limit 5 000 objektů k vyřadit byla odebrána.
    - [Poradce při potížích s vylepšené](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Nemůžete se připojit ke službě Active Directory s úroveň doménové funkčnosti systému Windows Server 2016.
- Není možné změnit skupina pro filtrování skupin po počáteční instalaci.
- Už vytvořit nový uživatelský profil na server Azure AD Connect pro každého uživatele provádějícího změnu hesla s podporou zpětným heslo.
- Není možné použít dlouhé celé číslo hodnoty synchronní obory pravidla.
- Pokud jsou nedostupné domény řadiče zaškrtávací políčko "zpětného zápisu zařízení" zakázáno.

## <a name="1086670"></a>1.0.8667.0
Uvolnění: srpen 2015

**Nové funkce:**

- Průvodce instalací Azure AD Connect je teď lokalizované pro všechny jazyky systému Windows Server.
- Přidaná podpora účtu odemknout při použití Azure AD Správa hesel.

**Pevné problémy:**

- Průvodce instalací Azure AD Connect dojde k chybě, pokud jiný uživatel pokračuje instalace spíše než ten, kdo prvním spuštění instalace.
- Pokud předchozí odinstalovat dojde k chybě Azure AD Connect čistě odinstalovat Azure AD Connect synchronizace, není možné přeinstalovat.
- Nejde nainstalovat Azure AD Connect pomocí Express instalaci, pokud uživatel není kořenovou doménu struktuře nebo použít jiné než anglické verzi služby Active Directory.
- Pokud plně kvalifikovaný název domény Active Directory uživatelský účet nelze přeložit, zobrazí se zavádějící chybová zpráva "Se nepodařilo potvrdit schématu".
- Pokud účet použitý na konektor služby Active Directory dojde ke změně mimo průvodce, průvodce se nepovede na pozdější se spustí.
- Azure AD Connect někdy nedaří nainstalovat řadiče domény.
- Nelze povolit a zakázání "Pracovní režimu", pokud byly přidány atributy rozšíření.
- Heslo zpětným selže v některých konfiguraci z důvodu špatné heslo na konektor služby Active Directory.
- DirSync nejde upgradovat, když název domény je použita při filtrování atribut.
- Při použití resetování hesla nadbytečné využití procesoru.

**Funkce odebrané náhledu:**

- Funkce Zobrazit dočasně odebráno [zpětným uživatele](active-directory-aadconnect-feature-preview.md#user-writeback) na základě reakcí od zákazníků náhled. Bude znovu přidán později při jsme vyřešili ujednaných svůj názor.

## <a name="1086410"></a>1.0.8641.0
Uvolnění: 2015 dne

**Počáteční verzi Azure AD Connect.**

Změněné název z Azure AD Sync Azure AD Connect.

**Nové funkce:**

- [Expresní nastavení](./connect/active-directory-aadconnect-get-started-express.md) instalace
- Můžete [nakonfigurovat služby AD FS](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- Můžete [upgradovat z DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [Zabránit náhodných odstraní](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- Zavádí [přípravu režimu](active-directory-aadconnectsync-operations.md#staging-mode)

**Nové funkce:**

- [Uživatel zpětného zápisu](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Skupina zpětného zápisu](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Zpětného zápisu zařízení](active-directory-aadconnect-feature-device-writeback.md)
- [Rozšíření Directory](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Vydání: 2015 květen

**Nový požadavek:**

- Azure AD Sync nyní vyžaduje .net framework verze 4.5.1 je třeba nainstalovat.

**Pevné problémy:**

- Heslo zpětným z Azure AD se nezdařilo s chybou servicebus připojení.

## <a name="104910413"></a>1.0.491.0413
Vydání: 2015 duben

**Pevné problémy a vylepšení:**

- Konektor služby Active Directory nezpracuje odstraní správně, pokud je povoleno odpadkový koš a ve struktuře existuje víc domén.
- Výkon operace importu jsme vylepšili konektoru Azure Active Directory.
- Pokud skupiny překročila limit členství (ve výchozím nastavení limit je nastavena na 50k objekty), došlo k odstranění skupiny v Azure Active Directory. Nové chování je, že bude platit skupině, vyvolá se chyba a exportu žádné nové změny členství.
- Nový objekt nelze zřízení, pokud již existuje v prostoru spojnice fázované odstranit s stejný název domény.
- Některé objekty označené pro synchronizuje průběhu synchronizace delta sice stejně jako připravené na požadovaném objektu.
- Vynucení synchronizaci hesel, odebere se taky seznamu upřednostňovaný řadiče domény.
- CSExportAnalyzer má problémy s některými státy objekty.

**Nové funkce:**

- Spojení se můžete připojit k "ANY" typ objektu v více hodnot.

## <a name="104850222"></a>1.0.485.0222
Vydání: únor 2015

**Vylepšení:**

- Vylepšení importu výkon.

**Pevné problémy:**

- Synchronizace hesel respektuje atribut cloudFiltered používá atribut filtrování. Filtrované objekty již nebude v rozsahu synchronizaci hesel.
- Synchronizace hesel nefunguje v méně častých situacích kde topologii měli příliš mnoho řadiče domény.
- "Přerušili časově server" při importu z Azure AD spojnice po správy zařízení je povolená v Azure AD/Intune.
- Připojení ke cizí objekty zabezpečení (FSP) z víc domén ve stejné struktuře způsobí chybu nejednoznačného spojení.

## <a name="104751202"></a>1.0.475.1202
Uvolnění: prosince 2014

**Nové funkce:**

- Teď podporuje proveďte synchronizaci hesel s atribut na základě filtrování. Další podrobnosti najdete v tématu [Synchronizace hesel k filtrování](active-directory-aadconnectsync-configure-filtering.md).
- Atribut msDS-ExternalDirectoryObjectID zapisuje zpět na AD. Tím přidáte podpora pro aplikace Office 365 pomocí OAuth2 pro přístup k oběma Online a místních poštovních schránek v hybridním nasazení serveru Exchange.

**Pevné upgradu problémy:**

- Novější verze technologie Pomocníka pro přihlášení je dostupné na serveru.
- Vlastní instalační cesta byla použita k instalaci Azure AD Sync.
- Kritérium neplatné vlastní spojení blokuje upgradu.

**Další opravy:**

- Pevná šablon pro Office Pro Plus.
- Problémy při instalaci pevné způsobená jména uživatelů, které začínají písmenem pomlčku.
- Pevná ztráty nastavení sourceAnchor při spuštění Průvodce instalací ještě jednou.
- Pevné trasování událostí pro Windows trasování synchronizace hesel

## <a name="104701023"></a>1.0.470.1023
Uvolnění: říjen 2014

**Nové funkce:**

- Synchronizace hesel z více místní AD Azure AD.
- Lokalizované uživatelské rozhraní instalace pro všechny jazyky systému Windows Server.

**Upgrade z GA AADSync 1.0**

Pokud už máte nainstalovaný Azure AD Sync, je další krok, který budete muset udělat v případě, že jste změnili libovolnému pravidlu synchronizaci mimo pole. Po upgradu na 1.0.470.1023 uvolněte, je nutné synchronizace, které jsou duplicitní pravidla, které jste změnili. U každého změněné pravidla synchronizace postupujte takto:

- Vyhledejte pravidlo synchronizace upravili a poznamenejte si změny.
- Odstranění pravidla synchronizace.
- Vyhledejte nové pravidlo synchronizace vytvořil Azure AD Sync a znovu použít změny.

**Oprávnění pro účet AD**

Účtu AD musí být oprávnění další mohli číst hash heslo z AD. Oprávnění udělujete jsou s názvem "Replikace změny v adresářích" a "Replikace adresáře změny všechny". Obě oprávnění musí být moct číst hash hesla

## <a name="104190911"></a>1.0.419.0911
Uvolnění: září 2014

**Počáteční verzi Azure AD Sync.**

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
