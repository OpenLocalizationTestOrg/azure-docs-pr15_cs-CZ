<properties
    pageTitle="Azure AD Connect synchronizace: jak změnit výchozí konfigurace | Microsoft Azure"
    description="Provede vás jak měnit konfigurace v Azure AD Connect synchronizovat."
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
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD Connect synchronizace: jak změnit výchozí konfigurace
Tomto tématu účel vás provede Naučte se změnit výchozí konfigurace v Azure AD Connect synchronizovat. Obsahuje postup pro některé běžné scénáře. Replikace by měla některé jednoduché vlastního konfiguraci měnit podle obchodních pravidel.

## <a name="synchronization-rules-editor"></a>Editor pravidel synchronizace
Editor pravidel synchronizace slouží k zobrazení a změna výchozí konfigurace. Najdete ji v nabídce Start ve skupině **Azure AD Connect** .  
![Nabídka Start se Editor pravidel synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Když otevřete ho, zobrazí výchozí pravidla mimo pole.

![Editor pravidel synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigace v editoru
Rozevírací nabídky v horní části editoru umožňují rychle najít z určitého pravidla. Například pokud chcete zobrazit pravidla, kde je součástí atributu proxyAddresses, změníte rozevírací seznamy následujícím způsobem:  
![Filtrování SRE](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Pokud chcete obnovit, filtrování a načíst svěže konfiguraci, stiskněte klávesu **F5** na klávesnici.

Pokud chcete vpravo nahoře musíte na tlačítko **Přidat nové pravidlo**. Kliknutím na toto tlačítko slouží k vytvoření vlastního pravidla.

Dole máte tlačítka pro působí na vybrané synchronizace pravidla. **Úpravy** a **Odstranění** to, co můžete očekávat, aby. **Export** vytvoří skript Powershellu k opětovnému vytvoření pravidla synchronizace. Tento postup umožňuje přesunutí pravidla synchronizace z jednoho serveru.

## <a name="create-your-first-custom-rule"></a>Vytvoření prvního vlastního pravidla
Nejběžnější změnit, aby se změny toků atribut. Data v adresáři vašeho zdrojového nemusí být jako Azure AD. V příkladu v této části budete chtít Ujistěte se, že křestní jméno uživatele, vždy **velkými**písmeny na začátku.

### <a name="disable-the-scheduler"></a>Zakázání Plánovač
Ve výchozím nastavení spuštěna [Plánovač](active-directory-aadconnectsync-feature-scheduler.md) každých 30 minut. Chcete, aby zkontrolovala, jestli že není od při provádění změn a řešení potíží s novou pravidel. Dočasném vypnutí Plánovač, spusťte PowerShell a spuštění`Set-ADSyncScheduler -SyncCycleEnabled $false`

![Zakázání Plánovač](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Vytvoření pravidla

1. Klikněte na tlačítko **Přidat nové pravidlo**.
2. Na stránce **Popis** zadejte následující údaje:  
![Příchozí pravidla filtrování](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
    - Název: Zadejte popisný název pravidla.
    - Popis: Některé vysvětlení někoho jiného pochopit, co je pravidlo pro.
    - Připojení systém: objekt najdete v systému. V tomto případě jsme vyberte konektor služby Active Directory.
    - Spojení typu objektu systém/Metaverse: Vyberte **uživatele** a **osoby** .
    - Typ vazby: Tuto hodnotu změňte se chcete **Připojit**.
    - Priority: Zadejte hodnotu, která je v systému jedinečný. Nižší číselné hodnoty označuje vyšší prioritu.
    - Značka: Nechte prázdné. Pouze z krabice pravidla od Microsoftu, má toto políčko vyplněné s hodnotou.
3. Na stránce **Scoping filtru** zadejte **givenName ISNOTNULL**.  
![Příchozí pravidla definování rozsahu filtru](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
Tato část slouží k určit, které objekty, že by měl použít pravidlo. Pokud prázdné, by pravidla platí pro všechny objekty uživatelů. Ale, který by obsahoval konferenční místnosti, účtů služeb a jiných objektů uživatele – lidé.
4. Na **Připojit se ke pravidla**nechejte prázdné.
5. Na stránce **transformace** přejděte typ toku **výraz**. Vyberte cílový atribut **givenName**a ve zdroji zadejte `PCase([givenName])`.
![Transformace příchozí pravidla](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
Modul synchronizace je velká a malá písmena názvu funkce i název atributu. Pokud zadáte něco špatně, zobrazí se upozornění při přidat pravidlo. Editor umožňuje uložit a pokračovat, takže byste měli znovu otevřít pravidlo a opravte pravidlo.
6. Klikněte na **Přidat** do uložit pravidlo.

Nová vlastní pravidla by měly být viditelné s ostatními synchronizace pravidly v systému.

### <a name="verify-the-change"></a>Zkontrolujte změny
Nové změnu budete chtít zkontrolujte, jestli funguje očekávaným způsobem a není vyvolání všechny chyby. V závislosti na počtu objektů, které máte dvěma různými způsoby tento krok.

1. Spuštění úplnou synchronizaci všech objektů
2. Náhled a úplné synchronizaci spustit na jeden objekt

Spuštěním **Služby synchronizace** z nabídky start. Postup v této části jsou všechna tento nástroj.

1. **Úplné synchronizaci všech objektů**  
Vyberte **spojnic** nahoře. Určení konektoru jste udělali změnu v předchozí části, v tomto případě Active Directory Domain Services, a vyberte ho. Zaškrtněte políčko **Spustit** z akce a vyberte **Úplné synchronizaci** a **OK**.
![Úplné synchronizace](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
Objekty se teď automaticky aktualizují metaverse. Chcete podívejte se na objekt v metaverse.

2. **Náhled a úplné synchronizaci na jeden objekt**  
Vyberte **spojnic** nahoře. Určení konektoru jste udělali změnu v předchozí části, v tomto případě Active Directory Domain Services, a vyberte ho. Vyberte **místo vyhledávací spojnici**. Vyhledejte objekt, který chcete použít k testování změny pomocí obor. Vyberte objekt a klikněte na tlačítko **Náhled**. Na obrazovce nový vyberte **Potvrdit náhled**.
![Potvrzení náhledu](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
Změna teď zaručuje metaverse.

**Podívejte se na objekt v metaverse**  
Chcete vybrat několik objektů vzorku a zkontrolujte, zda že by měly hodnotu a použije pravidlo. Vyberte **Metaverse hledat** v horní. Přidejte všechny filtry potřebujete zjistit souvisejících objektů. Ve výsledcích hledání otevřete objekt. Podívejte se na hodnoty atributu a taky ověřit ve sloupci **Synchronizace pravidla** očekávaného pravidlo použito jako.  
![Hledání Metaverse](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  
### <a name="enable-the-scheduler"></a>Povolení Plánovač
Je-li vše očekávaným způsobem, můžete povolit Plánovač znovu. V prostředí PowerShell, spusťte `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Další běžné toku změny atribut
V předchozí části popsány Naučte se změnit atribut toku. V této části jsou uvedeny některé další příklady. Postup pro vytvoření pravidla synchronizace zkrácený, ale úplný postup najdete v předchozí části.

### <a name="use-another-attribute-than-the-default"></a>Použijte jiný atribut než výchozí
Na Fabrikam je strukturu použití místní abecedy křestní jméno, příjmení a zobrazované jméno. Znázornění znaků latinky tyto atributy najdete v atributy rozšíření. Při vytváření globální seznam adres v Azure AD a Office 365 organizace chce tyto atributy být použita.

Výchozí konfigurace objekt ve struktuře místní vypadat takto:  
![Atribut toku 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Pokud chcete vytvořit pravidlo s ostatních toků atribut, postupujte takto:

- Spusťte **Editor pravidlo synchronizace** z nabídky start.
- **Vstupní** stále vybrán vlevo klepněte na tlačítko **Přidat nové pravidlo**.
- Pravidla zadejte název a popis. Vyberte na adresářová služba Active Directory a typů příslušný objekt.  **Typ vazby**vyberte **Připojit se**. Priorita vyberte číslo, které se nepoužívá jiný pravidlo. Pravidla mimo pole začínat 100, takže můžete v tomto příkladě použita hodnota 50.
![Atribut toku 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
- Nechte prázdné oboru (to znamená má použít pro všechny uživatele objekty ve struktuře).
- Nechte prázdné pravidla spojení (to znamená, aby pravidlo mimo pole zpracování všech spojení).
- V transformace vytvořte následující toků:  
![Atribut toku 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
- Klikněte na **Přidat** do uložit pravidlo.
- Přejděte na **Správce služby synchronizace**. Na **spojovací čáry**vyberte spojnici, kde je teď nově přidaná pravidlo. Vyberte **Spustit**a **úplné synchronizaci**. Úplné synchronizaci přepočítá všechny objekty pomocí pravidel pro aktuální.

Toto je výsledek pro stejný objekt s vlastního pravidla:  
![Atribut toku 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Délka atributů
Řetězec atributy jsou ve výchozím nastavení je indexovanou a maximální délka je 448 znaků. Pokud pracujete s atributy řetězce, které může obsahovat více, ujistěte se, patří v toku atribut:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>Změna userPrincipalSuffix
Atribut userPrincipalName ve službě Active Directory vždy je neznámý uživateli a nemusí být vhodné jako ID přihlášení. Azure AD Connect, který umožňuje instalaci průvodce synchronizační vyberete jiné atribut, například e-mailový. Ale v některých případech se počítá atribut. Například společnosti Contoso obsahuje dva Azure AD adresáře, jeden pro výrobní a jeden pro účely testování. Mají uživatelé na svých test klientovi na doménu použít jiný příponu ID přihlášení  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

V tomto výrazu přijmout vše nalevo od prvního @-sign (Word) a concatenate pevné řetězcem.

### <a name="convert-a-multi-value-to-a-single-value"></a>Převést na hodnotu jednoduchým s více hodnotami
Některé atributy ve službě Active Directory jsou ve schématu s více hodnotami, přestože vypadají jediné, hodnotami v uživatele služby Active Directory a počítačů. Příklad je atribut popis.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

Výraz v případě, že atribut má určité hodnotě, jsme trvat první položku (položky) v atributu, odeberte úvodní a koncové mezery (PROČISTIT) a zachovat nejdřív 448 znaky (vlevo) v řetězci.

### <a name="do-not-flow-an-attribute"></a>Nepostupují atribut
Scénář pro tento oddíl, najdete v [ovládací prvek atribut tok procesu](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Existují dva způsoby není na plovoucí dlaždice atribut. První je k dispozici v Průvodci instalací a umožňuje [Odebrat vybrané atributy](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Tato možnost funguje v případě nikdy synchronizovali atribut před. Ale pokud jste zahájili synchronizovat tento atribut a odeberte ho později s touto funkcí, klikněte sync engine zarážkami, u kterých Správa atribut a existujících hodnot zbývají Azure AD.

Pokud chcete odstranit hodnotu atributu a zkontrolujte, jestli že není v budoucnu toku, budete potřebovat vytvoření vlastního pravidla.

Na Fabrikam můžeme mít realizují, že některé atributy, které jsme synchronizace s cloudem by neměly být k dispozici. Chceme, abyste měli jistotu, že tyto atributy jsou odebrali Azure AD.  
![Atributy Chybná přípona](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

- Vytvoření nového příchozí pravidla synchronizace a naplnění popis ![popisy](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
- Vytvoření atributové toků typu **výrazu** a se zdrojem **AuthoritativeNull**. Literál typu **AuthoritativeNull** označuje, že hodnota musí být prázdné v více hodnot i v případě nižší pravidlo synchronizace priority se snaží k naplnění hodnotu.
![Transformace rozšíření atributů](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
- Uložte pravidlo synchronizace. Spuštěním **Služby synchronizace**, najděte spojnice, vyberte **Spustit**a **Úplné synchronizaci**. Tento krok přepočítá všechny toky atribut.
- Ověřte, zda zamýšlené změny chcete exportovat hledáním místo spojnice.
![Fázovanou odstranit](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="next-steps"></a>Další kroky

- Další informace o konfiguraci modelu v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Další informace o jazyce výrazů ve [Výrazech Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Přehled témat**

- [Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace](active-directory-aadconnectsync-whatis.md)
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
