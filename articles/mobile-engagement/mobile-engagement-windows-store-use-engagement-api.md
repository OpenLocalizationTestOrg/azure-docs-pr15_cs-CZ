<properties 
    pageTitle="Používání zapojení rozhraní API na univerzální systému Windows" 
    description="Používání zapojení rozhraní API na univerzální systému Windows"            
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-universal"></a>Používání zapojení rozhraní API na univerzální systému Windows

Tento dokument je doplněk dokumentu [jak integrovat zapojení na Windows univerzální](mobile-engagement-windows-store-integrate-engagement.md): poskytuje název hloubkové podrobnosti o tom, jak pomocí rozhraní API zapojení vykazování statistiky aplikace.

Mějte na paměti, že pokud chcete jenom zapojení sestav aplikace relace, aktivity, dojde k chybě a technické informace, pak nejjednodušší způsob je aby všechny vaše `Page` dílčí třídy dědí `EngagementPage` předmětu.

Pokud chcete udělat víc, třeba když potřebujete sestav aplikace zvláštní události, chyby a úlohy, nebo pokud máte zprávu aplikace aktivity jiným způsobem než té implementovaná v `EngagementPage` třídy, a pak je potřeba použít rozhraní API Engagement.

Rozhraní API zapojení poskytovanou `EngagementAgent` předmětu. Se dostanete na tyto metody prostřednictvím `EngagementAgent.Instance`.

I v případě modulu agent není aktualizována, každé volání rozhraní API je odložené a bude spuštěn znovu při agent je k dispozici.

##<a name="engagement-concepts"></a>Zapojení koncepty

Následující části upřesní běžných [Mobile zapojení koncepty](mobile-engagement-concepts.md) pro platformu Windows univerzální.

### <a name="session-and-activity"></a>`Session`a`Activity`

*Aktivity* obvykle souvisí s jednu stránku aplikací, to znamená *aktivity* začíná na stránce se zobrazí a zarážky, když máte zavřený stránky: to je případ, kdy SDK zapojení je integrována s použitím `EngagementPage` předmětu.

Ale *aktivity* také možné ovládat ručně pomocí rozhraní API Engagement. Umožňuje provádět rozdělit na dané stránce několik částí sub získat další informace o použití této stránky (například vědět, jak často a jak dlouho se používají dialogová okna v rámci této stránky).

##<a name="reporting-activities"></a>Vytváření sestav aktivit

### <a name="user-starts-a-new-activity"></a>Uživatel spustí nového aktivitu

#### <a name="reference"></a>Odkaz

            void StartActivity(string name, Dictionary<object, object> extras = null)

Budete muset volat `StartActivity()` každé změně činnosti uživatelů. První hovor na tuto funkci spustí novou relaci uživatele.

> [AZURE.IMPORTANT] V SDK automaticky volá metodu EndActivity, když máte zavřený aplikace. Proto doporučujeme volat metodu StartActivity kdykoli aktivity změn uživatele a nikdy volat metodu EndActivity od volání tato metoda vynutí aktuální relaci ukončit.

#### <a name="example"></a>Příklad

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Uživatel ukončí své aktuální činnosti.

#### <a name="reference"></a>Odkaz

            void EndActivity()

Dojde k ukončení aktivity a relace. Tuto metodu neměli volat, pokud skutečně nevíte, co právě děláte.

#### <a name="example"></a>Příklad

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Úlohy sestav

### <a name="start-a-job"></a>Zahájení projektu

#### <a name="reference"></a>Odkaz

            void StartJob(string name, Dictionary<object, object> extras = null)

Úloha slouží ke sledování úkolů certains průběhu určitého časového období.

#### <a name="example"></a>Příklad

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Ukončení projektu

#### <a name="reference"></a>Odkaz

            void EndJob(string name)

Jakmile úkol projektu sledována byl ukončen, je potřeba zavolat na metodu EndJob pro tento projekt zadáním názvu projektu.

#### <a name="example"></a>Příklad

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Vytváření sestav události

Existuje tři typy událostí:

-   Samostatný události
-   Události relace
-   Úlohy události

### <a name="standalone-events"></a>Samostatný události

#### <a name="reference"></a>Odkaz

            void SendEvent(string name, Dictionary<object, object> extras = null)

Samostatný můžete událostem mimo kontext relaci.

#### <a name="example"></a>Příklad

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Události relace

#### <a name="reference"></a>Odkaz

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Události relace se obvykle používají vykazování akce provádí uživatele během jeho relace.

#### <a name="example"></a>Příklad

**Bez data:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**S daty:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Úlohy události

#### <a name="reference"></a>Odkaz

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Události projektu se obvykle používají vykazování akce provádí uživatele průběhu projektu.

#### <a name="example"></a>Příklad

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Oznamování chyb

Existují tři typy chyb:

-   Samostatné chyby
-   Relace chyby
-   Chyb úloh

### <a name="standalone-errors"></a>Samostatné chyby

#### <a name="reference"></a>Odkaz

            void SendError(string name, Dictionary<object, object> extras = null)

Na rozdíl od relace chyby může dojít k chybám samostatného mimo kontext relaci.

#### <a name="example"></a>Příklad

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Relace chyby

#### <a name="reference"></a>Odkaz

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Relace chyby se obvykle používají k chybách ovlivňující ochranu uživatel během jeho relace.

#### <a name="example"></a>Příklad

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Chyb úloh

#### <a name="reference"></a>Odkaz

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Chyby můžete týkající se spuštěná úloha místo související s aktuální relaci uživatele.

#### <a name="example"></a>Příklad

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Vytváření sestav dojde k chybě

Agent poskytuje dva způsoby řešení chyb.

### <a name="send-an-exception"></a>Odeslání výjimky

#### <a name="reference"></a>Odkaz

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Příklad

Výjimky kdykoli můžete odeslat tak, že zavoláte:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Volitelný parametr můžete taky ukončit relaci zapojení ve stejnou dobu než odesílání k chybě. Postup volejte:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Pokud to uděláte, relace a práce se zavře jenom po odeslání k chybě.

### <a name="send-an-unhandled-exception"></a>Odeslání neošetřené výjimce

#### <a name="reference"></a>Odkaz

            void SendCrash(Exception e)

Zapojení také představuje způsob, jak odeslat neošetřené výjimky, pokud máte hlášení automatické **Pád** **Zakázané** Engagement. To je užitečné při použití uvnitř obslužné rutiny události UnhandledException aplikace.

Metoda bude **vždy** ukončit relaci zapojení a úlohy po volání.

#### <a name="example"></a>Příklad

Můžete ho implementovat vlastní rutiny UnhandledExceptionEventArgs. Například, přidejte `Current_UnhandledException` metodu `App.xaml.cs` souboru:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

V App.xaml.cs v "Veřejné App() {}" přidáte:

            Application.Current.UnhandledException += Current_UnhandledException;

##<a name="device-id"></a>Id zařízení

            String EngagementAgent.Instance.GetDeviceId()

Id zařízení zapojení můžete získat tak, že zavoláte tento způsob.

##<a name="extras-parameters"></a>Extra parametry

Libovolný dat může být připojen události, k chybě, aktivitu nebo úlohu. Tyto dat může vypadat pomocí do slovníku. Klíče a hodnoty může být libovolného typu.

Extra dat jsou serializovány, pokud chcete vložit vlastní typ extra budete muset přidat data smlouvu pro tento typ.

### <a name="example"></a>Příklad

Vytvoření nové třídy "Pracovníka".

            using System.Runtime.Serialization;
            
            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Potom přidáme `Person` instance na další.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Pokud můžete zpracování odložit jiné typy objektů, zkontrolujte, že metoda jejich ToString() implementovaná k vrácení lidské čitelné řetězce.

### <a name="limits"></a>Omezení

#### <a name="keys"></a>Klíče

Každý klíč v objektu musí odpovídat následující regulárních výrazů:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Znamená to, že klíče musí začínat alespoň jedno písmeno, následovaný písmena, číslice a podtržítka (\_).

#### <a name="size"></a>Velikost

Doplňky se omezuje **1024** znaků za volání.

##<a name="reporting-application-information"></a>Informace o vytváření sestav aplikace

### <a name="reference"></a>Odkaz

            void SendAppInfo(Dictionary<object, object> appInfos)

Funkce sledování informací (nebo všechny ostatní aplikace konkrétní informace) pomocí SendAppInfo() můžete ručně zprávu.

Všimněte si, že tato data odesílat postupně: jenom nejnovější hodnotu pro daný klíč bude nacházet pro dané zařízení. Jako extra událostí použití slovníku\<objektů, objektů\> připojit data.

### <a name="example"></a>Příklad

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Omezení

#### <a name="keys"></a>Klíče

Každý klíč v objektu musí odpovídat následující regulárních výrazů:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Znamená to, že klíče musí začínat alespoň jedno písmeno, následovaný písmena, číslice a podtržítka (\_).

#### <a name="size"></a>Velikost

Informace o aplikaci se omezí na **1 024** znaků za volání.

V předchozím příkladu je JSON poslané na serveru 44 znaků:

            {"birthdate":"1983-12-07","gender":"female"}

##<a name="logging"></a>Zapnout protokolování
###<a name="enable-logging"></a>Povolení protokolování

K vytvoření protokoly testovat v konzole integrovaném vývojovém prostředí je možné konfigurovat SDK.
Ve výchozím nastavení nejsou aktivní tyto protokoly. Chcete-li přizpůsobit, aktualizujte vlastnost `EngagementAgent.Instance.TestLogEnabled` na jednu hodnotu poskytuje společnost `EngagementTestLogLevel` výčet, například:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
 
