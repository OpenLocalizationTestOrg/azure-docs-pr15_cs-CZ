<properties
    title="required"
    pageTitle="Rozšíření vlastní markdown použité v naší technické články"
    description="Seznam vlastní markdown rozšíření podporující nástroj vložené video, poznámky a tipy, opakovaně použitelného obsahu a jiné položce v článcích technické azure.microsoft.com."
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## <a name="markdown-for-azuremicrosoftcom"></a>Markdown Azure.microsoft.com

Obecné markdown tipy najdete v tématu [Základy Markdown](https://help.github.com/articles/markdown-basics/) a naše [markdown cheatsheet](./media/documents/markdown-cheatsheet.pdf?raw=true). Pokud je potřeba vytvořit článek crosslinks v markdown, přečtěte si článek [propojení pokyny] (. / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/).

Azure.microsoft.com podporuje [samostatně bloky kódu](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks) a [zvýrazňování syntaxe](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting). Však ACOM podporuje pouze jeden syntaxe zvýraznění barevné schéma, bez ohledu na jazyk, který určíte v bloku kódu.

## <a name="custom-markdown-extensions-used-in-our-technical-articles"></a>Rozšíření vlastní markdown použité v naší technické články

Náš články použití GitHub flavored markdown pro většinu formátování článek - odstavce, odkazy, seznamy, záhlaví, atd. Ale používáme vlastní markdown rozšíření kde potřebujeme bohatší formátování vykreslená stránek na azure.microsoft.com. Tady je přípon souborů, které aktuálně používáme:

+ [Tipy a poznámek]
+ [Obsahuje]
+ [Vložené video]
+ [Technologie, tak i platformu voliče]

## <a name="notes-and-tips"></a>Tipy a poznámek

Můžete vybírat z 4 typů poznámky a tipy:

- AZURE. POZNÁMKA:
- AZURE. UPOZORNĚNÍ
- AZURE. TIPss
- AZURE. DŮLEŽITÉ

###<a name="usage"></a>Použití
Se obvykle používá poznámek a tipy šetrně celý článek. Když je používat, vyberte příslušný typ poznámky nebo tip:

- Použití AZURE. Poznámka: zvýrazněte neutrální nebo kladné informace, které zvýrazní nebo doplňuje klíčové body hlavní text. Poznámky poskytuje informace, které lze použít pouze v speciálních případů.

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- Použití AZURE. Upozornění uživatelům nějaké podmínky, které může způsobit potíže v budoucnu zobrazí upozornění. Například možnost určitých nebo provedení některých volba může trvale zamknout můžete do konkrétním scénáři.

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- Použití AZURE. Tip: Chcete-li pomoct svým uživatelům použít postupy a postupy do své potřeby text. Tip mohou také navrhnout alternativní metody, které nemusí být zřejmé. Tipy, ale nejsou důležité základy textu.

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- Použití AZURE. DŮLEŽITÉ informace, které je nezbytné k dokončení úkolu.

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

Když tyto poznámky a tipy podporují bloky kódu, obrázky, seznamy a odkazy, zkuste uchovávat své poznámky a tipy rychle a jednoduše. Pokud vytvoříte komplexní poznámky s velkým množstvím formátování najdete, který může být přihlášení, že které právě potřebujete jinou část v hlavním textu v článku. A příliš mnoho poznámek v článku možné nadbytečných a obtížně skenování nebo čtení.

###<a name="sample-markdown"></a>Ukázka markdown

Všechny ukázky ilustrují AZURE. POZNÁMKY. Použít TIP, upozornění nebo důležité, nahradí "Poznámka" v markdown:

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

Jednoho odstavce:

    > [AZURE.NOTE] Tento kurz, musíte mít účet Microsoft Azure active. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut.

Multiparagraph:

    > [AZURE.NOTE] Tento kurz, musíte mít účet Microsoft Azure active.
    >
    > Pokud nemáte účet, můžete [Vytvořit bezplatný zkušební účet](http://www.windowsazure.com/pricing/free-trial/) v jenom pár minut.

## <a name="includes"></a>Obsahuje

Opakovaně použitelné textu v naší GitHub úložiště je umístěn v souborech název "obsahuje". Obsahujících text, který se nemusí používat v několika články, musí obsahovat odkaz na tento soubor opakovaně použitelný informací. Zahrnout samotné je soubor jednoduché markdown (.md). Může obsahovat libovolný platný markdown, včetně textu, odkazů a obrázků. Všechny obsahují markdown soubory musí být v [/ obsahuje adresář](https://github.com/Azure/azure-content/tree/master/includes) v kořenové úložiště. Když je publikován v článku, zahrnout text integrována do publikovaného tématu.

- Použijeme specifické syntaxe neodkazuje zahrnutí.

- Mediální soubory, kterou chcete vložit zahrnutí musí vytvořit ve složce médií specifické pro zahrnout. Média složek pro obsahuje patří ve [složce azure obsah/obsahuje/média](https://github.com/Azure/azure-content/tree/master/includes/media). V adresáři médií nesmí obsahovat žádné obrázky v jeho kořenové. Pokud zahrnout nemá žádné obrázky, není odpovídající adresáře médií povinný.

###<a name="usage"></a>Použití

- Použití zahrnuje kdekoli budete potřebovat stejný text zobrazit v několika články.

- Obsahuje jsou určeny pro značné množství obsahu - odstavce nebo dvě, sdílené procedury nebo sdílený oddíl. Nepoužívejte je něco menší než větu; **není pro názvy produktů**.

- Ujistěte se, veškerého textu v zahrnutí napsané v celé věty nebo fráze, které nezávisí na předchozí text nebo následující článek, který odkazuje zahrnout. Ignorování návod vytvoří untranslatable řetězec v článku, který porušuje lokalizované prostředí. 

- Nelze vložit obsahuje v rámci obsahuje. Nejsou podporovány DPS publikování systému.

- Nesdíleli média mezi soubory. Pomocí jedinečný název pro každou zahrnout a článek do samostatného souboru. Obsahují mediální soubor ve složce media přidružené zahrnout.

- Nepoužívejte zahrnutí jako pouze obsah článku.  Obsahuje jsou určeny jako doplňkové obsahu ve zbývající části v článku.

- Jelikož všechny musí být v / obsahuje adresář, vždy je tato cesta k zahrnutí z článku

    .. / obsahuje

- MOŽNOST Neopakovat název_souboru odkaz v následujícím článku a zahrnout na odkaz nebo obrázek. Přidání "-zahrnout" na odkaz odkaz nebo média souboru chcete-li předejít opakující se odkaz:

 **Odkaz na**

 Změna: odata.org k: zahrnout odata.org

 **Obrázek odkazu**

 Změna: table.png k: include.png tabulky

###<a name="sample-markdown"></a>Ukázka markdown
Syntaxe pro přidání zahrnutí do článku si přečtěte následující dokumentaci je:

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

Příklad

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

První část zahrnout je název zahrnout bez cestu a bez přípony .md. Druhá část je relativní cestu k zahrnout do / obsahuje adresář s příponou .md.

###<a name="rendering"></a>Vykreslování

V dialogovém okně vykreslená GitHub zahrnout vykreslí následujícím způsobem:

 [AZURE. Zahrnout úložiště objektů blob postupy]

V vykreslená HTML na azure.microsoft.com, HTML z obsahuje sloučena do zbytku dokumentu ve formátu HTML. Však HTML obsahuje HTML komentář s původním zahrnout markdown názvu souboru a hash potvrdit GitHub. Tuto poznámku je součástí pro odstraňování potíží, takže zdroje obsahu se dají snadno identifikovat a součástí GitHub:

  ![](./media/custom-markdown-extensions/include.png)


## <a name="embedded-videos"></a>Vložené video

Náš technické články podporoval videa embeddeded v technické články, pokud jsou videí [9 kanálu](http://channel9.msdn.com/) webu společnosti Microsoft. Videa v kanálu 9 musí integrovaný s [azure.microsoft.com centrum Video](http://azure.microsoft.com/documentation/videos/home/). Aktuálně nepodporujeme vložený videa z YouTube; Pokud jste komunity Přispěvatel, jste Vítá vás odkaz na YouTube, pokud video, které chcete zobrazit účtován tam. Microsoft přispěvatelům použít Channel 9 a centru Video.

### <a name="usage"></a>Použití

- Ujistěte se, že je v Centru pro Video.

- Zkopírujte videa ID z popisné adresy URL videa v kanálu 9 nebo z centra Azure Video. Videa ID pro video na [http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/) je například **azure Plánovač neobvyklé naplánuje**.

### <a name="syntax"></a>Syntaxe

    > [AZURE.VIDEO video-id-string]

### <a name="rendering"></a>Vykreslování

Zapnuto GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

Publikovaný článek: [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## <a name="technology-and-platform-selectors"></a>Technologie, tak i platformu voliče

Použití technologie, tak i platformu switchers v technické články při vytváření více provedeních stejné článku rozdíly adresu v implementaci přes technologií pro server nebo platformách. Toto je obvykle nejčastěji pro naše mobilní platformy obsahu pro vývojáře. Dva různé typy voliče, [jednoduché voliče](#simple-selectors) a [mezi dvěma stranami voliče](#two-way-selectors)aktuálně existuje.

Protože stejný výběr markdown jsou uvedeny v každé téma do výběru, doporučujeme umístění na volič téma zahrnutí a potom odkazování na tomto zahrnout ve všech témat, které používají stejný výběr.

###<a id="simple-selectors"></a>Jednoduchý voliče

Jednoduchý (jednosměrné) voliče vykreslení jako sady přepínačů přímo pod ním. Používejte tato tlačítka zákazníci muset vybírat témata v jedné platformu nebo technologii sadu, například .NET, Node.js a Java.  Pro všechny voliče použít vlastní markdown přípony: Nepoužívejte HTML pro voliče.  

Najdete v článku [Začínáme s oznámení rozbočovače](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/) najdete v článku jak autor vytvořil 8 verzemi stejného článku, ale použitých voliče povolit navigaci v rámci uspořádat.

![Příklad jednoduchého výběr](./media/custom-markdown-extensions/selectors.PNG)

####<a name="syntax"></a>Syntaxe

    > [AZURE.SELECTOR]
    - [Propojit popisek #1](link #1 url)
    - [popisek odkazu #2](link #2 url)

Příklad:

    > [AZURE.SELECTOR]
    - [Univerzální Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone](../articles/notification-hubs-windows-phone-get-started/)
    - [iOS](../articles/notification-hubs-ios-get-started/)
    - [Android](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### <a name="rendering"></a>Vykreslování

Obrázek nad textem zobrazí vykreslování na azure.microsoft.com. Na stránce vykreslená GitHub voliče vykreslování jako seznam s odrážkami odkazů.

###<a id="two-way-selectors"></a>Mezi dvěma stranami voliče

Mezi dvěma stranami voliče umožňuje uživatelům vybírat témata tak obousměrnou matice. Při je to podstatné Azure technologie, například Mobile Services podporuje více back-end platformy, jakož i více klientů. Mějte na paměti toto:

- Během byl vytvořen jako `(Platform | Backend)`, dropwdown textu můžete nyní přizpůsobit.
- Pro každý bodu v matici nepotřebujete položky seznamu, ale jenom mít kde tématu adresy URL existuje který není duplicitní položky.
- Odkaz může být všechny adresy URL, i když je obecně další téma GitHub.

V tématu [Začínáme se službami Mobile](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/) najdete v článku jak autor vytvořil 15 verzemi stejného článku (9 platformy mobilních klientů a 2 platformách back-end), ale použitých voliče povolit navigaci v rámci uspořádat. Všimněte si, že 3 články nemají obě verze back-end.

![Příklad mezi dvěma stranami voliče](./media/custom-markdown-extensions/selector-list.png)

####<a name="syntax"></a>Syntaxe

    > [AZURE. Výběr seznamu (Dropdown1 | Dropdown2)]     -  [(Dropdown1Text1 | Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 | Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 | Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 | Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

Příklad:

    > [AZURE. Výběr seznamu (platformy | Back-end)]     -  [(iOS | .NET)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS | JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows univerzální C# | .NET)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows univerzální C# | JavaScript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone | .NET)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone | JavaScript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(Android | .NET)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(Android | JavaScript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(Xamarin iOS | JavaScript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android | JavaScript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### <a name="rendering"></a>Vykreslování

Obrázek nad textem zobrazí vykreslování na azure.microsoft.com. Na stránce vykreslená GitHub voliče vykreslování jako seznam s odrážkami odkazů.

<!--Anchors-->
[Tipy a poznámek]: #notes-and-tips
[Obsahuje]: #includes
[Vložené video]: #embedded-videos
[Technologie, tak i platformu voliče]: #technology-and-platform-selectors

###<a name="contributors-guide-links"></a>Přispěvatelům Průvodce odkazy

- [Článek Základní informace](./../README.md)
- [Index pokyny články](./contributor-guide-index.md)
