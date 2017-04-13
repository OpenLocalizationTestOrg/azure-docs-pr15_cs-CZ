
<properties
    pageTitle="Otevření aplikace z libovolného zařízení | Microsoft Azure"
    description="Zjistěte, jaké klienti jsou podporovány pro Azure RemoteApp a jak získat přístup ke aplikace."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="accessing-your-apps-in-azure-remoteapp"></a>Otevření aplikace v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Jednou z beauties Azure RemoteApp je, že se dostanete aplikace z libovolného zařízení. Ještě lépe můžete začít pracovat na jednom zařízení a bezproblémové přechodu na druhé zařízení a vystopovat doprava, kde jste skončili. Začněte tím budete muset stáhnout příslušný klient pro zařízení a přihlaste se ke službě.

V tomto tématu: přečtěte aktuálně podporovaných klientů a jak se před můžu vidíte, jak se přihlásit k RemoteApp ze všech klienti stáhnout.

## <a name="supported-clients"></a>Podporovaní klienti

Přístup k RemoteApp provedením následujících kroků, pokud vaše zařízení se systémem jednu z těchto operační systémy:

 - Windows 10 
 - Windows 8.1
 - Windows 8
 - Windows 7 Service Pack 1
 - Windows Phone 8.1
 - iOS
 - Mac OS X
 - Android


 Co dávat pozor tenké klienti? Jsou podporovány následující Windows vložený tenké klienti:

- Windows vložené standardní 7
- Windows 8 standardní vložené
- Vložené Windows 8.1 odvětví Pro
- Windows 10 IoT Enterprise


## <a name="downloading-the-client"></a>Stahování klienta

Bez ohledu na to, kterou platformu používáte klienta pro přístup k RemoteApp nepotřebujete najdete na stránce [Stažení klienta vzdálené plochy](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) .

Kliknutím na různé odkazy budou buď přímo spusťte stahování klienta nebo vám pošle že klientovi stáhnete stránky v obchodu app store pro tuto platformu. Instalace klienta podle pokynů na obrazovce.

Jakmile máte na svém zařízení nainstalovaný klienta a spuštění ho, přeskočíte na odpovídající části se dozvíte, jak se přihlásit k RemoteApp z tohoto klienta.

## <a name="android"></a>Android

Po instalaci aplikace Microsoft vzdálené plochy z obchodu Google Play, najdete ji v seznamu aplikací ve skupinovém rámečku **Vzdálená plocha**.

1. Spuštění aplikace přináší prázdné centrum připojení, pokud již už používáte aplikaci. Začínáme s Azure RemoteApp, klepněte přidat tlačítko **"" +""** a klepněte na **Azure RemoteApp**. 

     ![Prázdné centrum připojení](./media/remoteapp-clients/Android1.png)

2. Budete muset přihlásit pomocí e-mailové adresy a přístup ke službě. Klepněte na **Začínáme**.

    ![Přihlaste se na příkazovém řádku](./media/remoteapp-clients/Android2.png)

3. Na další stránce zadejte svoji **e-mailovou adresu** a klepněte na **pokračovat**. To zahájí proces přihlášení pomocí služby Azure Active Directory.

    ![První stránka Azure Active Directory](./media/remoteapp-clients/Android3.png)

4. Postupujte podle pokynů na obrazovce Přihlaste se pomocí svého účtu Microsoft (dříve nazývanou "Pověření") nebo ID organizace. Když se přihlásíte, mohou předkládat se stránkou, na výpis u všech pozvánek, které jste přijali. Pokud si nejste, vyberte pozvánky důvěryhodné a klepněte na **Hotovo**. 

    ![Stránka pozvánky](./media/remoteapp-clients/Android4.png)

5. Po přijetí své pozvánky, v seznamu aplikací, ke kterým máte přístup k stáhnout do zařízení a zpřístupnili v Centru pro připojení. Klepněte na některé z aplikací začít s ním pracovat.

    ![Připojení centra se informační kanál](./media/remoteapp-clients/Android5.png)

6. Pokud nemáte pozvánku ještě pořád vyzkoušet službu. Postup, klepněte na **Přejít na bezplatnou zkušební verzi** po zobrazení výzvy.

    ![Ukázka výzva kanálu](./media/remoteapp-clients/Android6.png)

7. To vám umožní přístup k sadě základní aplikací, které vám usnadní zahájení práce s RemoteApp.

    ![Ukázka kanálu pro Azure RemoteApp](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a>iOS

Po instalaci aplikace Microsoft vzdálené plochy z App storu můžete najít v seznamu aplikací v části **RD klienta**.

1. Spuštění aplikace přináší prázdné centrum připojení, pokud již už používáte aplikaci. Začínáme s Azure RemoteApp, klepněte přidat tlačítko **"" +""** a klepněte na **Přidat RemoteApp Azure**.

    ![Prázdné centrum připojení](./media/remoteapp-clients/IOS1.png)

2. Budete potřebovat Přihlaste se pomocí e-mailové adresy pro přístup ke službě, spusťte proces, zadejte svoji **e-mailovou adresu** a klepněte na **pokračovat**.

    ![Přihlaste se na příkazovém řádku](./media/remoteapp-clients/picture1.png)

3. Postupujte podle pokynů na obrazovce Přihlaste se pomocí svého účtu Microsoft (služby Live ID) nebo ID organizace. Když se přihlásíte, mohou předkládat se stránkou, na výpis u všech pozvánek, které jste přijali. Pokud si nejste, vyberte pozvánky důvěryhodné a klepněte na **Hotovo**.

    ![Stránka pozvánky](./media/remoteapp-clients/IOS3.png)

4. Po přijetí své pozvánky, v seznamu aplikací, ke kterým máte přístup k stáhnout do zařízení a k dispozici v Centru pro připojení. Klepněte na některé z aplikací ji spustit a začít s ním pracovat.

    ![Připojení centra se informační kanál](./media/remoteapp-clients/IOS4.png)

5. Pokud nemáte pozvánku ještě pořád vyzkoušet službu. Postup, klepněte na **Přejít na bezplatnou zkušební verzi** po zobrazení výzvy.

    ![Ukázka výzva kanálu](./media/remoteapp-clients/IOS5.png)

6. To vám umožní přístup k sadě základní aplikací, které vám usnadní zahájení práce s RemoteApp.

    ![Ukázka kanálu pro Azure RemoteApp](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a>Mac OS X

Po instalaci aplikace Microsoft vzdálené plochy z App storu můžete najít v seznamu aplikací v části **Microsoft Vzdálená plocha**.

1. Spuštění aplikace přináší prázdné centrum připojení, pokud již už používáte aplikaci. Chcete-li začít pracovat s Azure RemoteApp, klikněte na tlačítko **Azure RemoteApp** .

    ![Prázdné centrum připojení](./media/remoteapp-clients/Mac1.png)

2. Budete potřebovat k Přihlaste se pomocí e-mailové adresy pro přístup ke službě, spusťte tento proces klepněte na **Začínáme**.

    ![Přihlaste se na příkazovém řádku](./media/remoteapp-clients/Mac2.png)

3. Na další stránce zadejte svoji **e-mailovou adresu** a klepněte na **pokračovat**. Zahájení procesu pomocí služby Azure Active Directory přihlášení.

    ![První stránka Azure Active Directory](./media/remoteapp-clients/picture2.png)

4. Postupujte podle pokynů na obrazovce Přihlaste se pomocí svého účtu Microsoft (služby Live ID) nebo ID organizace. Když se přihlásíte, mohou předkládat se stránkou, na výpis u všech pozvánek, které jste přijali. Pokud si nejste, vyberte pozvánky důvěryhodné a zavřete dialogové okno.

    ![Stránka pozvánky](./media/remoteapp-clients/Mac4.png)

5. Po přijetí své pozvánky, v seznamu aplikací, ke kterým máte přístup k stáhnout do zařízení a zpřístupnili v Centru pro připojení. Poklikejte na některé z aplikací ji spustit a začít s ním pracovat.

    ![Připojení centra se informační kanál](./media/remoteapp-clients/Mac5.png)

6. Pokud nemáte pozvánku ještě pořád vyzkoušet službu. K tomu, klikněte na **Přejít na bezplatnou zkušební verzi** po zobrazení výzvy.

    ![Ukázka výzva kanálu](./media/remoteapp-clients/Mac6.png)

7. To vám umožní přístup k sadě základní aplikací, které vám usnadní zahájení práce s RemoteApp.

    ![Ukázka kanálu pro Azure RemoteApp](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a>Windows (všechny podporované verze kromě Windows Phone)

Po dokončení instalace, ale pokud je potřeba ji později ho najdete v seznamu aplikací pod názvem **Azure RemoteApp**se automaticky spustí klienta.

1. Spuštění klienta, první stránku, kterou uvidíte hřívací zařízení vás přivítá Azure RemoteApp. Chcete-li pokračovat, klikněte na **Začínáme**.

    ![Úvodní stránka klienta Azure RemoteApp](./media/remoteapp-clients/Windows1.png)

2. Další stránka spuštění znaménko procesu RemoteApp Azure pomocí služby Azure Active Directory. Tento proces by měl vypadat známé, pokud používáte službu v minulosti. Začněte psát svou **e-mailovou adresu** a klikněte na **pokračovat**.

    ![První výzvu Azure Active Directory](./media/remoteapp-clients/Windows2.png)

3. Postupujte podle pokynů na obrazovce Přihlaste se pomocí svého účtu Microsoft (služby Live ID) nebo ID organizace. Když se přihlásíte, mohou předkládat se stránkou, na výpis u všech pozvánek, které jste přijali. Pokud si nejste, vyberte pozvánky důvěryhodné a klikněte na tlačítko **Hotovo**.

    ![Stránka pozvánky klienta Azure RemoteApp](./media/remoteapp-clients/Windows3.png)

4. Po přijetí své pozvánky, v seznamu aplikací, ke kterým máte přístup k stáhnout do zařízení a k dispozici v Centru pro připojení. Poklikejte na některé z aplikací ji spustit a začít s ním pracovat.

    ![Centrum připojení klienta Azure RemoteApp](./media/remoteapp-clients/Windows4.png)

5. Pokud nikdo vám poslal pozvánku ještě, Nedělejte si starosti, zde se vztahuje máme! Pořád budete mít přístup ke kolekci ukázku, můžete vyzkoušet si službu.

    ![Ukázka kanálu pro Azure RemoteApp](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a>Windows Phone 8.1

Po instalaci aplikace Microsoft vzdálené plochy z obchodu Windows Phone 8.1, najdete ji v seznamu aplikací ve skupinovém rámečku **Vzdálená plocha**.

1. Spuštění aplikace přináší přímo do prázdné centrum připojení, pokud již už používáte aplikaci. Začít s Azure RemoteApp, klepněte přidat tlačítko **"" +""** v dolní části obrazovky.

    ![Prázdné centrum připojení](./media/remoteapp-clients/WinPhone1.png)

2. Další klepněte na **Azure RemoteApp**.

    ![Přidání stránky položky](./media/remoteapp-clients/WinPhone2.png)

3. Můžete třeba chcete Přihlaste se pomocí e-mailové adresy pro přístup ke službě, spusťte tento proces klepněte na **Připojit**.

    ![Přihlaste se na příkazovém řádku](./media/remoteapp-clients/WinPhone3.png)

4. Na další stránce zadejte svoji **e-mailovou adresu** a klepněte na **pokračovat**. Zahájení procesu pomocí služby Azure Active Directory přihlášení.

    ![První stránka Azure Active Directory](./media/remoteapp-clients/WinPhone4.png)

5. Postupujte podle pokynů na obrazovce Přihlaste se pomocí svého účtu Microsoft (služby Live ID) nebo ID organizace. Když se přihlásíte, mohou předkládat se stránkou, na výpis u všech pozvánek, které jste přijali. Pokud si nejste, vyberte pozvánky důvěryhodné a klepněte na **Uložit**.

    ![Stránka pozvánky](./media/remoteapp-clients/WinPhone5.png)

6. Po přijetí své pozvánky, v seznamu aplikací, ke kterým máte přístup k stáhnout do zařízení a zpřístupnili v Centru pro připojení. Klepněte na některé z aplikací ji spustit a začít s ním pracovat.

    ![Připojení centra se informační kanál](./media/remoteapp-clients/WinPhone6.png)

7. Pokud nemáte pozvánku ještě pořád vyzkoušet službu. Postup, klepněte na **Ano** po zobrazení výzvy.

    ![Ukázka výzva kanálu](./media/remoteapp-clients/WinPhone7.png)

8. To vám umožní přístup k sadě základní aplikací, které vám usnadní zahájení práce s RemoteApp.

    ![Ukázka kanálu pro Azure RemoteApp](./media/remoteapp-clients/WinPhone8.png)
 