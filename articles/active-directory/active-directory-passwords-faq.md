<properties
    pageTitle="Nejčastější dotazy: Azure AD heslo správy | Microsoft Azure"
    description="Časté otázky ke ke správě heslo v Azure AD, včetně resetování hesla, registrace, sestavy a zpětným místní služby Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="password-management-frequently-asked-questions"></a>Správa hesel nejčastější dotazy

> [AZURE.IMPORTANT] **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).

Následující seznam uvádí některé často kladené otázky pro všechno, co související se správou heslo.

Pokud s otázku, na kterou Nevím odpověď na nebo hledáte pomoc s určitého problému, které jsou protilehlé najít sami, můžete přečíst na pod zobrazíte, pokud jsme jste nichž se uplatní ji už.  Pokud nám to ještě neudělali, Nedělejte si starosti! Neváhejte všechny zeptejte se tam máte, které není popsaná v [Azure AD fóra](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) a jsme se vrátit k se hned, jak můžeme.

V tomto článku je rozdělen do následujících částí:

- [**Dotazy týkající se registrace resetování hesla**](#password-reset-registration)
- [**Dotazy týkající se resetování hesla**](#password-reset)
- [**Dotazy týkající se sestavy správy hesel**](#password-management-reports)
- [**Dotazy týkající se zpětným heslo**](#password-writeback)

## <a name="password-reset-registration"></a>Registrace resetování hesla
 - **Otázka: můžou svoje uživatele zaregistrovat svá data resetovat heslo?**

 > **A:** Ano, dokud je povolený resetování hesla a mají licenci, budou můžete přejít na portál registrace resetování hesla na http://aka.ms/ssprsetup zaregistrovat jejich ověřovací informace pro použití se resetování hesla. Uživatelé mohou také zaregistrovat přejdete na panelu aplikace access na http://myapps.microsoft.com, kliknutím na kartu profilu a kliknutím na webu Register pro resetování hesla možnost. Další informace o tom, jak získat uživatelé nakonfigurovány v tématu Jak získat uživatelé nakonfigurován pro resetování hesla pro vytvoření nového hesla.

 - **Otázka: můžou definovat data resetovat heslo jménem svoje uživatele?**

 > **A:** Ano, můžete to udělat pomocí DirSync nebo prostředí PowerShell nebo prostřednictvím portálu [Portálu Správa Azure](https://manage.windowsazure.com) nebo správce služeb Office. Další informace o této funkci v příspěvku na blogu zlepšení ochrany osobních údajů pro Azure AD MFA a telefonní čísla resetovat hesla a čtením zjistěte, jak data slouží heslo obnovit.

 - **Otázka: můžou synchronizovat data týkající se zabezpečení z místní?**

 > **A:** Ne, není možné dnes, ale nemůžeme jsou zvažovat.

 - **Otázka: můžou zaregistrovat svoje uživatele data tak, aby ostatní uživatelé nevidí tato data?**

 > **A:** Ano, když uživatelé zaregistrovat dat na portálu heslo resetovat registrace se uloží do soukromé ověřování polí, které jsou viditelné pouze globální správci a sám uživatel. Další informace o této funkci v příspěvku na blogu zlepšení ochrany osobních údajů pro Azure AD MFA a telefonní čísla resetovat hesla a čtením zjistěte, jak data slouží heslo obnovit.

 - **Otázka: Můj uživatelé mají zaregistrovat před použitím resetovat heslo?**

 > **A:** Ne, pokud na jejich jménem definujete dost ověřovací údaje, uživatelé nebudou mít k registraci. Resetování hesla bude správně fungovat, dokud správně formátovány dat uložených do příslušných polí v adresáři. Přečtěte si další informace o pomocí čtení zjistěte, jak data slouží resetování hesla.

 - **Otázka: můžou synchronizovat nebo nastavit telefonní ověřování, ověření e-mailu nebo alternativní ověřování telefonní pole jménem svoje uživatele?**

 > **A:** Nesynchronizujete ale nemůžeme zvažovat povolení této možnosti.

 - **Otázka: jak portálu registrace vědět, jaké možnosti zobrazit svoje uživatele?**

 > **A:** Na portálu registrace resetovat heslo jenom zobrazení možností, že jste povolili pro vaše uživatele do adresáře konfigurovat karty v části zásad resetování uživatelského hesla. To znamená, že pokud nepovolíte, například dotazy zabezpečení, pak uživatelé nebudou moct zaregistrujte si tuto možnost.

 - **Otázka: když je považován za uživatele registrovaná?**

 > **A:** Uživatel se považuje za registrované Pokud má aspoň definované N použitelné části informace o ověřování, kde N není číslo z ověřování metody požadováno nastavené na [Portálu Správa Azure](https://manage.windowsazure.com). Další informace najdete v tématu Přizpůsobení zásad resetování uživatelského hesla.


## <a name="password-reset"></a>Resetování hesla

 - **Otázka: jak dlouho má čekat přijímat-mailu, služby SMS nebo telefonního hovoru z resetovat heslo?**

 > **A:** E-mailové zprávy SMS a telefonní hovory by se ukládají v části minutu s malá sekund 5 – 20. Pokud není dostanete oznámení v tomto časový rámec, podívejte se do složky Nevyžádaná pošta, číslo nebo e-mailu spojení je tu očekáváte a jestli je ověření dat v adresáři správně naformátovaný. Další informace o formátování telefonní čísla a e-mailové adresy pro resetování hesla najdete v článku další použití dat o resetování hesla.

 - **Otázka: jaké jazyky podporuje resetovat heslo?**

 > **A:** Resetování hesel uživatelské rozhraní aplikace zprávy SMS a hlasové hovory mezi jsou lokalizované ve stejné 40 jazyky podporované ve službách Office 365. Jsou: arabština, bulharština, zjednodušenou, tradiční čínštinu, chorvatština, Česká, dánština, holandština, angličtina, estonština, finština, francouzština, němčina, řečtina, hebrejština, hindština, maďarština, indonéština, italština, japonština, Kazaština, korejština, litevština, litevština, malajština (Malajsie), norština (Bokmål), polština, portugalština (Brazílie), portugalština (Portugalsko), rumunština, ruština, srbština (latinka), slovenština, slovinština, španělština, švédština, thajština, turečtina, ukrajinština a vietnamština.

 - **Otázka: které části zkušenosti resetovat heslo získat značky, když nastavím organizační branding v mém adresáři společnosti konfigurace kartu?**

 > **A:** Na portálu resetovat heslo zobrazí loga vaší organizace a bude taky umožňují konfigurovat kontakt odkaz správce tak, aby ukazovaly na vlastní e-mailu nebo adresu URL. E-mailu, který se odešlou resetování hesla bude obsahovat logem vaší organizace, barvy (v tomto případě červená), název v textu e-mailu a přizpůsobit pod názvem. Viz příklad se všemi firemních prvky dole. Další informace najdete v tématu Obnovení hesla přizpůsobení vzhled a chování.

  ![][001]

 - **Otázka: jak můžou Vzdělávejte naše uživatele o tom, odkud chcete přejít o resetování hesla?**

 > **A:** Můžete poslat uživatelům https://passwordreset.microsoftonline.com přímo nebo můžete určit, aby je a klikněte na nemají přístup k účtu odkaz na všechny ID pracovní nebo školní přihlašovací obrazovka. Můžete neváhejte a publikovat tyto odkazy (vytvořit přesměrování adresy URL k nim) na místě, snadno přístupný uživatelům.

 - **Otázka: můžu používat tuto stránku z mobilního zařízení?**

 > **A:** Ano, na této stránce pracuje na mobilních zařízeních.

 - **Otázka: můžete podporují odemknutí účtů místní služby active directory když uživatelé resetování hesla?**

 > **A:** Ano, pokud uživatel obnoví heslo nebo heslo zpětným byl nasazeném všech verzích Azure AD Connect nebo verzích Azure AD Sync 1.0.0485.0222 nebo novější, pak účtu tohoto uživatele bude automaticky odemknout pro daného uživatele obnoví své heslo.

 - **Otázka: jak můžete integrovat heslo resetovat přímo do Moje uživatelské přihlašovací s počítačem?**

 > **A:** Toto není možné dnes. Ale pokud opravdu potřebujete tuto funkci a zákazník Azure AD Premium, můžete nainstalovat Microsoft správce identit bezplatně a nasazení řešení resetovat heslo místního nalezených řešení tohoto požadavku na.

 - **Otázka: můžu nastavit jiné zabezpečení otázky pro jiné národní prostředí?**

 > **A:** Ne, není možné dnes, ale nemůžeme jsou zvažovat.

 - **Otázka: kolik otázek jsme konfigurace ověřování možnost Dotazy zabezpečení?**

 > **A:** Konfigurace až 20 vlastní zabezpečení dotazy na [Portálu Správa Azure](https://manage.windowsazure.com).

 - **Otázka: jak dlouho dotazy zabezpečení pravděpodobně?**

 > **A:** Dotazy zabezpečení může být 3 až 200 znaků.

 - **Otázka: jak dlouho odpovědi na otázky zabezpečení pravděpodobně?**

 > **A:** Odpovědi může být 3 až 40 znaků.

 - **Otázka: jsou duplicitní odpovědi na otázky zabezpečení odmítnuté?**

 > **A:** Ano, jsme odmítnout duplicitní odpovědi na otázky zabezpečení.

 - **Otázka: může uživatel zaregistrovat více stejnou otázku zabezpečení?**

 > **A:** Ne, když uživatel zaregistruje určitou otázku, kterými váš kolega nemusí zaregistrujte si tuto otázku podruhé.

 - **Otázka: je možné nastavit minimální limit dotazy zabezpečení pro registraci a resetovat?**

 > **A:** Ano, jeden limit nastavit pro registraci a jiné pro obnovit. Dotazy zabezpečení 3 až 5 může být nutné pro registraci a 3 až 5 může být nutné pro obnovit.

 - **Otázka: Pokud uživatel zaregistroval více než maximální počet otázky muset obnovit, jak dotazy zabezpečení vyberou během resetovat?**

 > **A:** Dotazy zabezpečení N tím se vyberou náhodně mimo celkový počet dotazů, které uživatel zaregistroval, kde N je minimální počet otázky požadovaný pro resetování hesla. Například pokud má uživatel 5 dotazy zabezpečení registrované, ale jenom 3 vyžadovaných obnovit, 3 tyto 5 bude vybrané náhodně a zobrazena uživatelů v době obnovení. Pokud uživatel obdrží nepovedlo odpovědi na otázky, procesu výběru opakuje nechcete, aby hammering otázku.

 - **Otázka: zabránit uživatelům v pokoušející kolikrát za období čas (krátký) resetování hesla?**

 > **A:** Ano, existují některé funkce zabezpečení integrovaná resetování hesla. Uživatelé můžou jenom zkuste 5 pokusů resetování hesla v rámci hodinu před právě uzamčeno po dobu 24 hodin. Uživatelé mohou pouze pokusit ověřit telefonní číslo 5 číslem v rámci hodinu před právě uzamčeno po dobu 24 hodin. Uživatelé jenom zkuste jeden ověřování 5 číslem v rámci hodinu před právě uzamčeno po dobu 24 hodin.

 - **Otázka: pro jak dlouho platí e-mailu a služby SMS jednorázové heslo?**

 > **A:** Životnost relace pro resetování hesla je 105 minut. To znamená, že od začátku heslo resetovat operace, má uživatel 105 minut resetovat své heslo. E-mailu a služby SMS jednorázové heslo jsou neplatné po uplynutí tohoto časového období.


## <a name="password-management-reports"></a>Sestavy správy hesel

 - **Otázka: jak dlouho trvá u dat zobrazovaných na sestavy správy heslo?**

 > **A:** Data objevit na sestavy správy heslo 5 až 10 minut. Ho některých případech to může trvat až jednu hodinu zobrazit.

 - **Otázka: jak můžete filtrovat sestavy správy heslo?**

 > **A:** Kliknutím na malé lupu krajní vpravo od popisků sloupců, zobrazily v horní části sestavy můžete filtrovat sestavy správy heslo (viz snímek). Pokud chcete udělat bohatší filtrování, můžete si stáhnout sestavy do aplikace excel a vytvořte kontingenční tabulku.

  ![][002]

 - **Otázka: Jaký je maximální počet událostí se ukládají v sestavách správy heslo?**

 > **A:** Až 1 000 heslo resetovat heslo obnovit registrace událostí ukládány do sestavy správy heslo.  Pracujeme rozbalte toto číslo zahrnutí více akcí.

 - **Otázka: jak dlouhé zpět sestavy správy heslo go?**

 > **A:** Sestavy správy heslo zobrazit operace vyskytující se v posledních 30 dní. Jak nastavit delší dobu pracujeme. Prozatím v případě potřeby archivovat tato data můžete stáhnout sestavy pravidelně a uložte do samostatného umístění.

 - **Otázka: je maximální počet řádků, které se mohou objevit na sestavy správy heslo?**

 > **A:** Ano, než 1 000 řádků, může se zobrazit na některé z sestavy Správa hesel, zda jsou se zobrazují v uživatelském rozhraní nebo stažení. Jak zvýšit tímto limitem pracujeme.

 - **Otázka: existuje rozhraní API pro přístup k resetování hesla nebo zápisu dat sestav?**

 > **A:** Ano, najdete v tématu následující dokumentaci můžete zjistit, jak lze získat přístup k resetování hesla vykazování datového proudu.  [Zjistěte, jak získat přístup ke heslo resetovat podřízenosti události programově](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Heslo zpětného zápisu
 - **Otázka: Jak funguje zpětným heslo na pozadí?**

 > **A:** Podrobné vysvětlení, co se stane, když povolíte zpětným heslo, a jak dat prochází systému zpátky do místního prostředí najdete v článku [že jak heslo zpětným funguje](active-directory-passwords-learn-more.md#how-password-writeback-works) . Najdete v tématu [model zabezpečení zpětným heslo](active-directory-passwords-learn-more.md#password-writeback-security-model) v jak heslo zpětným funguje se dozvíte, jak můžeme zajistit, aby že heslo zpětným je velmi zabezpečené služba.

 - **Otázka: jak dlouho heslo zpětným trvat práce?  Existuje synchronizace zpoždění jako se synchronizací hash heslo?**

 > **A:** Heslo zpětným je rychlé. Je synchronní kanálů, což zkusíte zásadně hash synchronizace hesel. Heslo zpětným umožňuje uživatelům zjištění názorů uživatelů reálném čase o úspěchu jejich obnovení hesla nebo změna operace. Ve skupinovém rámečku 500 ms je průměrná čas pro úspěšné zpětným hesla.

 - **Otázka: pro jakým typům účtů funguje zpětným heslo?**

 > **A:** Heslo zpětným vyhovovat federovaný a synchronizaci hesel Hash'd uživatelů.

 - **Otázka: heslo zpětným vynutit zásady pro hesla svoji doménu?**

 > **A:** Ano, heslo zpětným vynucuje stáří hesla, historie, složitosti, filtry a jiných omezení, která může vložíte na místě na hesla místní domény.

 - **Otázka: je heslo zpětným zabezpečené?  Jak můžu jistotu, že se nebudou získat hacker?**

 > **A:** Ano, je velmi zabezpečené zpětným heslo. Přečtěte si další informace o 4 vrstvy zabezpečení implementovaná službou zpětným heslo, najdete v článku [model zabezpečení zpětným heslo](active-directory-passwords-learn-more.md#password-writeback-security-model) v jak heslo zpětným funguje.




## <a name="links-to-password-reset-documentation"></a>Odkazy na heslo resetovat si přečtěte následující dokumentaci
Tady jsou odkazy na všech stránkách resetování hesla Azure AD si přečtěte následující dokumentaci:

* **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).
* [**Jak to funguje**](active-directory-passwords-how-it-works.md) – Další informace o šest jednotlivých součástí tuto službu a co každý znamená
* [**Začínáme**](active-directory-passwords-getting-started.md) – zjistěte, jak vám umožní uživatelům obnovit a změnit jejich cloudu a místní hesla
* [**Vlastní**](active-directory-passwords-customize.md) – zjistěte, jak přizpůsobit vzhled a chování a nastavení služby potřebám vaší organizace
* [**Doporučené postupy**](active-directory-passwords-best-practices.md) : Naučte se nasadit rychlé a efektivní práce s hesly ve vaší organizaci
* [**Dozvědět se víc**](active-directory-passwords-get-insights.md) – Další informace o naše integrované možnosti vytváření sestav
* [**Poradce při potížích**](active-directory-passwords-troubleshoot.md) – zjistěte, jak rychle vyřešit problémy se službou
* [**Další informace**](active-directory-passwords-learn-more.md) – přejděte důkladné do technické podrobnosti Princip služby


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"
