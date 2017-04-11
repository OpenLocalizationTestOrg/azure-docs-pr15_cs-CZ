<properties
    pageTitle="Azure AD Connect synchronizace: Konfigurace filtrování | Microsoft Azure"
    description="Vysvětluje, jak konfigurovat filtrování v Azure AD Connect synchronizovat."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect synchronizace: Konfigurace filtrování
Filtrování, můžete určit objektů, které by se měly v Azure AD z místního adresáře. Výchozí konfigurace vezme všech objektů ve všech domén v nakonfigurované doménových. Obvykle jde doporučená konfigurace. Koncoví uživatelé pomocí úloh Office 365, jako jsou oznámení Exchange Online a Skypu pro firmy, využívat dokončení globální seznam adres, můžete poslat e-mailu a všem. S nastavením výchozích by dostanou stejné možnosti, které by se implementaci místního serveru Exchange nebo Lync.

V některých případech je nutné udělat pár změn výchozí konfigurace. Tady je pár příkladů:

- Plánujete používat [více Azure AD adresáře topologie](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory). Je třeba použití filtru k ovládací prvek, který objekt mají být synchronizovány s konkrétní Azure AD adresář.
- Spuštění pilotního nasazení Azure nebo Office 365 a chcete jenom podmnožiny uživatelů v Azure AD. V malých pilot není důležité, abyste měli dokončení globální seznam adres prokázat funkci.
- Máte spoustu účtů služeb a jiných jiné než osobní účty, které nechcete v Azure AD.
- Dodržování předpisů důvodů neodstraníte všechny uživatelské účty místní. Pouze zakážete je. Ale v Azure AD chcete jenom aktivní účty nacházet.

Tento článek popisuje, jak nakonfigurovat způsoby filtrování.

> [AZURE.IMPORTANT]Společnost Microsoft nepodporuje změnu nebo operace Azure AD Connect synchronizaci mimo akcemi dříve popsány. Některé z těchto akcí, můžete mít stavu nekonzistentní nebo nepodporované Azure AD Connect synchronizace a v důsledku toho Microsoft neposkytuje technickou podporu takové nasazení.

## <a name="basics-and-important-notes"></a>Základní informace a důležité poznámky
Synchronizace Azure AD Connect můžete povolit filtrování kdykoli. Pokud začnete s výchozí konfigurace synchronizace adresářů a potom i konfiguraci filtrování, objekty, které jsou filtrovány synchronizovány Azure AD. Výsledkem této změny budou odstraněny všechny objekty v Azure AD, které byly dříve synchronizované, ale byly potom filtrováno v Azure AD.

Před provedením změn k filtrování, ujistěte se, že jste [Zakázat naplánovaný úkol](#disable-scheduled-task) tak, aby omylem neexportujete změny, které jste ještě neověřili správně.

Protože filtrování objekty můžete odstranit řadu ve stejnou dobu, budete chtít Ujistěte se, že začnete exportu změny Azure AD správnost nové filtry. Po dokončení kroků konfigurace důrazně doporučujeme provedení [kroků ověření](#apply-and-verify-changes) před exportovat a proveďte změny Azure AD.

Ochrana před odstraněním mnoho objektů omylem, funkce [Zabránění náhodné odstraníte](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) byl dokončen ve výchozím nastavení. Pokud byste odstranili mnoho objektů kvůli filtrování (500 ve výchozím nastavení), budete muset postupujte podle kroků v tomto článku umožňuje odstraníte projít Azure AD.

Pokud používáte sestavení před listopad 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), měnit filtr konfigurace a používání synchronizace hesel, budete muset po dokončení konfigurace aktivace úplné synchronizaci všech hesel. Postup, jak aktivovat úplné synchronizaci hesel v tématu [Aktivace úplné synchronizaci všech hesel](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Pokud jste na 1.0.9125 nebo novější, pak běžná **úplné synchronizaci** taky vypočítá Pokud se má synchronizovat hesla a tento krok navíc už není potřeba.

Objekty **uživatelů** byly omylem odstranili v Azure AD filtrování chyby, můžete znovu vytvořte objektů uživatele v Azure AD tím, že odeberete filtrování konfigurace a potom synchronizace adresářů služby. Tato akce slouží k obnovení uživatelů z odpadkového koše v Azure AD. Nelze však obnovit jiné typy objektů. Například Pokud omylem odstranění skupiny zabezpečení a byla použita k ACL zdroje, skupiny a jeho ACL nelze obnovit.

Azure AD Connect odstraní pouze objekty, které se má jednou považovány za obor. Pokud jsou objekty v Azure AD, které se vytvářely v jiné sync engine a tyto objekty není v rozsahu, přidání filtrování neodebírat je. Například pokud spustíte DirSync server a vytvořili úplnou kopii celý adresář v Azure AD a nainstalovat nový synchronizace server Azure AD Connect souběžně s povoleným od začátku filtrováním, ho neodebere navíc objekty vytvořené pomocí DirSync.

Filtrování konfigurace je nezachová instalace nebo upgrade na novější verzi Azure AD Connect. Vždycky je nejvhodnější pro ověření, že konfigurace nezměnilo omylem po upgradu na novější verzi před spuštěním první synchronizace cyklické.

Pokud máte víc než jeden struktury, musíte filtrování konfigurace popsané v tomto tématu použít pro každý struktury (za předpokladu, že se má stejné konfigurace pro všechny).

### <a name="disable-scheduled-task"></a>Zakázání naplánovaný úkol
Zakázání předdefinované Plánovač, která aktivuje cyklu synchronizace každých 30 minut, postupujte takto:

1. Přejděte prostředí PowerShell příkazový řádek.
2. Spuštění `Set-ADSyncScheduler -SyncCycleEnabled $False` zakázat plánovač.
3. Proveďte požadované změny, jak je uvedeno v tomto tématu.
4. Spuštění `Set-ADSyncScheduler -SyncCycleEnabled $True` Plánovač znovu povolit.

**Pokud používáte Azure AD Connect sestavení před 1.1.105.0**  
Pokud chcete zakázat naplánovaný úkol, který spustí synchronizaci obrázku každé 3 hodiny, postupujte takto:

1. **Plánovač úloh** spusťte v nabídce start.
2. Přímo pod **Knihovna plánovač úloh**najdete úlohu s názvem **Plánovač Azure AD Sync**, klikněte pravým tlačítkem myši a vyberte **Zakázat**.  
![Plánovač úkolů](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Nyní můžete provést změny konfigurace a ruční spuštění modul synchronizace z konzoly pro **Správce služby synchronizace** .

Po dokončení všech filtrování změny nezapomeňte pocházet zpět a **Povolení** úkol znova.

## <a name="filtering-options"></a>Možnosti filtrování
Filtrování těmihle konfigurace může být použity pro nástroj synchronizace adresářů:

- [**Skupina založená**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups): filtrování na základě jedné skupiny lze nakonfigurovat pouze na počáteční instalace pomocí Průvodce instalací. Další není uvedené v tomto tématu.

- [**Domén**](#domain-based-filtering): Tato možnost umožňuje vybrat které domény které synchronizovat Azure AD. Umožňuje taky přidat a odeberte domény z konfigurace engine synchronizovat, pokud uděláte změny infrastrukturu místní po instalaci Azure AD Connect synchronizace.

- [**Organizační jednotka – na základě**](#organizational-unitbased-filtering): Tato možnost filtrování umožňuje vybrat, ke kterým organizačních jednotek synchronizovat s Azure AD. Tato možnost je pro všechny typy objektů ve vybrané organizačních jednotek.

- [**Na základě atribut**](#attribute-based-filtering): Tato možnost umožňuje filtrovat objekty na základě hodnot atribut na objekty. Mohou také obsahovat různé filtry pro typy jiný objekt.

Použití více možností filtrování ve stejnou dobu. Například můžete na základě OU filtrování pouze zahrnout objektů v jedné organizační jednotce a na stejné čas na základě atribut filtrování filtrovat další objekty. Při použití více filtrování metod filtry použijte logické a mezi filtry.

## <a name="domain-based-filtering"></a>Domén filtrování
Tato část obsahuje kroky pro nastavení vaší domény filtr. Pokud jste přidali nebo odebrání domény v doménové po instalaci Azure AD Connect, máte taky aktualizovat filtrování konfiguraci.

Změna doménového filtrování upřednostňovaný způsob je spuštěním instalaci průvodce a změňte [doménu a organizačních jednotek filtrování](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Průvodce instalací je automatizace všechny úkoly popsané v tomto tématu.

Pokud z nějakého důvodu nejde spustit Průvodce instalací by měl pouze postupujte takto.

Domén filtrování nastavení se skládá z těchto kroků:

- [Vyberte domény](#select-domains-to-be-synchronized) , která by měla být součástí synchronizace.
- U každé domény přidané a odebrané upravte, [Spustit profily](#update-run-profiles).
- [Použít a zkontrolujte změny](#apply-and-verify-changes).

### <a name="select-domains-to-be-synchronized"></a>Vyberte možnost domény k synchronizaci
**Pokud chcete nastavit doménu filtr, proveďte následující kroky:**

1. Přihlaste se k serveru, na kterém běží Azure AD Connect synchronizace pomocí účtu, který je členem skupiny zabezpečení **ADSyncAdmins** .
2. Spuštěním **Služby synchronizace** z nabídky start.
3. Vyberte **spojovací čáry** a vyberte v seznamu **spojnic** spojnice typu **Active Directory Domain Services**. Z **Akce**vyberte **Vlastnosti**.  
![Vlastnosti konektoru](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klikněte na **Konfigurovat oddíly adresáře**.
5. V seznamu **Vybrat oddíly adresáře** vyberte a podle potřeby zrušte zaškrtnutí políčka domény. Ověřte, že nejsou vybrány pouze oddílů, které chcete synchronizovat.  
![Oddíly](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
Pokud jste změnili vaší místní AD infrastrukturu a domény přidané nebo odebranými z struktuře, potom klikněte na tlačítko **Aktualizovat** získat aktualizovaný seznam. Když aktualizujete, zobrazí se výzva k zadání přihlašovacích údajů. Poskytnout žádné přihlašovací údaje pro čtení na adresářová služba Active Directory. Nemá tak, aby uživatel, který je předem vyplněné v dialogovém okně.  
![Požadována aktualizace](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Až budete hotovi, kliknutím na **OK**zavřete dialogové okno **Vlastnosti** . Pokud odebrání domény z struktuře, místní nabídka zpráva oznamující, že byla odebrána doménu a tuto konfiguraci bude vyčistit.
7. Pokračujte v nastavení [spuštění profilů](#update-run-profiles).

### <a name="update-run-profiles"></a>Aktualizace profilů
Pokud jste aktualizovali filtru domény, budete potřebovat k aktualizaci profilů.

1. V seznamu **spojnic** zkontrolujte, že je vybraná konektoru jste změnili v předchozím kroku. Z **Akce**vyberte **Konfigurovat profilů**.  
![Spojnice spustit profilů](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

Potřebujete upravit následujících profilů:

- Úplný Import
- Úplné synchronizaci
- Delta Import
- Synchronizace delta
- Export

Pro každý z pěti profilů pomocí následujících kroků pro každou doménu **přidali** :

1. Vyberte spustit profilu a klikněte na **Nový krok**.
2. Na stránce **Konfigurace krok** v rozevíracím seznamu **Typ** vyberte typ kroku se stejným názvem jako profil, který budete konfigurovat. Klepněte na tlačítko **Další**.  
![Spojnice spustit profilů](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. Na stránce **Konfigurace spojnice** v **oddílu** rozevíracího seznamu, vyberte název domény, kterou jste přidali do vaší domény filtr.  
![Spojnice spustit profilů](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. Zavřete dialogové okno **Konfigurovat spouštění profilu** , klikněte na **Dokončit**.

Pro každý z pěti profilů pomocí následujících kroků pro každou doménu **Odebrat** :

1. Vyberte spustit profil.
2. Pokud je argument **hodnota** atributu **oddíl** identifikátor GUID, vyberte spustit kroku a klikněte na tlačítko **Odstranit krok**.  
![Spojnice spustit profilů](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

Výsledek třeba, aby každou doménu, kterou chcete synchronizovat by měl být vedená jako krokem při každém spuštění profilu.

Zavřete dialogové okno **Konfigurovat profilů** , klikněte na **OK**.

- Dokončete konfiguraci, [použít a zkontrolujte změny](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Organizační jednotka – na základě filtrování
Upřednostňovaný způsob, jak změnit na základě OU filtrování se spuštěním instalaci průvodce a změňte [doménu a organizačních jednotek filtrování](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Průvodce instalací je automatizace všechny úkoly popsané v tomto tématu.

Pokud z nějakého důvodu nejde spustit Průvodce instalací by měl pouze postupujte takto.

**Pokud chcete nakonfigurovat organizační jednotka – na základě filtrování, proveďte následující kroky:**

1. Přihlaste se k serveru, na kterém běží Azure AD Connect synchronizace pomocí účtu, který je členem skupiny zabezpečení **ADSyncAdmins** .
2. Spuštěním **Služby synchronizace** z nabídky start.
3. Vyberte **spojovací čáry** a vyberte v seznamu **spojnic** spojnice typu **Active Directory Domain Services**. Z **Akce**vyberte **Vlastnosti**.  
![Vlastnosti konektoru](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klikněte na **Konfigurovat oddíly adresáře**, vyberte doménu, kterou chcete konfigurovat a klikněte na **kontejnery**.
5. Po zobrazení výzvy, poskytněte žádné přihlašovací údaje pro čtení na adresářová služba Active Directory. Nemá tak, aby uživatel, který je předem vyplněné v dialogovém okně.
6. V dialogovém okně **Vybrat kontejnery** zrušte organizačních jednotek, které nechcete synchronizovat s adresářem cloudu a klikněte na **OK**.  
![ORGANIZAČNÍ JEDNOTKA](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - Kontejner **počítače** by měla být vybraná počítače Windows 10 můžete být úspěšně nový nebo aktualizovaný Azure AD. Pokud spravujete připojen počítače jsou umístěny v jiných organizačních jednotek, ujistěte se, že ty, které jsou vybrány.
  - Pokud máte víc strukturami vztahy důvěryhodnosti by měla být vybraná kontejneru **ForeignSecurityPrincipals** . Tento kontejner umožňuje mezi doménovými zabezpečení členství ve skupině přeložit.
  - Pokud jste povolili funkci zpětného zápisu zařízení by měla být vybraná **RegisteredDevices** OU. Pokud používáte jinou funkci zpětného zápisu, například zpětným skupiny, zaškrtněte políčko těmto místům jsou.
  - Vyberte další OU, kde jsou umístěny uživatelé objektů InetOrgPerson, skupiny, kontakty a počítačů. Na obrázku vše Toto jsou umístěny v ManagedObjects OU.
7. Až budete hotovi, kliknutím na **OK**zavřete dialogové okno **Vlastnosti** .
8. Dokončete konfiguraci, [použít a zkontrolujte změny](#apply-and-verify-changes).

## <a name="attribute-based-filtering"></a>Filtrování podle atributů
Ujistěte se, jsou na listopad 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) nebo novější sestavit pro tento postup pro práci.

Atribut na základě filtrování způsobem flexibilní filtr objekty. Výkon [deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md) umožňuje řídit téměř každý aspekt, kdy mají být objektu synchronizovány s Azure AD.

Filtrování se dají použít i na [příchozí](#inbound-filtering) z Active Directory, aby metaverse a [odchozí](#outbound-filtering) z metaverse Azure AD. Doporučujeme použít filtrování na příchozí vzhledem k tomu, je nejjednodušší vybrat chcete zachovat. Filtrování odchozí lze používat pouze v případě potřeby ke objekty z více doménové před hodnocení může proběhnout.

### <a name="inbound-filtering"></a>Příchozí filtrování
Příchozí na základě filtrování používá výchozí konfigurace kde objekty přejdete na Azure AD musí mít cloudFiltered atribut metaverse není nastavena na hodnotu k synchronizaci. Pokud Tenhle atribut je nastavena na hodnotu **True**, se nesynchronizuje objektu. Neměla být nastavena na **hodnotu False** záměrné. Aby zkontrolovala, jestli že ostatní pravidla dokážou přispívat hodnotu, Tenhle atribut pouze by měl obsahovat hodnoty **True** nebo **hodnotu NULL** (nepřítomnosti).

V příchozí filtrování pomocí power **obor** rozhodnout objektů, které měli nebo neměli synchronizovat. Je to, kde můžete provést úpravy přizpůsobit své vlastní organizační požadavky. Modul obor má **skupiny** a **klauzule** rozhodnout, pokud pravidlo synchronizace by měl být na rozsah. **Skupina** obsahuje jednu nebo více **klauzule**. Je logické a mezi více klauzulí a logické nebo více skupin.

Dejte nám prohlédněte příklad:  
![Obor](./media/active-directory-aadconnectsync-configure-filtering/scope.png) se považují za **(oddělení = IT) nebo (oddělení = prodeje a c = US)**.

Ve vzorcích a kroků pomocí objektu uživatele jako příklad, ale můžete použít pro všechny typy objektů.

V následujících vzorcích hodnotu priority začínat 500. Tato hodnota zajišťuje, že tato pravidla jsou vyhodnoceny pravidla mimo pole (nižší prioritou, vyšší číselnou hodnotu).

#### <a name="negative-filtering-do-not-sync-these"></a>Pole Záporná filtrování, "nelze synchronizovat tyto"
V následujícím příkladu je odfiltrovat (nejsou synchronizovány) všichni uživatelé kterém **extensionAttribute15** obsahují hodnotu **NoSync**.

1. Přihlaste se k serveru, na kterém běží Azure AD Connect synchronizace pomocí účtu, který je členem skupiny zabezpečení **ADSyncAdmins** .
2. Spusťte **Editor pravidel synchronizace** z nabídky start.
3. Ujistěte se, že je vybraná **vstupní** a klikněte na **Přidat nové pravidlo**.
4. Zadejte pravidlo popisný název, jako například "*v z AD – uživatele DoNotSyncFilter*". Vyberte správné doménové **uživatele** jako **Typ objektu CS**a **osoba** **Typ objektu více hodnot**. **Typ vazby**vyberte **připojení** a v pořadí priorit níže zadejte hodnotu aktuálně nevyužitá jiné synchronizace pravidlo (například 500) a klikněte na tlačítko **Další**.  
![Příchozí 1 popis](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. V **Scoping filtr**klikněte na tlačítko **Přidat skupinu**, klikněte na **Přidat klauzuli**a v atributu vyberte **ExtensionAttribute15**. Ujistěte se, že operátoru je nastavený na **stejné** a do pole Hodnota zadejte hodnotu **NoSync** . Klikněte na tlačítko **Další**.  
![Příchozí 2 oboru](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Nechte prázdné pravidla **připojení** a klikněte na tlačítko **Další**.
7. Klikněte na **Přidat transformace**vyberte **Typ toku** k **Konstanta**, vyberte cílový atribut **cloudFiltered** a do pole zdroje zadejte **hodnotu True**. Klikněte na **Přidat** do uložit pravidlo.  
![Příchozí 3 transformace](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Dokončete konfiguraci, [použít a zkontrolujte změny](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Filtrování, "synchronizovat pouze tyto" pozitiv
Vyjádření kladné filtrování lze náročnější, protože máte taky zvážit objekty, které nejsou zjevných stáhnout nový nebo aktualizovaný, například konferenční místnosti.

Kladné filtrování možnost vyžaduje dva synchronizace pravidla. Jeden nebo více zástupců správný rozsah objektů k synchronizaci a druhý skutečné all synchronizace pravidla tento filtr se všechny objekty, které dosud nebyly určeny jako objekt, který se má synchronizovat.

V následujícím příkladu synchronizovat pouze mají hodnoty **prodeje**atribut oddělení objektů uživatele.

1. Přihlaste se k serveru, na kterém běží Azure AD Connect synchronizace pomocí účtu, který je členem skupiny zabezpečení **ADSyncAdmins** .
2. Spusťte **Editor pravidel synchronizace** z nabídky start.
3. Ujistěte se, že je vybraná **vstupní** a klikněte na **Přidat nové pravidlo**.
4. Zadejte popisný název, třeba "*v z AD – prodejní uživatele synchronizovat*" pravidlo. Vyberte správné doménové **uživatele** jako **Typ objektu CS**a **osoba** **Typ objektu více hodnot**. **Typ vazby**vyberte **připojení** a v pořadí priorit níže zadejte hodnotu aktuálně nevyužitá jiné synchronizace pravidlo (například 501) a klikněte na tlačítko **Další**.  
![Příchozí 4 popis](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. V **Scoping filtr**klikněte na tlačítko **Přidat skupinu**, klikněte na **Přidat klauzuli**a v atributu vyberte **oddělení**. Ujistěte se, že operátoru je nastavený na **stejné** a do pole Hodnota zadejte hodnotu **Prodej** . Klikněte na tlačítko **Další**.  
![Příchozí 5 oboru](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Nechte prázdné pravidla **připojení** a klikněte na tlačítko **Další**.
7. Klikněte na **Přidat transformace**vyberte **Typ toku** k **Konstanta**, vyberte cílový atribut **cloudFiltered** a do pole zdroje zadejte **hodnotu False**. Klikněte na **Přidat** do uložit pravidlo.  
![Příchozí 6 transformace](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
Toto je speciální případu místo, kam jste cloudFiltered explicitně nastavena na hodnotu False.

    Teď máme vytvořit pravidlo skutečné all synchronizace.

8. Zadejte pravidlo popisný název, jako například "*v z AD – uživatele skutečné all filtr*". Vyberte správné doménové **uživatele** jako **Typ objektu CS**a **osoba** **Typ objektu více hodnot**. **Typ vazby**vyberte **připojení** a v pořadí priorit níže zadejte hodnotu aktuálně nevyužitá další synchronizaci pravidlo (například 600). Jste vybrali priorit hodnotu vyšší (nižší prioritu) než předchozí pravidlo synchronizace ale taky vlevo některé místnosti, jsme můžete přidat víc pravidel filtrování synchronizace později, až budete chtít spustit synchronizaci další oddělení. Klikněte na tlačítko **Další**.  
![Příchozí 7 popis](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. Nechte prázdné **Scoping filtru** a klikněte na tlačítko **Další**. Prázdný filtr označuje, že pro všechny objekty budou platit pravidlo.
10. Nechte prázdné pravidla **připojení** a klikněte na tlačítko **Další**.
11. Klikněte na **Přidat transformace**vyberte **Typ toku** k **Konstanta**, vyberte cílový atribut **cloudFiltered** a do pole zdroje zadejte **hodnotu True**. Klikněte na **Přidat** do uložit pravidlo.  
![Příchozí 3 transformace](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Dokončete konfiguraci, [použít a zkontrolujte změny](#apply-and-verify-changes).

Pokud potřebujete, můžete vytvořit více pravidel první typu kde obsahovat více objektů v naší synchronizace.

### <a name="outbound-filtering"></a>Odchozí filtrování
V některých případech je nutné provést filtrování až po objekty připojili v metaverse. Může být například povinný atribut pošta můžete visiové ze struktury zdrojů a od struktuře účtu a zjistit, pokud se má synchronizovat objektu atributu userPrincipalName. V těchto případech vytvoříte filtrování na odchozího pravidla.

V tomto příkladu změníte filtrování tak pouze uživatelé, kde pošta a userPrincipalName končit @contoso.com jsou synchronizovány:

1. Přihlaste se k serveru, na kterém běží Azure AD Connect synchronizace pomocí účtu, který je členem skupiny zabezpečení **ADSyncAdmins** .
2. Spusťte **Editor pravidel synchronizace** z nabídky start.
3. V části **Typ pravidla**klikněte na **odchozí**.
4. Najděte pravidla s názvem **na AAD – SOAInAD připojení uživatele**. Klikněte na **Upravit**.
5. V místní nabídce odpovězte **Ano** vytvořit kopii pravidla.
6. Na stránce **Popis** změňte přednost před nepoužitý hodnotu, například 50.
7. Na levém navigačním panelu klikněte na **Filtr Scoping** . Klikněte na **Přidat klauzule**, v atributu vyberte **Pošta**, vyberte operátor **ENDSWITH**a typ hodnoty **@contoso.com**. Klikněte na **Přidat klauzule**, vyberte atribut **userPrincipalName**, vyberte operátor **ENDSWITH**a typ hodnoty **@contoso.com**.
8. Klikněte na **Uložit**.
9. Dokončete konfiguraci, [použít a zkontrolujte změny](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Použití a ověřte změny
Po provedení změny konfigurace, musí být použity pro objekty již v systému. Také je možné, že má být zpracován objekty nesynchronizujete v modulu synchronizace a modul synchronizace musí číst zdrojového systému znovu a zkontrolujte jeho obsah.

Pokud jste změnili konfiguraci pomocí **domény** nebo filtrování **organizační jednotku** , budete muset udělat **úplné import** následovaný **Delta synchronizace**.

Pokud jste změnili konfigurace pomocí **atribut** filtrování, je potřeba udělat **úplné synchronizaci**.

Uděláte to takto:

1. Spuštěním **Služby synchronizace** z nabídky start.
2. Vyberte **spojnic** a v seznamu **spojnic** vyberte místo, kam jste udělali změnu dříve konfigurace spojnice. Z **Akce**zaškrtněte políčko **Spustit**.  
![Spojnice spustit](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. Do pole **Spustit profily**vyberte operace uvedené v předchozí části. Pokud potřebujete k spuštění dvě akce, spusťte druhé po dokončení první z nich (sloupci **Stav** je **nečinný** pro vybrané spojovací).

Po synchronizaci budou všechny změny připravené k exportu. Před skutečným proveďte požadované změny v Azure AD, který chcete ověřte správnost tyto změny.

1. Zahájení do příkazového řádku a přejděte na`%Program Files%\Microsoft Azure AD Sync\bin`
2. Spusťte:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Název konektoru najdete v synchronizaci služby. Obsahují podobná "contoso.com – AAD" název pro Azure AD.
3. Spusťte:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Nyní máte otevřený soubor ve složce % temp % s názvem export.csv, který může být zkontrolován v aplikaci Microsoft Excel. Tento soubor obsahuje všechny změny, které chcete exportovat.
5. Proveďte potřebné změny dat nebo konfiguraci a spuštění tento postup opakujte (Importovat synchronizovat a ověření) až do změny, které mají být exportována očekává.

Jakmile budete spokojeni, exportujte do Azure AD změny.

1. Vyberte **spojnic** a vyberte v seznamu **spojnic** Azure AD spojnice. Z **Akce**zaškrtněte políčko **Spustit**.
2. Do pole **Spustit profily**zaškrtněte políčko **Exportovat**.
3. Pokud změny konfigurace odstranili mnoho objektů, potom se zobrazí chyba ve exportovat u je číslo větší než nakonfigurované mezní hodnota (ve výchozím nastavení 500). Pokud se zobrazí tato chyba, budete muset dočasně vypnout funkci [Zabránění náhodných odstraní](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md).

Teď je čas na Plánovač znovu povolit.

1. **Plánovač úloh** spusťte v nabídce start.
2. Přímo pod **Knihovna plánovač úloh**najdete úlohu s názvem **Plánovač Azure AD Sync**, klikněte pravým tlačítkem myši a vyberte **Povolit**.

## <a name="next-steps"></a>Další kroky
Další informace o konfiguraci [Azure AD Connect synchronizovat](active-directory-aadconnectsync-whatis.md) .

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
