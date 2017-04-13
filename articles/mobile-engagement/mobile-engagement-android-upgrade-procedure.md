<properties 
    pageTitle="Integrace Azure mobilní zapojení Android SDK" 
    description="Nejnovější aktualizace a postupy pro Android SDK pro zapojení Mobile Azure"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-android" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="upgrade-procedures"></a>Upgrade postupy

Pokud jste už máte integrovaný starší verzí systému naše SDK aplikace, je nutné zvážit následující skutečnosti při upgradu SDK.

Možná bude potřeba postup několik Pokud zmeškané více verzích SDK. Pokud například můžete migrovat z 1.4.0 do 1.6.0 budete muset nejdřív použijte postup "z 1.4.0 1.5.0" pak "z 1.5.0 1.6.0" postup.

Bez ohledu na verzi upgradujete z, máte k nahrazení `mobile-engagement-VERSION.jar` novým.

##<a name="from-420-to-421"></a>Z 4.2.0 4.2.1

Tento krok možné provést skutečně libovolná verze SDK, ale zlepšování zabezpečení při integraci Reach aktivity.

Měli byste teď přidat `exported="false"` na všechny Reach činnosti.

Aktivity reach by měl vypadat takto vaše `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

##<a name="from-400-to-410"></a>Z 4.0.0 4.1.0

SDK nyní úchyt nový oprávnění model od Android M.

Pokud používáte funkce umístění nebo celkového oznámení přečtěte si [Tento oddíl](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Kromě nový model oprávnění podporujeme teď konfigurace umístění funkce za běhu.
Je nám stále kompatibilní s seznamu parametrů pro umístění ale teď změněny. Použití runtime konfigurace, odeberte v následujících částech z vaší ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

a přečtěte si [postup aktualizované](mobile-engagement-android-integrate-engagement.md#location-reporting) použijte runtime konfigurace.

##<a name="from-300-to-400"></a>Z 3.0.0 4.0.0

### <a name="native-push"></a>Nativní připínáčku

Nativní nabízených (GCM/ADM) se teď používá taky pro v oznamování v aplikaci, musíte nakonfigurovat nativní nabízených přihlašovací údaje pro každý typ nabízených kampaně.

Pokud již není prosím [Tento](mobile-engagement-android-integrate-engagement-reach.md#native-push)postup.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Integrace reach upraven v ``AndroidManifest.xml``.

Nahrazení takto:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Podle

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

Je pravděpodobně načítání obrazovky teď po kliknutí na oznámení (plus pár text nebo obsah webu) nebo hlasování.
Budete muset přidat u těchto kampaní práce v 4.0.0:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Zdroje informací

Vložení nového `res/layout/engagement_loading.xml` souboru do projektu.

##<a name="from-240-to-300"></a>Z 2.4.0 3.0.0

Popisují následující kroky migrace SDK integrace služby Capptain zaměstnanecké přidružení Capptain zabezpečení, které do aplikace technologii Azure Mobile Engagement. Při migraci ze starší verze, obraťte se na webu Capptain migrovat do 2.4.0 nejdřív a pak použijte následující postup.

>[AZURE.IMPORTANT] Capptain a Mobile využití nejsou stejné služby a postupu uvedeného dole zvýrazní jenom jak migrovat aplikaci klienta. Migrace SDK v aplikaci se nemigrují dat z servery Capptain serverům Mobile Engagement.

### <a name="jar-file"></a>SKLENICE soubor

Nahrazení `capptain.jar` tak, že `mobile-engagement-VERSION.jar` ve vaší `libs` složky.

### <a name="resource-files"></a>Zdrojové soubory

Každý soubor prostředku, kterou jsme použili (předchází `capptain_`) má nová nahrazuje (předponou `engagement_`).

Pokud jste přizpůsobili tyto soubory, budete muset znovu použít vlastní nastavení na nové soubory **všechny identifikátory souborů prostředků mít taky přejmenovat**.

### <a name="application-id"></a>ID aplikace

Teď můžete zapojit používá ke konfiguraci identifikátory SDK například identifikátor aplikace připojovací řetězec.

Je nutné použít `EngagementAgent.init` metoda ve Spouštěči aktivitách takto:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Připojovací řetězec pro aplikaci se zobrazí na portál Azure.

Odeberte všechny volání `CapptainAgent.configure` jako `EngagementAgent.init` nahradí příslušný způsob.

`appId` Už není možné konfigurovat pomocí `AndroidManifest.xml`.

Odstraňte tento oddíl z vaší `AndroidManifest.xml` pokud ji máte:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Rozhraní API Java

Každý volání na libovolnou Java třídu naše SDK má přejmenovat; například `CapptainAgent.getInstance(this)` nutné přejmenovat `EngagementAgent.getInstance(this)`, `extends CapptainActivity` nutné přejmenovat `extends EngagementActivity` atd.

Pokud byly integrovaný s výchozí agent předvoleb souborů, výchozí název souboru je teď `engagement.agent` a klíčem je `engagement:agent`.

Při vytváření webu oznámení, pořadače Javascript je teď `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Hodně změn došlo k dispozici, služba nesdílí už a spoustu příjemce už nejsou exportovat.

Deklarace služby je teď jednodušší; Odeberte všechny metadata uvnitř a záměru filtr a přidejte `exportable=false`.

Plus všechno, co je přejmenovat používat engagement.

Teď vypadá:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Pokud chcete povolit test protokoly, údaji meta teď byl přesunutý do značku aplikace a přejmenování se:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Všechny ostatní metadata jste právě přejmenovat, tady je úplný seznam (kurzu přejmenovat jen ty, které používáte):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play a SmartAd sledování byla odebrána z SDK musíte k odebrání bez náhrady:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

Aktivity Reach jsou teď deklarovány takto:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Pokud nemáte vlastní Reach aktivity, potřebujete jenom změnit záměru akce, které se shodují buď `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` nebo `com.microsoft.azure.engagement.reach.intent.action.POLL`.

Přejmenování vysílání příjemce plus teď přičteme `exported=false`. Tady je úplný seznam příjemců s novou specifikaci, (kurzu přejmenovat jen ty, které používáte):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Sledování příjemce byla odebrána, takže musíte odebrat v této části:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Všimněte si, že deklarace vaší implementací vysílání sluchátko **EngagementMessageReceiver** změnila v `AndroidManifest.xml`. Je to proto rozhraní API pro odesílání a odebrání libovolného typu XMPP zpráv z libovolného typu XMPP entity a rozhraní API pro odesílání a přijímání zpráv mezi zařízeními mohly být odebrány. Tím musíte taky následující zpětná zabránění vaší implementací **EngagementMessageReceiver** :

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

a

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Odstraňte všechny volání na **EngagementAgent** pro:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

a

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Může dojít k ovlivnění proguard konfigurace tak, že rebranding, pravidla hledanou teď jako:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
