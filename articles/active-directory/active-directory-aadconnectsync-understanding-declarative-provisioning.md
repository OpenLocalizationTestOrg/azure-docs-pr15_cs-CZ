<properties
    pageTitle="Azure AD Connect synchronizace: Principy deklarativní zřizování | Microsoft Azure"
    description="Tento článek vysvětluje deklarativní zřizovací model konfigurace v Azure AD Connect."
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
    ms.date="08/29/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect synchronizace: Principy deklarativní zřízení
Toto téma vysvětluje model konfigurace v Azure AD Connect. Model se nazývá deklarativní zřizování a umožňuje změníte snadno konfiguraci. Hodně věcí popsaných v tomto tématu se Upřesnit a nejsou nutné pro většinu scénářů zákazníka.

## <a name="overview"></a>Základní informace
Deklarativní zřizování zpracovává objekty chodit ze zdrojového připojeného adresáře a určuje, jak objektů a atributů by měl být Transformovat ze zdroje na cíl. Objekt zpracovat v kanálu synchronizace a potrubí je stejný pro příchozí a odchozí pravidla. Pravidlo pro příchozí připojení, je z mezery spojnice metaverse a odchozího pravidla metaverse až mezerou spojnice.

![Synchronizace kanálem k odesílání zpráv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

Kanálu má několik různých moduly. Každý z nich je zodpovědný za jednoho pojmu v objektu synchronizace.

![Synchronizace kanálem k odesílání zpráv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

- Zdroje, zdrojový objekt
- [Obor](#scope)najde všechny synchronizace pravidla, která jsou v rozsahu
- [Připojení](#join), určuje relaci mezi prostor konektoru a metaverse
- [Transformace](#transform), jak by měl transformovat atributy vypočte a toku
- [Priorita](#precedence)odstraňuje konfliktní atribut příspěvků
- Cíl cílového objektu

## <a name="scope"></a>Rozsah
Vyhodnocení objektu a určuje pravidla, která jsou v rozsahu a měl by být součástí zpracování modul obor. V závislosti na hodnotách atributy objektu jsou různé synchronizace pravidla vyhodnocena jako v rozsahu. Například zakázáno s žádné poštovní schránky serveru Exchange uživatel různých pravidel než povolený uživatel s poštovní schránkou.  
![Rozsah](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Rozsah je definován jako skupiny a věty. Klauzule jsou uvnitř skupiny. Logické a používá mezi všechny klauzule ve skupině. Například (oddělení IT a zemí = = Dánsko). Logickým operátorem nebo se používá mezi skupinami.

![Rozsah](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Obor na tomto obrázku se považují za (oddělení IT a zemí = = Dánsko) nebo (země = Švédsko). Pokud je buď skupině 1 nebo 2 vyhodnocen jako true a pak pravidla je v rozsahu.

Modul obor podporuje tyto operace.

Operace | Popis
--- | ---
ZNAMÉNKO ROVNÁ NOTEQUAL | Porovnání řetězec, který bude vyhodnocen, pokud je hodnota rovna hodnotě atributu. S více hodnotami atributy najdete v článku ISIN a ISNOTIN.
LESSTHAN LESSTHAN_OR_EQUAL | Řetězec porovnat, který bude vyhodnocen, pokud je hodnota menší než hodnota hodnoty v atributu.
OBSAHUJE, NOTCONTAINS | Porovnání řetězec, který bude vyhodnocen, pokud hodnota najdete někde uvnitř hodnoty v atributu.
STARTSWITH NOTSTARTSWITH | Porovnání řetězec, který bude vyhodnocen, pokud je hodnota na začátku hodnoty v atributu.
ENDSWITH NOTENDSWITH | Porovnání řetězec, který bude vyhodnocen, pokud je hodnota na konci hodnoty v atributu.
GREATERTHAN GREATERTHAN_OR_EQUAL | Porovnání řetězec, který bude vyhodnocen, pokud je hodnota větší než hodnoty v atributu.
FUNKCE ISNULL ISNOTNULL | Vyhodnotí, jestli chybí atribut z objektu. Pokud atribut nejsou prezentovat a tedy null, pravidla je v rozsahu.
ISIN ISNOTIN | Vyhodnotí, pokud je argument hodnota účastní definovaný atribut. Tato operace je s více hodnotami variant rovná a NOTEQUAL. Atribut by měla být atribut s více hodnotami a pokud hodnotu najdete v žádné hodnoty atributu, klikněte na pravidlo v rozsahu.
ISBITSET ISNOTBITSET | Vyhodnotí, jestli je nastavená konkrétní bit. Například mohou sloužit k vyhodnocení bity řízení účtu uživatele zobrazíte-li uživatele povolit nebo zakázat.
ISMEMBEROF ISNOTMEMBEROF | Hodnota musí obsahovat DN do skupiny do pole spojnice. Pokud je členem skupiny určené objekt pravidlo je v rozsahu.

## <a name="join"></a>Spojení
Vyhledání vztah mezi objekt ve zdroji a objekt v cílovém zodpovídá modulu spojení v kanálu synchronizace. Na příchozí pravidla této relaci bude objekt na spojnici místo hledání relace k objektu v metaverse.  
![Spojení mezi cs a více hodnot](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Cílem je dosáhnout že pokud je objekt již metaverse vytvořil jiný spojnice by měl být spojené s. Například ve struktuře účtu zdroje uživateli struktuře účtu musí být vzájemně spojena s uživateli struktuře zdroje.

Spojení slouží převážně příchozí pravidla spojovat spojnice rozmístění objektů u jednoho objektu metaverse.

Spojení jsou definovány jako jednu nebo více skupin. Do skupiny máte klauzulí. Logické a používá mezi všechny klauzule ve skupině. Logickým operátorem nebo se používá mezi skupinami. Skupiny se zpracovávají v pořadí, odshora dolů. Nalezený jedné skupině obsahuje právě jeden ke shodě s objektu v cílovém, jsou vyhodnoceny žádná spojení pravidla. Pokud žádný nebo více než jednoho objektu nachází, zpracování nadále další skupiny pravidel. Z tohoto důvodu musí být pravidla vytvořené v pořadí nejčastěji explicitní první a další rozmazaný na konci.  
![Připojit se ke definice](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
Spojení na tomto obrázku se zpracovávají shora dolů. Nejdřív kanálu synchronizace uvidí, pokud se shoduje na číslo zaměstnance. V opačném případě druhé pravidlo uvidí název účtu lze nastavit spojovat objekty. Pokud to není shodu buď, pravidlo třetí a konečné odpovídá více rozmazaný pomocí jméno uživatele.

Pokud byla vyhodnocena všechna pravidla spojení a bude přesně jedna shoda, použije se **Typ vazby** na stránce **Popis** . Pokud je tato možnost nastavená **zřízení**, se vytvoří nový objekt v cílovém.  
![Poskytování nebo spojení](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Objekt pouze si měli jedno pravidlo jednoho synchronizace s pravidly spojení v rozsahu. Pokud existuje více pravidel synchronizace kde je definována spojení, dojde k chybě. Priorita neslouží k vyřešení konfliktů spojení. Objekt musí mít pravidlo spojení v rozsahu atributů plovoucí dlaždice se stejným směrem příchozí nebo odchozí. Pokud budete potřebovat atributy toku příchozích a odchozích u jednoho objektu, musíte mít příchozí a odchozí synchronizace pravidlo s spojení.

Odchozí připojení má speciální chování při pokusu o zřízení objekt, který bude mezerou cílový spojnice. Atribut DN slouží k Nejdřív zkuste obrácené pořadí spojení. Pokud je už objektu prostor konektoru cílové s stejný název domény, jsou spojeny objekty.

Modul spojení je vyhodnocen pouze po kdy nové pravidlo synchronizace stává obor. Když se připojil objektu, není odpojování i v případě, že již nesplňuje kritéria spojení. Pokud budete chtít disjoin objektu, musíte přejít synchronizace pravidlo, které připojen objekty mimo rozsah.

### <a name="metaverse-delete"></a>Odstranit Metaverse
Objekt metaverse zůstane dlouho co je, že jedno pravidlo synchronizace v rozsahu s **Typ vazby** nastavený na **poskytování** nebo **StickyJoin**. StickyJoin se používá při spojnice není povolená zřízení nový objekt, který metaverse, ale když se připojil, musí být odstraní ve zdroji před odstraněním metaverse objektu.

Při odstranění objektu metaverse všech objektů přidružených pravidlo odchozí synchronizace pro **poskytování** označené pro odstranění.

## <a name="transformations"></a>Transformací
Transformace umožňují určit, jak by měl atributy tok ze zdroje byste do cíle. Toků může mít jednu z následujících **typů toku**: přímo, konstanta nebo výraz. Přímé toku přecházel hodnotě atributu jako-se žádné další transformace. Konstantní hodnota nastaví určité hodnoty. Výraz použije deklarativní zřizovací jazyk výraz vyjádřit, jak mají být transformace. Podrobné informace o jazyce výrazu najdete v tématu [Principy deklarativní zřizovací jazyka](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) .

![Poskytování nebo spojení](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

Zaškrtávací políčko **použít jednou** Určuje, že atribut jen stanovit při vytvoření objektu. Například tuto konfiguraci lze nastavit počáteční heslo pro nový objekt uživatele.

### <a name="merging-attribute-values"></a>Sloučení hodnot atributů
Toky atribut je nastavení a zjistit, pokud s více hodnotami atributy by měly být sloučeny z několika různých spojnic. Výchozí hodnota je **aktualizace**, které označuje, že by měl win pravidlo synchronizace s nejvyšší prioritu.

![Sloučení typy](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Je také **Sloučit** a **MergeCaseInsensitive**. Tyto možnosti umožňují sloučení hodnot z různých zdrojů. Například jej lze sloučit atribut člena nebo proxyAddresses z několika různých doménových. Když použijete tuto možnost, musíte použít všechna pravidla synchronizace v rozsahu pro konkrétní objekt stejného typu hromadné korespondence. Nelze vytvářet **aktualizace** od jednu spojnici a **Sloučit** z jiného. Pokud se pokusíte, dojít k chybě.

Rozdíl mezi **sloučení** a **MergeCaseInsensitive** se dozvíte, jak zpracuje atribut duplicitní hodnoty. Modul synchronizace zajišťuje, aby duplicitní hodnoty nejsou vložená do cílového atributu. S **MergeCaseInsensitive**duplicitní hodnoty s pouze rozdíl v případě, že nebudou nacházet. Třeba byste neměli uvidíte obě "SMTP:bob@contoso.com" a "smtp:bob@contoso.com" v cílovém atributu. **Sloučení** je pouze prohlížíte přesné hodnoty a více hodnot pouze je rozdíl v případě se může vyskytovat.

Možnost **Nahradit** je stejná jako **aktualizace**, ale není použit.

### <a name="control-the-attribute-flow-process"></a>Ovládací prvek tok procesu atribut
Při nastavení více pravidel příchozí synchronizace přispívat na stejný metaverse atribut, priority slouží k určení nápoje. Pravidlo synchronizace s nejvyšší prioritou (nejnižší číselnou hodnotu) bude přispívat hodnotu. Stejný neděje odchozího pravidla Synchronizace pravidlo s nejvyšší prioritou wins nebo do nich přispívat hodnotu připojeného adresáře.

V některých případech spíše než přispívat určité hodnotě, pravidla synchronizace měli zjistit chování ostatní pravidla. Existují některé speciální literály použít pro tento případ.

Pro příchozí pravidla synchronizace literál **NULL** mohou sloužit k označení, že tok má žádnou hodnotu přispívat. Další pravidlo s nižší prioritou do ní můžete přidávat hodnotu. Když žádné pravidlo přispěla hodnotu, atribut metaverse zmizí. Odchozího pravidla Pokud **NULL** konečná hodnota po zpracování všech pravidel synchronizace pak hodnotu odebere se v adresáři připojení.

Literál typu **AuthoritativeNull** je podobný na **hodnotu NULL** , ale s tím rozdílem, že žádná nižší priorita pravidla do ní můžete přidávat hodnotu.

Tok atributů můžete také použít **IgnoreThisFlow**. Je podobný NULL smysl označující, že nemusíte už nic přispívat. Rozdíl je, že ho neodebírat již existující hodnotu v cílovém. Je jako tok atribut nebyl tam.

Tady je příklad:

V *na AD - hybridní nasazení Exchange uživatele* jsou k dispozici následující:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Tento výraz se považují za: Pokud poštovní schránky uživatele se nachází v Azure AD, pak tok atributu z Azure AD pro AD. V opačném případě nepostupují nic zpátky ke službě Active Directory. V tomto případě je by zachovat stávající hodnotě ve službě Active Directory.

### <a name="importedvalue"></a>ImportedValue
Funkce ImportedValue se liší od ostatních funkcí, protože název atributu musí být uzavřeny v uvozovkách spíše než hranatých závorek:  
`ImportedValue("proxyAddresses")`.

Obvykle při synchronizaci atribut používá očekávané hodnotě, i když není zatím exportovat nebo chybu byla přijata při exportu ("nahoře věžový"). Příchozí synchronizace předpokládá, že atribut, který není zatím nenastal postupně připojeného adresář dosáhne ho. V některých případech je důležité synchronizovat pouze hodnoty, které potvrzuje adresáři připojeného ("hologram a delta importovat věžový").

Příklad této funkce najdete v pravidlech synchronizaci mimo pole *v z AD – uživatele běžné ze serveru Exchange*. V hybridním Exchange přidané Exchange online by měl synchronizovat jen když bylo potvrzeno, aby byl úspěšně exportován hodnotu:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Priorita
Při pokusu o několik pravidel synchronizace přispívat stejnou hodnotu atributu byste do cíle, hodnotu priority slouží k určení nápoje. Pravidlo s nejvyšší prioritou nejmenší číselnou hodnotu, bude atribut konflikt přispívat.

![Sloučení typy](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Toto řazení lze definovat podrobnější toky atribut pro malou skupinu objektů. Například mimo z pole – pravidla zkontrolujte, že atributy z účtu povolené (**Uživatel AccountEnabled**) priorit z jiných účtů.

Je to možné definovat priorit mezi spojnicemi. Spojovací čáry, který umožňuje lepší daty přispívat hodnoty.

### <a name="multiple-objects-from-the-same-connector-space"></a>Více objektů na stejné místo spojnice
Pokud budete mít několik objektů na stejné místo spojnice připojený ke stejnému metaverse objektu, je třeba upravit priority. Pokud několika objekty jsou v rozsahu stejné pravidlo synchronizace, pak modul synchronizace nedokáže při určování priorit. Je nejednoznačných které zdrojový objekt by měl přispět hodnotu metaverse. Konfigurace hlásí nejednoznačných i v případě atributy ve zdroji stejné hodnoty.  
![Více objektů připojený ke stejnému objektu více hodnot](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

V tomto scénáři budete muset změnit rozsah synchronizace pravidla tak, aby objekty zdroje používat různé synchronizace pravidla v rozsahu. Který umožňuje definovat různých priorita.  
![Více objektů připojený ke stejnému objektu více hodnot](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Další kroky

- Další informace o jazyce výrazů ve [Výrazech Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Přečtěte si téma Jak deklarativní zřizování je použité mimo předdefinovaných v [Principy výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md).
- Postup provádění změn na praktické pomocí deklarativní zřizování v [tom, jak změnit výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md).
- Pokračujte ve čtení, jak uživatelé a kontakty Společná práce v [Principy uživatelů a kontakty](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Přehled témat**

- [Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace](active-directory-aadconnectsync-whatis.md)
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)

**Odkazy na témata**

- [Azure AD Connect synchronizace: Přehled funkcí](active-directory-aadconnectsync-functions-reference.md)
