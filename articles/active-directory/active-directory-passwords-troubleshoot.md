<properties
    pageTitle="Řešení potíží: Správa Azure AD hesel | Microsoft Azure"
    description="Řešení běžných problémů kroky pro správu Azure AD hesel, včetně resetovat, změna, zpětného zápisu, registrace, a jaké informace chcete zahrnout při hledáte pomoc."
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
    ms.date="08/12/2016"
    ms.author="asteen"/>

# <a name="how-to-troubleshoot-password-management"></a>Jak řešit problémy s Správa hesel

> [AZURE.IMPORTANT] **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).

Pokud máte problémy s Správa hesel, může vám tento článek pomoct. Většina problémy, které můžou nastat můžete vyřešit s několika jednoduchými kroky odstraňování potíží, které si můžete přečíst o pod řešit problémy s nasazením:

* [**Informace chcete zahrnout, pokud potřebujete pomoc**](#information-to-include-when-you-need-help)
* [**Potíže s konfigurací Správa hesel na portálu Správa Azure**](#troubleshoot-password-reset-configuration-in-the-azure-management-portal)
* [**Problémy se sestavami Managment heslo na portálu Správa Azure**](#troubleshoot-password-management-reports-in-the-azure-management-portal)
* [**Problémy s heslo resetovat registrace portálu**](#troubleshoot-the-password-reset-registration-portal)
* [**Problémy s heslo resetovat portálu**](#troubleshoot-the-password-reset-portal)
* [**Problémy s zpětným heslo**](#troubleshoot-password-writeback)
  - [Kódy chyb v protokolu událostí heslo zpětného zápisu](#password-writeback-event-log-error-codes)
  - [Problémy s připojením zpětným heslo](#troubleshoot-password-writeback-connectivity)

Pokud jste vyzkoušeli navrhované řešení problémů s postupem a spuštěné na problémy, můžete odeslat dotaz na [Azure AD fóra](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) nebo kontaktujte podporu a jsme se podívejte se na váš problém hned, jak můžeme.

## <a name="information-to-include-when-you-need-help"></a>Informace chcete zahrnout, pokud potřebujete pomoc

Pokud se nemůžete vyřešit problém podle pokynů níže, můžete kontaktovat naše pracovníci technické podpory. Když budete kontaktovat, doporučujeme obsahují následující informace:

 - **Obecný popis chyby** – k čemu přesně chybová zpráva nebyla uživatele najdete v článku?  Pokud jste udělali žádná chybová zpráva, popisují neočekávané chování, které jste si všimli podrobně.
 - **Stránka** – jaké stránky umístěné když jste viděli chyby do schránky (uveďte adresu URL)?
 - **Datum / čas / časové pásmo** – jaký byl přesné datum a čas viděli chyby do schránky (včetně časové pásmo)?
 - **Podpora kód** – jaký byl kód podpory přihlášení vygenerované při uživatel viděli chybu (našli toto, reprodukujte chyby, a pak klikněte na odkaz podporují kód v dolní části obrazovky a posílání pracovníka podpory GUID, který je výsledkem).
   - Pokud jste na stránce bez kód podpory dole, stiskněte F12 a vyhledávání ID zabezpečení a CID a odeslat tyto dva výsledky pracovník technické podpory.

    ![][001]

 - **ID uživatele** – jaký byl ID uživatele, kterému (například viděli chyby do schránkyuser@contoso.com)?
 - **Informace o uživateli** – byl uživatel federované, synchronizovali, hash heslo jenom cloudu?  Získá uživatel přiřazenou licenci AAD Premium nebo AAD základní?
 - **Protokolu událostí** – Pokud používáte zpětným heslo a chybu je ve vaší místní infrastruktury, přejděte prosím zip kopii protokolu událostí aplikace ze serveru Azure AD Connect a posílání spolu s žádosti o.

Vložení těchto informací vám pomůže nás vyřešit váš problém co nejdříve.


## <a name="troubleshoot-password-reset-configuration-in-the-azure-management-portal"></a>Poradce při potížích s konfigurací resetovat heslo v portálu pro správu Azure
Pokud dojde k chybě při konfiguraci heslo resetovat, nejspíš vás bude vyřešit provedením následujících kroků:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Chyba případu</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>K čemu chyby uživatele zobrazit?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Řešení</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nevidím části <strong>Zásad resetování hesla uživatele </strong>na kartě <strong>Konfigurovat</strong> v portálu pro správu Azure</p>
            </td>
            <td>
              <p>V části <strong>Zásady resetování hesla uživatele </strong>se nezobrazí na kartě <strong>Konfigurovat</strong> v portálu pro správu Azure.</p>
            </td>
            <td>
              <p>K tomu může dojít, pokud nemáte licenci AAD Premium nebo AAD základní přiřazená správce této operace. </p>
              <p>Opravit to přiřadil licenci AAD Premium nebo AAD základní předmětné účtu správce tak, že přejdete na kartu <strong>licence</strong> a zkuste to znovu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Proč nevidím některou z možností konfigurace v části <strong>Zásady resetování hesla uživatele</strong> popsaných v dokumentaci.</p>
            </td>
            <td>
              <p>V části <strong>Zásady resetování hesla uživatele </strong>viditelné, ale jenom příznak, který se zobrazí pod ní příznak <strong>Pro resetování hesla uživatele podpora</strong> .</p>
            </td>
            <td>
              <p>Zbytek uživatelské rozhraní se zobrazí při přechodu na příznak <strong>Pro resetování hesla uživatele podpora</strong> pro <strong>Ano.</strong></p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Proč nevidím možnost určitý konfigurace.</p>
            </td>
            <td>
              <p>Například se nezobrazí možnost <strong>počet dní před uživatel musí potvrdit jejich o kontaktech</strong> při posouvání v části <strong>Zásady resetování hesla uživatele</strong> (nebo další příklady stejný problém).</p>
            </td>
            <td>
              <p>Počet prvků uživatelského rozhraní jsou skryté, dokud není potřeba. Zkuste povolení všechny možnosti na stránce, pokud chcete zobrazit.</p>
              <p>Další informace o všech ovládacích prvků, které jsou k dispozici najdete v článku <a href="active-directory-passwords-customize.md#password-management-behavior">Správa hesel chování</a> .</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Proč nevidím možnost konfigurace <strong>Psaní zase hesla do místního nasazení</strong></p>
            </td>
            <td>
              <p>Možnost <strong>Psát zpět hesla do místního nasazení</strong> není zobrazená na kartě <strong>Konfigurovat</strong> v portálu pro správu Azure</p>
            </td>
            <td>
              <p>Tato možnost je viditelné, pokud jste stáhli Azure AD Connect a nakonfigurovali zpětným heslo. Když uděláte to této možnosti se zobrazí a umožňuje povolit nebo zakázat zpětným z cloudu.</p>
              <p>Další informace o tom, jak to udělat naleznete v tématu <a href="active-directory-passwords-getting-started.md#step-2-enable-password-writeback-in-azure-ad-connect">Povolení zpětným heslo v Azure AD Connect</a> .</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-management-reports-in-the-azure-management-portal"></a>Poradce při potížích s sestavy správy heslo v portálu pro správu Azure
Pokud dojde k chybě při použití sestavy správy hesel, bude pravděpodobně možné rozlišit pomocí následujícího postupu řešení potíží:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Chyba případu</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>K čemu chyby uživatele zobrazit?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Řešení</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Proč nevidím všechny podřízené Správa hesla</p>
            </td>
            <td>
              <p>Sestavy <strong>aktivity resetování hesla</strong> a <strong>Resetování hesel registrace aktivity</strong> nejsou viditelné v části sestavy <strong>Protokolu aktivitu</strong> na kartě <strong>sestavy</strong> .</p>
            </td>
            <td>
              <p>K tomu může dojít, pokud nemáte AAD Premium nebo AAD základní licence přiřazené k této operace správce. </p>
              <p>Opravit to přiřadil licenci AAD Premium nebo AAD základní předmětné účtu správce tak, že přejdete na kartu <strong>licence</strong> a zkuste to znovu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Registrace uživatele zobrazit tisknutím</p>
            </td>
            <td>
              <p>Když uživatel zaregistruje alternativní e-mailu, mobilního telefonu a dotazy zabezpečení, každá zobrazit jako samostatných řádcích místo jedná plná čára.</p>
            </td>
            <td>
              <p>V současné době při přihlášení uživatele jsme nelze se předpokládá, že bude všechno prezentovat na stránce registrace registraci. Proto jsme aktuálně protokolu každý jednotlivé část dat, které máte zaregistrované jako zvláštní událost.</p>
              <p>Pokud chcete agregovat data, můžete stáhnout sestavy a otevřete data jako kontingenční tabulky v Excelu tím větší flexibilitu.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-registration-portal"></a>Poradce při potížích s portálu heslo resetovat registrace
Pokud dojde k chybě při registraci pro resetování hesla uživatele, bude pravděpodobně možné rozlišit pomocí následujícího postupu řešení potíží:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Chyba případu</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>K čemu chyby uživatele zobrazit?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Řešení</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Adresář není povoleno pro resetování hesla</p>
            </td>
            <td>
              <p>Správce povolená, můžete tuto funkci lze použít.</p>
            </td>
            <td>
              <p>Přejděte na příznak <strong>Pro resetování hesla uživatele podpora</strong> <strong>Ano</strong> a klepněte na <strong>Uložit</strong> na kartě portálu Správa Azure adresáře konfigurace. Pokud nemáte Azure AD Premium nebo základní licence přiřazené k správce této operace.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Uživatel nemá přiřazenou licenci AAD Premium nebo AAD základní</p>
            </td>
            <td>
              <p>Správce povolená, můžete tuto funkci lze použít.</p>
            </td>
            <td>
              <p>Přiřadíte licenci Azure AD Premium nebo Azure AD základní uživateli klikněte v části <strong>licence</strong> na portálu Správa Azure. Pokud nemáte Azure AD Premium nebo základní licence přiřazené k správce této operace.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Chyba při zpracování požadavku</p>
            </td>
            <td>
              <p>Uživateli se zobrazí chybu, které je tento pokyn:</p>
              <p>

              </p>
              <p>Chyba při zpracování požadavku </p>
              <p>Při pokusu o resetování hesla.</p>
            </td>
            <td>
              <p>Příčinou může být mnoho problémů, ale obvykle se tato chyba způsobená buď o služby výpadku nebo konfigurační problém, který nelze přeložit. </p>
              <p>Pokud se zobrazí tato chyba a je dopad vaší firmě, kontaktujte podporu a můžeme vám pomůžou typu co nejdříve. Přečtěte si <a href="#information-to-include-when-you-need-help">informace, když potřebujete pomoc s zobrazil</a> zobrazíte by měl obsahovat k pracovník technické podpory na podporu rychlou rozlišení.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-the-password-reset-portal"></a>Poradce při potížích s portálu resetování hesla
Pokud dojde k chybě při obnovení hesla pro uživatele, bude pravděpodobně možné rozlišit pomocí následujícího postupu řešení potíží:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Chyba případu</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>K čemu chyby uživatele zobrazit?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Řešení</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Adresář není povoleno pro resetování hesla</p>
            </td>
            <td>
              <p>Váš účet není povoleno pro resetování hesla</p>
              <p>Je nám líto, ale správce nebyla nastavení účtu pro použití s tuto službu. </p>
              <p>

              </p>
              <p>Pokud byste chtěli, jsme kontaktujte správce ve vaší organizaci o resetování vašeho hesla.</p>
            </td>
            <td>
              <p>Přejděte na příznak <strong>Pro resetování hesla uživatele podpora</strong> <strong>Ano</strong> a klepněte na <strong>Uložit</strong> na kartě portálu Správa Azure adresáře konfigurace. Pokud nemáte Azure AD Premium nebo základní licence přiřazené k správce této operace.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Uživatel nemá přiřazenou licenci AAD Premium nebo AAD základní</p>
            </td>
            <td>
              <p>Během jsme resetování hesla k účtu správce automaticky, jsme kontaktujte správce vaší organizace, aby za vás.</p>
            </td>
            <td>
              <p>Přiřadíte licenci Azure AD Premium nebo Azure AD základní uživateli klikněte v části <strong>licence</strong> na portálu Správa Azure. Pokud nemáte Azure AD Premium nebo základní licence přiřazené k správce této operace.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Adresář aktivované řešení resetování hesla, ale má uživatel chybějící nebo formátované mailová ověřovací informace</p>
            </td>
            <td>
              <p>Váš účet není povoleno pro resetování hesla</p>
              <p>Je nám líto, ale správce nebyla nastavení účtu pro použití s tuto službu. </p>
              <p>

              </p>
              <p>Pokud byste chtěli, jsme kontaktujte správce ve vaší organizaci o resetování vašeho hesla.</p>
            </td>
            <td>
              <p>Zajistěte, aby že se správně vytvořená o kontaktech na souboru v adresáři před pokračováním. Zjistit, <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">Jaká data se používá tak, že resetování hesla</a> informace o tom, jak nakonfigurovat ověřovací údaje v adresáři tak, aby uživatelům nezobrazuje tato chyba.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Adresář aktivované řešení resetování hesla, ale uživatel obsahuje pouze jeden údaj o kontaktech na soubor, při nastavení zásad vyžaduje dva kroky ověření</p>
            </td>
            <td>
              <p>Váš účet není povoleno pro resetování hesla</p>
              <p>Je nám líto, ale správce nebyla nastavení účtu pro použití s tuto službu. </p>
              <p>

              </p>
              <p>Pokud byste chtěli, jsme kontaktujte správce ve vaší organizaci o resetování vašeho hesla.</p>
            </td>
            <td>
              <p>Zajistěte, aby že tento uživatel má aspoň dva správně nakonfigurovaném způsoby kontaktu (například mobilního telefonu a Telefon do kanceláře) pokračovat. Zjistit, <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">Jaká data se používá tak, že resetování hesla</a> informace o tom, jak nakonfigurovat ověřovací údaje v adresáři tak, aby uživatelům nezobrazuje tato chyba.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Adresář aktivované řešení resetování hesla uživatele správně nakonfigurovaný, ale uživatel nemá kontaktovat </p>
            </td>
            <td>
              <p>Ale ne!  Došlo k neočekávané chybě při kontaktovat vás.</p>
            </td>
            <td>
              <p>To může být výsledkem chyba dočasné služby nebo špatně nakonfigurované o kontaktech, který jsme nerozpoznal správně. Pokud uživatel čeká 10 sekund, zkuste znovu a "kontaktujte svého správce" odkaz se zobrazí. Zkuste znova kliknout odešle znovu volání, že kliknete na "kontaktujte svého správce" vám pošle e-mail s formuláři uživateli, heslo nebo globálních správců (v tomto pořadí priorit) požadování hesla resetovat provádět daného uživatelského účtu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nikdy uživateli resetování hesla služby SMS nebo telefonního hovoru</p>
            </td>
            <td>
              <p>Kliknutí uživatele "text Já" nebo "zavolejte mi" a nikdy nic přijímá.</p>
            </td>
            <td>
              <p>Může to být výsledek formátované mailová telefonní číslo v adresáři. Musí být telefonní číslo ve formátu "+ ccc xxxyyyzzzzXeeee". Další informace o formátování telefonní čísla pro použití s resetování hesla najdete v článku, <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">Jaká data používá resetování hesla</a>.</p>
              <p>Pokud požadujete rozšíření na směrovány na uživatele, mějte na paměti této resetování hesla nepodporuje rozšíření, přestože zadáte v adresáři (budou vynechány před odesláním hovor). Zkuste použít číslo bez rozšíření, nebo integrace rozšíření do telefonní číslo ve vaší Ústředně.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nikdy se uživateli resetovat heslo e-mailu</p>
            </td>
            <td>
              <p>Uživatel klikne "e-mailem Já" a nikdy nic přijímá.</p>
            </td>
            <td>
              <p>Nejběžnější příčinou tohoto problému je, že zprávu odmítl pomocí filtru nevyžádané pošty. Kontrola nevyžádané pošty, složky Nevyžádaná pošta nebo Odstraněná pošta pro e-mailu.</p>
              <p>Taky zajistit, že se musí správné e-mailovou zprávu: velkým množstvím lidí velmi podobné e-mailové adresy a skončily kontrola nesprávnou složku Doručená pošta pro zprávu. Pokud ani jeden z těchto možností pracovat, je také možné, že je poškozený e-mailovou adresu v adresáři, zkontrolujte, ujistěte se, že je správnou e-mailovou adresu a zkuste to znovu. Další informace o formátování e-mailové adresy pro resetování hesla najdete v článku <a href="active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset">Jaká data používá resetování hesla</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nastavím zásady resetovat heslo, ale když účet správce použije resetování hesla, že není zásada</p>
            </td>
            <td>
              <p>Resetování hesla účty správců najdete v článku stejné možnosti povoleno pro resetování hesla, e-mailu a mobilní telefon, bez ohledu na to, jaké zásady nastaveno na kartě <strong>Konfigurovat</strong> v části <strong>Zásady resetování uživatelského hesla</strong> .</p>
            </td>
            <td>
              <p>Možnosti nakonfigurované části <strong>Zásad resetování uživatelského hesla</strong> karty <strong>Konfigurovat</strong> jen u koncoví uživatelé ve vaší organizaci.</p>
              <p>Spravuje Microsoft a ovládací prvky resetování hesla správce zásad pro zajištění nejvyšší úrovně zabezpečení</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Uživatel zabráněno pokoušející opakovaně pokoušeli v den resetování hesla</p>
            </td>
            <td>
              <p>Uživateli se zobrazí chybová zpráva:</p>
              <p>

              </p>
              <p>Stiskněte klávesovou zkratku jinou možnost.</p>
              <p>Jste se pokusili ověření vašeho účtu opakovaně pokoušeli v poslední 1 hod. Z bezpečnostních důvodů budete muset počkat 24 hodin, než zkuste to znovu. </p>
              <p>Pokud byste chtěli, jsme kontaktujte správce ve vaší organizaci o resetování vašeho hesla.</p>
            </td>
            <td>
              <p>Jsme implementovat mechanismus omezení automatické k blokování uživatelů z pokusem o resetování hesla opakovaně pokoušeli v krátký časový úsek. K tomu dojde, pokud:</p>
              <ol class="ordered">
                <li>
Uživatel se pokusí ověřit telefonní číslo 5 číslem 1 hodinu.<br\><br\></li>
                <li>
Uživatel se pokusí používat bránu dotazy zabezpečení 5 číslem za hodinu.<br\><br\></li>
                <li>
Uživatel se pokusí resetování hesla pro stejného uživatelského účtu 5 číslem za hodinu.<br\><br\></li>
              </ol>
              <p>Pokud to pokud chcete opravit, musí uživatel počkejte 24 hodin za poslední pokus o a uživatel pak budou moct pořizovat resetovat své heslo.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Uživateli se zobrazí chyba při ověřování přihlásil telefonní číslo</p>
            </td>
            <td>
              <p>Při pokusu o ověření telefonu se použít jako metody ověřování, uživateli se zobrazí chybová zpráva:</p>
              <p>

              </p>
              <p>Nesprávný telefonní číslo zadané.</p>
            </td>
            <td>
              <p>K této chybě dochází, když telefonní číslo zadali neodpovídá telefonní číslo na soubor.</p>
              <p>Zkontrolujte, že uživatel zadá úplné telefonní číslo, včetně plošný graf a země kódu, když se pokusíte použít metodu telefonu pro resetování hesla.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Chyba při zpracování požadavku</p>
            </td>
            <td>
              <p>Uživateli se zobrazí chybu, které je tento pokyn:</p>
              <p>

              </p>
              <p>Chyba při zpracování požadavku </p>
              <p>Při pokusu o resetování hesla.</p>
            </td>
            <td>
              <p>Příčinou může být mnoho problémů, ale obvykle se tato chyba způsobená buď o služby výpadku nebo konfigurační problém, který nelze přeložit. </p>
              <p>Pokud se zobrazí tato chyba a je dopad vaší firmě, kontaktujte podporu a můžeme vám pomůžou typu co nejdříve. Přečtěte si <a href="#information-to-include-when-you-need-help">informace, když potřebujete pomoc s zobrazil</a> zobrazíte by měl obsahovat k pracovník technické podpory na podporu rychlou rozlišení.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback"></a>Poradce při potížích s zpětným heslo
Pokud dojde k chybě při povolení, zakázání nebo pomocí hesla zpětným, nejspíš vás bude vyřešit podle následujícího postupu řešení potíží:

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Chyba případu</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>K čemu chyby uživatele zobrazit?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Řešení</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Chyby rychlého připojení Obecné a při spuštění</p>
            </td>
            <td>
              <p>Heslo resetovat není služba místně s chybou 6800 ve počítače Azure AD Connect protokolu událostí.</p>
              <p>

              </p>
              <p>Po rychlého připojení federované nebo algoritmus hash hesla synchronizovaných uživatelů nemůžou resetovat hesla.</p>
            </td>
            <td>
              <p>Při zapnuté funkci zpětným heslo, modul Synchronizace volání knihovnu zpětným provádět konfigurace (rychlého připojení) tak, že mluvit cloudové služby rychlého připojení. Veškeré nalezené během rychlého připojení nebo při spuštění WCF koncový bod pro heslo zpětným chyby povede chyb v protokolu událostí na počítači Azure AD Connect v protokolu událostí.</p>
              <p>Během restartování ADSync služby pokud zpětného zápisu o konfiguraci, koncového bodu WCF bude spuštěn. Pokud spuštění koncový bod nepovede, jsme bude jednoduše protokolu událostí 6800 a spuštěním služby synchronizace, aby. Informace o stavu této události znamená, že heslo zpětným nebyl spuštění koncového bodu. Podrobnosti v protokolu událostí pro tuto událost (6800) spolu s položky protokolu událostí vygenerovat PasswordResetService součásti výskyt znamená Proč nelze spustit koncový bod. Zkontrolujte tyto chyby v protokolu událostí a znovu spustíte Azure AD Connect, pokud heslo zpětným stále nefunguje. Pokud potíže potrvají, zkuste zakázat a znovu povolit zpětným heslo.</p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Pokud se uživatel snaží resetování hesla nebo odemknutí účet s podporou zpětným heslo, se nezdaří. Kromě toho se zobrazí události v protokolu událostí na Azure AD Connect obsahující: "synchronizace Engine vrácena chyba hr = 800700CE, zprávy = názvu souboru nebo rozšíření je příliš dlouhá" po dokončení operace odemknout.
                            </p>
            </td>
            <td>
              <p>K tomu může dojít, pokud vám to upgradovali ze starších verzí Azure AD Connect nebo DirSync. Upgrade ke starším verzím Azure AD Connect nastavte 254 znak heslo pro účet Azure AD Management Agent (novějších verzích se nastavení délka hesla 127 znaků). Tyto dlouhá hesla fungovat AD spojnice Import a Export operace ale nejsou podporovány operaci odemknout.
                            </p>
            </td>
            <td>
              <p>[Vyhledání účet služby Active Directory](active-directory-aadconnect-accounts-permissions.md#active-directory-account) pro Azure AD Connect a resetovat heslo obsahovat maximálně 127 znaků. Otevřete **Službu synchronizace** z nabídky Start. Přejděte ke **spojnicím** a najděte **Konektor služby Active Directory**. Vyberte ho a klikněte na **Vlastnosti**. Přejděte na stránku **přihlašovací údaje** a zadejte nové heslo. Kliknutím na **OK** zavřete stránku.
                            </p>
            </td>
          </tr>
                    <tr>
            <td>
              <p>Chyba konfigurace během instalace Azure AD Connect zpětného zápisu.</p>
            </td>
            <td>
              <p>V posledním kroku proces instalace Azure AD Connect zobrazí chyba označující, že nejde nakonfigurovat zpětným heslo.</p>
              <p>

              </p>
              <p>Azure AD připojení protokolu událostí obsahuje chybu 32009 s textem "Chyba zobrazuje auth tokenu".</p>
            </td>
            <td>
              <p>K této chybě dochází v obou případech:</p>
              <ul>
                <li class="unordered">
Zadali jste nesprávné heslo pro účet globálního správce zadaný na začátku tohoto procesu instalace Azure AD Connect.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Jste se pokusili Federovaný uživatel určenou uvedené na začátku tohoto procesu instalace Azure AD Connect účtu globálního správce.<br\><br\></li>
              </ul>
              <p>Opravit tuto chybu, ujistěte se, že nejste pomocí federované účtu globálního správce zadaná na začátku tohoto procesu instalace Azure AD Connect a správnost zadané heslo.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Chyba konfigurace během instalace Azure AD Connect zpětného zápisu.</p>
            </td>
            <td>
              <p>Protokol Azure AD Connect počítač událostí obsahuje chybu 32002 vyvolané PasswordResetService.</p>
              <p>

              </p>
              <p>Přečte Chyba: "Chyba připojení k ServiceBus tokenu poskytovatele nemohl poskytnout tokenu zabezpečení..."</p>
              <p>

              </p>
            </td>
            <td>
              <p>Příčina tato chyba je služba resetovat heslo spuštěná v místním prostředí není možné se připojit k koncový bod služby bus v cloudu. Tato chyba je obvykle zpravidla způsobená pravidla brány firewall blokuje odchozí připojení na konkrétní port nebo webové adresy.</p>
              <p>

              </p>
              <p>Zkontrolujte, jestli že brány firewall povolí odchozí připojení tyto věci:</p>
              <ul>
                <li class="unordered">
Všechny přenosy přes TCP 443 (HTTPS)<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Odchozí připojení <br\><br\></li>
              </ul>
              <p>

              </p>
              <p>Po aktualizaci následujícími pravidly restartovat počítač Azure AD Connect a zpětným heslo by měly začít fungovat znovu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Heslo zpětným koncový bod místní není dostupný</p>
            </td>
            <td>
              <p>Po ukončení práce pro některé čas federované nebo algoritmus hash hesla synchronizovaných uživatelů nemůžou resetovat hesla.</p>
            </td>
            <td>
              <p>V některých případech méně častých může selhat opětovné spuštění po opětovném spuštění Azure AD Connect službu zpětným heslo. V těchto případech nejdřív zkontrolujte, zda heslo zpětným vypadá to povolené na prem. Lze to provést pomocí Průvodce Azure AD Connect nebo prostředí powershell (viz HowTos výše). Pokud se funkci aktivní, zkuste povolení nebo zakázání funkce znovu pomocí prostředí PowerShell nebo uživatelského rozhraní. V tématu "krok 2: povolení zpětným heslo v počítači synchronizaci adresářů &amp; její konfiguraci" v <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">tom, jak povolit nebo zakázat heslo zpětným</a> Další informace o tom, jak to udělat.</p>
              <p>

              </p>
              <p>Pokud to nepomůže, zkuste úplně odinstalovat a znova nainstalujte Azure AD Connect.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Chyby oprávnění</p>
            </td>
            <td>
              <p>Federovaná nebo synchronizaci hesel hash'd uživatelé pokusí obnovit jejich hesel najdete v článku chyba odeslali a heslo, že došlo k potížím služby.</p>
              <p>

              </p>
              <p>Kromě toho během operace resetovat heslo, možná uvidíte, že chyba týkající se správy agent byla odmítnuta na protokoly událostí místní.</p>
            </td>
            <td>
              <p>Pokud se zobrazí tyto chyby v protokolu událostí, potvrďte, že účet AD MA (zadaného v průvodci v době konfigurace) s potřebným oprávněním pro zpětným heslo.</p>
              <p>

              </p>
              <p>VŠIMNĚTE si, že po je uveden tímto oprávněním může trvat až 1 hodinu u oprávnění, která chcete skapat dolů přes sdprop pozadí úkolu na řadiče domény. </p>
              <p>Heslo resetovat pracovat musí razítko na popisovače zabezpečení objektu uživatele, jehož heslo obnovuje oprávnění. Až toto oprávnění se objeví v objektu uživatele, resetovat hesla zůstanou dojde k chybě oznamující odepření přístupu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Chyba při konfiguraci zpětným heslo z Průvodce konfigurací Azure AD Connect </p>
            </td>
            <td>
              <p>Chyba "Nelze MA vyhledejte" v Průvodci při povolení nebo zakázání zpětným heslo</p>
            </td>
            <td>
              <p>Je známý problém v vydanou verzi Azure AD Connect, které manifesty v následujících situacích:</p>
              <ol class="ordered">
                <li>
Konfigurace Azure AD Connect pro klienta abc.com (domény ověřeno) pomocí pověření. Výsledkem AAD spojnice s názvem "abc.com – AAD" vzniku.<br\><br\></li>
                <li>
Potom potom změníte pověření AAD konektoru (pomocí staré synchronizace uživatelského rozhraní) na (vezměte v úvahu, že je stejném klientovi ale jiným názvem domény). <br\><br\></li>
                <li>
Teď se pokusíte povolit nebo zakázat zpětným heslo. Průvodce vytvářet název konektoru pomocí pověření, jako "abc.onmicrosoft.com – AAD" a předejte rutinu zpětným heslo. Tím se nepovede, protože neexistuje žádná spojnice vytvořili s tímto názvem.<br\><br\></li>
              </ol>
              <p>To byl odstraněn v naší nejnovější sestavení. Pokud máte starší sestavení jedním řešení je získáte pomocí rutiny prostředí powershell povolit nebo zakázat funkce. V tématu "krok 2: povolení zpětným heslo v počítači synchronizaci adresářů &amp; její konfiguraci" v <a href="active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords">tom, jak povolit nebo zakázat heslo zpětným</a> Další informace o tom, jak to udělat.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Nejde o resetování hesla pro uživatele v speciální skupiny například Domain Admins / podnikový správce atd.</p>
            </td>
            <td>
              <p>Federované nebo synchronizaci hesel hash'd uživatelů, kteří jsou součástí chráněné skupiny a pokusit se o resetování jejich hesel najdete v článku chyba odeslali a heslo, že došlo k potížím služby.</p>
            </td>
            <td>
              <p>Privilegovaných uživatelů ve službě Active Directory jsou chráněné pomocí AdminSDHolder. V tématu <a href="https://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx">http://technet.microsoft.com/magazine/2009.09.sdadminholder.aspx</a> další podrobnosti. </p>
              <p>

              </p>
              <p>To znamená popisovače zabezpečení na tyto objekty jsou pravidelně zkontrolována podle jednoho v souladu s AdminSDHolder a se neobnoví, pokud se liší. Další oprávnění, které jsou potřebné pro heslo zpětným proto není skapat pro tyto uživatele. Může být výsledkem zpětným heslo nefunguje pro tyto uživatele. V důsledku toho nepodporujeme Správa hesla pro uživatele v těchto skupinách protože konce model zabezpečení AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Obnovení operace dojde k chybě s objektem a se nepodařilo najít</p>
            </td>
            <td>
              <p>Federované nebo synchronizaci hesel hash'd uživatelé pokusí obnovit jejich hesel najdete v článku chyba odeslali a heslo, že došlo k potížím služby.</p>
              <p>

              </p>
              <p>Kromě toho během operace resetovat heslo, může zobrazit chyba v protokolu událostí služby Azure AD Connect označující chyba "Objekt nebyl nalezen".</p>
            </td>
            <td>
              <p>Tato chyba obvykle značí, že nemůžete najít objektu uživatele v prostředí spojnice AAD neboli propojené více hodnot nebo objekt místo spojnice AD sync engine. </p>
              <p>

              </p>
              <p>Poradce při potížích s, ujistěte se, že uživatel se nakonec synchronizuje z místní AAD pomocí aktuální instance aplikace Azure AD Connect a zkontrolovat stav objektů v spojnice mezery a více hodnot. Zkontrolujte, zda objekt AD CS spojnice tak, aby objekt více hodnot pomocí pravidla "Microsoft.InfromADUserAccountEnabled.xxx".</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Obnovení dojde k chybě operace s více eror nalezené shody</p>
            </td>
            <td>
              <p>Federované nebo synchronizaci hesel hash'd uživatelé, kteří se pokusí o resetování jejich hesel najdete v článku chyba odeslali a heslo, že došlo k potížím služby.</p>
              <p>

              </p>
              <p>Kromě toho během operace resetovat heslo, může zobrazit chyba v protokolu událostí služby Azure AD Connect označující chybu "Více maches nalezen".</p>
            </td>
            <td>
              <p>Tento údaj označuje, že modul Synchronizace zjistili, že je objekt více hodnot připojit k více objektů služby AD CS prostřednictvím "Microsoft.InfromADUserAccountEnabled.xxx". Znamená to, jestli má uživatel v víc doménové účet povolené. </p>
              <p>

              </p>
              <p>Tento scénář aktuálně nepodporuje zpětným heslo.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Operace heslo nepovede, zobrazí se chyba konfigurace.</p>
            </td>
            <td>
              <p>Operace heslo nepovede, zobrazí se chyba konfigurace. Protokol událostí aplikace obsahuje chybu Azure AD Connect 6329 s textem: 0x8023061f (operaci synchronizace hesel není povolený na tento Agent pro správu se nezdařila.)</p>
            </td>
            <td>
              <p>K tomu dojde, pokud dojde ke změně konfigurace Azure AD Connect přidáte&nbsp;strukturu nové AD (nebo na odebrat a znova přidat existující struktuře) <strong>Po</strong> zapnuto funkci zpětným heslo. Operace heslo pro uživatele v těchto nově přidaný doménových se nezdaří. Tento problém zakázat a znovu povolit funkci heslo zpětným po dokončení změny konfigurace doménové.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zpět psaní hesel, které obnoveny uživatelé funguje správně, ale psaní zase hesla měnit uživatele nebo obnovit uživatele správcem nezdaří.</p>
            </td>
            <td>
              <p>Při pokusu o resetování hesla jménem uživatele z portálu Správa Azure, se zobrazí zpráva: "službu resetovat heslo spuštěné v místním prostředí podporuje správce resetování uživatelského hesla. Upgradujte na nejnovější verzi Azure AD Connect tento problém vyřešíte."</p>
            </td>
            <td>
              <p>V takovém případě verzi modul Synchronizace nepodporuje konkrétní heslo zpětným operaci, která byla použita. Verze Azure AD Connect později než 1.0.0419.0911 podporuje všechny operace správy hesel, včetně hesla resetovat zpětného zápisu, zpětným změnit heslo a inicializovaný správce heslo resetovat zpětným z portálu Správa Azure.&nbsp; Verze DirSync později než 1.0.6862 podporují pouze zpětným resetovat heslo. Tento problém můžete vyřešit, doporučujeme nainstalovat nejnovější verzi Azure AD Connect nebo připojení Azure Active Directory (Další informace najdete v tématu <a href="active-directory-aadconnect">Nástroje pro integraci adresářů</a>) při řešení tohoto problému a využívejte heslo zpětným ve vaší organizaci.</p>
            </td>
          </tr>
        </tbody></table>


## <a name="password-writeback-event-log-error-codes"></a>Kódy chyb v protokolu událostí heslo zpětného zápisu
Osvědčený postup při řešení problémů s heslo zpětným je ke kontrole tohoto protokolu událostí na počítači Azure AD Connect. Protokol událostí bude obsahovat události ze dvou zdrojů potřebné pro zpětným heslo. Zdroji PasswordResetService popisuje operace a problémy související s operací zpětným heslo. Zdroji ADSync popisuje operace a otázky týkající se nastavení hesla ve vašem prostředí AD.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Kód</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Pojmenování / zpráv</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Zdroje</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Popis</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>6329</p>
            </td>
            <td>
              <p>NICHŽ: MMS(4924) 0x80230619 – "omezení zabrání heslo změněné na aktuální zadanému."</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Tato událost nastane při službu zpětným heslo k nastavení hesla u svého místního adresáře, který nesplňuje stáří hesla, historie, složitost nebo filtrování požadavky na domény.</p>
              <ul>
                <li class="unordered">
Pokud jste heslo minimální věk a jste nedávno změnili heslo do okna počet minut, nebudete moct znovu změnit heslo, dokud nedosáhnete zadaný stáří ve vaší doméně. Pro účely testování minimální věk je třeba nastavit na hodnotu 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Pokud máte požadavky na historii hesla povolené a pak je nutné vybrat heslo, které nepoužíval v poslední doby N, kde N je nastavení historii hesel. Pokud vyberete hesla používaného v poslední doby N, potom zobrazí se selhání v tomto případě. Pro účely testování historie je třeba nastavit na hodnotu 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Pokud máte složitost hesel, když se uživatelé pokusí změna nebo resetování hesla všem poznámkám budou vynucené.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Pokud jste vytvořili heslo filtry povolené a uživatel vybere heslo, které odpovídají filtrování kritériím, klikněte obnovit nebo změnit se nezdaří.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>HR 8023042</p>
            </td>
            <td>
              <p>Synchronizace Engine vrácena chyba hr = 80230402, zpráva = pokus o získat objekt nezdařilo jsou duplicitní položky se stejnou ukotvení</p>
            </td>
            <td>
              <p>ADSync</p>
            </td>
            <td>
              <p>Při zapnuté funkci stejné id uživatele ve víc domén, dojde k této události.  Například pokud jsou synchronizaci strukturami účtu/zdroje a mají stejné uživatelské id prezentovat a zapnutí v každém, této chybě může dojít.  </p>
              <p>K této chybě může dojít, pokud používáte atribut-jedinečné ukotvení (jako alias nebo UPN) a dva uživatelé sdílet stejnou atributu ukotvení.</p>
              <p>Tento problém vyřešíte ověřte, že nemáte žádné duplicitní uživatele v rámci svojí domény a, že používáte atribut jedinečné ukotvení pro každého uživatele.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31001</p>
            </td>
            <td>
              <p>PasswordResetStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje službu místní rozpoznání požadavek federované pro vytvoření nového hesla nebo synchronizaci hesel hash'd uživatele pocházející z cloudu. Tato událost je že první událost v každé heslo resetovat operace zpětného zápisu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31002</p>
            </td>
            <td>
              <p>PasswordResetSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že uživatel zaškrtnuté nové heslo během operace resetovat heslo, jsme rozhodli, že toto heslo splňuje požadavky na podnikové heslo a toto heslo byl úspěšně zapsán zpátky do místního prostředí AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31003</p>
            </td>
            <td>
              <p>PasswordResetFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje vybraný heslo uživatele a hesla úspěšně doručené do místního prostředí, ale nemůžeme pokus o nastavení hesla v místním prostředí AD, došlo k chybě. Tato situace může nastat z několika důvodů:</p>
              <ul>
                <li class="unordered">
Heslo uživatele nesplňuje věku, historie a složitosti, nebo filtrovat požadavky na domény. Zkuste tento problém vyřešíte úplně nové heslo.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Účtu služby MA nemáte potřebná oprávnění k nastavení nového hesla na je uživatelský účet.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Uživatelský účet je ve skupině chráněné například správce domény nebo enterprise, které nepovoluje operace nastavit heslo.<br\><br\></li>
              </ul>
              <p>V tématu <a href="#troubleshoot-password-writeback">Poradce při potížích s zpětným heslo</a> se dozvíte víc o jaké situtions tuto chybu může způsobovat.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31004</p>
            </td>
            <td>
              <p>OnboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost dojde, když povolíte heslo zpětným s Azure AD Connect a označuje jsme spuštění rychlého připojení v organizaci povolili heslo zpětným webové služby.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31005</p>
            </td>
            <td>
              <p>OnboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje procesu rychlého připojení byl úspěšný a že jsou připravené k použití funkce zpětným heslo.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31006</p>
            </td>
            <td>
              <p>ChangePasswordStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že služba místní zjištěno požadavek na změnu hesla pro federované nebo synchronizaci hesel hash'd uživatele pocházející z cloudu. Tato událost je první událost do každé operace zpětného zápisu změnit heslo.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31007</p>
            </td>
            <td>
              <p>ChangePasswordSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že uživatel zaškrtnuté nové heslo během operace změnit heslo, jsme rozhodli, že toto heslo splňuje požadavky na podnikové heslo a toto heslo byl úspěšně zapsán zpátky do místního prostředí AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31008</p>
            </td>
            <td>
              <p>ChangePasswordFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje vybraný heslo uživatele a hesla úspěšně doručené do místního prostředí, ale nemůžeme pokus o nastavení hesla v místním prostředí AD, došlo k chybě. Tato situace může nastat z několika důvodů:</p>
              <ul>
                <li class="unordered">
Heslo uživatele nesplňuje věku, historie a složitosti, nebo filtrovat požadavky na domény. Zkuste tento problém vyřešíte úplně nové heslo.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Účtu služby MA nemáte potřebná oprávnění k nastavení nového hesla na je uživatelský účet.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Uživatelský účet je ve skupině chráněné například správce domény nebo enterprise, které nepovoluje operace nastavit heslo.<br\><br\></li>
              </ul>
              <p>V tématu <a href="#troubleshoot-password-writeback">Poradce při potížích s zpětným heslo</a> se dozvíte víc o jaké situacích tuto chybu může způsobovat.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31009</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Místní služba zjištěná požadavek federované pro vytvoření nového hesla nebo synchronizaci hesel hash'd uživatele pocházející z správce jménem uživatele. Tato událost je první událost do každé inicializovaný správce heslo resetovat zpětným operace.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31010</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Správce zaškrtnuté nové heslo během operace resetovat heslo správce inicializovaný, jsme rozhodli, že toto heslo splňuje požadavky na podnikové heslo a toto heslo byl úspěšně zapsán zpátky do místního prostředí AD.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31011</p>
            </td>
            <td>
              <p>ResetUserPasswordByAdminFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Správce vybrané hesla jménem uživatele a toto heslo úspěšně doručené do místního prostředí, ale nemůžeme pokus o nastavení hesla v místním prostředí AD, došlo k chybě. Tato situace může nastat z několika důvodů:</p>
              <ul>
                <li class="unordered">
Heslo uživatele nesplňuje věku, historie a složitosti, nebo filtrovat požadavky na domény. Zkuste tento problém vyřešíte úplně nové heslo.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Účtu služby MA nemáte potřebná oprávnění k nastavení nového hesla na je uživatelský účet.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Uživatelský účet je ve skupině chráněné například správce domény nebo enterprise, které nepovoluje operace nastavit heslo.<br\><br\></li>
              </ul>
              <p>V tématu <a href="#troubleshoot-password-writeback">Poradce při potížích s zpětným heslo</a> se dozvíte víc o jaké situtions tuto chybu může způsobovat.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31012</p>
            </td>
            <td>
              <p>OffboardingEventStart </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost dojde, když zakážete heslo zpětným s Azure AD Connect a označuje jsme spuštění offboarding v organizaci povolili heslo zpětným webové služby.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31013</p>
            </td>
            <td>
              <p>OffboardingEventSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje procesu offboarding úspěšné a úspěšně zakázat možnost zpětným heslo.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31014</p>
            </td>
            <td>
              <p>OffboardingEventFail </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že proces offboarding nebyl úspěšný. Může to být kvůli chybě oprávnění k účtu správce cloudu a místní určenému během konfigurace, nebo protože se pokoušíte použít federované cloudu globální správce, když zakážete zpětným heslo. Pokud to pokud chcete opravit, zkontrolujte na správu oprávnění a že nepoužíváte žádné federované účtu při konfiguraci funkci zpětným heslo.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31015</p>
            </td>
            <td>
              <p>WriteBackServiceStarted </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že služba heslo zpětným byla úspěšně spuštěna a je připraven k přijímaní žádostí správy heslo z cloudu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31016</p>
            </td>
            <td>
              <p>WriteBackServiceStopped </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že služba heslo zpětným zastavila a všechny žádosti o řízení heslo z cloudu nebude úspěšná.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31017</p>
            </td>
            <td>
              <p>AuthTokenSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že jsme úspěšně získat token povolení globálního správce během instalace Azure AD Connect určena k zahájení procesu offboarding nebo rychlého připojení.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>31018</p>
            </td>
            <td>
              <p>KeyPairCreationSuccess </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že jsme vytvořili úspěšně šifrovací klíč hesla, který bude sloužit k šifrování hesel z cloudu zasílané do místního prostředí.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32000</p>
            </td>
            <td>
              <p>UnknownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje k neznámé chybě během operace správy heslo. Podívejte se na text výjimky v případě další podrobnosti. Pokud máte potíže, zkuste zakázat a znovu povolit zpětným heslo. Pokud to nepomůže, vkládat kopii protokolu událostí spolu s id sledování zadané zasvěcených osob do pracovník technické podpory.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32001</p>
            </td>
            <td>
              <p>ServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že chybu připojení ke cloudové heslo resetovat služby a obecně bude proveden poté službu místní se nemůže připojit k webové službě resetovat heslo. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32002</p>
            </td>
            <td>
              <p>ServiceBusError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje došlo k chybě připojení k instanci služby bus vašeho klienta. Tato situace může nastat, protože jsou ve vašem místním prostředí blokování odchozí připojení. Zaškrtněte políčko brány firewall zajistit, aby umožňoval připojení přes TCP 443 a <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>a zkuste to znovu. Pokud máte stále potíže, zkuste zakázat a znovu povolit zpětným heslo.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32003</p>
            </td>
            <td>
              <p>InPutValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje neplatným vstupní předán naše rozhraní API webových služeb. Akci opakujte.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32004</p>
            </td>
            <td>
              <p>DecryptionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že došlo k chybě dešifrování heslo, které přišla z cloudu. Může to být kvůli dešifrování klíčové neshodu mezi Cloudová služba a místního prostředí. Pokud chcete tento problém vyřešíte zakázat a znovu povolit zpětným heslo v místním prostředí.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32005</p>
            </td>
            <td>
              <p>ConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Během rychlého připojení doporučujeme uložit informace specifické pro klienta v souboru konfigurace v místním prostředí. Tato událost označuje došlo k chybě při uložení tohoto souboru nebo, pokud tam spuštění služby chybu čtení souboru. Tento problém můžete vyřešit, zkuste zakázání a opětovné povolení zpětného zápisu heslo vynutit znovu psát tento soubor. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32006</p>
            </td>
            <td>
              <p>EndPointConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ZASTARALÉ – Tato událost není k dispozici v Azure AD Connect, pouze velmi brzy verzích DirSync, který podporuje zpětného zápisu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32007</p>
            </td>
            <td>
              <p>OnBoardingConfigUpdateError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Během rychlého připojení jsme odeslat data z cloudu na místní heslo resetovat služby. Tato data se pak došlo k zápisu souboru v paměti před odesláním ke službě synchronizace k bezpečnému ukládání tyto informace na disku. Tato událost označuje potíže s psaní nebo aktualizace dat v paměti. Tento problém můžete vyřešit, zkuste zakázání a opětovné povolení zpětného zápisu heslo vynutit znovu psát tuto konfiguraci.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32008</p>
            </td>
            <td>
              <p>ValidationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že jsme dostali neplatnou odpověď z webové služby resetovat heslo. Tento problém můžete vyřešit, zkuste zakázání a opětovné povolení zpětným heslo.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32009</p>
            </td>
            <td>
              <p>AuthTokenError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost znamená, že jsme nešlo získat povolení tokenu zadaným během instalace Azure AD Connect účtu globálního správce. Tuto chybu může způsobovat chybná uživatelské jméno a heslo určeným pro účtu globálního správce nebo je federované protože zadán účtu globálního správce. Tento problém můžete vyřešit, opětovným spuštěním konfigurace s správné uživatelské jméno a heslo a ujistěte se, správce spravované (jen cloudu nebo synchronizaci hesel by) účtu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32010</p>
            </td>
            <td>
              <p>CryptoError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje došlo k chybě při generování heslo šifrovací klíč nebo dešifrování heslo, které přichází z cloudové služby. Tato chyba pravděpodobně označuje problém s prostředí. Podívejte se na podrobné informace o protokolu událostí chcete dozvědět víc a tento problém vyřešit. Můžete také vyzkoušet zakázat a znovu povolit službu zpětným heslo pro tento problém vyřešíte.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32011</p>
            </td>
            <td>
              <p>OnBoardingServiceError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že službu místní nelze správně komunikovat s heslo resetovat webové služby zahájení procesu rychlého připojení. Příčinou může být pravidlo brány firewall nebo problém začíná auth token pro vašeho klienta. Pokud to pokud chcete opravit, zkontrolujte, že je neblokuje odchozí připojení po TCP 443 nebo TCP 9350 9354 nebo <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>a, který není federovaný účtu správce AAD pomocí integrovaného. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32012</p>
            </td>
            <td>
              <p>OnBoardingServiceDisableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ZASTARALÉ – Tato událost není k dispozici v Azure AD Connect, pouze velmi brzy verzích DirSync, který podporuje zpětného zápisu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32013</p>
            </td>
            <td>
              <p>OffBoardingError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že službu místní nelze správně komunikovat s heslo resetovat webové služby zahájení procesu offboarding. Příčinou může být pravidlo brány firewall nebo problém začíná token se tak mohli ověřovat pro vašeho klienta. Pokud to pokud chcete opravit, zkontrolujte, zda neblokuje odchozí připojení přes 443 nebo <a href="https://ssprsbprodncu-sb.accesscontrol.windows.net/">https://ssprsbprodncu-sb.accesscontrol.windows.net/</a>a že účtu správce AAD používáte k offboard není federovaný. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32014</p>
            </td>
            <td>
              <p>ServiceBusWarning </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že museli jsme opakovat se připojit k vašemu tenantovi služby bus instanci. Normální podmínek to by neměly být důležité, ale pokud se zobrazí tato událost opakovaně pokoušeli, zvažte Kontrola připojení k síti Bus služeb, zejména pokud je to vysokou latencí nebo pomalé připojení.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>32015</p>
            </td>
            <td>
              <p>ReportServiceHealthError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Abyste mohli sledovat stav služby zpětným heslo, jsme odesláno prezenční signál data naše heslo resetovat webová služba každých 5 minut. Tato událost označuje, že došlo k chybě při posílání tyto informace stavu webové služby cloudu. Tyto informace stavu nezahrnuje OII nebo Umožňujících data a je čistě prezenční signál a statistiky služby základní tak, aby nabízíme informací o stavu služby v cloudu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33001</p>
            </td>
            <td>
              <p>ADUnKnownError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že došlo k neznámé chybě vrácené AD, vyhledejte události ze zdroje ADSync Další informace o tato chyba v protokolu událostí server Azure AD Connect.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33002</p>
            </td>
            <td>
              <p>ADUserNotFoundError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že uživatele, kterému je pokusu o resetování nebo změna hesla nebyl nalezen v místním adresáři. Tato situace může nastat po uživatel byl místně odstraněný, ale ne v cloudu, nebo pokud se vyskytne problém se synchronizací. Kontrola synchronizace protokoly, stejně jako poslední několik synchronizace spustit podrobnosti o další informace.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33003</p>
            </td>
            <td>
              <p>ADMutliMatchError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Po resetování hesla nebo požadavek na změnu pochází z cloudu, používáme ukotvení cloudu zadané při nastavování Azure AD Connect zjistit, jak propojit žádosti uživatele v místním prostředí. Tato událost označuje jsme našli dvou uživatelů v adresáři místní se stejným atributem ukotvení cloudu. Kontrola synchronizace protokoly, stejně jako poslední několik synchronizace spustit podrobnosti o další informace.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33004</p>
            </td>
            <td>
              <p>ADPermissionsError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost znamená, že účet služby Agent pro správu nemá potřebná oprávnění nastavení nového hesla daného účtu. Zajistěte MA účet v jeho struktuře obnovit a změna oprávnění hesla u všech objektů ve struktuře.  Další informace o tom, udělejte toto, přečtěte si téma <a href="active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions">Krok 4: nastavení potřebná oprávnění služby Active Directory</a>.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33005</p>
            </td>
            <td>
              <p>ADUserAccountDisabled </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje jsme pokusu o resetování nebo změna hesla u účtu, který byl zakázán místně. Povolení účtu a akci opakujte.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33006</p>
            </td>
            <td>
              <p>ADUserAccountLockedOut </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Událost označuje jsme pokusu o resetování nebo změna hesla u účtu, který byl zablokovaní, místní nasazení. Uzamčení může dojít, pokud uživatel vyzkoušeli ke změně nebo resetování hesla operace opakovaně pokoušeli krátké. Odemknutí účet a akci opakujte.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33007</p>
            </td>
            <td>
              <p>ADUserIncorrectPassword </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že uživatel zadal nesprávné aktuální heslo při provádění heslo změnit operace. Zadejte aktuální heslo a zkuste to znovu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>33008</p>
            </td>
            <td>
              <p>ADPasswordPolicyError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost nastane při službu zpětným heslo k nastavení hesla u svého místního adresáře, který nesplňuje stáří hesla, historie, složitost nebo filtrování požadavky na domény.</p>
              <ul>
                <li class="unordered">
Pokud jste heslo minimální věk a jste nedávno změnili heslo do okna počet minut, nebudete moct znovu změnit heslo, dokud nedosáhnete zadaný stáří ve vaší doméně. Pro účely testování minimální věk je třeba nastavit na hodnotu 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Pokud máte požadavky na historii hesla povolené a pak je nutné vybrat heslo, které nepoužíval v poslední doby N, kde N je nastavení historii hesel. Pokud vyberete hesla používaného v poslední doby N, potom zobrazí se selhání v tomto případě. Pro účely testování historie je třeba nastavit na hodnotu 0.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Pokud máte složitost hesel, když se uživatelé pokusí změna nebo resetování hesla všem poznámkám budou vynucené.<br\><br\></li>
              </ul>
              <ul>
                <li class="unordered">
Pokud jste vytvořili heslo filtry povolené a uživatel vybere heslo, které odpovídají filtrování kritériím, klikněte obnovit nebo změnit se nezdaří.<br\><br\></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>33009</p>
            </td>
            <td>
              <p>ADConfigurationError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>Tato událost označuje, že došlo zápisu problém hesla zpátky do místního adresáře z důvodu konfigurační problém se službou Active Directory. Zkontrolujte protokol událostí aplikace počítače Azure AD Connect zpráv ze služby ADSync Další informace o jaké došlo k chybě. </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34001</p>
            </td>
            <td>
              <p>ADPasswordPolicyOrPermissionError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ZASTARALÉ – Tato událost není k dispozici v Azure AD Connect, pouze velmi brzy verzích DirSync, který podporuje zpětného zápisu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34002</p>
            </td>
            <td>
              <p>ADNotReachableError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ZASTARALÉ – Tato událost není k dispozici v Azure AD Connect, pouze velmi brzy verzích DirSync, který podporuje zpětného zápisu.</p>
            </td>
          </tr>
          <tr>
            <td>
              <p>34003</p>
            </td>
            <td>
              <p>ADInvalidAnchorError </p>
            </td>
            <td>
              <p>PasswordResetService</p>
            </td>
            <td>
              <p>ZASTARALÉ – Tato událost není k dispozici v Azure AD Connect, pouze velmi brzy verzích DirSync, který podporuje zpětného zápisu.</p>
            </td>
          </tr>
        </tbody></table>

## <a name="troubleshoot-password-writeback-connectivity"></a>Poradce při potížích s připojením zpětným heslo

Pokud jste zaznamenali přerušení služby součástí heslo zpětným Azure AD Connect, tady jsou některé rychlé kroky, kterými můžete vyřešit takto:

 - [Restartování Azure AD připojení služby synchronizace](#restart-the-azure-AD-Connect-sync-service)
 - [Zakázat a znovu povolit funkci zpětným heslo](#disable-and-re-enable-the-password-writeback-feature)
 - [Nainstalujte nejnovější verzi Azure AD Connect](#install-the-latest-azure-ad-connect-release)
 - [Poradce při potížích s zpětným heslo](#troubleshoot-password-writeback)

Obecně doporučujeme provést tyto kroky v pořadí výše k obnovení služby co nejrychlejším způsobem.

### <a name="restart-the-azure-ad-connect-sync-service"></a>Restartování Azure AD připojení služby synchronizace
Opětovným spuštěním služby synchronizace připojení Azure AD můžou pomoct řešit problémy s připojením nebo jiné přechodná problémy se službou.

 1. Jako správce klepněte na tlačítko **Start** na serveru se službou **Azure AD Connect**.
 2. Do vyhledávacího pole zadejte **"services.msc"** a stiskněte klávesu **Enter**.
 3. Vyhledejte položku **Microsoft Azure AD Connect** .
 4. Klikněte pravým tlačítkem myši na položku služby, klikněte na **Restartovat**a počkejte na dokončení operace.

    ![][002]

Tyto kroky znovu navázat spojení s cloudovou službu a vyřešit žádná přerušení, které může docházet.  Pokud opětovným spuštěním služby synchronizace váš problém nevyřeší, doporučujeme, aby se pokusíte zakázat a znovu povolit funkci heslo zpětným jako dalším krokem.

### <a name="disable-and-re-enable-the-password-writeback-feature"></a>Zakázat a znovu povolit funkci zpětným heslo
Zakázání a znovu povolit funkci heslo zpětným pomáhají řešit problémy s připojením.

 1. Jako správce otevřete **Průvodce konfigurací Azure AD Connect**.
 2. V dialogovém okně **připojit k Azure AD** zadejte svoje **přihlašovací údaje Azure AD globální správce**
 3. V dialogovém okně **připojení do služby AD DS** zadejte svoje **přihlašovací údaje Správce služby AD domény**.
 4. V dialogovém okně **jedinečně identifikující vaši uživatelé** klikněte na tlačítko **Další** .
 5. V dialogovém okně **volitelné funkce** zrušte zaškrtnutí políčka **zpětného zápisu heslo** .

    ![][003]

 6. Na tlačítko **Další** přejděte na další stránky dialogové okno beze změny nic až se dostanete na stránce **Připraveno ke konfiguraci** .
 7. Ujistěte se, že na stránce Konfigurovat **heslo zpětného zápisu možnost jako neaktivní** a potom na tlačítko zelené **Konfigurovat** uložte provedené změny.
 8. V dialogovém okně **Dokončeno** volbu **synchronizovat teď** a klikněte na tlačítko **Dokončit** zavřete průvodce.
 9. Znovu otevřete **Průvodce konfigurací Azure AD Connect**.
 10.    **Opakujte kroky 2-8**, s výjimkou zajistit, aby se **zpětného zápisu zaškrtnout heslo** **volitelné funkce** obrazovky znovu povolit službu.

    ![][004]

Tyto kroky znovu navázat spojení s naše Cloudová služba a vyřešit žádná přerušení, které může docházet.

Pokud zakázat a znovu povolit funkci zpětným heslo se váš problém nevyřeší, doporučujeme zkuste znovu nainstalovat Azure AD Connect jako dalším krokem.

### <a name="install-the-latest-azure-ad-connect-release"></a>Nainstalujte nejnovější verzi Azure AD Connect
Přeinstalace balíček Azure AD Connect vyřeší jakékoli problémy s konfigurací, které můžou mít vliv možnost buď připojení ke službám cloudu a práce s hesly ve vašem místním prostředí AD.
Doporučujeme, můžete tento krok provést až po pokusu o prvních dvou výše popsaných kroků.

 1. Stáhněte si nejnovější verzi Azure AD Connect [tady](active-directory-aadconnect.md#install-azure-ad-connect).
 2. Vzhledem k tomu, že jste již nainstalovali Azure AD Connect, bude potřebujete pouze provést místní upgrade aktualizovat instalaci Azure AD Connect na nejnovější verzi.
 3. Spuštění stažený balíčku a postupujte podle na obrazovce pokyny aktualizujte počítač Azure AD Connect.  Žádná další je nutné ruční Pokud jste přizpůsobili mimo pole synchronizace pravidla v takovém případě měli byste **před pokračováním upgrade a znovu nasadit ručně je po dokončení těchto obecnějším údajům**.

Tyto kroky znovu navázat spojení s naše Cloudová služba a vyřešit žádná přerušení, které může docházet.

Pokud instalaci nejnovější verze server Azure AD Connect váš problém nevyřeší, doporučujeme zkuste zakázání a opětovné povolení zpětným heslo jako poslední krok po instalaci nejnovější synchronizace QFE.

Pokud se váš problém nevyřeší, pak doporučujeme přečtěte si téma [Poradce při potížích s zpětným heslo](#troubleshoot-password-writeback) a [hesla Azure AD nejčastější dotazy týkající se správy](active-directory-passwords-faq.md) , abyste zjistili, pokud váš problém může popisované tam.


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Odkazy na heslo resetovat si přečtěte následující dokumentaci
Tady jsou odkazy na všech stránkách resetování hesla Azure AD si přečtěte následující dokumentaci:

* **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).
* [**Jak to funguje**](active-directory-passwords-how-it-works.md) – Další informace o šest jednotlivých součástí tuto službu a co každý znamená
* [**Začínáme**](active-directory-passwords-getting-started.md) – zjistěte, jak vám umožní uživatelům obnovit a změnit jejich cloudu a místní hesla
* [**Vlastní**](active-directory-passwords-customize.md) – zjistěte, jak přizpůsobit vzhled a chování a nastavení služby potřebám vaší organizace
* [**Doporučené postupy**](active-directory-passwords-best-practices.md) : Naučte se nasadit rychlé a efektivní práce s hesly ve vaší organizaci
* [**Dozvědět se víc**](active-directory-passwords-get-insights.md) – Další informace o naše integrovaném možnosti vytváření sestav
* [**Nejčastější dotazy týkající se**](active-directory-passwords-faq.md) -odpovědi na časté otázky
* [**Další informace**](active-directory-passwords-learn-more.md) – přejděte důkladné do technické podrobnosti Princip služby



[001]: ./media/active-directory-passwords-troubleshoot/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-troubleshoot/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-troubleshoot/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-troubleshoot/004.jpg "Image_004.jpg"
