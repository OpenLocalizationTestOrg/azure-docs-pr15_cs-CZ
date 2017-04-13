<properties 
    pageTitle="Store-sendgrid-Java-How-to-send-email-example" 
    description="Jak odeslat e-mailu pomocí SendGrid z Java v Azure nasazení" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Jak odeslat e-mailu pomocí SendGrid z Java v Azure nasazení

Následující příklad ukazuje, jak můžete pomocí SendGrid posílat e-maily z webové stránky hostované v Azure. Výsledné aplikace se výzva k zadání hodnot e-mailu, jak je vidět na následující obrazovce snímek.

![Formulář e-mailu][emailform]

Výsledný e-mailu bude vypadat podobně jako na následující snímek obrazovky.

![E-mailové zprávy][emailsent]

Musíte použít kód v tomto tématu následujícím způsobem:

1. Získáte sklenic po javax.mail g, například z <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Přidejte sklenic po g cesty buildu Java.
3. Pokud používáte zatmění vytvořit tuto aplikaci Java, můžete zahrnout knihoven SendGrid v souboru nasazení aplikace (WAR) pomocí funkce sestavení zatmění na nasazení. Pokud nepoužíváte zatmění vytvořit tuto aplikaci Java, zkontrolujte knihoven jsou zahrnuty ve stejné Azure roli jako aplikace Java a přidá do cesty třídy aplikace.


Můžete taky může mít vlastní SendGrid uživatelské jméno a heslo, abyste mohli poslat e-mailu. Začínáme s SendGrid, najdete v článku [jak odeslat e-mailu pomocí SendGrid z Java](store-sendgrid-java-how-to-send-email.md).

Kromě toho znalosti informace k [Vytvoření Ahoj prezentace aplikace pro Azure v zatmění](http://msdn.microsoft.com/library/windowsazure/hh690944), nebo společně s jinými postupy pro hostování aplikace Java v Azure, pokud nepoužíváte zatmění, důrazně doporučujeme.

## <a name="create-a-web-form-for-sending-email"></a>Vytvoření webového formuláře pro odeslání e-mailu

Následující kód znázorňuje vytvoření webového formuláře k načtení dat uživatele k odesílání e-mailu. Pro účely tohoto obsahu souboru JSP názvem **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>Vytvořit kód pro odeslání e-mailu

Následující kód, který se nazývá po dokončení na formulář v emailform.jsp, vytvoří e-mailu a odešle ji. Pro účely tohoto obsahu souboru JSP názvem **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Kromě odesílání e-mailu, emailform.jsp poskytuje výsledku pro uživatele. Příklad je následující snímek obrazovky:

![Odeslání hromadné výsledek][emailresult]

## <a name="next-steps"></a>Další kroky

Nasazení aplikace emulátoru výpočetním prohlížeče spustit emailform.jsp, zadejte hodnoty do formuláře, klikněte na **Odeslat e-mailu**a potom zobrazte výsledky v sendemail.jsp.

Tento kód získali předvedení použití SendGrid v Java na Azure. Než nasadíte Azure ve výrobním, můžete přidat další zpracování chyb nebo jiných funkcí. Příklad: 

* Můžete použít Azure úložiště objektů BLOB nebo SQL databázi k ukládání e-mailové adresy a e-mailové zprávy, místo pomocí webového formuláře. Informace o používání objektů BLOB Azure úložiště v Java najdete v tématu [používání služba úložiště objektů Blob z Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Informace o používání databáze SQL v Java najdete v článku [Použití databáze SQL v Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Můžete použít `RoleEnvironment.getConfigurationSettings` k načtení SendGrid uživatelské jméno a heslo z nastavení konfigurace vašeho nasazení, namísto načítat jsou tyto hodnoty pomocí webového formuláře. Informace o `RoleEnvironment` třídy najdete v tématu [použití knihovna Runtime služby Azure v JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) a Runtime služby Azure balíčku přečtěte následující dokumentaci <http://dl.windowsazure.com/javadoc>.
* Další informace o používání SendGrid v Java najdete v článku [jak odeslat e-mailu pomocí SendGrid z Java](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
