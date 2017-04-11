<properties
   pageTitle="Návod, jak vytvořit datové služby pro Marketplace | Microsoft Azure"
   description="Podrobné pokyny, jak vytvářet, certifikace a nasazení datové služby pro nákup na Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="mapping-an-existing-web-service-to-odata-through-csdl"></a>Mapování stávající webové služby na OData pomocí CSDL

>[AZURE.IMPORTANT] **V současné době jsme už rychlého připojení všechny nové vydavatelé datové služby. Nové dataservices nebude získat schváleno seznam.** Pokud máte s aplikací business SaaS chcete publikovat na AppSource můžete najít další informace [v tomto poli](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo vývojář služby rádi byste publikovat na webu Azure Marketplace můžete najít další informace [v tomto poli](https://azure.microsoft.com/marketplace/programs/certified/).

Tento článek obsahuje přehled o tom, jak používat CSDL mapovat stávající služba kompatibilní služby OData. Vysvětluje, jak vytvořit dokument mapování (CSDL), která transformací vstupní žádosti z klienta prostřednictvím služby volání a výstup (data) zpátky ke klientovi prostřednictvím OData kompatibilní kanálu. Společnosti Microsoft Azure Marketplace zpřístupňuje služby koncoví uživatelé pomocí protokolu OData. Služby, které jsou zveřejněné příslušným poskytovatelé obsahu (dat vlastníci) jsou zveřejněným v různých formulářů, například REST, SOAP, atd.

## <a name="what-is-a-csdl-and-its-structure"></a>Co je CSDL a jeho strukturu?
CSDL (Konceptuální schéma Definition Language) je specifikace definování jak popisují webové služby nebo databáze v běžných znění XML k webu Azure Marketplace.

Jednoduchý přehled **požádat o toku:**

  `Client -> Azure Marketplace -> Content Provider’s Web Service (Get, Post, Delete, Put)`

**Tok dat** je v opačném směru:

  `Client <- Azure Marketplace <- Content Provider’s WebService`

**Obrázek 1** diagramy, jak klienta byste měli získat data z obsahu poskytovatele (služba) prostřednictvím webu Azure Marketplace.  CSDL používá komponentu mapování/transformace zpracovat žádosti a data předávat obsahu poskytovatele služeb a klienta.

*Obrázek 1: Podrobné směrování z klienta na poskytovatele obsahu prostřednictvím webu Azure Marketplace*

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-1.png)

Pozadí na Atom Atom Pub a protokolu OData němuž sestavit rozšíření Azure Marketplace, ujistěte se, revize: [http://msdn.microsoft.com/library/ff478141.aspx](http://msdn.microsoft.com/library/ff478141.aspx)

Úryvek z horní odkaz:     *"účel Open Data protocol (dále jen OData) je poskytovat ZBÝVAJÍCÍ protokol pro operace CRUD styl (vytvořit, číst, aktualizovat a odstranit) u zdroje vystavit jako datové služby. "Datové služby" je koncový bod kde není data vystavená z jedné nebo více "kolekcí" každý položkami žádný nebo více"", které se skládají z zadali s názvem dvojice. OData je publikován společnost Microsoft ve skupinovém rámečku standardy OASIS (organizace pro rozvoj z strukturované informace standardy) tak, aby každý uživatel, který chce můžete taky vytvořit servery, klienty nebo nástroje bez poplatků nebo omezení."*

### <a name="three-critical-pieces-that-have-to-be-defined-by-the-csdl-are"></a>Jsou tři zásadní, které mají být definované CSDL:

- **Koncový bod** ze služby poskytovatele webové adresy (identifikátor URI) služby
- Definice parametrů předávané předávat na vstupu poskytovateli služeb **parametrů dat** odesílaného obsahu poskytovatele služby dolů na datový typ.
- **Schéma** dat se vrací žádosti o služby schématu dat doručována službou poskytovatele obsahu, včetně kontejneru, kolekcí/tabulek, proměnné/sloupců a datových typů.

Na následujícím obrázku vidíte přehled tok ze kdy klienta zadá příkazu OData (volání webové službě poskytovatel obsahu) získali výsledky/data zpět.

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-2.png)

### <a name="steps"></a>Pomocí následujících kroků:

1. Klient odešle žádost o prostřednictvím služby volání naplněný souhrnnými vstupní parametry podle XML k webu Azure Marketplace
2. CSDL slouží k ověření volání služby.
    - Formátovaný služby volání potom odesílá obsahu poskytovatelů služby Azure Marketplace
3. Webservice spustí a preforms akce příkaz Http (tedy GET) vrátí data z Azure Marketplace, kde jsou požadovaná data (pokud existuje) zpřístupňuje ve formátu XML k desktopovému klientovi pomocí mapování definované v CSDL.
4. Klient odeslaný data (pokud existuje) ve formátu XML nebo JSON

## <a name="definitions"></a>Definice

### <a name="odata-atom-pub"></a>Pub OData ATOM

Rozšíření chcete ATOM pub, přičemž každá položka představuje jeden řádek výsledku nastavení. Části obsahu položky rozšířeného obsahují hodnoty v řádku – dvojic hodnoty klíče. Další informace najdete tady: [https://www.odata.org/documentation/odata-version-3-0/atom-format/](https://www.odata.org/documentation/odata-version-3-0/atom-format/)

### <a name="csdl---conceptual-schema-definition-language"></a>CSDL – Konceptuální schéma Definition Language

Umožňuje definování funkcí (SPROCs) a osoby, které jsou k dispozici prostřednictvím databáze. Další informace najdete tady: [http://msdn.microsoft.com/library/bb399292.aspx](http://msdn.microsoft.com/library/bb399292.aspx)  

> [AZURE.TIP] Klikněte na rozevírací seznam **jiných verzí** a vybrat verzi, pokud se nezobrazuje v článku.

### <a name="edm---entry-data-model"></a>EDM - položka datového modelu
- Přehled: [http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx][OverviewLink] [OverviewLink]: http://msdn.microsoft.com/library/vstudio/ee382825 (v=vs.100).aspx
- Náhled: [http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx][PreviewLink] [PreviewLink]: http://msdn.microsoft.com/library/aa697428 (v=vs.80).aspx
- Datové typy: [http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx][DataTypesLink] [DataTypesLink]: http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx

Následující ukazuje podrobný zleva doprava tok ze kdy klienta zadá příkazu OData (volání webové službě poskytovatel obsahu) k získání výsledků/data zpět:

  ![Kreslení](media/marketplace-publishing-data-service-creation-odata-mapping/figure-3.png)


## <a name="csdl-basics"></a>Základní informace o CSDL

CSDL (Konceptuální schéma Definition Language) je specifikace definování jak popisují webové služby nebo databáze v běžných znění XML k webu Azure Marketplace. Popisuje důležité CSDL kusů, **udělá předávání dat ze zdroje dat z Azure Marketplace možných.** Tady jsou popsané hlavních částí:

- Informace o rozhraní popisující všechny veřejně dostupných funkcí (FunctionImport uzel)
- Informace o datových typech pro všechny zprávy requests(input) a responses(outputs) zprávy (uzly EntityContainer/entit/typ entity)
- Vazba informace o transport Protocol (protokol) je použit (uzly záhlaví)
- Informace o adrese pro vyhledání určenou službu (BaseURI atribut)

Ve zkratce CSDL představuje smlouva platformy a jazyk nezávislé mezi služby o synchronizaci adresářů a poskytovatele služeb. Použití CSDL, klienta vyhledejte webové databáze služby a služby a spustit žádné veřejně dostupných funkcí.

### <a name="relating-a-csdl-to-a-database-or-a-collection"></a>Spojení CSDL s údaji o databáze nebo kolekci
**Specifikace CSDL**

CSDL je XML gramatiky popisující webové služby. Specifikace samotné je rozdělen 4 hlavní prvky: entit, FunctionImport; Obor názvů a typ entity.

Aby tato odběru srozumitelnější umožňuje importovat CSDL do tabulky.

Mějte na paměti;

  CSDL představuje smlouva platformy a jazyk nezávislé mezi **služby o synchronizaci adresářů** a **poskytovatele služeb**. Použití CSDL, **klienta** můžete vyhledat **webové databáze služby a služby** a spustit žádné z jejích veřejně dostupný **funkcí.**

Pro datové služby čtyři části CSDL můžete považovat z hlediska databáze, tabulky, sloupce a postup úložiště přihlašovacích údajů.

Týkající se takto pro datové služby:

- EntityContainer ~ = databáze
- Entit ~ = tabulky
- Typ entity ~ = sloupců
- FunctionImport ~ = uložená procedura

**Povolené akce protokolu HTTP**
- ZÍSKÁNÍ – vrátí hodnoty z databáze (vrátí kolekci)
- PŘÍSPĚVEK – slouží k předání dat a volitelné návratové hodnoty z databáze (vytvořit novou položku v kolekci zpáteční id/URI)
- ODSTRANĚNÍ – odstranění dat z databáze (odstraní kolekce)
- Vložit – aktualizovat data do databáze (nahradit kolekce nebo vytvořte)

## <a name="metadatamapping-document"></a>Metadata/mapování dokumentu

Metadata/mapování dokumentu se používá k přiřadit existující web služby poskytovatelů obsahu tak, že ho můžete vystavit jako webové služby OData systémem Azure Marketplace. Je založená na CSDL a implementuje několik rozšíření CSDL tak, aby zahrnoval potřeb ZBÝVAJÍCÍ základě webovým službám vystaven prostřednictvím webu Azure Marketplace. Rozšíření se nacházejí v [http://schemas.microsoft.com/dallas/2010/04](http://schemas.microsoft.com/dallas/2010/04) názvů.

Následující příklad CSDL: (kopírování a vkládání příkladu CSDL do editoru XML a změňte podle služby.  Vložte do CSDL mapování kartě DataService při vytváření služby [Azure Marketplace portál publikování](https://publish.windowsazure.com)).

**Podmínky:** Termíny uživatelského rozhraní (PPUI) [Portál publikování](https://publish.windowsazure.com) týkající se CSDL podmínek.
- Nabízet "Název" v PPUI souvisí MyWebOffer
- Společnost, v PPUI souvisí **Zobrazované jméno Publisheru** na [Webu Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) uživatelského rozhraní
- Vaše rozhraní API se vztahuje k webu nebo datové služby (plán v PPUI)

**Hierarchie:** 
 společnost (poskytovatel obsahu) vlastní nabídek, které mají plánů, a to service(s), které zarovnejte s rozhraním API.

### <a name="webservice-csdl-example"></a>Příklad WebService CSDL

Připojení k službě, která je vystavení koncového webové aplikace (třeba C# aplikace)

        <?xml version="1.0" encoding="utf-8"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyWebOffer" Alias="MyOffer" xmlns="http://schemas.microsoft.com/ado/2009/08/edm" xmlns:d="http://schemas.microsoft.com/dallas/2010/04" >
        <!-- EntityContainer groups all the web service calls together into a single offering. Every web service call has a FunctionImport definition. -->
          <EntityContainer Name="MyOffer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
        @Name is used as reference by FunctionImport @EntitySet. And is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition near the bottom of this file. -->
            <EntitySet Name="MyEntities" EntityType="MyOffer.MyEntityType" />
        <!-- Add a FunctionImport for every service method. Multiple FunctionImports can share a single return type (EntityType). -->
        <!-- ReturnType is either Raw() for a stream or Collection() for an Atom feed. Ex. of Raw: ReturnType=”Raw(text/plain)” -->
        <!—EntitySet is the entityset defined above, and is needed if ReturnType is not Raw -->
        <!-- BaseURI attribute defines the service call, replace & with the encode value (&amp;).
        In the input name value pairs {param} represents passed in value.
        Or the value can be hard coded as with AccountKey. -->
        <!-- AllowedHttpMethods optional (default = “GET”), allows the CSDL to specifically specify the verb of the service, “Get”, “Post”, “Put”, or “Delete”. -->
        <!-- EncodeParameterValues, True encodes the parameter values, false does not. -->
        <!-- BaseURI is translated into an URITemplate which defines how the web service call is exposed to marketplace customers.
        Ex. https://api.datamarket.azure.com/mycompany/MyOfferPlan?name={name}
        BaseURI is XML encoded, the {...} point to the parameters defined below.
        Marketplace will read the parameters from this URITemplate and fill the values into the corresponding parameters of the BaseUri or RequestBody (below) during calls to your service.  
        It is okay for @d:BaseUri to include information only for Marketplace consumption, it will not be exposed to end users. i.e. the hardcoded AccountKey in the above BaseURI does not show up in the client facing URITemplate. -->
            <FunctionImport Name="MyWebServiceMethod"
                            EntitySet="MyEntities"
                            ReturnType="Collection(MyOffer.MyEntityType)"
        d:AllowedHttpMethods="GET"
        d:EncodeParameterValues="true"
        d:BaseUri="http://services.organization.net/MyServicePath?name={name}&amp;AccountKey=22AC643">
        <!-- Definition of the RequestBody is only required for HTTP POST requests and is optional for HTTP GET requests. -->
        <d:RequestBody d:httpMethod="POST">
                <!-- Use {} for placeholders to insert parameters. -->
                <!-- This example uses SOAP formatting, but any POST body can be used. -->
            <!-- This example shows how to pass userid and password via the header -->
                <![CDATA[<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:MyOffer="http://services.organization.net/MyServicePath">
                  <soapenv:Header/>
                  <soapenv:Body>
                    <MyOffer:ws_MyWebServiceMethod>
                      <myWebServiceMethodRequest>
                        <!--This information is not exposed to end users. -->
                        <UserId>userid</UserId>
                        <Password>password</Password>
                        <!-- {name} is replaced with the value read from @d:UriTemplate above -->
                        <Name>{name}</Name>
                        <!-- Parameters can be used more than once and are not limited to appearing as the value of an element -->
                        <CustomField Name="{name}" />
                        <MyField>Static content</MyField>
                      </myWebServiceMethodRequest>
                    </MyOffer:ws_MyWebServiceMethod>
                  </soapenv:Body>
                </soapenv:Envelope>      
              ]]>
        </d:RequestBody>
        <!-- Title, Rights and Description are optional and used to specify values to insert into the ATOM feed returned to the end user.  You can specify the element to contain a fixed message by providing a value for the element (this is the default value).  @d:Map is an XPath expression that points into the response returned by your service and is optional.  -->
        <d:Title d:Map="/MyResponse/Title">Default title.</d:Title>
        <d:Rights>© My copyright. This is a fixed response. It is okay to also add a d:Map attribute to override this text.</d:Rights>
        <d:Description d:Map="/MyResponse/Description"></d:Description>
        <d:Namespaces>
        <d:Namespace d:Prefix="p"  d:Uri="http://schemas.organization.net/2010/04/myNamespace" />
        <d:Namespace d:Prefix="p2" d:Uri="http://schemas.organization.net/2010/04/MyNamespace2" />
        </d:Namespaces>
        <!-- Parameters of the web service call:
        @Name should match exactly (case sensitive) the {…} placeholders in the @d:BaseUri, @d:UriTemplate, and d:RequestBody, i.e. “name” parameter in above BaseURI.
        @Mode is always "In", compatibility with CSDL
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx
        @d:Nullable indicates whether the parameter is required.
        @d:Regex - optional, attribute to describe the string, limiting unwanted input at the entry of the system
        @d:Description - optional, is used by Service Explorer as help information
        @d:SampleValues - optional, is used by Service Explorer as help information. Multiple Sample values are separated by '|', e.g. "804735132|234534224|23409823234"
        @d:Enum - optional for string type. Contains an enumeration of possible values separated by a '|', e.g. d:enum="english|metric|raw". Will be converted in a dropdown list in the Service Explorer.
        -->
        <Parameter name="name" Mode="In" Type="String" d:Nullable="false" d:Regex="^[a-zA-Z]*$" d:Description="A name that cannot contain any spaces or non-alpha non-English characters"
        d:Enum="George|John|Thomas|James"
        d:SampleValues="George"/>
        <Parameter Name=" AccountKey" Mode="In" Type="String" d:Nullable="false" />

        <!-- d:ErrorHandling is an optional element. Use it define standardized errors by evaluating the service response. -->
        <d:ErrorHandling>
        <!-- Any number of d:Condition elements are allowed, they are evaluated in the order listed.
        @d:Match is an Xpath query on the service response, it should return true or false where true indicates an error.
        @d:httpStatusCode is the error code to return if an response matches the error.
        @d:errorMessage is the user friendly message to return when an error occurs.
        -->
        <d:Condition d:Match="/Result/ErrorMessage[text()='Invalid token']" d:HttpStatusCode="403" d:ErrorMessage="User cannot connect to the service." />
        </d:ErrorHandling>
           </FunctionImport>

            <!-- The EntityContainer defines the output data schema -->
        </EntityContainer>
        <!-- The EntityType @d:Map defines the repeating node (an XPath query) in the response (output data schema). -->
        <!-- If these nodes are outside a namespace, add the prefix in the xpath. -->
        <!--
        @Name - define your user readable name, will become an XML element in the ATOM feed, so comply with the XML element naming restrictions (no spaces or other illegal characters).
        @Type is the EDM.SimpleType of the parameter, see http://msdn.microsoft.com/library/bb399548(v=VS.100).aspx.
        @d:Map uses an Xpath query to point at the location to extract the content from your services response.
        The "." is relative to the repeating node in the EntityType @d:Map Xpath expression.
        -->
            <EntityType Name="MyEntityType" d:Map="/MyResponse/MyEntities">
        <Property Name="ID" d:IsPrimaryKey="True" Type="Int32"  Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="Amount" Type="Double"   Nullable="false" d:Map="./Remaining[@Amount]"/>
        <Property Name="City"   Type="String"   Nullable="false" d:Map="./City"/>
        <Property Name="State"  Type="String"   Nullable="false" d:Map="./State"/>
        <Property Name="Zip"    Type="Int32"    Nullable="false" d:Map="./Zip"/>
        <Property Name="Updated"    Type="DateTime" Nullable="false" d:Map="./Updated"/>
        <Property Name="AdditionalInfo" Type="String" Nullable="true"
        d:Map="./Info/More[1]"/>
            </EntityType>
        </Schema>

> [AZURE.TIP] Podívejte se na další příklady CSDL webové služby v článku [Příklady mapování stávající webové služby k protokolu OData pomocí CSDLs](marketplace-publishing-data-service-creation-odata-mapping-examples.md)

###<a name="dataservice-csdl-example"></a>Příklad DataService CSDL

Připojí k služba, která je vystavení tabulky nebo zobrazení databáze jako koncový bod příkladu zobrazuje že dvě rozhraní API pro Data základní závislosti rozhraní API CSDL (můžete použít zobrazení, spíše než tabulek).

        <?xml version="1.0"?>
        <!-- The namespace attribute below is used by our system to generate C#. You can change “MyCompany.MyOffer” to something that makes sense for you, but change “MyOffer” consistently throughout the document. -->
        <Schema Namespace="MyCompany.MyDataOffer" Alias="MyOffer" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ado/2009/08/edm">
        <!-- EntityContainer groups all the data service calls together into a single offering. Every web service call has a FunctionImport definition. -->
        <EntityContainer Name="MyOfferContainer">
        <!-- EntitySet is defined for CSDL compatibility reasons, not required for ReturnType=”Raw”
            Think of the EntitySet as a Service
        @Name is used in the customer facing UI as name of the Service.
        @EntityType is used to point at the type definition (returned set of table columns). -->
        <EntitySet Name="CompanyInfoEntitySet" EntityType="MyOffer.CompanyInfo" />
        <EntitySet Name="ProductInfoEntitySet" EntityType="MyOffer.ProductInfo" />
        </EntityContainer>
        <!-- EntityType defines result (output); the table (or view) and columns to be returned by the data service.)
            Map is the schema.tabel or schema.view
            dals.TableName is the table Name
            Name is the name identifier for the EntityType and the Name of the service exposed to the client via the UI.
            dals:IsExposed determines if the table schema is exposed (generally true).
            dals:IsView (optional) true if this is based on a view rather than a table
            dals:TableSchema is the schema name of the table/view
        -->
        <EntityType
        Map="[dbo].[CompanyInfo]"
        dals:TableName="CompanyInfo"
        Name="CompanyInfo"
        dals:IsExposed="true"
        dals:IsView="false"
        dals:TableSchema="dbo"
        xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <!-- Property defines the column properties and the output of the service.
            dals:ColumnName is the name of the column in the table /view.
            Type is the emd.SimpleType
            Nullable determines if NULL is a valid output value
            dals.CharMaxLenght is the maximum length of the output value
            Name is the name of the Property and is exposed to the client facing UI
            dals:IsReturned is the Boolean that determines if the Service exposes this value to the client.
            IsQueryable is the Boolean that determines if the column can be used in a database query
            (For data Services: To improve Performance make sure that columns marked ISQueryable=”true” are in an index.)
            dals:OrdinalPosition is the numerical position x in the table or the View, where x is from 1 to the number of columns in the table.
            dals:DatabaseDataType is the data type of the column in the database, i.e. SQL data type dals:IsPrimaryKey indicates if the column is the Primary key in the table/view.  (The columns marked ISPrimaryKey are used in the Order by clause when returning data.)
        -->
        <Property dals:ColumnName="data" Type="String" Nullable="true" dals:CharMaxLength="-1" Name="data" dals:IsReturned="true" dals:IsQueryable="false" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="ticker" Type="String" Nullable="true" dals:CharMaxLength="10" Name="ticker" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        <EntityType Map="[dbo].[ProductInfo]" dals:TableName="ProductInfo" Name="ProductInfo" dals:IsExposed="true" dals:IsView="false" dals:TableSchema="dbo" xmlns:dals="http://schemas.microsoft.com/dallas/2010/04">
        <Property dals:ColumnName="companyid" Type="Int32" Nullable="true" Name="companyid" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="2" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="id" Type="Int32" Nullable="false" Name="id" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="true" dals:OrdinalPosition="1" dals:NumericPrecision="10" dals:DatabaseDataType="int" />
        <Property dals:ColumnName="product" Type="String" Nullable="true" dals:CharMaxLength="50" Name="product" dals:IsReturned="true" dals:IsQueryable="true" dals:IsPrimaryKey="false" dals:OrdinalPosition="3" dals:DatabaseDataType="nvarchar" />
        </EntityType>
        </Schema>

## <a name="see-also"></a>Viz taky
- Pokud vás zajímají výukové Principy konkrétní uzly a jejich parametry v tomto článku [Datových služeb OData mapování uzlů](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) definice a vysvětlivky, příklady a použít případu kontext.
- Pokud vás zajímají revizí příkladů naleznete v tomto článku jsou [Data Service OData mapování příklady](marketplace-publishing-data-service-creation-odata-mapping-examples.md) najdete v článku ukázkový kód a interpretaci syntaxi kódu a kontext.
- Pokud chcete vrátit do předepsaném cesta publikování datové služby Azure Marketplace, v tomto článku [Data služby publikování Guide](marketplace-publishing-data-service-creation.md).
