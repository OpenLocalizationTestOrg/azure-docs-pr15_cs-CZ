<properties
    pageTitle="Jak používat zapojení rozhraní API na Androidu"
    description="Nejnovější Android SDK - použití zapojení rozhraní API na Androidu"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>Jak používat zapojení rozhraní API na Androidu

Tento dokument je doplněk dokumentu [Možnosti rozšířené možnosti vytváření sestav pro Android SDK zapojení Mobile](mobile-engagement-android-advanced-reporting.md). Poskytuje hloubkové podrobnosti o tom, jak pomocí rozhraní API zapojení vykazování statistiky aplikace.

Mějte na paměti, že pokud chcete jenom zapojení sestav aplikace relace, aktivity, dojde k chybě a technické informace, pak nejjednodušší způsob je aby všechny vaše `Activity` dílčí třídy dědí odpovídající `EngagementActivity` předmětu.

Pokud chcete udělat víc, třeba když potřebujete sestav aplikace zvláštní události, chyby a úlohy, nebo pokud máte zprávu aplikace aktivity jiným způsobem než té implementovaná v `EngagementActivity` třídy, a pak je potřeba použít rozhraní API Engagement.

Rozhraní API zapojení poskytovanou `EngagementAgent` předmětu. Instance tohoto třídy lze získat tak, že zavoláte `EngagementAgent.getInstance(Context)` statické metody (Všimněte si, že `EngagementAgent` vrácené se jedná jednoznačné).

##<a name="engagement-concepts"></a>Zapojení koncepty

Následující části upřesní běžných [Mobile zapojení koncepty](mobile-engagement-concepts.md), pro platformu Android.

### <a name="session-and-activity"></a>`Session`a`Activity`

Pokud uživatel zůstává víc než několik sekund, než nedělá mezi dvěma *aktivity*, jeho posloupnost *aktivity* rozdělen do dvou různých *relace*. Tyto několik sekund, než se označují jako "časový limit relace".

*Aktivity* obvykle souvisí s jednu obrazovku aplikací, to znamená *aktivity* , spustí se na obrazovce se zobrazí a zarážky, když máte zavřený obrazovky: to je případ, kdy SDK zapojení je integrována s použitím `EngagementActivity` třídy.

Ale *aktivity* také možné ovládat ručně pomocí rozhraní API Engagement. Díky rozdělit na dané monitoru v několik částí sub získat další informace o použití této obrazovce (například známé intervalu a jak dlouho se používají dialogová okna v rámci této obrazovce).

##<a name="reporting-activities"></a>Vytváření sestav aktivit

> [AZURE.IMPORTANT] Nemusíte sestavy jako aktivity popsaná v této části, pokud používáte `EngagementActivity` předmětu a jeho variant způsobem popsaným v tématu Jak integrovat zapojení na Android dokumentu.

### <a name="user-starts-a-new-activity"></a>Uživatel spustí nového aktivitu

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Budete muset volat `startActivity()` každé změně činnosti uživatelů. První hovor na tuto funkci spustí novou relaci uživatele.

Nejlepší místo pro volání tato funkce je na jednotlivé aktivity `onResume` zpětné.

### <a name="user-ends-his-current-activity"></a>Uživatel ukončí své aktuální činnosti.

            EngagementAgent.getInstance(this).endActivity();

Budete muset volat `endActivity()` aspoň jednou po dokončení uživatel jeho poslední aktivity. To, že uživatel současné době nečinnosti a že relaci uživatele zavírat nemusí jednou časový limit relace vyprší platnost informuje o SDK zapojení (Pokud jste volali `startActivity()` před vypršením platnosti časový limit relace, je jednoduše obnoven relace).

Nejlepší místo pro volání tato funkce je na jednotlivé aktivity `onPause` zpětné.

##<a name="reporting-events"></a>Vytváření sestav události

### <a name="session-events"></a>Události relace

Události relace se obvykle používají vykazování akce provádí uživatele během jeho relace.

**Příklad bez dalších dat:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Příklad navíc daty:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Samostatný události

Na rozdíl od události relace můžete událostem samostatného mimo kontext relaci.

**Příklad:**

Předpokládejme, že chcete-li sestavu událostí při aktivaci vysílání příjemce:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Oznamování chyb

### <a name="session-errors"></a>Relace chyby

Relace chyby se obvykle používají k chybách ovlivňující ochranu uživatel během jeho relace.

**Příklad:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Samostatné chyby

Na rozdíl od relace chyby může dojít k chybám samostatného mimo kontext relaci.

**Příklad:**

Následující příklad ukazuje, jak a nahlaste chybou pokaždé, když se stane paměť zhoršeným na telefonu, je spuštěná aplikace obrázku.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Úlohy sestav

### <a name="example"></a>Příklad

Předpokládejme, že chcete nahlásit trvání proces přihlášení:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Chyby sestavy průběhu projektu

Chyby můžete týkající se spuštěná úloha místo související s aktuální relaci uživatele.

**Příklad:**

Předpokládejme, že chcete nahlásit chybu během jste přihlášení obrázku:

[...] veřejné void přihlašovacího (místní kontext,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Vytváření sestav události průběhu projektu

Události můžete souviset s spuštěná úloha místo související s aktuální relaci uživatele.

**Příklad:**

Předpokládejme máme sociálních sítí a používáme úlohy sestavy celkový čas, kdy uživatel připojen k serveru. Uživatele můžete pořád ve spojení i pozadí při mu používá jiná aplikace nebo spící telefonu, není k dispozici žádné relace.

Uživatel může přijímat zprávy od jeho přátel, toto je úlohu událost.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Další parametry

Libovolný dat může být připojen události, chyby, aktivity a úlohy.

Tato data můžete strukturu, použije Androidu příruček třídy (ve skutečnosti to funguje jako navíc parametrů v Android způsoby). Poznámka: svazku může obsahovat pole nebo jiné instance příruček.

> [AZURE.IMPORTANT] Pokud vložíte parcelable nebo serializovatelný parametry, ujistěte se, jejich `toString()` metoda implementovaná k vrácení čitelné řetězce. Serializovatelný tříd, které obsahují není přechodná pole, které nejsou serializovatelný provede Android dojde k chybě, když budou volat`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Nejsou podporovány sparse matic v navíc parametry, to znamená, nesmí být serializován jako maticový. Můžete měli převést na standardní matice než ho začnete používat v další parametry.

### <a name="example"></a>Příklad

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Omezení

#### <a name="keys"></a>Klíče

Každý klíč v `Bundle` musí odpovídat následující regulárních výrazů:

`^[a-zA-Z][a-zA-Z_0-9]*`

Znamená to, že klíče musí začínat alespoň jedno písmeno, následovaný písmena, číslice a podtržítka (\_).

#### <a name="size"></a>Velikost

Doplňky se omezuje na **1 024** znaků za hovoru (jednou zakódovaný JSON službou zapojení).

V předchozím příkladu JSON odeslaných na server je 58 znaků:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Informace o vytváření sestav aplikace

Můžete ručně nahlásit sledování informací (nebo všechny ostatní aplikace konkrétní informace) pomocí `sendAppInfo()` (funkce).

Všimněte si, že tyto informace odesílat postupně: jenom nejnovější hodnotu pro daný klíč bude nacházet pro dané zařízení.

Jako události extra třídy příruček slouží k abstraktní informace o aplikaci, Všimněte si, že matice nebo dílčí svazky se použije jako paušální řetězce (pomocí serializaci JSON).

### <a name="example"></a>Příklad

Tady je ukázka kódu odeslat gender uživatele a DatumNarození:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Omezení

#### <a name="keys"></a>Klíče

Každý klíč v `Bundle` musí odpovídat následující regulárních výrazů:

`^[a-zA-Z][a-zA-Z_0-9]*`

Znamená to, že klíče musí začínat alespoň jedno písmeno, následovaný písmena, číslice a podtržítka (\_).

#### <a name="size"></a>Velikost

Informace o aplikaci se omezuje **1024** znaků na volání (jednou zakódovaný JSON službou zapojení).

V předchozím příkladu je JSON poslané na serveru 44 znaků:

            {"expiration":"2016-12-07","status":"premium"}
