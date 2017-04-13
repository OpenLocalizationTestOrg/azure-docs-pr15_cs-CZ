<properties 
    pageTitle="Azure mobilní zapojení - integrace back-end" 
    description="Připojení Azure Mobile zapojení se back-end služby SharePoint k vytvoření kampaně ze služby SharePoint" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure mobilní zapojení - rozhraní API integrace

V systému automatické marketingové vytváření a aktivace marketingových kampaní taky probíhají automaticky. K tomuto účelu - Azure Mobile zapojení umožňuje vytváření takových automatické marketingových kampaní taky pomocí rozhraní API. 

Obvykle zákazníky pomocí rozhraní front-end Mobile zapojení provést oznámení/hlasování atd jako součást marketingových kampaní. Ale jakmile marketingových kampaní k zdokonaleným, je třeba využít datové uzamčené v systémech back-end (třeba všechny CRM nebo cm systému jako SharePoint) tak, aby plně automatizovaného kanálu můžete vytvářet v mobilní zapojení dynamicky na základě dat v vyplývající z systémy back-end vytvoří kampaní. 

![][5]

Tento kurz prochází situace, kde firemní uživatel SharePoint naplní seznam služby SharePoint s marketingovou dat a automatického procesu vyzvedne položek ze seznamu připojení k systému Mobile zapojení pomocí dostupných rozhraní REST API pro vytvoření marketingové kampaně ze dat služby SharePoint. 
 
> [AZURE.IMPORTANT] Obecně můžete v tomto příkladu jako výchozí bod pro vědět, jak má volání všechny Mobile zapojení REST API jako obsahuje podrobné informace o dvou klíčové aspekty volání rozhraní API - ověřování a předejte parametry. 

## <a name="sharepoint-integration"></a>Integrace se Sharepointem
1. Takto vypadá ukázkový seznam služby SharePoint. **Název**, **kategorie**, **NotificationTitle**, **zpráva** a **adresu URL** slouží k vytvoření oznámení. Je sloupec s názvem **IsProcessed** používané k automatizaci procesem vzorku ve formě konzoly programu. Můžete buď spustit tento konzoly programu jako Azure WebJob tak, že je možné naplánovat nebo můžete přímo pracovní postup služby SharePoint do aplikace vytváření a zapnutí oznámení při vložení položky do seznamu služby SharePoint. V tomto příkladu použijeme konzoly program, který prochází položek v seznamu služby SharePoint, vytvořit oznámení v Azure Mobile zapojení pro každý z nich a nakonec označí příznak **IsProcessed** splnit při vytvoření úspěšné oznámení.

    ![][1]

2. Jsme pomocí kódu z výběru *vzdálené ověřování ve službě SharePoint Online pomocí objektového modelu klienta* [tady](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) ověření se seznamem služby SharePoint.
 
3. Po ověření jsme opakovaně prochází položky seznamu zjistěte nově vytvořený položek (který budou mít **IsProcessed** = false). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Integrace zapojení Mobile
1.  Jakmile se nám najít položky, které vyžadují zpracování - jsme extrahovat informace potřebné k vytvoření oznámení z položky seznamu a volání `CreateAzMECampaign` k jejímu vytvoření a následně `ActivateAzMECampaign` s jeho aktivací. Toto jsou v podstatě rozhraní REST API hovory volá ohledně back-end Mobile Engagement. 

2.  Mobile zapojení REST API vyžadují **záhlaví HTTP se tak mohli ověřovat schéma základní ověřování** , která se skládá z `ApplicationId` a `ApiKey` kterou můžete získat z portálu Microsoft Azure. Ujistěte se, že používáte klávesu s oddílem **rozhraní api klíče** a *Ne* v oddílu **sdk klíče** . 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Při vytváření oznámení type kampaně - [si přečtěte následující dokumentaci](https://msdn.microsoft.com/library/azure/mt683750.aspx). Potřebujete, abyste měli jistotu, že zadáváte kampaně `kind` i *oznámení* a [datové](https://msdn.microsoft.com/library/azure/mt683751.aspx) předáním jako FormUrlEncodedContent. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Až budete mít oznámení vytvořili, zobrazí se něco jako následující na portálu Mobile zapojení (Všimněte si, že stav = Koncept a aktivovaný = není k dispozici)

    ![][3]

5. `CreateAzMECampaign`vytvoří kampaň oznámení a vrátí jeho Id volajícího. `ActivateAzMECampaign`vyžaduje toto Id jako parametr aktivujte kampaně. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Až budete mít oznámení aktivaci, zobrazí se něco jako následující na portálu zapojení Mobile:

    ![][4]

7. Hned po aktivaci kampaně získá všechna zařízení, které splňují kritérium na kampaně začnou zobrazují oznámení. 

8. Všimnete si také, že položka seznamu označené IsProcessed = false byla nastavena na hodnotu True po vytvoření kampaně oznámení.  

V tomto příkladu vytvoření jednoduchého oznámení kampaně určující převážně požadované vlastnosti. Můžete přizpůsobit velmi podobným způsobem jako lze z portálu pomocí informací [v tomto poli](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
