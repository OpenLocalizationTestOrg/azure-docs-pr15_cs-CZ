<properties 
   pageTitle="Začínáme s úložiště jezera dat pomocí rozhraní REST API | Microsoft Azure" 
   description="Použití WebHDFS REST API provádět operace s jezera úložiště dat" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Začínáme s úložiště jezera dat Azure pomocí rozhraní REST API

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [Prostředí PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [ROZHRANÍ REST API](data-lake-store-get-started-rest-api.md)
- [Azure rozhraní příkazového řádku](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

V tomto článku se dozvíte používání dat jezera úložiště REST API a WebHDFS REST API provádět správu účtů, jakož i na úložiště jezera dat Azure samostatných systému souborů. Úložiště jezera dat Azure zpřístupňuje vlastní REST API pro operace správy účtu. A proto úložiště jezera dat je kompatibilní s HDFS a Hadoop ekosystému podporuje však pomocí WebHDFS REST API pro operace systému souborů.

>[AZURE.NOTE] Podrobné informace o rozhraní REST API Podpora úložiště jezera dat, přečtěte si článek Principy [Azure dat jezera úložiště REST API](https://msdn.microsoft.com/library/mt693424.aspx).

## <a name="prerequisites"></a>Zjistit předpoklady pro

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

- **Vytvoření aplikace služby Azure Active Directory**. Ověření dat jezera úložiště aplikaci Azure AD pomocí aplikace Azure AD. Způsoby různých ověření s Azure AD, které jsou **ověření koncových uživatelů** nebo **služeb**. Pokyny a další informace o tom, jak ověřit najdete v tématu [Ověřit s úložiště jezera dat pomocí služby Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

- [otáčení](http://curl.haxx.se/). Tento článek ukazuje, jak lze volat rozhraní REST API účtu úložiště jezera dat pomocí otočení.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak můžu ověřit, pomocí služby Azure Active Directory?

Ověření pomocí služby Azure Active Directory můžete dvěma způsoby.

### <a name="end-user-authentication-interactive"></a>Ověřování koncových uživatelů (interaktivní)

V tomto scénáři aplikace zobrazí výzvu k přihlášení a všechny operace provádí v souvislosti s uživateli. Proveďte následující kroky pro interaktivní ověřování.

1. Až aplikaci přesměrování uživatele na následující adresu URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<Identifikátor URI PŘESMĚROVÁNÍ > musí být zakódovaný pro použití v adrese URL. Ano, https://localhost, dosáhnete `https%3A%2F%2Flocalhost`)

    Pro účely tohoto kurzu můžete nahradit hodnoty zástupný symbol v adrese URL nad a vložit ho do panelu Adresa prohlížeči. Budete přesměrováni ověření pomocí Azure přihlašovací jméno. Jakmile se úspěšně přihlásit, zobrazí se v adresním řádku prohlížeče odpověď. Odpověď na bude mít v tomto formátu:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Získání kódu se tak mohli ověřovat z odpovědi. Tento kurz zkopírujte kód pro ověření z panelu Adresa v prohlížeči a předávání v pozvánce na příspěvek tokenu koncový bod, jak je ukázáno v následujícím příkladu:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] V tomto případě \<PŘESMĚROVÁNÍ URI > nemusí kódování.

3. Odpověď na je JSON objekt, který obsahuje přístupový token (například `"access_token": "<ACCESS_TOKEN>"`) a token aktualizace (například `"refresh_token": "<REFRESH_TOKEN>"`). Vaše aplikace používá přístupový token při přístupu k úložiště jezera dat Azure a aktualizace token získat další přístupový token, když skončí platnost přístupový token.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Když skončí platnost přístupový token, se požádat o nové přístupový token pomocí tokenu aktualizace jak je ukázáno v následujícím příkladu:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Další informace o ověřování uživatelů interaktivní najdete v článku [Povolení kód udělit toku](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Služba Služba ověřování (interaktivním)

V tomto scénáři je aplikace obsahuje vlastní přihlašovací údaje k provádění operací. V takovém případě vydá žádost příspěvek jako takové, jaké vidíte níže. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Výstup tohoto požadavku bude obsahovat token se tak mohli ověřovat (symbolem `access-token` do výstupu níže), který bude následně předejte s rozhraní REST API volání. Uložte tento token ověřování z textového souboru; budete potřebovat, dál v tomto článku.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Tento článek používá **interaktivním** přístup. Další informace o interaktivním (volání služby služby) najdete v článku [služby pro volání služeb pomocí přihlašovacích údajů](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Vytvoření účtu úložiště jezera dat

Tuto operaci je založená na rozhraní REST API volání definované [tady](https://msdn.microsoft.com/library/mt694078.aspx).

Pomocí následujícího příkazu otočení. Nahrazení ** \<yourstorename >** s názvem vaší jezera úložiště.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

V příkazu výše nahraďte \< `REDACTED` \> tokenu se tak mohli ověřovat načítáte dříve. Žádost o datové části pro tento příkaz je součástí **input.json** soubor, který je k dispozici pro `-d` výše uvedených parametrů. Obsah souboru input.json vypadat takto:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Vytvoření složek v úložišti jezera účtu

Tuto operaci je založená na rozhraní REST API WebHDFS volání definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Pomocí následujícího příkazu otočení. Nahrazení ** \<yourstorename >** s názvem vaší jezera úložiště.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

V příkazu výše nahraďte \< `REDACTED` \> tokenu se tak mohli ověřovat načítáte dříve. Tento příkaz vytvoří složku nazvanou **mytempdir** kořenové složce účtu jezera úložiště.

Pokud úspěšném byste měli vidět odpověď takto:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Seznam složek v úložišti jezera účtu

Tuto operaci je založená na rozhraní REST API WebHDFS volání definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Pomocí následujícího příkazu otočení. Nahrazení ** \<yourstorename >** s názvem vaší jezera úložiště.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

V příkazu výše nahraďte \< `REDACTED` \> tokenu se tak mohli ověřovat načítáte dříve.

Pokud úspěšném byste měli vidět odpověď takto:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Odeslání dat do účtu úložiště jezera dat

Tuto operaci je založená na rozhraní REST API WebHDFS volání definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Odeslání dat pomocí rozhraní REST API WebHDFS je ve dvou krocích, jak je vysvětleno dále.

1. Odešlete žádost HTTP umístění bez odeslání souboru data, která chcete nahrát. V následující příkaz nahraďte ** \<yourstorename >** s názvem vaší jezera úložiště.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Výstup pro tento příkaz bude mít obsahovat dočasné přesměrování adresy URL, třeba následujícím příkladu.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. Nyní musíte podat jiné žádost HTTP umístění proti adresa URL uvedená pro vlastnost **umístění** v odpovědi. Nahrazení ** \<yourstorename >** s názvem vaší jezera úložiště.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    Výstup bude vypadat podobně jako takto:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Načtení dat z účtu úložiště jezera dat

Tuto operaci je založená na rozhraní REST API WebHDFS volání definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Čtení dat z úložiště jezera dat účet je proces se dvěma kroky.

* Nejdřív odeslat požadavek GET proti koncový bod `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Vrátí umístění pro odeslání další požadavek GET.
* Potom odeslat požadavek GET proti koncový bod `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Zobrazí se obsah souboru.

Avšak, protože neexistuje žádný rozdíl vstupních parametrů mezi prvním a druhým krokem, můžete použít `-L` parametr zadejte tam žádost o první. `-L`možnost v podstatě spojuje dva požadavky do jedné a provede otočení zopakování požadavku na nové místo. Nakonec výstup z všechny žádosti o volání se zobrazí, třeba následujícího obrázku. Nahrazení ** \<yourstorename >** s názvem vaší jezera úložiště.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Měli byste vidět výstup podobná této:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Přejmenování souboru v úložišti jezera účtu

Tuto operaci je založená na rozhraní REST API WebHDFS volání definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Pomocí následujícího příkazu otočení přejmenování souboru. Nahrazení ** \<yourstorename >** s názvem vaší jezera úložiště.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Měli byste vidět výstup podobná této:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Odstraňte soubor z účtu úložiště jezera dat

Tuto operaci je založená na rozhraní REST API WebHDFS volání definované [zde](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Pomocí následujícího příkazu otočení při odstraňování souboru. Nahrazení ** \<yourstorename >** s názvem vaší jezera úložiště.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Měli byste vidět výstup takto:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Odstranění účtu úložiště jezera dat

Tuto operaci je založená na rozhraní REST API volání definované [zde](https://msdn.microsoft.com/library/mt694075.aspx).

Odstranění účtu úložiště jezera dat pomocí příkazu následující otočení. Nahrazení ** \<yourstorename >** s názvem vaší jezera úložiště.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Měli byste vidět výstup takto:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Viz taky

- [Zdrojová Data velký spuštěné kompatibilní se službou Azure dat jezera úložiště přihlašovacích údajů](data-lake-store-compatible-oss-other-applications.md)
 
