<properties 
    pageTitle="Hladce streamování pro Windows Store App kurz | Microsoft Azure" 
    description="Naučte se používat Azure Media Services vytvoření aplikace pro Windows Store C# k ovládacímu prvku XML MediaElement vyhlazenými proudu přehrávání obsahu." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Vytvoření hladký přenos aplikace pro Windows Store

Hladký přenos klienta SDK pro Windows 8 umožňuje vývojářům vytváření aplikace pro Windows Store, které můžete přehrávat na vyžádání a živou hladký přenos obsahu. Kromě základní přehrávání hladký přenos obsahu že v SDK také poskytuje bohaté funkce jako Microsoft PlayReady ochranu, kvality zvuku úroveň omezení, Live DVR přechodu na jiný, naslouchají aktualizací stavu (například změny na úrovni kvality) a chybové události můžete vysílat datovými proudy atd. Další informace podporovaných funkcí naleznete v tématu [poznámky k verzi](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Další informace najdete v tématu [přehrávače Framework pro Windows 8](http://playerframework.codeplex.com/). 

Tento kurz obsahuje čtyři spolupráci:

1. Vytvoření základní vyhlazenými streamování úložiště přihlašovacích údajů aplikace
2. Přidání posuvníku řídit průběh médií
3. Vyberte hladký přenos datových proudů
4. Vyberte hladký přenos stopy

##<a name="prerequisites"></a>Zjistit předpoklady pro

- Windows 8 32bitová verze nebo 64bitová verze. [Windows 8 Enterprise hodnocení](http://msdn.microsoft.com/evalcenter/jj554510.aspx) můžete získat z MSDN.
- Visual Studio 2012 nebo Visual Studio Express 2012 (nebo novější). Zkušební verze můžete získat z [tady](http://www.microsoft.com/visualstudio/11/downloads).
- [Microsoft hladký přenos klienta SDK pro Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


Dokončené řešení pro každé lekce můžete stáhnout z ukázek kódu vývojář MSDN (Galerie kódu): 

- [Lekce 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - jednoduché Windows 8 hladký přenos Media Player 
- Určit [Lekce 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - jednoduché Windows 8 hladký přenos přehrávače s posuvník 
- [Lekce 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - Windows 8 hladký přenos přehrávač s vybranou toku  
- [Lekce 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - Windows 8 hladký přenos přehrávač s vybranou sledování.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Lekce 1: Vytvoření základní vyhlazenými streamování úložiště přihlašovacích údajů aplikace

V této lekci vytvoříte aplikace pro Windows Store se ovládací prvek MediaElement přehrát vyhlazenými obsahu.  Spuštění aplikace vypadá takto:

![Příklad aplikace hladkým datových proudů Windows Store][PlayerApplication]
 
Další informace o vývoji aplikací pro Windows Store najdete v článku [vyvíjet skvělé aplikace pro Windows 8](http://msdn.microsoft.com/windows/apps/br229512.aspx). V této lekci obsahuje následující postupy:

1.  Vytvoření projektu pro Windows Store
2.  Návrh uživatelského rozhraní (XAML)
3.  Úprava souboru kódu
4.  Sestavit a otestovat aplikace

**Vytvoření projektu pro Windows Store**

1.  Spusťte aplikaci Visual Studio 2012 nebo vyšší.
2.  V nabídce **soubor** klikněte na **Nový**a potom klikněte na **projekt**.
3.  V dialogovém okně Nový projekt zadejte nebo vyberte následující hodnoty:

Jméno|Hodnota
---|---
Skupina šablon|Nainstalovaný/šablony/jazyka Visual Basic a Windows Store
Šablony|Prázdné aplikace (XAML)
Jméno|SSPlayer
Umístění|C:\SSTutorials
Název řešení|SSPlayer
Vytvoření řešení v adresáři|(vybraná)

4.  Klikněte na **OK**.

**Chcete-li přidat odkaz na hladký přenos klienta SDK**

1.  V Průzkumníku klikněte pravým tlačítkem myši **SSPlayer**a potom klikněte na **Přidat odkaz**.
2.  Zadejte nebo vyberte následující hodnoty:

Jméno|Hodnota
---|---
Referenční skupina|Windows/rozšíření
Odkaz|Vyberte Microsoft hladký přenos klienta SDK pro Windows 8 a Microsoft Visual C++ Runtime balíčku
    
3.  Klikněte na **OK**. 

Po přidání odkazy, je nutné vybrat cílové platformy (x64 nebo x86), pro konfiguraci platformy jakýkoli procesor není vhodná přidávání odkazů.  V Průzkumníku uvidíte žlutá značka upozornění pro tyto přidat odkazy.

**Pro návrh uživatelského rozhraní přehrávače**

1.  V Průzkumníku poklikejte na **MainPage.xaml** ho otevřete v návrhovém zobrazení.
2.  Vyhledejte ** &lt;mřížky&gt; ** a ** &lt;/Grid&gt; ** značky souboru XAML a vložte následující kód mezi dvěma značky:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    Ovládací prvek MediaElement slouží k přehrávání média. Posuvník s názvem sliderProgress bude v další lekci slouží k určení průběhu média.

3.  Stiskněte **Kombinaci kláves CTRL + S** uložte soubor.

Ovládací prvek MediaElement nepodporuje hladký přenos obsahu z krabice. Povolení podpory hladký přenos, musíte zaregistrovat tok bajtů rutiny hladký přenos tak, že přípona názvu souboru a typ MIME.  Registrace, použijte metodu MediaExtensionManager.RegisterByteStremHandler Windows.Media názvů.

V tomto souboru XAML některé obslužné rutiny události souvisí s ovládacími prvky.  Je třeba definovat tyto obslužné rutiny události.

**Chcete-li změnit souboru kódu**

1.  V Průzkumníku klikněte pravým tlačítkem myši **MainPage.xaml**a klikněte na tlačítko **Zobrazit kód**.
2.  V horní části souboru, přidejte následující text pomocí příkazu:

        using Windows.Media;

3.  Na začátku třídy **MainPage** přidáte člena následující data:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  Na konci konstruktoru **MainPage** přidejte následující dva řádky:

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Na konci třídě **MainPage** dřívější následující kód:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Obslužná rutina události sliderProgress_PointerPressed je definován tady.  Existují další funguje udělat, spusťte ho práce, která se bude vztahovat v další lekci tohoto kurzu.
6.  Stiskněte **Kombinaci kláves CTRL + S** uložte soubor.

Dokončené souboru kódu se vypadat takto:

![Codeview aplikace Visual Studio aplikace hladkým streamování pro Windows Store][CodeViewPic]

**Kompilace a otestovat aplikace**

1.  V nabídce **sestavení** klikněte na **Správce konfigurace**.
2.  Změna **aktivní řešení platformy** podle vývojové platformy.
3.  Stisknutím klávesy **F6** kompilaci projektu. 
4.  Stisknutím klávesy **F5** spustit aplikaci.
5.  V horní části aplikace můžete použít výchozí hladký přenos URL nebo zadejte jiný název. 
6.  Klikněte na **Nastavení zdroje**. Protože **Automatické přehrávání** aktivované ve výchozím nastavení, se automatické přehrávání média.  Můžete určit pomocí **přehrávat**, tlačítka **pozastavení** a **zastavení** .  Můžete upravit hlasitost média pomocí svislý posuvník.  Ale jezdec vodorovného posuvníku kontroly průběhu médií není implementovaná plně ještě. 

Jste dokončili lesson1.  V této lekci použijte ovládací prvek MediaElement přehrávání hladký přenos obsahu.  V dalším lekci přidáte posuvník pro ovládací prvek průběhu hladký přenos obsahu.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Lekce 2: Přidání posuvníku řídit průběh médií
V Lekce 1 vytvořený aplikace pro Windows Store s ovládacím prvkem MediaElement XAML přehrávání hladký přenos obsahu médií.  Přijde některé funkce základní médií jako spustit, zastavit nebo pozastavit.  V této lekci přidáte ovládací prvek panel jezdec do aplikace.

V tomto kurzu používáme časovače aktualizovat pozici posuvníku na základě aktuální pozice na ovládací prvek MediaElement.  Posuvník počátečního a koncového času taky potřeba aktualizovat v případě živý obsah.  Lepší máte v případě aktualizace adaptivní zdroje.

Média zdroje jsou objekty, které mohou generovat multimediální data.  Překladači zdroje má adresa URL nebo bajt toku a vytvoří zdrojem příslušného média tento obsah.  Překládání zdroje je standardní způsob, jak aplikace vytvářet zdroje média. 

V této lekci obsahuje následující postupy:

1.  Zaregistrovat rutinu hladký přenos 
2.  Přidání obslužné rutiny události na úrovni správce adaptivní zdroje
3.  Přidání úrovně obslužné rutiny adaptivní zdroje
4.  Přidání MediaElement obslužné rutiny události
5.  Přidat posuvník panel souvisejícího kódu
6.  Sestavit a otestovat aplikace

**K registraci tok bajtů rutiny hladký přenos a předejte propertyset**

1.  V Průzkumníku **MainPage.xaml**klikněte pravým tlačítkem a pak klikněte na **Zobrazit kód**.
2.  Na začátku tohoto souboru, přidejte následující text pomocí příkazu:

        using Microsoft.Media.AdaptiveStreaming;

3.  Na začátku třídy MainPage přidáte členy následující data:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  Uvnitř konstruktoru **MainPage** přidáte následující kód po **. Inicializace Components();** řádek a řádky kódu registrace napsané v dřívější výuky:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  Uvnitř konstruktoru **MainPage** upravit můžete přidat dvěma způsoby RegisterByteStreamHandler čtvrtou parametry:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Stiskněte **Kombinaci kláves CTRL + S** uložte soubor.

**Pro přidání rutiny události na úrovni správce adaptivní zdroje**

1.  V Průzkumníku **MainPage.xaml**klikněte pravým tlačítkem a pak klikněte na **Zobrazit kód**.
2.  Uvnitř třídy **MainPage** přidáte člena následující data:

        private AdaptiveSource adaptiveSource = null;

3.  Na konci třídy **MainPage** přidání následující rutiny události:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  Na konci konstruktoru **MainPage** přidejte následující řádek pro přihlášení k odběru je událost open použita adaptivní zdroje:
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += nové AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Stiskněte **Kombinaci kláves CTRL + S** uložte soubor.

**Chcete-li přidat úroveň obslužné adaptivní zdroje**

1.  V Průzkumníku **MainPage.xaml**klikněte pravým tlačítkem a pak klikněte na **Zobrazit kód**.
2.  Uvnitř třídy **MainPage** přidáte člena následující data:
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  Na konci třídy **MainPage** přidejte následující obslužné rutiny události:

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  Na konci metodu **mediaElement AdaptiveSourceOpened** přidáte následující kód pro přihlášení k odběru událostí:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Stiskněte **Kombinaci kláves CTRL + S** uložte soubor.

Stejné události, které jsou k dispozici na adaptivní správce úroveň zdroje také použité k funkce společná pro všechny prvky médií v aplikaci pro zpracování. Každý AdaptiveSource obsahuje vlastní události a všechny události AdaptiveSource bude kaskádových v části AdaptiveSourceManager.

**Chcete-li přidat mediální prvek obslužné rutiny události**

1.  V Průzkumníku **MainPage.xaml**klikněte pravým tlačítkem a pak klikněte na **Zobrazit kód**.
2.  Na konci třídy **MainPage** přidejte následující obslužné rutiny události:
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  Na konci konstruktoru **MainPage** přidáte následující kód do dolního indexu na události:
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Stiskněte **Kombinaci kláves CTRL + S** uložte soubor.

**Přidání posuvníku týkající se kódu**

1.  V Průzkumníku **MainPage.xaml**klikněte pravým tlačítkem a pak klikněte na **Zobrazit kód**.
2.  Na začátku tohoto souboru přidejte následující text pomocí příkazu:
    
        using Windows.UI.Core;

3.  Uvnitř třídy **MainPage** přidáte členy následující data:
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  Na konci konstruktoru **MainPage** přidáte následující kód:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  Na konci třídy **MainPage** přidáte následující kód:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Poznámka:** CoreDispatcher slouží ke změnám vlákno uživatelského rozhraní není vlákna uživatelského rozhraní. V případě kritický podprocesem odesílatele může vývojář rozhodnete sdělit nám odesílatele poskytovanou má v úmyslu aktualizovat prvek uživatelského rozhraní.  Příklad:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  Na konci metodu **mediaElement_AdaptiveSourceStatusUpdated** přidáte následující kód:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  Na konci metodu **MediaOpened** přidáte následující kód:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Stiskněte **Kombinaci kláves CTRL + S** uložte soubor.

**Kompilace a otestovat aplikace**

1. Stisknutím klávesy **F6** kompilaci projektu. 
2.  Stisknutím klávesy **F5** spustit aplikaci.
3.  V horní části aplikace můžete použít výchozí hladký přenos URL nebo zadat jiný název. 
4.  Klikněte na **Nastavení zdroje**. 
5.  Otestujte posuvníku.

Jste dokončili Lekce 2.  V této lekci jste přidali posuvník aplikace. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Lekce 3: Vyberte hladký přenos datových proudů
Hladký přenos datových proudů je schopen proudů s vícejazyčných zvuku stop, které jsou lze vybrat v prohlížeče.  V této lekci umožní publikem vyberte proudů. V této lekci obsahuje následující postupy:

1. Upravit soubor XAML
2. Upravte soubor behand kódu
3. Sestavit a otestovat aplikace


**Chcete-li upravit soubor XAML**

1. V Průzkumníku klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **Zobrazení návrhu**.
2. Vyhledejte &lt;Grid.RowDefinitions&gt;a upravte RowDefinitions tak, že vypadá jako:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. Uvnitř &lt;mřížky&gt;&lt;/Grid&gt; značky, přidat následující kód pro definování ovládací prvek seznam uživatelů můžete zobrazit seznam dostupných datových proudů, takže vyberte datových proudů:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Stiskněte **Kombinaci kláves CTRL + S** uložte změny.


**Chcete-li změnit souboru kódu**

1. V Průzkumníku klikněte pravým tlačítkem myši **MainPage.xaml**a klikněte na tlačítko **Zobrazit kód**.
2. Uvnitř oboru SSPlayer přidat novou třídu: #region třídy toku
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. Na začátku třídy MainPage, přidejte následující proměnné definice:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. Uvnitř třídy MainPage přidejte následující oblasti:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Vyhledejte požadovanou metodu mediaElement_ManifestReady, přidat následující kód na konci funkci:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    Pokud MediaElement manifestu hotovou, kód obdrží seznam dostupných datových proudů, takže naplní seznamu uživatelské rozhraní se seznamem.

6. Uvnitř třídy MainPage vyhledejte uživatelské rozhraní tlačítka kliknete oblast události a potom přidáte definici následující funkce:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Kompilace a otestovat aplikace**

1. Stisknutím klávesy **F6** kompilaci projektu. 
2.  Stisknutím klávesy **F5** spustit aplikaci.
3.  V horní části aplikace můžete použít výchozí hladký přenos URL nebo zadejte jiný název. 
4.  Klikněte na **Nastavení zdroje**. 
5.  Výchozí jazyk je audio_eng. Zkuste se přepíná mezi audio_eng a audio_es. Každém, vyberte nový toku, musí kliknout na tlačítko Odeslat.

Jste dokončili Lekce 3.  V této lekci přidáte funkce zvolit datových proudů.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Lekce 4: Vyberte hladký přenos stopy
Hladký přenos prezentace může obsahovat více videosoubory zakódovaný s různé úrovně kvality (tocích) a jejich řešení. V této lekci umožní uživatelům vybrat stop. V této lekci obsahuje následující postupy:

1. Upravit soubor XAML
2. Upravte soubor behand kódu
3. Sestavit a otestovat aplikace

**Chcete-li upravit soubor XAML**

1. V Průzkumníku klikněte pravým tlačítkem **MainPage.xaml**a potom klikněte na **Zobrazení návrhu**.
2. Vyhledejte &lt;mřížky&gt; označení s názvem **gridStreamAndBitrateSelection**, přidat následující kód na konci značku:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Stisknutím **Kombinace kláves CTRL + S** uložte změny he


**Chcete-li změnit souboru kódu**

1. V Průzkumníku klikněte pravým tlačítkem myši **MainPage.xaml**a klikněte na tlačítko **Zobrazit kód**.
2. Uvnitř oboru SSPlayer přidáte novou třídu:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. Na začátku třídy MainPage, přidejte následující proměnné definice:
    
        private List<Track> availableTracks;

4. Uvnitř třídy MainPage přidejte následující oblasti:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Vyhledejte požadovanou metodu mediaElement_ManifestReady, přidat následující kód na konci funkci:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Uvnitř třídy MainPage vyhledejte uživatelské rozhraní tlačítka kliknete oblast události a potom přidáte definici následující funkce:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Kompilace a otestovat aplikace**

1. Stisknutím klávesy **F6** kompilaci projektu. 
2.  Stisknutím klávesy **F5** spustit aplikaci.
3.  V horní části aplikace můžete použít výchozí hladký přenos URL nebo zadejte jiný název. 
4.  Klikněte na **Nastavení zdroje**. 
5.  Ve výchozím nastavení jsou vybrány všechny drah videa proudu. Experiment bit jejich změn, můžete vybrat nejnižší bit rychlosti k dispozici a pak vyberte nejvyšší bit rychlosti k dispozici. Po každé změně musí klikněte na Odeslat.  Zobrazí se změny kvalitu videa.

Jste dokončili Lekce 4.  V této lekci přidáte funkce zvolit stop.


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Další zdroje:
- [Vytvoření aplikace hladkým streamování JavaScript Windows 8 s pokročilých funkcí](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Hladký streamování technický přehled](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
