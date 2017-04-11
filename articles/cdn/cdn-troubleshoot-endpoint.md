<properties
    pageTitle="Poradce při potížích s Azure CDN koncové body vrácení 404 stav | Microsoft Azure"
    description="Poradce při potížích 404 kódy odpověď s Azure CDN koncové body."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Poradce při potížích s koncové body CDN návratu 404 stavy

Tento článek vám pomůže řešit problémy s [koncové body CDN](cdn-create-new-endpoint.md) vrácení chyby 404.

Pokud potřebujete další pomoc kdykoli v tomto článku můžete kontaktovat Azure odborníků na [webu MSDN Azure a fórech přetečení zásobníku](https://azure.microsoft.com/support/forums/). Můžete taky můžete poslat taky případ Azure podpory. Přejděte na [Web podpory Azure](https://azure.microsoft.com/support/options/) a klikněte na **Získat podporu**.

## <a name="symptom"></a>Příznaku

Jste vytvořili CDN profilu a koncový bod, ale obsahu vám přijde mít k dispozici na CDN.  Uživatelé, kteří se pokusí o přístup k obsahu pomocí adresy URL CDN dostávat stavů HTTP 404. 

## <a name="cause"></a>Příčina

Existuje několik možných příčin, včetně:

- Původ souboru nezobrazuje k CDN
- Koncový bod je špatně nakonfigurované, které jsou příčinou CDN hledání ve špatném místě
- Hostiteli odmítnutí záhlaví Host (hostitel) ze zprávy CDN
- Koncový bod neměl času se rozšíří v celém CDN

## <a name="troubleshooting-steps"></a>Návody na řešení potíží

> [AZURE.IMPORTANT] Po vytvoření koncového bodu CDN, nebude okamžitě k dispozici pro použití, jako vyžaduje čas pro registraci se rozšíří prostřednictvím CDN.  <b>Azure CDN z Akamai</b> profilů provede šíření obvykle do jedné minuty.  <b>Azure CDN z Verizon</b> profilů šíření obvykle dokončí do 90 minut, ale v některých případech může trvat déle.  Pokud postupujte podle pokynů v tomto dokumentu a stále se vám zobrazuje 404 odpovědi, můžete počkat několik hodin ke kontrole znovu před otevřením požadavek podpory můžete.

### <a name="check-the-origin-file"></a>Kontrola původ souboru

Nejdřív jsme ověřte požadovaný soubor chceme režim cached je k dispozici v naší origin a veřejně přístupný.  Nejrychlejším způsobem, jak to udělat, je v v soukromé nebo okno relace otevřete prohlížeč a přejděte na soubor.  Stačí zadejte nebo vložte adresu URL do pole Adresa a zobrazit, pokud bude výsledkem soubor, který očekáváte.  V tomto příkladu budu používat souboru v úložišti Azure účtu přístupných osobám s postižením na mám `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Jak vidíte, předá úspěšně test.

![Úspěch!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Když to je nejrychlejší a nejsnadnější způsob ověření, že je soubor veřejně dostupný, některé síťové konfigurace ve vaší organizaci může dát dojem, že tento soubor je veřejně dostupné, pokud je ve skutečnosti viditelné pouze pro uživatele síti (i když je hostovaný v Azure).  Pokud máte externí prohlížeč odkud můžete otestovat, jako je mobilní zařízení, které není připojený k síti vaší organizace nebo virtuálního počítače v Azure, který by nejlepší.

### <a name="check-the-origin-settings"></a>Zkontrolujte nastavení origin

Teď můžeme ověření, že soubor je veřejně k dispozici na Internetu, můžeme zkontrolujte nastavení našeho origin.  Na [Portálu Azure](https://portal.azure.com)přejděte do vlastního profilu CDN a klikněte na koncový bod, který jste Poradce při potížích.  Ve výsledném zásuvné **koncový bod** klikněte na počátek.  

![Koncový bod zásuvné se zvýrazněným origin](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Zobrazí se zásuvné **Origin** . 

![Origin zásuvné](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Typ Origin a název hostitele

Ověřte správnost **Origin typ** a ověření **Origin hostname**.  V tomto příkladu `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, je hostname část adresy URL `cdndocdemo.blob.core.windows.net`.  Jak vidíte snímek, je to správná.  Azure úložiště v prohlížeči a typů cloudové služby pole **Origin hostname** je rozevírací seznam, jsme nemuseli starat o to pravopis správně.  Pokud používáte vlastní origin, je však *Opravdu kritické* , které vaše hostname správně napsaný!

#### <a name="http-and-https-ports"></a>Porty protokolu HTTP a HTTPS

Dalším krokem, přečtěte si možná je **HTTP** a **porty HTTPS**.  Ve většině případů 80 a 443, jsou správné a budete potřebovat žádné změny.  Ale pokud je origin server přijímá na jiný port, muset tady reprezentovat.  Pokud si nejste jistí, podívejte se jen na adrese URL původ souboru.  Specifikace HTTP a HTTPS zadejte porty 80 a 443 jako výchozí. V poli Moje adresa URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, není zadán port, ať se dosadí výchozí 443 a nastavení je správné.  

Však vyslovte adresu URL pro váš origin soubor, který dříve testováno `http://www.contoso.com:8080/file.txt`.  Poznámka `:8080` na konci segmentu název hostitele.  Řeknete tak prohlížeče používat port `8080` připojení k webovému serveru v `www.contoso.com`, takže budete muset zadat 8080 v poli **HTTP port** .  Je důležité mít na paměti, že nastavení portů pouze vliv na jaké port koncový bod používá k načtení informací z původ.

> [AZURE.NOTE] Koncové body **Azure CDN z Akamai** nepovolují široká TCP port pro typů.  Seznam origin porty, které nejsou povolené najdete v článku [Azure CDN z Akamai povolené Origin porty](https://msdn.microsoft.com/library/mt757337.aspx).  
  
### <a name="check-the-endpoint-settings"></a>Zkontrolujte nastavení koncového bodu

Zpět na zásuvné **koncový bod** klikněte na tlačítko **Konfigurovat** .

![Koncový bod zásuvné se zvýrazněným tlačítkem konfigurovat](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Zobrazí se na koncový bod **Konfigurovat** zásuvné.

![Konfigurace zásuvné](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokoly

U **protokolů**zkontrolujte, že je vybrán protokol použit klienty.  Stejný protokol používá klient bude používat pro přístup k původ, takže je důležité, abyste měli ty origin správně nakonfigurované v předchozí části.  Koncový bod přijímá jenom na výchozí HTTP a HTTPS porty 80 a 443, bez ohledu na to porty origin.

Chci vrátit naše hypotetické příklad s `http://www.contoso.com:8080/file.txt`.  Jak budete pamatovat, Contoso zadané `8080` jako jejich HTTP portu, ale taky Předpokládejme jsou určeny `44300` jako jejich port HTTPS.  Pokud vytvořeno koncový bod s názvem `contoso`, bude jejich hostname koncový bod CDN `contoso.azureedge.net`.  Žádost o `http://contoso.azureedge.net/file.txt` je žádost HTTP tak koncový bod byste použili, HTTP na portu 8080 načíst z původ.  Žádost o zabezpečené přes protokol HTTPS, `https://contoso.azureedge.net/file.txt`, způsobí koncového bodu na port 44300 používat HTTPS při retriving souboru z původ.

#### <a name="origin-host-header"></a>Záhlaví Origin Host (hostitel)

**Záhlaví hostitele Origin** hodnotu host záhlaví poslané na počátek při každém požadavku.  Ve většině případů třeba stejný jako **Origin hostname** jsme dříve ověření.  Nesprávný hodnotu do pole nezpůsobí obecně 404 stavy, ale může způsobit ostatní 4xx stavy, podle toho, co původ očekává.

#### <a name="origin-path"></a>Origin cesta

Nakonec jsme ověřte naše **cestu Origin**.  Ve výchozím nastavení je toto pole prázdné.  Toto pole by měl použít jenom Pokud chcete zúžit obor hostovaná origin prostředky, které mají být k dispozici na CDN.  

Například v mém koncový bod můžu chtěli všechny zdroje v mém účtu úložiště, aby byl k dispozici, tak jsem prázdné **Origin cestu** .  To znamená, že žádost o `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` výsledkem spojení z mé koncový bod k `cdndocdemo.core.windows.net` , který požaduje `/publicblob/lorem.txt`.  Podobně žádost o `https://cdndocdemo.azureedge.net/donotcache/status.png` výsledkem koncový bod žádosti o `/donotcache/status.png` z původ.

Ale co dělat, když nechcete používat CDN pro každé umístění na můj origin?  Vyslovte I chtěli vystavit `publicblob` cestu.  Mám zadat */publicblob* v poli Moje **Origin cesty** , který způsobí koncový bod vložení */publicblob* před každý požadavek odkazy na počátek.  To znamená, že žádosti o `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` nyní skutečně potrvá žádost o část adresy URL, `/publicblob/lorem.txt`a připojte `/publicblob` na začátek. Výsledkem je žádost o `/publicblob/publicblob/lorem.txt` z původ.  Pokud tato cesta není překládal skutečný soubor, vrátí hodnotu původ 404 stav.  Správnou adresu URL k načtení lorem.txt v tomto příkladu by být ve skutečnosti `https://cdndocdemo.azureedge.net/lorem.txt`.  Poznámku, jsme Nezahrnovat cestu */publicblob* vůbec, protože je žádost o část adresy URL `/lorem.txt` a přidá koncový bod `/publicblob`, následek `/publicblob/lorem.txt` žádost předáním původ.
