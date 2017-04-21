
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Příklad konfiguračního souboru pro zabezpečení řetězec připojení


Je jeví jako nesprávná umístění připojovací řetězec jako literálů v kódu C#. Je lepší umístění připojovací řetězec v souboru konfigurace. Zde můžete upravit řetězec bez nutnosti překompilovat.

Předpokládejme zkompilované C# aplikace se nazývá **ConsoleApplication1.exe**a že tento .exe umístěná v **bin\debug\* * adresář.

V tomto příkladu jsou uloženy většina díly připojovací řetězec v souboru konfigurace s názvem přesně **ConsoleApplication1.exe.config**. Tento soubor konfigurace rovněž musí nacházet v **bin\debug\**.

Ve formátu XML následující konfiguračního souboru zobrazí připojovací řetězec s názvem **ConnectionString4NoUserIDNoPassword**. Kód C# vyhledá tento řetězec.

Je třeba upravit reálnou názvy v zástupný text:

- {your_serverName_here}
- {your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



Pro tento obrázek zvolili jsme vynechat dvěma parametry:

- ID uživatele = {your_userName_here};
- Heslo = {your_password_here};


Můžete je zahrnout, ale v některých případech je lepší aplikace získání jsou tyto hodnoty z klávesnice vstupní uživatelem. To záleží.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
