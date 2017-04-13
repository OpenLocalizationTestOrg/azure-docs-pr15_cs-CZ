<properties 
    pageTitle="Jak někomu zavolat z Twilio (Java) | Microsoft Azure" 
    description="Zjistěte, jak chcete někomu zavolat z webové stránky pomocí Twilio v aplikaci Java na Azure." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Jak někomu zavolat pomocí Twilio v aplikaci Java na Azure 

Následující příklad ukazuje, jak můžete Twilio uskutečnit hovor z webové stránky hostované v Azure. Výsledné aplikace se výzva k zadání hodnot telefonní hovor, jak je vidět na následující obrazovce snímek.

![Azure volání formuláře pomocí Twilio a Java][twilio_java]

Musíte použít kód v tomto tématu následujícím způsobem:

1. Získání účtu Twilio a ověřování tokenu. Začínáme s Twilio, jsou vyhodnoceny ceny za [http://www.twilio.com/pricing][twilio_pricing]. Můžete se přihlásit na [https://www.twilio.com/try-twilio][try_twilio]. Informace o rozhraní API poskytovanou Twilio najdete v tématu [http://www.twilio.com/api][twilio_api].
2. Získání Twilio SKLENICE. Na [https://github.com/twilio/twilio-java][twilio_java_github], můžete stáhnout GitHub zdrojů a vytvořit vlastní SKLENICE nebo stáhnout předdefinovaných SKLENICE (s nebo bez závislosti).
Kód v tomto tématu napsané pomocí předdefinovaných SKLENICE TwilioJava 3.3.8 s závislosti.
3. SKLENICE dodejte cesty buildu Java.
4. Pokud používáte zatmění vytvořit tuto aplikaci Java, součástí Twilio JAR souboru nasazení aplikace (WAR) pomocí funkce sestavení zatmění na nasazení. Pokud nepoužíváte zatmění vytvořit tuto aplikaci Java, zajistěte Twilio JAR je součástí stejnou Azure roli jako aplikace Java a přidá do cesty třídy aplikace.
5. Ujistěte se, cacerts keystore obsahuje Equifax zabezpečené certifikační autorita certifikátu s MD5 otisku 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (pořadové číslo je 35:DE:F4:CF a otisku SHA1 D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Certifikační autorita (CA) certifikát pro [https://api.twilio.com] [ twilio_api_service] služba, která se nazývá při použití rozhraní API Twilio. Informace o tom, že tento certifikační vaší JDK cacert úložiště najdete v tématu [Přidání certifikátu do úložiště certifikátů CA Java][add_ca_cert].

Kromě toho znalost informací k [Vytvoření Ahoj světa aplikací pomocí nástrojů Azure pro Eclipse][azure_java_eclipse_hello_world], nebo s jinými postupy pro hostování aplikace Java v Azure, pokud nepoužíváte zatmění důrazně doporučujeme.

## <a name="create-a-web-form-for-making-a-call"></a>Vytvoření webového formuláře pro volání

Následující kód znázorňuje vytvoření webového formuláře k načtení uživatelská data týkající se uplatňování hovoru. Pro účely tohoto příkladu vytvoření nového projektu dynamické web s názvem **TwilioCloud**a **callform.jsp** bylo přidáno jako soubor JSP.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Vytvoření kódu pro volání
Následující kód, který se nazývá dokončení uživatele formuláře zobrazeného callform.jsp, vytvoří zprávu volání a vygeneruje hovor. Pro účely v tomto příkladě se soubor JSP se nazývá **makecall.jsp** a přidání **TwilioCloud** projektu. (Použijte svůj účet Twilio a ověřování token místo hodnot zástupný symbol přiřazené **accountSID** a **pomocí** následující kód).

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Kromě volání, zobrazí makecall.jsp koncový bod Twilio, verze rozhraní API a stav hovoru. Příklad je následující snímek obrazovky:

![Azure volání odpovědí pomocí Twilio a Java][twilio_java_response]

## <a name="run-the-application"></a>Spusťte aplikaci
Tady jsou hlavních kroků ke spuštění aplikace; Podrobnosti pro tyto kroky, najdete na [Vytvoření knihovny Ahoj prezentace aplikace pomocí Azure sada nástrojů pro Eclipse][azure_java_eclipse_hello_world].

1. Exportujte TwilioCloud WAR ke složce Azure **approot** . 
2. Úprava **startup.cmd** na Rozbalit TwilioCloud WAR.
3. Kompilace aplikace pro emulátoru výpočetním.
4. Spusťte nasazení ve výpočetním emulátoru.
5. Otevřete prohlížeč a spusťte **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Zadejte hodnoty do formuláře, klikněte na **tento hovor**a potom zobrazte výsledky v makecall.jsp.

Když budete chtít nasazení Azure, překompilujte nasazení do cloudu, nasazení Azure a http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp se spouštějí v prohlížeči (Nahraďte hodnotu pro *your_hosted_name*).

## <a name="next-steps"></a>Další kroky
Tento kód získali zobrazit základní funkce pomocí Twilio v Java na Azure. Než nasadíte Azure ve výrobním, můžete přidat další zpracování chyb nebo jiných funkcí. Příklad:

* Místo použití webového formuláře, můžete použít databázi SQL Azure úložiště objektů BLOB nebo ukládat telefonní čísla a hovoru textem. Informace o používání objektů BLOB Azure úložiště v Java najdete v tématu [používání služba úložiště objektů Blob z Java][howto_blob_storage_java]. Informace o používání databáze SQL v Java, najdete v článku [Použití databáze SQL v Java][howto_sql_azure_java].
* Můžete použít **RoleEnvironment.getConfigurationSettings** k získání ID účtu Twilio a ověřovací token z nastavení konfigurace vašeho nasazení místo pevných kódování hodnoty v makecall.jsp. Informace o třídě **RoleEnvironment** najdete v tématu [použití knihovna Runtime služby Azure v JSP] [ azure_runtime_jsp] a Runtime služby Azure balíčku přečtěte následující dokumentaci [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Kód makecall.jsp přiřadí Twilio-za předpokladu, že adresy URL, [http://twimlets.com/message][twimlet_message_url], proměnné **adresu Url** . Tato adresa URL nabízí Twilio Markup Language (TwiML) odpověď informující Twilio jak pokračovat v hovoru. Například může obsahovat TwiML, je vrácená ** &lt;Přivítejte&gt; ** slovesné, jehož výsledkem text přehrát nahlas volání příjemci. Místo pomocí adresy URL poskytnutá Twilio, může vytvářet vlastní služby pro odpověď na požadavek na Twilio; Další informace najdete v tématu [jak Twilio použití hlasu a funkce služby SMS v Java][howto_twilio_voice_sms_java]. Další informace o TwiML můžete najít na [http://www.twilio.com/docs/api/twiml][twiml]a další informace o ** &lt;Přivítejte&gt; ** a další akce Twilio, najdete na [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Přečíst pokyny pro Twilio zabezpečení na [https://www.twilio.com/docs/security][twilio_docs_security].

Další informace o Twilio najdete v tématu [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Viz taky
* [Jak používat Twilio pro hlasovou a služby SMS možnosti jazyka Java][howto_twilio_voice_sms_java]
* [Přidání certifikátu do úložiště certifikátů CA Java][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
