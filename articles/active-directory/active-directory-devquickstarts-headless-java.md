<properties
    pageTitle="Azure AD Java Začínáme | Microsoft Azure"
    description="Jak vytvářet aplikace Java příkazového řádku, který se přihlašuje uživatelů pro přístup k rozhraní API."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Přístup k rozhraní API s Azure AD pomocí Java příkazového řádku aplikace

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD díky rychle a jednoduše využívající Správa identit webovou aplikaci, poskytuje jediné přihlašovací a odhlašovací s jenom několik řádků kódu.  Ve webových aplikacích Java lze provést pomocí společnosti Microsoft provádění ADAL4J komunity řízené úsilím.

  Tady použijeme ADAL4J na:
- Uživatel se přihlaste do aplikace používají Azure AD zprostředkovatele identit.
- Zobrazte některé informace o uživateli.
- Nejdřív se př uživatele mimo aplikaci.

K tomuto účelu budete muset:

1. Registrace aplikace s Azure AD
2. Nastavení aplikace používat knihovnu ADAL4J.
3. Použijte knihovnu ADAL4J vydat žádosti o přihlášení a odhlašovací Azure AD.
4. Vytiskněte dat o uživateli.

Jak začít, [Stáhněte si aplikaci osnovu](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) nebo [Stáhnout dokončeným ukázkovým](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  Taky musíte tenanta Azure AD ve kterých se registruje aplikace.  Pokud žádný nemáte už, [Přečtěte si, jak si ho založit](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. registrace aplikace s Azure AD
Povolení aplikace pro ověřování uživatelů, Nejdřív musíte zaregistrovat nové aplikace ve vašem klientovi.

- Přihlaste se k portálu Správa Azure.
- V levém navigačním podokně klikněte na **Služby Active Directory**.
- Vyberte klienta, kde se chcete zaregistrovat aplikace.
- Klikněte na kartu **aplikace** a klikněte na Přidat v dolní okraj.
- Postupujte podle pokynů a vytvořte nové **webové aplikace a/nebo WebAPI**.
    - **Název** aplikace bude popisovat aplikace pro koncové uživatele
    - **Přihlašovací adresa URL** je základní adresa URL aplikace.  Osnovu výchozí hodnota je `http://localhost:8080/adal4jsample/`.
    - **Identifikátor URI aplikace** je jedinečný identifikátor aplikace.  Názvů, je použít `https://<tenant-domain>/<app-name>`, například`http://localhost:8080/adal4jsample/`
- Po dokončení zápisu AAD přiřadí aplikace klienta jedinečný identifikátor.  Musíte mít tuto hodnotu v dalších částech tak zkopírujte ho na kartě Konfigurovat.

Jednou na portálu aplikace vytvořte **Aplikace tajná** aplikace a zkopírujte dolů.  Je nutné jej brzy.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. nastavení aplikace pro použití ADAL4J knihovny a požadavcích pomocí Maven
Tady jsme budete konfigurovat ADAL4J použít ověřování protokolem OpenID připojení.  ADAL4J se použijí k problému žádosti o přihlášení a odhlašovací, spravovat relace uživatele a získat informace o uživateli, mimo jiné.

-   V kořenovém adresáři projektu, otevření/vytvoření `pom.xml` a vyhledejte `// TODO: provide dependencies for Maven` a nahraďte následujícím:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. soubor PublicClient java vytvořit

Výše uvedené, použijeme rozhraní API grafu k načtení dat o přihlášený uživatel. Tento postup lze snadno nám by měl vytvoříme souboru představující **Objekt adresáře** a jednotlivých souborů pro **uživatele** tak, aby bylo možné použít vzorek ú Java.

1. Vytvoření souboru s názvem `DirectoryObject.java` který používáme k ukládání základní údaje o všech DirectoryObject (který můžete neváhejte pro účely to později jiné dotazy grafu, můžete provést). Je můžete vyjmout a vložit to z následujícího seznamu:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Kompilaci a spuštění výběru

Změna zase oddálíte kořenového adresáře a spusťte tento příkaz Vytvoření ukázky jenom umístění spolupráce pomocí `maven`. Bude použit `pom.xml` souboru jste sami napsali závislostí.

`$ mvn package`

Nyní byste měli mít `adal4jsample.war` soubor je vaše `/targets` adresář. Můžete nasadit, v kontejneru Tomcat a navštivte adresu URL 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
Je velmi snadné nasazení WAR s nejnovější Tomcat servery. Jednoduše přejít na `http://localhost:8080/manager/` a postupujte podle pokynů na odeslání vašeho "adal4jsample.war" soubor. Bude autodeploy za vás se správný koncový bod.

##<a name="next-steps"></a>Další kroky

Blahopřejeme! Nyní máte pracovní Java aplikace, která má možnost bezpečně ověřování uživatelů, rozhraní API webových pomocí OAuth 2.0 volat a získat základní informace o uživateli.  Pokud jste to ještě neudělali, teď je čas na naplnění vašeho klienta s někteří uživatelé.

Pro informaci dokončeným ukázkovým (bez konfigurace hodnoty) [je k dispozici jako ZIP tady](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), nebo můžete zkopírovat ho GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

