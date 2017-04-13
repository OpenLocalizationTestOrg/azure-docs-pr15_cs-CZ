<properties
    pageTitle="Azure mobilní zapojení webového rozhraní API SDK | Microsoft Azure"
    description="Nejnovější aktualizace a postupy pro Web SDK pro zapojení Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Použití rozhraní API Azure Mobile zapojení ve webové aplikaci

Tento dokument je doplněk dokumentů, která sděluje, jak [integrovat zapojení Mobile ve webové aplikaci](mobile-engagement-web-integrate-engagement.md). Poskytuje podrobné informace o používání rozhraní API zapojení Mobile Azure vykazování statistiky aplikace.

Rozhraní API zapojení Mobile poskytovanou `engagement.agent` objektu. Výchozí Azure Mobile zapojení Web SDK alias je `engagement`. Můžete upravit tento alias z konfigurace SDK.

## <a name="mobile-engagement-concepts"></a>Zapojení Mobile konceptů

Následující části upřesní běžných [Mobile zapojení koncepty](mobile-engagement-concepts.md) pro platformu web.

### <a name="session-and-activity"></a>`Session`a`Activity`

Pokud uživatel zůstává nedělá na několik sekund mezi dvěma aktivity, jeho posloupnost aktivity rozdělit na dvě různých relace. Tyto několik sekund, než se označují jako časový limit relace.

Pokud webová aplikace není deklarovat konec aktivit uživatelů samostatně (tak, že zavoláte `engagement.agent.endActivity` funkci), serveru Mobile zapojení automaticky vyprší relace uživatele do tří minut po zavření stránky aplikace. Je místo toho možnost vypršení časového limitu serveru relace.

### `Crash`

Automatické sestavy nezachycené výjimky JavaScript nejsou vytvářeny ve výchozím nastavení. Však můžete nahlásit havaruje ručně pomocí `sendCrash` fungovat (viz oddíl pro generování sestav havaruje).

## <a name="reporting-activities"></a>Vytváření sestav aktivit

Vykazování činnosti uživatelů obsahuje když uživatel spustí nového aktivitu a když uživatel skončí platnost aktuální činnosti.

### <a name="user-starts-a-new-activity"></a>Uživatel spustí nového aktivitu

    engagement.agent.startActivity("MyUserActivity");

Budete muset volat `startActivity()` každý činnosti uživatelů čas změny. První hovor na tuto funkci spustí novou relaci uživatele.

### <a name="user-ends-the-current-activity"></a>Uživatel ukončí aktuální činnosti.

    engagement.agent.endActivity();

Budete muset volat `endActivity()` aspoň jednou po dokončení uživatel poslední aktivity. To informuje o tom, SDK Web zapojení Mobile, že uživatel aktuálně nedělá ani, že relaci uživatele musí být uzavřen až vyprší časový limit relace. Pokud jste volali `startActivity()` před vypršením platnosti časový limit relace, je jednoduše obnovit relaci.

Protože neexistuje žádné spolehlivé hovor přijímáte při zavření okna Tabulka, je často obtížně srozumitelný nebo nesrozumitelný zachytit konec aktivit uživatelů ve webovém prostředí. Proto serveru Mobile zapojení automaticky vyprší relace uživatele do tří minut po zavření aplikace na stránce.

## <a name="reporting-events"></a>Vytváření sestav události

Oznámení o událostech zahrnuje relace a samostatnou události.

### <a name="session-events"></a>Události relace

Události relace obvykle používají vykazování akce provádí uživatele během této relace.

**Příklad bez dalších dat:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Příklad navíc daty:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Samostatný události

Na rozdíl od události relace můžete událostem samostatného mimo kontext umístění relaci.

Když použijete ``engagement.agent.sendEvent`` namísto ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Oznamování chyb

Oznámení o chybách zahrnuje chybám relace a samostatně.

### <a name="session-errors"></a>Relace chyby

Relace chyby obvykle slouží k chyby, které mají vliv na uživatele během této relace.

**Příklad bez dalších dat:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Příklad navíc daty:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Samostatné chyby

Na rozdíl od relace chyby může dojít k chybám samostatného mimo kontext umístění relaci.

Když použijete `engagement.agent.sendError` namísto `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Úlohy sestav

Vytváření sestav na úlohy zahrnuje vykazování chyby a událostí, ke kterým dochází při úlohy a vytváření sestav funkce dojde k chybě.

**Příklad:**

Pokud chcete sledovat žádost AJAX, použijete takto:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Oznamování chyb průběhu projektu

Chyby můžete týkající se spuštěná úloha místo pro aktuální relaci uživatele.

**Příklad:**

Pokud chcete zobrazovat chyby selže žádost AJAX:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Vytváření sestav události průběhu projektu

Události můžete souviset s spuštěná úloha místo pro aktuální relaci uživatele Poděkování `engagement.agent.sendJobEvent` (funkce).

Tato funkce funguje přesně jako `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Vytváření sestav dojde k chybě

Použití `sendCrash` funkce reportování dojde k chybě ručně.

`crashid` Argument je řetězec, který identifikuje typ dojde k chybě.
`crash` Argument je obvykle zásobníku zhroucení jako řetězec.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Další parametry

Libovolný dat můžete připojit k události, chyby, aktivity nebo projektu.

Data lze libovolný objekt JSON (ale ne matici nebo základní typ).

**Příklad:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Omezení

Limity, které se vztahují k další parametry jsou v oblastech regulární výrazy pro klíče, typy hodnot a velikost.

#### <a name="keys"></a>Klíče

Každý klíč v objektu musí odpovídat následující regulárních výrazů:

    ^[a-zA-Z][a-zA-Z_0-9]*

To znamená, že klíče musí začínat alespoň jedno písmeno, následovaný písmena, číslice a podtržítka (\_).

#### <a name="values"></a>Hodnoty

Hodnoty jsou omezené na řetězce, číslo a logické typy.

#### <a name="size"></a>Velikost

Doplňky se omezuje 1 024 znaků za hovoru (po SDK Mobile zapojení Web kódování ho JSON).

## <a name="reporting-application-information"></a>Informace o vytváření sestav aplikace

Můžete ručně nahlásit sledování informací (nebo všechny ostatní informace specifické pro aplikaci) pomocí `sendAppInfo()` (funkce).

Všimněte si, že tyto informace odesílat postupně. Jenom nejnovější hodnotu pro určité klávesy bude nacházet pro konkrétní zařízení.

Jako extra události můžete použít libovolný objekt JSON abstraktní informace o aplikaci. Všimněte si, že matice nebo podřízené objekty je považováno za ploché řetězce (pomocí serializaci JSON).

**Příklad:**

Tady je ukázka kódu pro odesílání gender uživatele a datum narození:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Omezení

Limity, které se vztahují k informací aplikace jsou v oblastech regulární výrazy pro klíče a velikost.

#### <a name="keys"></a>Klíče

Každý klíč v objektu musí odpovídat následující regulárních výrazů:

    ^[a-zA-Z][a-zA-Z_0-9]*

To znamená, že klíče musí začínat alespoň jedno písmeno, následovaný písmena, číslice a podtržítka (\_).

#### <a name="size"></a>Velikost

Informace o aplikaci se omezí na 1 024 znaků za hovoru (po SDK Mobile zapojení Web kódování ho JSON).

V tomto příkladu je JSON odeslaných na server 44 znaků:

    {"birthdate":"1983-12-07","gender":"female"}
