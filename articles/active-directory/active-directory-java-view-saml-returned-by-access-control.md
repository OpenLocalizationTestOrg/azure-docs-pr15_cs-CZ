<properties
    pageTitle="Zobrazení SAML vrácené služba Řízení přístupu (Java)"
    description="Zjistěte, jak zobrazit SAML vrácené služba Řízení přístupu v aplikacích Java hostitelem Azure."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Jak zobrazit SAML vrácená službou Azure přístup ovládacího prvku

Tento průvodce vám ukáže, jak chcete zobrazit podkladovém zabezpečení výraz SAML (Markup Language) vrátíte zpět do aplikace služby Řízení přístupu (ACS) Azure. Průvodce vytvoří na téma [jak ověřování uživatelů webu s Azure přístup ovládací prvek služby pomocí zatmění][] zadáním kódu, který zobrazuje informace SAML. Dokončené aplikace bude vypadat podobně jako tento.

![Příklad SAML výstupu][saml_output]

Další informace o ACS naleznete v části [Další kroky](#next_steps) .

> [AZURE.NOTE]
> Filtr ovládací prvek služby Azure přístup je náhled technologii komunity. Jako předběžnou verzi softwaru není podporována dříve Microsoft.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Dokončete úkoly v této příručce dokončete vzorku v [tom, jak ověřit uživatelům webu s Azure přístup ovládací prvek služby pomocí zatmění][] a ho použít jako výchozí bod pro účely tohoto návodu.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>Přidání knihovnu JspWriter na vaše sestavení cesty a nasazení Tvůrce dotazů

Přidání knihovny obsahující třídu **javax.servlet.jsp.JspWriter** tak, aby vaše sestavení cesty a nasazení Tvůrce dotazů. Pokud používáte Tomcat, knihovna je **jsp api.jar**, která se nachází ve složce **knihovny** Apache.

1. V prvku zatmění Průzkumník projektu klikněte pravým tlačítkem **MyACSHelloWorld**, klikněte na **Vytvořit cestu**, klikněte na **Konfigurovat cesta k vytvoření**, klikněte na kartu **knihovny** a potom klikněte na **Přidat externí JARs**.
2. V dialogovém okně **Výběr JAR** přejděte potřebné SKLENICE, vyberte ho a klepněte na tlačítko **Otevřít**.
3. Dialog **Vlastnosti pro MyACSHelloWorld** pořád otevřený klikněte na **Nasazení sestavení**.
4. V dialogu **Sestavení nasazení Web** klikněte na **Přidat**.
5. V dialogovém okně **Nové sestavení směrnice** klikněte na možnosti **Jazyka Java sestavit cestu položky** a klikněte na tlačítko **Další**.
6. Vyberte odpovídající knihovnu a klikněte na tlačítko **Dokončit**.
7. Klikněte na **OK** zavřete dialogové okno **Vlastnosti MyACSHelloWorld** .

## <a name="modify-the-jsp-file-to-display-saml"></a>Úprava JSP soubor zobrazíte SAML

Úprava **index.jsp** použít následující kód.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>Spusťte aplikaci

1. Spuštění aplikace v počítači emulátoru nebo nasadit Azure pomocí kroků uvedených v [tom, jak ověřit uživatelům webu s Azure přístup ovládací prvek služby pomocí zatmění][].
2. Spusťte prohlížeč a otevření webové aplikace. Po přihlášení do aplikace uvidíte informace o SAML, včetně výraz zabezpečení poskytovanou zprostředkovatele identit.

## <a name="next-steps"></a>Další kroky

Další prozkoumejte ACS na funkce a experimentovat s složitější scénáře, najdete v článku [Access 2.0 služby ovládacího prvku][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Služba řízení přístupu 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Jak ověřit uživatelů webu se službou Azure přístup ovládací prvek prostřednictvím zatmění]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 