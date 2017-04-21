<properties
pageTitle="Instalace a nastavení nástroje pro vytváření GitHub"
description="Nástroje a ukáže, jak získat pro vytváření Azure obsah v GitHub."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Instalace a nastavení nástroje pro vytváření GitHub

Postupujte podle kroků v tomto článku nastavení nástroje pro přispět k Azure technické dokumentace. Přispěvatelům neformální a občas pravděpodobně můžete použít GitHub uživatelského rozhraní pokynů v kroku 2.

Pokud nevíte libovolná, můžete zkontrolovat některé libovolná terminologie: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Kromě toho podprocesu StackOverflow Glosář termínů libovolná můžete setkat v této sadě kroků: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Obsah

- [Vytvoření účtu GitHub a nastavení profilu](#create-a-github-account-and-set-up-your-profile)
- [Registrace k Disqus](#sign-up-for-disqus)
- [Zjistěte, jestli potřebujete skutečně postupujte takto](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [Oprávnění v GitHub](#permissions-in-github)
- [Instalace libovolná pro Windows](#install-git-for-windows)
- [Povolení dvojúrovňové ověřování](#enable-two-factor-authentication)
- [Instalace markdown editor](#install-a-markdown-editor)
- [Konfigurace Atom](#configure-atom)
- [Rozvětvené úložiště a zkopírujte ho do počítače](#fork-the-repository-and-copy-it-to-your-computer)
- [Konfigurace svoje uživatelské jméno a e-mailu místně](#configure-your-user-name-and-email-locally)
- [Další kroky](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Vytvoření účtu GitHub a nastavení profilu

Přispívat Azure technické obsahu, musíte mít účet [GitHub](http://www.github.com) .

Pokud jste si Microsoft Přispěvatel, musíte nastavit váš účet GitHub tak, aby jasně identifikované jako zaměstnanec společnosti Microsoft. Nastavení profilu následujícím způsobem:

- **Profilových obrázků**: obrázek můžete (povinné)
- **Jméno**: vaše jméno a příjmení jméno (povinné)
- **E-mailu**: svůj e-mail (nepovinné) společnosti Microsoft
- **Společnosti**: Microsoft Corporation (povinné)
- **Umístění**: seznam vaší polohy (volitelné)

Tento profil by měl vypadat váš profil:

<p align="center">
 ![Příklad GitHub profilu](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Registrace k Disqus

Všechny publikované Azure technický článek obsahuje komentáře toku poskytovaná Disqus.

 ![Discus logo](./media/tools-and-setup/discus.png)

Pokud jste zaměstnance společnosti Microsoft, a pokud jsou autora nebo přispěvatele na články, budete muset registraci Disqus, abyste se mohli zúčastnit proudu komentáře článku.

1. Registrace k účtu na [http://www.disqus.com/](http://www.disqus.com/)
2. Váš profil vyplňte následujícím způsobem:

 - **Celé jméno**: jméno a příjmení zobrazené v výpis knihy adresu Microsoft plus závorkách informace, které je váš alias plus @MSFT. Formát: *jméno příjmení [alias@MSFT] *
 - **Umístění**: vaší polohy
 - **Krátké životopis**: Nadpis

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Zjistěte, jestli potřebujete skutečně postupujte takto

Možná budete muset postupujte podle kroků v tomto článku. To záleží na požadovaný typ obsahu příspěvek má nebo potřebujete zdůraznit.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Odeslání pouze text změnit na existující články

Potřebujete-li pouze nebo mají být textový aktualizace k existujícímu článku, pravděpodobně nepotřebujete sledovat zbývající kroky. Editor markdown založené na webu společnosti GitHub umožňuje odeslat změny. Stačí klikněte na odkaz GitHub v článku znalostní báze, který chcete změnit:

 ![Příklad GitHub profilu](./media/tools-and-setup/contributetogit.png)

 Klikněte na ikonu úpravy v GitHub verzi tohoto článku

 ![Příklad GitHub profilu](./media/tools-and-setup/editicon.PNG)

 Otevře se editor snadno použitelné web, který usnadňuje odeslat změny. Nemusíte postupujte podle kroků v tomto článku.

###<a name="all-other-changes"></a>Všechny ostatní změny
Uživatelské rozhraní GitHub nepodporuje vytváření nových souborů a přetahování obrázky. Však při práci v uživatelském rozhraní, správu poboček nepřehledné, obvykle doporučujeme nainstalovat nástroje a další příkazy pro vytváření a správě články. Pokud chcete použít v uživatelském rozhraní, přečtěte si článek:

- [Vytváření souborů na Github](https://github.com/blog/1327-creating-files-on-github)
- [Nahrajte soubory do vašeho úložiště](https://github.com/blog/2105-upload-files-to-your-repositories)

Pro následující typů práce doporučujeme nainstalovat a zjistěte, jak používat nástroje:

 - Článek hlavních změn
 - Vytváření a publikování nový článek
 - Přidání nové obrázky nebo aktualizaci obrázky
 - Aktualizace článek období dnů bez publikování změny jednotlivých dnů
 - Vytváření obsahu pro vydání, který má přejdete určitý den v určitém čase

##<a name="permissions-in-github"></a>Oprávnění v GitHub

Kdokoli pod svým účtem GitHub do ní můžete přidávat Azure technického obsahu pomocí našich veřejné úložiště na [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content). Žádné zvláštní oprávnění jsou potřeba.

Pokud jste Microsoft PM nebo Redaktor, kdo pracuje na Azure obsahu, musíte pracovat v naší soukromé obsahu úložiště, azure obsah cena. Navštivte [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) požádat o další oprávnění, která vám umožní přispívat prostřednictvím soukromé repo - Přihlaste se k GitHub pomocí tlačítka > klikněte na Azure > klikněte na **Připojit se ke týmu** nebo **připojení jinému týmu**a potom vyhledejte a připojení ke skupině **azure obsah číst** .

## <a name="install-git-for-windows"></a>Instalace libovolná pro Windows

Instalace libovolná pro Windows z [http://git-scm.com/download/win](http://git-scm.com/download/win). Tento soubor ke stažení nainstaluje systému správy verzí libovolná a nainstaluje libovolná flám, aplikaci příkazového řádku, který budete používat chcete provést interakci s místní úložiště libovolná.

Přijměte výchozí nastavení; Pokud budete potřebovat příkazy k dispozici v rámci příkazového řádku Windows, vyberte požadovanou možnost, že ji povolí.

<p align="center">
 ![Příklad GitHub profilu](./media/tools-and-setup/gitbashinstall.png)

(Poznámka: Toto je stejná jako "Github pro Windows". "Github pro systém Windows" je jiný nástroj na základě grafického rozhraní, která budou fungovat i, pokud chcete důkladně si sami. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Povolení dvojúrovňové ověřování

Pokud pracujete v úložišti soukromé obsahu máte na účtu GitHub ověření dvou faktor (2FA). Je potřeba v úložišti soukromé.

Aby, postupujte podle pokynů v obou následujících GitHub tématech nápovědy:

- [Informace o produktu dvojúrovňové ověřování](https://help.github.com/articles/about-two-factor-authentication/)
- [Vytváření přístupový token pro použití příkazového řádku](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Při vytváření tokenu vyberte všechny obory k dispozici v uživatelském rozhraní vytváření tokenů ([Podrobnosti o každý obor](https://developer.github.com/v3/oauth/#scopes))

Po povolení 2FA máte přístupový token místo hesla GitHub na příkazovém řádku zadejte při pokusu o přístup k úložišti GitHub z příkazového řádku. Přístupový token není ověřovací kód, který se zobrazí v textové zprávě, když nastavíte 2FA. Je dlouhé řetězce, který vypadá přibližně takto: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Několik poznámky o tomto:

- Když vytvoříte přístupový token, uložte ho ve textový soubor usnadňují snadno kdykoliv ho budete potřebovat.

- Později když potřebujete vložit tokenu, upozorní vložte do příkazového řádku dvěma způsoby:

 - Klikněte na ikonu v levém horním rohu okna příkazového řádku > Upravit > Vložit.
 - Klikněte pravým tlačítkem myši na ikonu v levém horním rohu okna a klikněte na vlastnosti > Možnosti > Režim rychlých úprav. Tímto postupem nakonfigurováno příkazového řádku, můžete vložit kliknutím pravým tlačítkem myši v okně příkazového řádku.

## <a name="install-a-markdown-editor"></a>Instalace markdown editor

Vytváříme obsahu pomocí zápisu jednoduché "markdown" v souborech, spíše než komplexního "marže" (HTML, XML, atd.). Ano budete muset nainstalovat markdown editor.

- **Atom**: většině amerických v editoru společnosti GitHub Atom markdown: [http://atom.io](http://atom.io). Nevyžaduje licenci pro použití business. Má kontrola pravopisu.

- **Poznámkový blok**: Poznámkový blok můžete použít pro velmi lightweight možnost.

- **Prose**: Toto je editor markdown lightweight Elegantní, online a otevřít zdroj, který nabízí náhled. Navštivte [http://prose.io](http://prose.io) a povolte Prose v úložišti.

- **[Visual Studio kód](https://www.visualstudio.com/products/code-vs.aspx)** - společnosti Microsoft položky do tohoto pole.

## <a name="configure-atom"></a>Konfigurace Atom

Pokud používáte Atom, musíte nastavit několik věcí.

- Ve výchozím nastavení pomocí 2 prostory pro karty Atom, ale Markdown očekává 4 mezery. Pokud ponecháte na výchozí dvou, váš článek budou vypadat skvěle v místní náhledu, ale když není importované do Azure. Ano, konfigurace Atom 4 mezery používat – toto nastavení můžete najít ve skupinovém rámečku soubor > Nastavení > Nastavení > karta délka.
- Budete pravděpodobně taky chcete zapnout měkké zalomení v této části, která nemá stejný jako "zalamování" v poznámkovém bloku.
- Zapnout markdown náhledu, klikněte na balíčků > Náhled Markdown > Přepnout náhled. Ctrl a Shift-M slouží k přepínání náhled zobrazení HTML.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>Rozvětvené úložiště a zkopírujte ho do počítače

1. Vytvoření větve úložiště v GitHub – přejděte na pravé horní části stránky a klikněte na tlačítko větev. Pokud se zobrazí výzva, vyberte svůj účet jako umístění, kde by měl být vidlice vytvořen. Tím vytvoříte kopie úložiště v rámci vašeho účtu libovolná centrální. Obecně technické autoři a správci program třeba vidlice azure – obsah cena, soukromé repo. Přispěvatelům komunity muset vidlice azure obsahem, veřejné repo. Potřebujete rozvětvené jednorázové; Po první nastavení Pokud chcete zkopírovat vaší vidlice do jiného počítače, stačí spustit příkazy, které následují v této části můžete zkopírovat repo s vaším počítačem.  Pokud budete chtít vytvořit větve obou úložištích, bude potřeba vytvořit větev pro každý úložiště.

2. Zkopírujte osobní přístup tokenu, kterou jste získali od [https://github.com/settings/tokens](https://github.com/settings/tokens). Můžete chvíli přijmout výchozí oprávnění pro token.  Uložení tokenu osobní přístup z textového souboru pro pozdější použití.

3. Dál pak zkopírujte úložiště s vaším počítačem pomocí svých přihlašovacích údajů vložené v řetězci příkazu.  K tomuto účelu otevřete libovolná flám a spustit jako správce. Na příkazovém řádku zadejte následující příkaz.  Tento příkaz vytvoří adresář azure-content(-pr) ve vašem počítači.  Pokud používáte výchozí umístění, bude c:\users<your Windows user name>\azure-content(-pr).

Veřejné repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Soukromé repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Tento příkaz klonovat například mohl vypadat nějak takhle:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Nastavení připojení vzdálené úložiště a konfigurace přihlašovacích údajů

Vytvoření odkazu na kořenové úložiště zadáním tyto příkazy. Nastaví se připojení k úložišti v GitHub, aby mohli získat nejnovější změny do místního počítače a znovu použít změny pro GitHub. Tento příkaz také nakonfiguruje tokenu místně tak, že nebudete muset pokaždé, když se pokusíte přístup k odesílání dat repo a vidlice na GitHub zadejte svoje jméno a heslo.

Veřejné repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Soukromé repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Obvykle to trvá dlouho. Až to uděláte, nemusíte znovu stůl nebo znova zadejte svoje přihlašovací údaje. Pouze nutné forks (vidlice) do místního počítače znovu zkopírujte Pokud nastavíte nástroje na jiném počítači.


## <a name="configure-your-user-name-and-email-locally"></a>Konfigurace svoje uživatelské jméno a e-mailu místně

Abyste měli jistotu, že jsou uvedeny správně jako Přispěvatel, budete muset konfigurace svoje uživatelské jméno a e-mailu místně na libovolná.

1. Spusťte libovolná flám a přepněte do azure obsahu nebo azure obsah cena:

   ````
   cd azure-content
   ````

 nebo

   ````
   cd azure-content-pr
   ````

2. Konfigurace své uživatelské jméno tak, aby odpovídala svoje jméno při nastavování ho ve vašem profilu GitHub:

    ````
    git config --global user.name "John Doe"
    ````
3. Konfigurace e-mailu tak, aby odpovídala primární e-mailu určená ve vašem profilu GitHub; Pokud jste MSFT zaměstnance, třeba e-mailové adresy MSFT:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Typ `git config -l` a zkontrolujte nastavení místní za účelem zajištění uživatelské jméno a e-mailu v konfiguraci, jsou správné.

##<a name="next-steps"></a>Další kroky

- Princip typ obsahu, který, do kterých patří technické obsahu repo a vědět, co nepatří. V tématu [pokyny obsahu kanálu](./content-channel-guidance.md)!
- Postupujte podle [těchto kroků vytvořit nebo změnit článek a odešlete ji pro publikování](./git-commands-for-master.md).
- Zkopírujte [šablonu markdown](../markdown templates/markdown-template-for-new-articles.md) jako základ pro nový článek.
- Použijte [Tento kontrolní seznam pro ověření žádosti o vyžádané bude kritériím kvality](./contributor-guide-pr-criteria.md) pro sloučení.


###<a name="contributors-guide-navigation"></a>Navigace Průvodce přispěvatelů

- [Článek Základní informace](./../README.md)
- [Index pokyny články](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
