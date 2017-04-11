Správa identit se stejně důležité v cloudu veřejné beze místně. Pokud chcete s tímto, Azure podporuje několik různých cloudu identity technologií. Jsou to například tyto:

- Spusťte Windows Server služby Active Directory (běžně označovanému jako právě AD) v cloudu pomocí virtuálních počítačích vytvářené pomocí Azure virtuálních počítačích. Tento přístup má smysl při práci v Azure pro rozšíření místních datacentra do cloudu.


- Azure Active Directory můžete dát svůj uživatelé jednotného přihlašování k [jako služba (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) aplikací. Společnosti Microsoft Office 365 používá technologii, například a spuštěné v Azure nebo dalších platformách cloudu můžete je použít i.


- Aplikace spuštěné v cloudu a místní slouží Azure Active Directory, řízení přístupu k protokolu umožňují uživatelům v používání identit z Facebooku, Google, Microsoft a ostatní zprostředkovatelé identit jiní.


Tento článek popisuje všechny tři z těchto možností.

## <a name="table-of-contents"></a>Obsahy

- [Spuštění služby Active Directory pro Windows Server na virtuálních počítačích](#adinvm)

- [Pomocí služby Azure Active Directory](#ad)

- [Pomocí řízení přístupu Azure Active Directory](#ac)


## <a name="adinvm"></a>Spuštění služby Active Directory pro Windows Server na virtuálních počítačích

Systém Windows Server AD v Azure virtuálních počítačích je podobná spuštění místní. [Obrázek 1](#fig1) ukazuje typický příklad toho, jak to vypadá.

![Azure Active Directory v virtuálního počítače](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Obrázek 1: Služby Active Directory pro Windows Server, můžete spustit v Azure virtuálních počítačích připojené k datacentru místní organizace pomocí Azure virtuální sítě.

V tomto příkladu běží Windows Server AD VMs vytvořené pomocí Azure virtuálních počítačích technologie IaaS platformy. Tyto VMs a dalších budou seskupené do virtuální sítě připojené k místní datacentru pomocí Azure virtuální sítě. Virtuální sítě odřízne skupiny cloudu virtuálních počítačích, které spolupracují s místní síti prostřednictvím virtuální privátní sítě (VPN) připojení. Udělat toto oprávnění umožňuje tyto Azure virtuálních počítačích vypadat jako jakékoli jiné podsítě místní datacentra. Jak je vidět na obrázku, dvě tyto VMs běží řadiče domény systému Windows Server AD. Virtuálních počítačích v virtuální sítě může běžet aplikacemi, například SharePoint nebo použit jiným způsobem, jako je třeba pro vývoj a testování. Místní datacentra běží také dva řadiče domény systému Windows Server AD.

Připojením řadiče domény v cloudu pomocí těm, kteří používají místně několika způsoby:

- Proveďte všechny uvedené v části domény služby Active Directory.

- Vytvoření samostatného AD domény místního i schránkami v cloudu, které jsou součástí struktuře stejné.

- Vytvoření samostatného strukturami AD v cloudu a místní, potom je připojíte strukturami pomocí vztahy mezi doménovými nebo Windows Server Active Directory Federation Services (AD FS), které můžete taky spustit ve virtuálních počítačích na Azure.

Jakékoli volby, správce zkontrolujte, že požadavky ověřování z místních uživatelů přejděte do cloudu domény řadiče pouze v případě potřeby, protože odkaz na cloud je pravděpodobně nižší než místní sítě. Dalším faktorem vzít v úvahu spojovací cloudu a místní domény řadiče je přenosy generovaný replikace. Jsou obvykle řadiče domény v cloudu vlastní AD webu, což umožňuje správcům naplánování četnosti replikace probíhá. Azure poplatky za přenosů z Azure datacentru, i když není bajtů Odeslaná pošta v, které by mohly ovlivnit možnosti replikace správce. Také stojí ukazující že během Azure podporují vlastní systému DNS (Domain Name), tuto službu chybí funkce požadované službou Active Directory (například podpora pro dynamické DNS a SRV záznamy). Z toho důvodu systémem Windows Server AD v Azure vyžaduje nastavení vlastního servery DNS v cloudu.

Systém Windows Server AD v Azure VMs můžete smysl v několika různých situacích. Tady je pár příkladů:

- Pokud používáte Azure virtuálních počítačích jako rozšíření vlastní datacentra, můžete narazit aplikace v cloudu, potřebujete místní domény řadiče zpracovávání co je třeba požadavky ověřování systému Windows nebo LDAP dotazů. SharePoint, například spolupracuje často se službou Active Directory a tak i když je možné spouštět farmě služby SharePoint na Azure pomocí místního adresáře, nastavte si řadiče domény v cloudu podstatně zvyšují výkon. (Je třeba si uvědomit, že to není potřeba, ale; hodně aplikace by umožnit spuštění úspěšně v cloudu pomocí pouze řadiče domény místního.)

- Předpokládejme, že vzdálených pobočkové nemá zdroje, které chcete spustit vlastní řadiče domény. V současné době jeho uživatelé musí ověřit řadiče domény na druhé straně světa: jsou přihlášení pomalé. Službou Active Directory v Azure ve bližší datacentru Microsoft můžete urychlit to aniž by bylo další servery na pobočce v.

- Organizace, která používá Azure havárie obnovení zachová kratších seznamů aktivní VMs v cloudu, včetně řadiče domény. Pak můžete ještě počítat rozbalte tento web v případě potřeby převzít chyb kdekoli jinde.

Existují i další možnosti. Například nejste požadované připojení systému Windows Server AD v cloudu pro místní datacentra. Pokud byste chtěli spustit farmě Sharepointu, který podávané množství konkrétní sadu uživatelů, například všechny kterým by přihlásit pouze s identitami cloudové, můžete vytvořit strukturu samostatného na Azure. Používání této technologie závisí na jaké své cíle. (Pro podrobnější informace o použití systému Windows Server AD s Azure, [najdete tady](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Pomocí služby Azure Active Directory

Jakmile aplikace SaaS k častěji, vyvolají zjevných Otázka: jaký druh adresářové služby tyto aplikace cloudového používejte? Odpověď společnosti Microsoft na tuto otázku je Azure Active Directory.

Používání této adresářové služby v cloudu dvěma způsoby:

- Jednotlivců a organizacím používajícím pouze SaaS aplikace můžete spolehnout Azure Active Directory jako jejich jediným adresářové služby.

- Organizace, na kterých běží Windows Server Active Directory můžete připojit jejich místního adresáře služby Azure Active Directory a potom použijte a udělte tak jejich uživatelé jednotného přihlašování k aplikacím SaaS.


[Obrázek 2](#fig2) ukazuje první z následujících dvou možností, kde Azure Active Directory tvoří všechna potřebného.

![Azure Active Directory v virtuálního počítače](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Obrázek 2: Azure Active Directory vám organizace uživatelé jednotného přihlašování k aplikacím SaaS, včetně služeb Office 365.

Jak je vidět na obrázku je Azure AD služba více klienta. To znamená, že současně podporuje mnoha různými způsoby uspořádání, ukládání adresáře informace o uživatelích na každý z nich. V tomto příkladu uživatel v organizaci A pokouší pro přístup k aplikaci SaaS. Tato aplikace může být součástí Office 365, například SharePoint Online, nebo je něco jiného - aplikací od jiných výrobců můžete také použít tuto technologii. Protože Azure AD podporuje protokol SAML 2.0, potřebného z aplikace, stačí možnost komunikovat pomocí tohoto standardní. (Ve skutečnosti aplikace, které využívají Azure AD by umožnit spuštění ve všech datacentru, nikoli pouze Azure datacentra.)

Při přístupu uživatele aplikace SaaS (krok 1), nebude zahájen procesu. Pokud chcete použít tuto aplikaci, uživatel musí poskytnout token vydán Azure AD.

Tento token obsahuje informace, který identifikuje uživatele a digitálně podepsaných tak, že Azure AD. Jak tokenu uživatel ověří sám Azure AD poskytnutím uživatelské jméno a heslo (krok 2). Potom Azure AD vrátí tokenu mu potřebuje (krok 3).

Tento token se přemístí SaaS aplikace (krok 4), která ověří tokenu podpisu a používá jeho obsah (krok 5). Aplikace se obvykle používají identity informace, které token obsahuje rozhodnout, jaké informace má uživatel přístup a případně jiným způsobem.

Pokud aplikace potřebuje více informací o uživateli než co je součástí tokenu, je to požádat přímo z Azure AD pomocí rozhraní API Azure AD grafu (krok 6). V první verzi Azure AD je velmi jednoduché schéma adresáře: obsahuje jenom uživatelé a skupiny a vztahy mezi nimi. Aplikace můžete použít tyto informace se naučit používat připojení mezi uživateli. Předpokládejme například, že aplikace je potřeba vědět, kdo je správcem tohoto uživatele se rozhodnout, jestli je povolený přístup k některé bloku data. Je to další pomocí dotazu Azure AD pomocí rozhraní API grafu.

Rozhraní API grafu používá běžnému RESTful protokol, který umožňuje snadno použít většina klientů včetně mobilní zařízení. Rozhraní API taky podporuje rozšíření definované OData přidáním akcí například což je dotazovací jazyk umožnit přístup k datům klientů zvýšíte jeho přínos způsoby. (Další informace o OData najdete v tématu [Představení OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Protože rozhraní API grafu můžete použít další informace o vztahy mezi uživateli, umožní aplikace pochopit sociální graf, který je vložen do schématu Azure AD pro konkrétní organizaci (což je proč nazývá rozhraní API grafu). A a dokončit ověření Azure AD pro rozhraní API grafu požadavky, aplikace používá OAuth 2.0.

Pokud organizace nepoužívá služby Active Directory pro Windows Server - nemá žádný místní servery nebo domény – a využívá výhradně cloudu aplikace, které používají Azure AD, používat jenom tento adresář cloudu by udělit podniku uživatelé jednotného přihlašování ke všem poznámkám. Ještě během tento scénář získá nejčastěji používaných každý den, většina organizací stále použít místní domény vytvořené službou Active Directory, Windows Server. Azure AD má užitečné roli, kterou chcete přehrát tady stejně, jak ukazuje [Obrázek 3](#fig3) .

![Azure Active Directory ve počítače virtuální](./media/identity/identity_03_AD.png)
<a id="fig3"></a>obrázek 3: organizace můžete vytvořit federaci systému Windows Server služby Active Directory s Azure Active Directory a udělte jeho uživatelé jednotného přihlašování k aplikacím SaaS.

V tomto scénáři chce uživatel v organizaci B přístup k aplikaci SaaS. Předtím ale, musí správci adresáře organizace Vytvořme relaci federaci s Azure AD pomocí služby AD FS, jak ukazuje obrázek. Tyto správci musíte taky nakonfigurovat synchronizaci dat mezi organizace místního systému Windows Server AD a Azure AD. To automaticky zkopíruje uživatele a informace o skupinách z místního adresáře Azure AD. Všimněte si to umožňuje: ve skutečnosti je organizace rozšíření jeho místního adresáře do cloudu. Kombinování systému Windows Server AD a Azure AD tímto způsobem vám bude radit organizace adresářová služba, která lze spravovat jako jedna entita při přetrvávají stopu obou místních i cloudových.

Použití Azure AD, nejdřív přihlásit na svůj místní domény Active Directory jako obvykle (krok 1). Když uživatel pokusí o přístup k aplikaci SaaS (krok 2), výsledkem procesu federace Azure AD jí vystavení tokenu pro tuto aplikaci (krok 3). (Další informace o fungování federace najdete v tématu [na základě deklarací Identity pro systém Windows: technologií a scénáře](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Dřív, tento token obsahuje informace, který identifikuje uživatele a digitálně podepsaných tak, že Azure AD. Tento token se přemístí SaaS aplikace (krok 4), která ověří tokenu podpisu a používá jeho obsah (krok 5). A je z předchozího scénáře SaaS aplikace můžete používat rozhraní API grafu zobrazíte další informace o tomuto uživateli Pokud potřeby (krok 6).

V současné době Azure AD není úplně nahradit místního systému Windows Server AD. Zmíněná už adresáři cloudu má mnohem jednodušší schéma a také chybí akcí například zásad skupiny, možnost uložení informací o počítačích a podpory pro LDAP. (Ve skutečnosti počítače se systémem Windows se nedají konfigurovat umožníte uživatelům Přihlaste se k němu nic ale Azure AD pomocí – to není podporováno.) Místo toho počáteční cíle Azure AD obsahuje nechat podnikových aplikací access uživatelé v cloudu bez udržovat samostatná přihlášení a uvolněním místního adresáře správci z místního adresáře ruční synchronizace s každé SaaS aplikace, kterou jejich organizace používá. V průběhu času však očekávají, že tento adresářové služby cloudu při řešení může používat větší okruh scénáře.

## <a name="ac"></a>Pomocí řízení přístupu Azure Active Directory

Cloudové identity technologie mohou sloužit k řešení velkého množství problémů. Azure Active Directory možnost poskytnout organizace uživatelé jednotného přihlašování ke několik SaaS aplikací, třeba. Ale technologií identity v cloudu lze také jiným způsobem.

Předpokládejme například, že aplikace chce svým uživatelům, aby se přihlaste pomocí tokeny vydané více *Zprostředkovatelé identit jiní (IdPs)*. Spoustu poskytovatelů jinou identitu existují dnes, včetně Facebook Google, Microsoft a ostatním, a aplikací často uživatelům přihlášení pomocí jedné z těchto identit. Proč má aplikace to vůbec měl chcete zachovat svůj vlastní seznam uživatelů a hesla při můžete místo toho spolehnout identity, které již existují? Přijetí existující identit usnadňuje životnost i pro uživatele, kteří mají jeden méně uživatelské jméno a heslo si zapamatovat, uživatelům vytvářet aplikace, který už není potřeba spravovat svoje vlastní seznamy uživatelská jména a hesla.

Ale při každé zprostředkovatele identit problémy určitý druh token, těchto tokenů nejsou standardní - každý IdP obsahuje vlastní formát. Kromě toho informace v těchto tokenů nechová standardní. Aplikace, která chce přijmout tokeny vydané vyslovte, Facebook, Google a Microsoft před sebou na výzvu psaní jedinečný kód pro zpracování všech těchto různé formáty.

Ale Proč to udělat? Proč nelze vytvořit místo toho zprostředkovatele, které mohou generovat jeden tokenu formátů s na běžné číselné informace o identitě? Tento přístup by zkontrolujte životnost jednodušší pro vývojáře, kteří vytvářejí aplikací, protože budou muset teď zpracovávat jediný druh tokenu. Azure Active Directory, řízení přístupu k tomu přesně, poskytnutí zprostředkovatele v cloudu pro práci s různorodého tokeny. [Obrázek 4](#fig4) ukazuje, jak to funguje

![Azure Active Directory v počítači virtuální](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>obrázek 4: Azure Active Directory, řízení přístupu usnadňuje aplikací přijmout identity tokeny vydán jinou identitu poskytovatelů.

Proces začne, když uživatel pokusí o přístup k aplikaci z prohlížeče. Aplikace přesměruje jí IdP podle svého výběru (a že aplikace taky považuje za důvěryhodné). Uživatel se ověří sebe na tento IdP jako je třeba po zadání uživatelského jména a hesla (krok 1) a IdP vrátí token obsahující informace o svém (krok 2).

Jak je vidět na obrázku podporuje řízení přístupu k oblasti jiné cloudové IdPs, včetně účtů vytvořil Google Yahoo, Facebook, Microsoft (dříve Windows Live ID) a kteréhokoliv poskytovatele OpenID. Podporuje také identity tvořena Azure Active Directory a prostřednictvím federace se službou AD FS, služby Active Directory pro Windows Server. Cílem je pokrýval nejčastěji používaná identit dnes, jestli máte vydané IdPs v cloudu a místní.

Jakmile uživatele prohlížeče token IdP ze svého zvolené IdP, odešle to tokenu řízení přístupu (krok 3). Řízení přístupu ověří tokenu, jak zajistit, že ho skutečně byl vydán tento IdP a potom vytvoří nový token podle pravidel definované pro tuto aplikaci. Jako Azure Active Directory řízení přístupu do více klienta služby, ale klienti aplikací spíše než zákazníka organizace. Každá aplikace dostane vlastní obor názvů, jak ukazuje obrázek a můžete definovat různých pravidla týkající se tak mohli ověřovat a další.

Tato pravidla, aby každý aplikace správce určit, jak se tokenů z různých IdPs transformovat do token řízení přístupu. Například pokud jiný IdPs používají odlišné typy představující uživatelská jména, pravidla řízení přístupu můžete převést všechny tyto do běžné zadejte uživatelské jméno. Řízení přístupu pošle tento nový token zpátky do prohlížeče (krok 4), která odešle žádost (krok 5). Jakmile bude mít token řízení přístupu, aplikace ověří, že tento token skutečně byl vydán řízení přístupu a potom používá informace, které obsahuje (krok 6).

Během tohoto procesu může zdát trochu složité, ve skutečnosti umožňuje životnost výrazně zjednodušuje poznámkové bloky aplikace. Namísto zpracovat různorodého tokeny obsahující různé jednotlivé informace, aplikace přijímat identit vydané více Zprostředkovatelé identit jiní při ještě přijímá jenom jeden token známé informacemi. Také spíše než vyžadují jednotlivé aplikace nakonfigurovat tak, aby zabezpečení různých IdPs, tyto vztahy důvěryhodnosti zachovány místo pomocí řízení přístupu: aplikace potřebujete jenom důvěřovat.

Je vhodné poukázání nic o řízení přístupu je svázané se Windows – může použít jenom taky aplikací Linux přijaté pouze identity Google a Facebooku. A i když je řízení přístupu je součástí řady Azure Active Directory, můžete si ho jako zcela odlišným služba z co je popsaný v předchozí části. Obě technologie práci s identit, budou adresy úplně různých problémů (i když Microsoft řečeno, které očekává integrovat dvou nastane okamžik)

Práce s identity je důležité téměř každé aplikaci. Cíl řízení přístupu je usnadněte tak vývojářům vytvářet aplikace, které přijímají identit z různých identity poskytovatelů. Vložením tuto službu v cloudu Microsoft má k dispozici pro všechny aplikace v libovolné platformy.

##<a name="about-the-author"></a>O autorovi

David Chappell je Chappell hlavní části a partnerů [www.davidchappell.com](http://www.davidchappell.com) v Kalifornie San Francisco.
