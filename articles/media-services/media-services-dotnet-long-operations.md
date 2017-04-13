<properties 
    pageTitle="Dotazování dlouho probíhajících operace | Microsoft Azure" 
    description="Toto téma ukazuje, jak hlasování dlouho probíhajících operace." 
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


#<a name="delivering-live-streaming-with-azure-media-services"></a>Prezentovat živá streamování s Azure Media Services

##<a name="overview"></a>Základní informace

Microsoft Azure Media Services nabízí rozhraní API, které odešlou Media Services spuštění operace (například: vytvoření, spustit, zastavit nebo odstranit kanál). Tyto operace jsou časově náročné.

Media Services .NET SDK poskytuje rozhraní API, která odešlete žádost a počkejte na dokončení operace (interně rozhraní API jsou dotazování průběhu operace některé intervalech). Třeba při volání na kanál. Start(), metodu vrátí po spuštění kanál. Můžete také asynchronní verze: očekávat kanálu. StartAsync() (informace o založeným na úkolech asynchronní najdete v tématu [klepněte na](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). Rozhraní API, které žádost operace Odeslat a potom hlasování stavu až do dokončení operace se označují jako "hlasování metody". Těchto metod (zejména verze asynchronní) se doporučuje aplikace klienta RTF nebo stavové služby.

Existuje scénáře, které aplikace nelze čekat dlouhodobé žádost http a chce hlasování průběhu operace ručně. Příklad typického bude v prohlížeči interakce s příslušnosti webové služby: prohlížeči požádá o vytvoření kanálu, webové služby spustí dlouhodobé operaci a vrátí ID operace do prohlížeče. V prohlížeči může požádejte webová služba stav operace podle identifikátor Media Services .NET SDK poskytuje rozhraní API, která jsou užitečné pro tento scénář. Tato rozhraní API se označují jako "není hlasování metody".
"-Hlasování metody" mít takto vzor pro pojmenování: odeslání*název operace*operace (například SendCreateOperation). Odeslání*název operace*operace metody vrátí **IOperation** objektu. vrácený objekt obsahuje informace, které slouží ke sledování operaci. Metody OperationAsync*název operace*odeslat vrátí **úkolu<IOperation>**.

V současné době podporují následující třídy není hlasování metod: **kanál**, **StreamingEndpoint**a **Program**.

Dotazování pro stav operace, použijte metodu **GetOperation** ve třídě **OperationBaseCollection** . Kontrola stavu operace pomocí následující intervalů: **kanálu** a **StreamingEndpoint** operací, použijte 30 sekund. **Program** operací použijte 10 sekund.


##<a name="example"></a>Příklad

Následující příklad definuje třídy s názvem **ChannelOperations**. Tento definice třídy může být výchozí bod pro definici vaší webové služby třídy. Pro zjednodušení použít následující příklady není asynchronní verze metody.

V příkladu také ukáže, jak pomocí této třídy klienta.

###<a name="channeloperations-class-definition"></a>Definice ChannelOperations třídy

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelInput001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelPreview CreateChannelPreview()
        {
            return new ChannelPreview
            {
                AccessControl = new ChannelAccessControl
                {
                    IPAllowList = new List<IPRange>
                    {
                        new IPRange
                        {
                            Name = "TestChannelPreview001",
                            Address = IPAddress.Parse("0.0.0.0"),
                            SubnetPrefixLength = 0
                        }
                    }
                }
            };
        }
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>Kód klienta

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
