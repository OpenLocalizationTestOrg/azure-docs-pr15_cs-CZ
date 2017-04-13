<properties 
    pageTitle="Jak někomu zavolat z Twilio (.NET) | Microsoft Azure" 
    description="Zjistěte, jak chcete někomu zavolat a odeslání zprávy SMS ke službě rozhraní API Twilio na Azure. Ukázky napsané v .NET." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Jak někomu zavolat pomocí Twilio web roli v Azure

Tato příručka demonstruje použití Twilio uskutečnit hovor z webové stránky hostované v Azure. Výsledné aplikace výzvu k zadání hodnot telefonní hovor, jak ukazuje následující obrázek.

![Azure volání formuláře pomocí Twilio a technologie ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Zjistit předpoklady pro

Budete potřebovat k následujícím postupem použít kód v tomto tématu:

1. Získání účtu Twilio a ověřování tokenu. Začínáme s Twilio registraci na [https://www.twilio.com/try-twilio][try_twilio]. Je možné vyhodnotit ceny za [http://www.twilio.com/pricing][twilio_pricing]. Informace o rozhraní API poskytovanou Twilio najdete v tématu [http://www.twilio.com/voice/api][twilio_api].
2. Přidáte knihovnu Twilio .NET vaše role web. Najdete v části "Přidání knihoven Twilio role do web projektu" dál v tomto tématu.

Měli byste vědět s vytvářením roli základní web na Azure.

## <a name="howtocreateform"></a>Postup: vytvoření webového formuláře pro volání

<a id="use_nuget"></a>Přidání knihoven Twilio do projektu role web:

1.  Otevřete řešení ve Visual Studiu.
2.  Klikněte pravým tlačítkem myši **odkazy**.
3.  Klikněte na **Spravovat NuGet balíčků**.
4.  Klikněte na **Online**.
5.  Do pole hledání online zadejte *twilio*.
6.  Klikněte na **instalovat** na balíček Twilio.

Následující kód znázorňuje vytvoření webového formuláře k načtení uživatelská data týkající se uplatňování hovoru. V tomto příkladu je vytvořen role ASP.NET webu s názvem **TwilioCloud** .

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Postup: vytvoření kódu pro volání
Následující kód, který je místo toho, když uživatel dokončí formuláře, vytvoří zprávu volání a vygeneruje hovor. V tomto příkladu kód spustit v obslužné rutiny události při klepnutí na tlačítko ve formuláři. (Použijte svůj účet Twilio a ověřování token místo hodnot zástupný symbol přiřazené **accountSID** a **pomocí** následující kód).

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

Při volání a jsou zobrazeny koncový bod Twilio, verze rozhraní API a stav hovoru. Následující obrázek ukazuje výstup z výběru spustit.

![Azure volání odpovědí pomocí Twilio a ASP.NET][twilio_dotnet_basic_form_output]

Další informace o TwiML můžete najít na [http://www.twilio.com/docs/api/twiml][twiml]. Další informace o &lt;Přivítejte&gt; a další akce Twilio, najdete na [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Další kroky
Tento kód získali zobrazit základní funkce pomocí Twilio v roli ASP.NET web na Azure. Než nasadíte Azure ve výrobním, můžete přidat další zpracování chyb nebo jiných funkcí. Příklad:

* Místo použití webového formuláře, můžete použít úložiště objektů Blob Azure nebo instanci databáze SQL Azure ukládat telefonní čísla a hovoru textem. Informace o používání objektů BLOB Azure, přečtěte si, [jak používat službu úložiště objektů Blob Azure v .NET][howto_blob_storage_dotnet]. Informace o používání databáze SQL, přečtěte si, [jak používat databázi SQL Azure v aplikacích .NET][howto_sql_azure_dotnet].
* Můžete použít RoleEnvironment.getConfigurationSettings k získání ID účtu Twilio a ověřovací token z nastavení konfigurace vašeho nasazení místo pevné kódování hodnoty ve formuláři. Informace o třídě RoleEnvironment najdete v tématu [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Přečíst pokyny pro Twilio zabezpečení na [https://www.twilio.com/docs/security][twilio_docs_security].
* Další informace o Twilio na [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Viz taky
* [Jak používat Twilio pro hlasovou a možnosti služby SMS z Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
