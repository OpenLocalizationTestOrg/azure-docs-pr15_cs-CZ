<properties
    pageTitle="Vytvoření a nasazení aplikace Java rozhraní API aplikace služby Azure"
    description="Informace o vytvoření balíčku aplikace Java API a nasazení služby Azure aplikace."
    services="app-service\api"
    documentationCenter="java"
    authors="bradygaster"
    manager="mohisri"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="get-started-article"
    ms.date="08/31/2016"
    ms.author="rachelap"/>

# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Vytvoření a nasazení aplikace Java rozhraní API aplikace služby Azure

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Tento kurz ukazuje, jak vytvořit aplikaci Java a nasadit na Azure aplikace služby rozhraní API aplikace pomocí [Libovolná]. Pokyny v tomto kurzu může být zahájen v operačním systému, který může spouštění jazyka Java. Kód v tomto kurzu se vytvořené pomocí [Maven]. [Jax RS] slouží k vytvoření RESTful služby a vygeneruje se podle [Swagger] metadat specifikaci [Swagger Editor].

## <a name="prerequisites"></a>Zjistit předpoklady pro

1. [Vývojář Java Kit 8] \(nebo novější)
1. [Maven] nainstalované v počítači vývoj
1. [Libovolná] nainstalované v počítači vývoj
1. Placené nebo [bezplatné zkušební] předplatné [Microsoft Azure]
1. Nastavit informace HTTP test poznamenat do [pošťák]

## <a name="scaffold-the-api-using-swaggerio"></a>Scaffold rozhraní API pomocí Swagger.IO

V editoru swagger.io online, můžete zadat kód Swagger JSON nebo YAML představující strukturu vaší rozhraní API. Až budete mít rozhraní API plochy navržený, můžete exportovat kód pro různé platformy a rámce. V následující části kód scaffolded upraví zahrnout mock funkce. 

Tato ukázka bude začínat Swagger JSON textu, která vložíte do editoru swagger.io, které bude použito k vygenerování kódu pro využívání JAX-r pro přístup k rozhraní REST API koncový bod. Pak budete upravovat scaffolded kód, který chcete vrátit mock data, simulace rozhraní REST API integrované nad mechanismus trvalé data.  

1. Zkopírujte následující kód Swagger JSON do schránky:

        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }

1. Přejděte do [Online Swagger Editor]. Jednou, klikněte na položku **Soubor -> Vložit JSON** .

    ![Položka nabídky Vložit JSON][paste-json]

1. Vložte kontaktů seznamu rozhraní API Swagger JSON kterou jste si zkopírovali. 

    ![Vložení kódu JSON do Swagger][pasted-swagger]

1. Zobrazte si přečtěte následující dokumentaci stránky a rozhraní API souhrn vykreslovat v editoru. 

    ![Zobrazení Swagger generovaného dokumenty][view-swagger-generated-docs]

1. Výběrem možnosti nabídky **JAX-r-> Generovat serveru** scaffold kód serverovou budete později upravovat přidáte mock implementaci. 

    ![Generovat kód položka nabídky][generate-code-menu-item]

    Po vytvoření kód je budete zadali ZIP soubor ke stažení. Tento soubor obsahuje kód generované uživatelské rozhraní generátor kódu Swagger a všechny přidružené vytvořte skriptů. Rozbalení knihovně celý adresář k počítači, vývoj. 

## <a name="edit-the-code-to-add-api-implementation"></a>Upravit kód přidáte implementace rozhraní API

V této části nahradíte kód generovaný Swagger serverovou implementaci pomocí vlastního kódu. Nový kód vrátí entity ArrayList kontaktu volajícího klientovi. 

1. Otevřete soubor modelu *Contact.java* , která se nachází ve složce *src/gen/java / / v/swagger/modelu* pomocí [Aplikace Visual Studio kód] nebo Oblíbené textového editoru. 

    ![Soubor otevřít kontaktů modelu][open-contact-model-file]

1. Přidejte následující konstruktor třídy **kontaktu** . 

        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }

1. Otevření souboru *ContactsApiServiceImpl.java* služby implementaci, který se nachází ve složce *src/hlavní/java / / v/swagger/rozhraní api/impl* pomocí [Aplikace Visual Studio kód] nebo Oblíbené textovém editoru.

    ![Soubor s kódem otevřít služby kontakt][open-contact-service-code-file]

1. Kód v souboru přepsat tento nový kód mock implementaci přidáte kód služby. 

        package io.swagger.api.impl;

        import io.swagger.api.*;
        import io.swagger.model.*;
        import com.sun.jersey.multipart.FormDataParam;
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
        import java.io.InputStream;
        import com.sun.jersey.core.header.FormDataContentDisposition;
        import com.sun.jersey.multipart.FormDataParam;
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;

        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
  
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
  
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
  
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
            
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }

1. Otevřete příkazový řádek a změňte adresáře do kořenové složky aplikace.

1. Spusťte následující příkaz Maven Sestavte kód a spusťte ho z app serveru molo místně. 

        mvn package jetty:run

1. Měli byste vidět okna příkazového odráží, že molo spustila kódu port 8080. 

    ![Soubor s kódem otevřít služby kontakt][run-jetty-war]

1. Použijte [pošťák] tak, aby žádost o "zobrazí všechny kontakty" rozhraní API metodu http://localhost:8080/rozhraní api/kontakty.

    ![Volat kontakty rozhraní API][calling-contacts-api]

1. Použijte [pošťák] , aby žádost o metodě "získat určitým kontaktem" rozhraní API c:\ http://localhost:8080/rozhraní api/kontaktů/2.

    ![Volat kontakty rozhraní API][calling-specific-contact-api]

1. Nakonec vytvořte soubor Java WAR (webový archiv) spuštěním následujícího příkazu Maven v konzole. 

        mvn package war:war

1. Vytvořenou souboru WAR umístit do **cílové** složky. Přejděte do **cílové** složky a přejmenujte soubor WAR **ROOT.war**. (Zkontrolujte, že tento formát odpovídá psaní velkých písmen).

         rename swagger-jaxrs-server-1.0.0.war ROOT.war

1. Nakonec provádět následující příkazy z kořenové složky aplikace vytvořte složku **nasazení** nasazení souboru WAR Azure pomocí. 

         mkdir deploy
         mkdir deploy\webapps
         copy target\ROOT.war deploy\webapps
         cd deploy

## <a name="publish-the-output-to-azure-app-service"></a>Publikovat výstup aplikace služby Azure

V této části budete Naučte se vytvářet nové rozhraní API aplikace na portálu Azure, Příprava tuto aplikaci rozhraní API pro hostování aplikace Java a nasazení nově vytvořený soubor WAR aplikace služby Azure spusťte novou aplikaci rozhraní API. 

1. Vytvoření nové aplikace rozhraní API [Azure portál]kliknutím na položku **nový Web -> + Mobile -> rozhraní API aplikace** , zadáním podrobnosti aplikace a potom klikněte na **vytvořit**.

    ![Vytvoření nového rozhraní API aplikace][create-api-app]

1. Po vytvoření aplikace pro rozhraní API otevřete zásuvné **Nastavení** vaše aplikace a potom klikněte na položku **Nastavení aplikace** . Vyberte nejnovější verze Java v dostupných možnostech vyberte nejnovější Tomcat v nabídce **Web kontejneru** a potom klikněte na **Uložit**.

    ![Nastavení jazyka Java v zásuvné rozhraní API aplikace][set-up-java]

1. Klikněte na položku nastavení **nasazení přihlašovací údaje** a zadejte uživatelské jméno a heslo, které chcete použít pro publikování souborů do rozhraní API aplikace. 

    ![Nasazení nastavit přihlašovací údaje][deployment-credentials]

1. Klikněte na položku nastavení **nasazení zdroje** . Jednou, klikněte na tlačítko **Zvolit zdroj** , vyberte možnost **Místní úložiště libovolná** a klikněte na **OK**. Tím vytvoříte úložišti libovolná spuštěné v Azure, obsahující přidružení pomocí rozhraní API aplikace. Pokaždé, když Potvrdit kód *předlohy* větví libovolná úložiště, kód bude publikovaných do svého živou rozhraní API aplikace instanci. 

    ![Nastavení nové místní úložiště libovolná][select-git-repo]

1. Zkopírujte adresu URL nového libovolná úložiště do schránky. Uložte jako bude ve chvíli důležité. 

    ![Nastavení nové úložiště libovolná aplikace][copy-git-repo-url]

1. Libovolná nabízená soubor WAR do online úložiště. K tomuto účelu přejděte do složky **nasazení** , kterou jste vytvořili dříve, takže můžete snadno Potvrdit kód do úložiště v aplikaci služby. Jakmile budete v okně konzoly a přešli do složky, kde se nachází složka webapps, zadejte následující příkazy libovolná spuštění procesu a aktivováno vypnout na nasazení. 

         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master

    Po vystavení požadavku **nabízených** budete požádáni pro heslo, které jste vytvořili pro nasazení přihlašovací údaje. Když zadáte svoje přihlašovací údaje, byste měli vidět portálu zobrazení, aktualizace nasazené.

1. Pokud používáte ještě jednou pošťák strefíte nově nasazený rozhraní API aplikace spuštěné v aplikaci služby Azure, uvidíte, že chování konzistentní a teď ho podle očekávání vrací o kontaktech, a pomocí jednoduchý kód změny Swagger.io generované uživatelské rozhraní kódu jazyka Java. 

    ![Použití jazyka Java rozhraní REST API kontakty live v Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a>Další kroky

V tomto článku jste mohli začít se souborem Swagger JSON a některé generované uživatelské rozhraní kódu jazyka Java získané editoru Swagger.io. V ní jednoduchých úprav a libovolná nasazení proces výsledkem s funkční rozhraní API aplikace napsané v Java. Další výuková zobrazuje jak [používat rozhraní API aplikace klientů JavaScript pomocí CORS][App Service API CORS]. Vyšší kurzy v řadě ukazují, jak implementovat a tak mohli ověřovat.

Pokud chcete vytvořit v tomto příkladu, další informace o [Úložiště SDK jazyka Java] uchovávat objektů BLOB JSON. Nebo můžete použít v [Dokumentu DB Java SDK] uložit informace o kontaktech do dokumentu DB Azure. 

Další informace o používání jazyka Java v Azure naleznete v článku [Středisko pro vývojáře Java].

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure portálu]: https://portal.azure.com/
[Dokumentace databáze Java SDK]: ../documentdb/documentdb-java-application.md
[Bezplatná zkušební verze]: https://azure.microsoft.com/pricing/free-trial/
[Libovolná]: http://www.git-scm.com/
[Středisko pro vývojáře Java]: /develop/java/
[Vývojář Java Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax r]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger Editor]: http://editor.swagger.io/
[Pošťák]: https://www.getpostman.com/
[Úložiště SDK jazyka Java]: ../storage/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger Editor]: http://editor.swagger.io/
[Kód Visual Studio]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
