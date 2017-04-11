<properties
    pageTitle="Migrace z mobilních služeb do mobilní aplikace pro aplikaci služby"
    description="Zjistěte, jak snadno migrovat služby mobilní aplikace pro mobilní aplikaci služby aplikace"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="adrianha"/>

# <a name="article-top"></a>Migrovat stávající služba Azure mobilní aplikace služby Azure

S [všeobecně dostupná Azure aplikaci služby]Azure Mobilní služba webech lze snadno migrovat na místě a využívejte všechny funkce služby Azure aplikace.  Tento dokument vysvětluje, co můžete očekávat při migraci webu služby Azure mobilní aplikace služby Azure.

## <a name="what-does-migration-do"></a>Migrace k čemu slouží k vašemu webu

Migrace služby Azure Mobile změní služby mobilní aplikace pro [Aplikaci služby Azure] beze změny kód.  Vaše oznámení rozbočovače SQL datové připojení, nastavení ověřování, naplánovaných úloh a název domény zůstat beze změny.  Mobilní klienti pomocí služby Azure Mobile dál normálně.  Migrace restartuje služby, jakmile bude převedena na aplikaci služby Azure.

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Proč by měl migrace webu

Microsoft doporučuje, že nejdřív migrujete služby Azure Mobile umožní využít výhod Azure aplikaci služby včetně:

  *  Nové funkce host, včetně [WebJobs] a [vlastní názvy domén].
  *  Připojení k místních zdrojů pomocí [VNet] kromě [Hybridní připojení].
  *  Sledování a odstraňování potíží s novou Relic nebo [Přehledy aplikace].
  *  Předdefinované DevOps nástrojů, včetně [přípravu sloty]vrácení zpět a v výrobní testování.
  *  [Automatické měřítko]Vyrovnávání zatížení a [Sledování výkonu].

Další informace o výhodách aplikaci služby Azure naleznete v tématu [Mobilní porovnání aplikace služby] .

## <a name="before-you-begin"></a>Než začnete

Před zahájením hlavní práce na svém webu, byste měli [obecnějším údajům služby Mobile] skripty a databáze SQL.

## <a name="migrating-site"></a>Migrace weby

Proces migrace migruje všem webům v rámci jedné oblasti Azure.

Migrace webu:

  1.  Přihlaste se k [Azure klasické portálu].
  2.  Vyberte službu, mobilní telefon v oblasti, kterou chcete migrovat.
  3.  Klikněte na tlačítko **migrace na aplikaci služby** .

    ![Tlačítko migrace][0]

  4.  Přečtěte si přenést do dialogového okna aplikace služby.
  5.  Do zobrazeného pole zadejte název služby Mobile.  Například pokud váš název domény je contoso.azure mobile.net, zadejte _contoso_ do příslušného pole.
  6.  Klikněte na tlačítko popisky.

Sledování stavu migrace v monitoru aktivity. Váš web je uvedený jako *migraci* na portálu klasické Azure.

  ![Monitor aktivity migrace][1]

Každý migrace může trvat ze 3 15 minut za mobilní služba migrací.  Během migrace zůstávají k dispozici webu.
Na konci procesu migrace se nerestartuje webu.  Během procesu restart může trvat několik sekund, než je k dispozici tento web.

## <a name="finalizing-migration"></a>Dokončení migrace

Plán testování webu z mobilního klienta na konci procesu migrace.  Zajistěte, aby že mohli provádět všechny běžné akce klienta bez změny mobilnímu klientovi.  

### <a name="update-app-service-tier"></a>Vyberte odpovídající aplikaci služby ceny osy

Máte větší flexibilitu při ceny po přechodu na aplikaci služby Azure.

  1.  Přihlaste se k [portálu Azure].
  2.  Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název služby migrované Mobile.
  3.  Nastavení zásuvné otevře ve výchozím nastavení.
  4.  V nabídce nastavení klikněte na možnost **Plán služeb aplikací** .
  5.  Klikněte na dlaždici **Ceny osy** .
  6.  Klikněte na odpovídající vašim požadavkům na dlaždici a potom klikněte na **Vybrat**.  Budete muset klikněte na **Zobrazit vše** k dispozici cena za úrovně.

Jako výchozí bod doporučujeme následující úrovně:

| Mobilní služba ceny osy | Vrstvy ceny aplikace služeb |
| :-------------------------- | :----------------------- |
| Uvolnit                        | Bezplatné F1                  |
| Základní                       | Základní B1                 |
| Standardní                    | Standardní S1              |

Výběr správné ceny osy aplikace je značné míry.  Podívejte se do [Aplikace služby ceny] podrobnosti o cenách nové aplikace služby.

> [AZURE.TIP]Aplikace služby standardní osy obsahuje přístup k řadu funkcí, které chcete použít, včetně [přípravu sloty], automatické zálohování a automatické měřítko.  Podívejte se na nové funkce v režimu tam!

### <a name="review-migration-scheduler-jobs"></a>Revize migrované Plánovač úloh

Plánovač úloh nezobrazí do přibližně 30 minut po migraci.  Naplánovaných úloh dál běží na pozadí.
Chcete-li zobrazit naplánovaných úloh po jsou viditelné znovu:

  1.  Přihlaste se k [portálu Azure].
  2.  Vyberte **Procházet >**, zadejte **Kalendář** v rozevíracím seznamu _Filtr_ a pak vyberte **Plánovač kolekcí**.

Existuje omezený počet bezplatné Plánovač úloh dostupné dělají po migraci.  Prohlédněte si svůj použití a [Azure Plánovač plány jednotného zasílání zpráv].

### <a name="configure-cors"></a>Konfigurace CORS v případě potřeby

Sdílení zdrojů mezi origin je způsob okno umožňující povolit webu pro přístup k rozhraní API webových na jinou doménu.  Pokud používáte mobilní služby Azure s přidruženým webem, budete muset konfigurace CORS jako součást migrace.  Pokud pracujete s mobilní služby Azure výhradně z mobilních zařízení, potom CORS nemusí být nakonfigurované kromě v některých případech.

Nastavení migrované CORS jsou k dispozici jako **MS_CrossDomainWhitelist** nastavení aplikace.  Migrace webu do zařízení aplikaci služby CORS:

  1.  Přihlaste se k [portálu Azure].
  2.  Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název služby migrované Mobile.
  3.  Nastavení zásuvné otevře ve výchozím nastavení.
  4.  Klikněte na **CORS** v nabídce rozhraní API.
  5.  Zadání všech typů povolené do příslušného pole, stisknutím klávesy Enter po každém z nich.
  6.  Po správnost seznamu povolených typů, klikněte na tlačítko Uložit.

> [AZURE.TIP]Jednou z výhod používání služby Azure aplikace je na stejném místě můžete spustit vašeho webu a mobilních služeb.  Další informace naleznete v části [Další kroky](#next-steps) .

### <a name="download-publish-profile"></a>Stáhněte si nový profil publikování

Profil publikování webu se změní při migrace na aplikaci služby Azure.  Pokud chcete publikovat svůj web aplikace Visual Studio, musíte nový profil publikování.  Pokud si chcete stáhnout nový profil publikování:

  1.  Přihlaste se k [portálu Azure].
  2.  Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název služby migrované Mobile.
  3.  Klikněte na tlačítko **získat Publikovat profil**.

Stažení souboru PublishSettings s vaším počítačem.  Za normálních okolností je označená jako _název webu_. PublishSettings.  Importujte nastavení publikování do existujícího projektu:

  1.  Otevřete Visual Studia a služba Azure Mobile projektu.
  2.  Klikněte pravým tlačítkem v **Okně Průzkumník** projektu a vyberte **publikovat...**
  3.  Klikněte na **Import**
  4.  Klikněte na **Procházet** a vyberte svůj stažený soubor nastavení publikovat.  Klikněte na tlačítko **OK**
  5.  Klikněte na **Ověřit připojení** k zajištění práce nastavení publikovat.
  6.  Klepnutí na tlačítko **Publikovat** svůj web.


## <a name="working-with-your-site"></a>Práce s po migraci webu

Začít s ním pracovat nové aplikace služby [Azure portál] po migraci.  Následuje několik poznámek na konkrétní operacích, které jste použili k provedení na [Klasické portál Azure], spolu s jejich ekvivalent aplikaci služby.

### <a name="publishing-your-site"></a>Stažení a migrované webu publikování

Váš web je k dispozici prostřednictvím libovolná nebo ftp a můžete znova publikovat s různými různých mechanismy, včetně WebDeploy TFS, Mercurial, GitHub a FTP.  Nasazení přihlašovací údaje uživatele se migrují s ostatními členy webu.  Pokud nenastavil přihlašovacích údajů nasazení nebo si je nepamatujete, můžete je obnovit:

  1. Přihlaste se k [portálu Azure].
  2. Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název služby migrované Mobile.
  3. Nastavení zásuvné otevře ve výchozím nastavení.
  4. Klikněte na **Nasazení pověření** publikování nabídky.
  5. Zadejte nové přihlašovací údaje nasazení do příslušných polí a potom klikněte na tlačítko Uložit.

Pomocí těchto přihlašovacích údajů klonovat webu s libovolná nebo nastavit automatické nasazení z GitHub, TFS nebo Mercurial.  Další informace najdete v článku [si přečtěte následující dokumentaci nasazení služby Azure aplikace].

### <a name="appsettings"></a>Nastavení aplikace

Většina nastavení pro migrované Mobilní služba je k dispozici prostřednictvím nastavení aplikace.  Seznam nastavení aplikace můžete získat z [Azure portálu].
Zobrazení nebo změna nastavení aplikace:

  1. Přihlaste se k [portálu Azure].
  2. Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název služby migrované Mobile.
  3. Nastavení zásuvné otevře ve výchozím nastavení.
  4. V hlavní nabídce na příkaz **Nastavení aplikace** .
  5. Přejděte do oddílu nastavení aplikace a najděte aplikaci nastavení.
  6. Klikněte na požadovanou hodnotu nastavení aplikace pro úpravu hodnoty.  Klepněte na tlačítko **Uložit** uložte hodnotu.

Aktualizujte více nastavení aplikace ve stejnou dobu.

> [AZURE.TIP]Existují dvě nastavení aplikace stejné hodnoty.  Například se může zobrazit _ApplicationKey_ a _MS\_ApplicationKey_.  Aktualizujte svá nastavení obou aplikace ve stejnou dobu.

### <a name="authentication"></a>Ověřování

Všechna nastavení ověřování jsou k dispozici jako nastavení aplikace v migrované webu.  Aktualizovat nastavení ověřování, musí změnit nastavení příslušné aplikaci.  Následující tabulka zobrazuje nastavení příslušné aplikace pro vašeho poskytovatele ověření:

| Zprostředkovatel          | ID klienta                 | Tajná klienta                | Další nastavení             |
| :---------------- | :------------------------ | :--------------------------- | :------------------------- |
| Účet Microsoft | **MS\_MicrosoftClientID**  | **MS\_MicrosoftClientSecret** | **MS\_MicrosoftPackageSID** |
| Facebooku          | **MS\_FacebookAppID**      | **MS\_FacebookAppSecret**     |                            |
| Twitter           | **MS\_TwitterConsumerKey** | **MS\_TwitterConsumerSecret** |                            |
| Google            | **MS\_GoogleClientID**     | **MS\_GoogleClientSecret**    |                            |
| Azure AD          | **MS\_AadClientID**        |                              | **MS\_AadTenants**          |

Poznámka: **MS\_AadTenants** uložená jako seznam oddělený čárkami domén klienta (pole "Povolené klienti" na portálu mobilní služby).

> [AZURE.WARNING] **Nepoužívejte metody ověřování v nabídce nastavení**
>
> Azure aplikaci služby poskytuje samostatný "bez použití kódu" a tak mohli ověřovat systém v části _ověřování / se tak mohli ověřovat_ nabídka nastavení a možnost _Mobile ověřování_ (nepoužívají se) v nabídce nastavení.  Tyto možnosti jsou kompatibilní s migrované služby Azure Mobile.  Můžete využít ověřování aplikace služby Azure [upgrade webu](app-service-mobile-net-upgrading-from-mobile-services.md) .

### <a name="easytables"></a>Data

Na kartě _Data_ v mobilních služeb byla nahrazena _Snadno tabulek_ v Azure portálu.  Přístup k snadno tabulky:

  1. Přihlaste se k [portálu Azure].
  2. Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název služby migrované Mobile.
  3. Nastavení zásuvné otevře ve výchozím nastavení.
  4. Klikněte na **snadno tabulky** v nabídce mobilní.

Můžete přidat tabulky kliknutím na tlačítko **Přidat** nebo po kliknutí na název tabulky přístup k existující tabulky.  Existují různé operace můžete udělat z této zásuvné, včetně:

* Změna oprávnění tabulky
* Úpravy provozní skriptů
* Správa schématu tabulky
* Odstranění tabulky
* Vymazání obsahu tabulky
* Odstranění určité řádky tabulky

### <a name="easyapis"></a>ROZHRANÍ API

Na kartě _rozhraní API_ v mobilních služeb byla nahrazena _Snadno rozhraní API_ v Azure portálu.  Přístup k rozhraní API snadno:

  1. Přihlaste se k [portálu Azure].
  2. Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název služby migrované Mobile.
  3. Nastavení zásuvné otevře ve výchozím nastavení.
  4. Klikněte na **Snadno rozhraní API** v nabídce mobilní.

Migrované rozhraní API jsou už uvedené v zásuvné.  Můžete také přidat rozhraní API z této zásuvné.  Ke správě konkrétní rozhraní API, klikněte na rozhraní API.
Z nového zásuvné můžete upravit oprávnění a upravovat skripty pro rozhraní API.

### <a name="on-demand-jobs"></a>Plánovač úloh

Všechny Plánovač úloh jsou k dispozici prostřednictvím části kolekce Plánovač úloh.  Přístup k Plánovač úloh:

  1. Přihlaste se k [portálu Azure].
  2. Vyberte **Procházet >**, zadejte **Kalendář** v rozevíracím seznamu _Filtr_ a pak vyberte **Plánovač kolekcí**.
  3. Vyberte kolekci projektu pro web.  Je název _název webu_– úlohy.
  4. Klikněte na **Nastavení**.
  5. V části SPRAVOVAT klikněte na tlačítko **Plánovač úloh** .

Naplánovaných úloh jsou uvedeny četnost, které jste zadali před migrací.  Na vyžádání úlohy jsou zakázány.  Spuštění úlohy na vyžádání:

  1. Vyberte úlohu, kterou chcete spustit.
  2. V případě potřeby zaškrtněte políčko **Povolit** povolte úlohu.
  3. Klikněte na **Nastavení**a pak **plán**.
  4. Vyberte opakování **jednou**a potom klikněte na tlačítko **Uložit**

Úlohy na vyžádání se nacházejí v `App_Data/config/scripts/scheduler post-migration`.  Doporučujeme převedení všech projektů na vyžádání [WebJobs] nebo [funkce].  Vytvářet nové Plánovač úloh jako [WebJobs] nebo [funkce].

### <a name="notification-hubs"></a>Oznámení rozbočovače

Přihlašovacích údajů mobilní používá oznámení rozbočovače nabízených oznámení.  Následující nastavení aplikace jsou používaný k propojení centru oznámení služby Mobile po migraci:

| Nastavení aplikace                    | Popis                              |
| :------------------------------------- | :--------------------------------------- |
| **MS\_PushEntityNamespace**             | Centrální Namespace oznámení           |
| **MS\_NotificationHubName**             | Název centrální oznámení                |
| **MS\_NotificationHubConnectionString** | Oznámení o centrální připojovacího řetězce   |
| **MS\_NamespaceName**                   | Alias pro MS_PushEntityNamespace      |

Vaše centrum oznámení se spravuje přes [Azure portálu].  Zapište si název centrální oznámení (můžete najít to nastavením aplikace):

  1. Přihlaste se k [portálu Azure].
  2. Vyberte **Procházet**>, vyberte **Rozbočovače upozornění**
  3. Klikněte na název upozornění centrální přidružené mobilních služeb.

> [AZURE.NOTE]Pokud vaše oznámení Centrum typu "Kombinovaná", se nezobrazuje.  "Smíšených" Zadejte oznámení, že rozbočovače využít rozbočovače oznámení a starší funkce Bus služby.  Před pokračováním [Převést smíšených obory názvů] .  Po dokončení převodu se zobrazí v [Azure portál]rozbočovače oznámení.

Další informace najdete v dokumentaci [Rozbočovače oznámení] .

> [AZURE.TIP]Funkce správy rozbočovače upozornění na [Azure portál] se stále nacházejí v náhledu.  [Klasický portál Azure] zůstávají k dispozici pro správu všechny rozbočovače upozornění.

### <a name="legacy-push"></a>Nastavení nabízených starší verze

Pokud jste nakonfigurovali nabízených na vaše mobilní služba před uvedením na rozbočovače oznámení, používáte _starší verze nabízená_.  Pokud používáte nabízených a rozbočovači oznámení uvedené v konfiguraci nevidíte, je pravděpodobné, že používáte _starší připínáčku_.  Tato funkce se migrují s všech dalších funkcí.  Ale doporučujeme upgradovat oznámení rozbočovače brzy po dokončení migrace.

Během provádění změn nastavení starší nabízených (s výjimkou důležitá certifikát APNS) jsou dostupné v nastavení aplikace.  Aktualizujte certifikát APNS nahrazením příslušný soubor v systému souborů.

### <a name="app-settings"></a>Nastavení jiné aplikace

Následující nastavení další aplikace jsou migrované ze služby Mobile a k dispozici v části *Nastavení* > *Nastavení aplikace*:

| Nastavení aplikace              | Popis                             |
| :------------------------------- | :-------------------------------------- |
| **MS\_MobileServiceName**         | Název aplikace                    |
| **MS\_MobileServiceDomainSuffix** | Předpona domény. tj Azure mobile.net |
| **MS\_ApplicationKey**            | Klávesa aplikace                    |
| **MS\_hlavního klíče.**                 | Hlavní klíč aplikace                     |

Klávesu application a hlavní klíč shodná s klíči aplikace z původního mobilní služby.  Klávesu Application se odesílá zejména mobilních klientů ověřuje jejich použití mobilního rozhraní API.

### <a name="cliequivalents"></a>Příkazového řádku ekvivalenty

Dále můžete příkazu _azure mobilní_ ke správě webu služby Azure Mobile.  Místo toho mnoho funkcí nahradily příkazu _azure webu_ .  Ekvivalenty běžnými příkazy pomocí následující tabulky:

| _Azure Mobile_ Příkaz                     | Příkaz odpovídající _Server Azure_            |
| :----------------------------------------- | :----------------------------------------- |
| mobilní umístění                           | ze seznamu webů                         |
| mobilní seznamu                                | seznam webů                                  |
| mobilní zobrazit _název_                         | Zobrazit webu _název_                           |
| mobilní restart _název_                      | restartování webu _název_                        |
| mobilní opětovném nasazení _název_                     | Web nasazení opětovném nasazení _commitId_ _název_ |
| mobilní klíč nastavit _název_ _Typ_ _hodnoty_       | Odstranit _klíče_ webu appsetting _název_ <br/> appsetting webu přidejte _klíč_=_hodnoty_ _název_ |
| konfigurace mobilního seznam _název_                  | seznam appsetting webu _název_                |
| konfigurace mobilního získat _název_ _klíč_             | Web appsetting zobrazit _klíč_ _název_          |
| mobilní konfigurace nastavení _názvu_ _klíč_             | Odstranit _klíče_ webu appsetting _název_ <br/> appsetting webu přidejte _klíč_=_hodnoty_ _název_ |
| seznam mobilní domény _název_                  | seznam domény webu _název_                    |
| mobilní domény přidejte _název_ _domény_          | Přidání _domény_ _název_ domény webu            |
| Odstranit mobilní domény _název_                | Odstranit _domény_ webů domény _název_         |
| mobilní měřítko zobrazit _název_                   | Zobrazit webu _název_                           |
| mobilní měřítko změnit _název_                 | _režim_ režimu měřítko webu _název_ <br /> Web měřítko instance _instance_ _název_ |
| mobilní appsetting seznam _název_              | seznam appsetting webu _název_                |
| mobilní appsetting přidat _název_ _klíč_ _hodnota_ | appsetting webu přidejte _klíč_=_hodnoty_ _název_   |
| mobilní appsetting odstranit _název_ _klíč_      | Odstranit _klíče_ webu appsetting _název_        |
| mobilní appsetting zobrazit _název_ _klíč_        | Odstranit _klíče_ webu appsetting _název_        |

Aktualizovat nastavení nabízených oznámení nebo ověřování aktualizací odpovídající nastavení aplikace.
Úpravy souborů a publikování webu prostřednictvím protokolu ftp nebo libovolná.

### <a name="diagnostics"></a>Protokolování diagnostiky a

Protokolování diagnostiky zakázaná obvykle v aplikaci služby Azure.  Protokolování diagnostiky povolte:

  1. Přihlaste se k [portálu Azure].
  2. Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název služby migrované Mobile.
  3. Nastavení zásuvné otevře ve výchozím nastavení.
  4. Vyberte **Protokoly pro diagnostiku** v nabídce funkce.
  5. Klikněte **na** následující protokolů: **Aplikace protokolování (systém souborů)**, **podrobných chybových zpráv**a **Sledování žádosti o se nezdařila**
  6. Klikněte na **Systém souborů** pro protokolování webového serveru
  7. Klikněte na tlačítko **Uložit**

Zobrazení protokolů:

  1. Přihlaste se k [portálu Azure].
  2. Vyberte **všechny zdroje** nebo **Služby aplikace** klikněte na název služby migrované Mobile.
  3. Klikněte na tlačítko **Nástroje**
  4. V nabídce dodržovat vyberte **protokolu proudu** .

Protokoly se zobrazují v okně, jak se vytvářejí.  Můžete také stáhnout protokolech na pozdější analýzy pomocí svých přihlašovacích údajů nasazení. Další informace najdete v dokumentaci [protokolování] .

## <a name="known-issues"></a>Známé problémy

### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Odstranění klonovat mobilní aplikace poštovním způsobí, že výpadku webu

Pokud klonovat služby migrované mobilní pomocí prostředí PowerShell Azure a pak odstranit klonovat, položky DNS pro služby Výroba zmizí.  Váš web je již byly přístupné z Internetu.  

Řešení: Pokud chcete vytvořit kopii webu, přihlaste se pomocí portálu.

### <a name="changing-webconfig-does-not-work"></a>Změna Web.config nefunguje

Pokud máte web ASP.NET, se změní na `Web.config` soubor získat nelze použít.  Aplikace služby Azure vytvoří vhodnou `Web.config` soubor během spouštění na podporu modul runtime služby Mobile.  Přepsat některá nastavení (například vlastní záhlaví) pomocí souboru XML transformace.  Vytvoření souboru v s názvem `applicationHost.xdt` -tento soubor musí končit `D:\home\site` adresářové služby Azure.  Nahrát `applicationHost.xdt` souboru prostřednictvím skriptu vlastního nasazení nebo přímo pomocí Kudu.  Na následujícím obrázku je příklad dokumentu:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Další informace najdete v dokumentaci [XDT transformace vzorky] na GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Správci přenosy nelze přidat migrované mobilní služby

Při vytváření profil správce přenosy nemůžete si vybrat přímo migrované mobilních služeb do profilu.  Použití "externí koncový bod."  Externí koncový bod lze přidat pouze prostřednictvím Powershellu.  Další informace najdete v tématu [kurz přenosy správce](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Další kroky

Teď, když aplikace přenesení službě aplikace, existují i další funkce, které můžete použít:

  * Nasazení [přípravu sloty] umožňují fáze změny na svůj web a provádět A a B testování.
  * [WebJobs] poskytují náhrady samolepicích práce naplánovaná na vyžádání.
  * Můžete [nepřetržitě nasazení] webu propojením webu GitHub, TFS nebo Mercurial.
  * [Aplikace přehledy] slouží ke sledování webu.
  * Slouží k webu a rozhraní API Mobile z téhož kódu.

### <a name="upgrading-your-site"></a>Upgrade webu Mobile služby Azure mobilní aplikace SDK

  * Pro projekty na základě Node.js serveru nové [Mobilní aplikace Node.js SDK] obsahuje několik nových funkcí. Například můžete teď proveďte místní vývoje a ladění, použít libovolná verze Node.js nad 0,10 a si můžete přidat jakékoli Express.js middleware.

  * Pro. Na základě čistého server projekty nové [mobilní aplikace SDK NuGet balíčků](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) mít větší flexibilitu NuGet závislosti.  Tyto balíčky podporuje nové ověřování aplikace služby a vytvořte pomocí libovolné projektu ASP.NET. Další informace o upgradu, najdete v článku [upgradovat svůj stávající .NET Mobile Service aplikace služby](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[Aplikace služby ceny]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Přehledy aplikace]: ../application-insights/app-insights-overview.md
[Automatické měřítko]: ../app-service-web/web-sites-scale.md
[Azure aplikace služby]: ../app-service/app-service-value-prop-what-is.md
[Azure dokumentace k nasazení aplikace služby]: ../app-service-web/web-sites-deploy.md
[Azure klasické portálu]: https://manage.windowsazure.com
[Azure portálu]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Plány Azure Plánovač]: ../scheduler/scheduler-plans-billing.md
[neustále nasazení]: ../app-service-web/app-service-continuous-deployment.md
[Převedení smíšených obory názvů]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[vlastní názvy domén]: ../app-service-web/web-sites-custom-domain-name.md
[Fiddler]: http://www.telerik.com/fiddler
[všeobecně dostupná aplikace služby Azure]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybridní připojení]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[Zapnout protokolování]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Mobilní aplikace Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[Mobilní porovnání aplikace služby]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Oznámení rozbočovače]: ../notification-hubs/notification-hubs-push-notification-overview.md
[sledování výkonu]: ../app-service-web/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[Obecnějším údajům Mobile Service]: ../mobile-services/mobile-services-disaster-recovery.md
[přípravná sloty]: ../app-service-web/web-sites-staged-publishing.md
[VNet]: ../app-service-web/web-sites-integrate-with-vnet.md
[WebJobs]: ../app-service-web/websites-webjobs-resources.md
[XDT transformace vzorky]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Funkce]: ../azure-functions/functions-overview.md
