<properties
    pageTitle="Používání rozhraní API zapojení na iOS"
    description="Nejnovější iOS SDK - používání rozhraní API zapojení na iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>Používání rozhraní API zapojení na iOS

Tento dokument je doplněk dokument jak integrovat zapojení IOS: poskytuje název hloubkové podrobnosti o tom, jak pomocí rozhraní API zapojení vykazování statistiky aplikace.

Mějte na paměti, že pokud chcete jenom zapojení sestav aplikace relace, aktivity, dojde k chybě a technické informace, pak nejjednodušší způsob je aby všechny vaše vlastní `UIViewController` objekty dědí od odpovídající `EngagementViewController` předmětu.

Pokud chcete udělat víc, třeba když potřebujete sestav aplikace zvláštní události, chyby a úlohy, nebo pokud máte zprávu aplikace aktivity jiným způsobem než té implementovaná v `EngagementViewController` třídy, a pak je potřeba použít rozhraní API Engagement.

Rozhraní API zapojení poskytovanou `EngagementAgent` předmětu. Instance tohoto třídy lze získat tak, že zavoláte `[EngagementAgent shared]` statické metody (Všimněte si, že `EngagementAgent` vrácené se jedná jednoznačné).

Před žádná rozhraní API volání `EngagementAgent` objektu, musí být spuštěn tak, že zavoláte metodu`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Zapojení koncepty

Následující části upřesní běžných [Mobile zapojení koncepty](mobile-engagement-concepts.md) pro platformu iOS.

### <a name="session-and-activity"></a>`Session`a`Activity`

*Aktivity* obvykle souvisí s jednu obrazovku aplikací, to znamená *aktivity* , spustí se na obrazovce se zobrazí a zarážky, když máte zavřený obrazovky: to je případ, kdy SDK zapojení je integrována s použitím `EngagementViewController` třídy.

Ale *aktivity* také možné ovládat ručně pomocí rozhraní API Engagement. Díky rozdělit na dané monitoru v několik částí sub získat další informace o použití této obrazovce (například známé intervalu a jak dlouho se používají dialogová okna v rámci této obrazovce).

##<a name="reporting-activities"></a>Vytváření sestav aktivit

### <a name="user-starts-a-new-activity"></a>Uživatel spustí nového aktivitu

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Budete muset volat `startActivity()` každé změně činnosti uživatelů. První hovor na tuto funkci spustí novou relaci uživatele.

### <a name="user-ends-his-current-activity"></a>Uživatel ukončí své aktuální činnosti.

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] Můžete by **nikdy** tuto funkci volat sami, kromě případu, kdy chcete rozdělit jedno použití aplikace do několika relace: volání tuto funkci tak, že skončí aktuální relaci okamžitě, Ano, hovoru na pozdější `startActivity()` by spusťte novou relaci. Tato funkce nazývá SDK automaticky, když máte zavřený aplikace.

##<a name="reporting-events"></a>Vytváření sestav události

### <a name="session-events"></a>Události relace

Události relace se obvykle používají vykazování akce provádí uživatele během jeho relace.

**Příklad bez dalších dat:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Příklad navíc daty:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Samostatný události

Na rozdíl od události relace samostatného události lze mimo kontext relaci.

**Příklad:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Oznamování chyb

### <a name="session-errors"></a>Relace chyby

Chyby relace se obvykle používají vykazování chyby ovlivňující ochranu uživatel během jeho relace.

**Příklad:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Samostatné chyby

Na rozdíl od relace chyby samostatného chyby lze mimo kontext relaci.

**Příklad:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Úlohy sestav

**Příklad:**

Předpokládejme, že chcete nahlásit trvání proces přihlášení:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Chyby sestavy průběhu projektu

Chyby můžete týkající se spuštěná úloha místo související s aktuální relaci uživatele.

**Příklad:**

Předpokládejme, že chcete nahlásit chybu během procesu přihlášení:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Události průběhu projektu

Události můžete souviset s spuštěná úloha místo související s aktuální relaci uživatele.

**Příklad:**

Předpokládejme máme sociálních sítí a používáme úlohy sestavy celkový čas, kdy uživatel připojen k serveru. Uživatel může přijímat zprávy od jeho přátel, toto je úlohu událost.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Další parametry

Libovolný dat může být připojen události, chyby, aktivity a úlohy.

Tento dat může vypadat, použije jeho iOS NSDictionary předmětu.

Všimněte si, že může obsahovat extra `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` nebo jiné `NSDictionary` instance.

> [AZURE.NOTE] Další parametr serializován ve formátu JSON. Pokud chcete předat různým objektům než jsme je popsali výše, musíte ve svojí třídě implementovat následujícím způsobem:
>
             -(NSString*)JSONRepresentation;
>
> Metoda by měly vrátit formátovanými jako JSON objektu.

### <a name="example"></a>Příklad

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Omezení

#### <a name="keys"></a>Klíče

Každý klíč v `NSDictionary` musí odpovídat následující regulárních výrazů:

`^[a-zA-Z][a-zA-Z_0-9]*`

Znamená to, že klíče musí začínat alespoň jedno písmeno, následovaný písmena, číslice a podtržítka (\_).

#### <a name="size"></a>Velikost

Doplňky se omezuje na **1 024** znaků za hovoru (jednou zakódovaný JSON agentem zapojení).

V předchozím příkladu JSON odeslaných na server je 58 znaků:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Informace o vytváření sestav aplikace

Můžete ručně nahlásit sledování informací (nebo všechny ostatní aplikace konkrétní informace) pomocí `sendAppInfo:` (funkce).

Všimněte si, že tyto informace odesílat postupně: zůstanou, jenom nejnovější hodnotu pro daný klíč pro dané zařízení.

Extra události, jako `NSDictionary` třídy se používá k abstraktní informace o aplikaci, Všimněte si, že matice nebo dílčí slovníky se použije jako paušální řetězce (pomocí serializaci JSON).

**Příklad:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Omezení

#### <a name="keys"></a>Klíče

Každý klíč v `NSDictionary` musí odpovídat následující regulárních výrazů:

`^[a-zA-Z][a-zA-Z_0-9]*`

Znamená to, že klíče musí začínat alespoň jedno písmeno, následovaný písmena, číslice a podtržítka (\_).

#### <a name="size"></a>Velikost

Informace o aplikaci se omezuje na **1 024** znaků za hovoru (jednou zakódovaný JSON agentem zapojení).

V předchozím příkladu je JSON poslané na serveru 44 znaků:

    {"birthdate":"1983-12-07","gender":"female"}
