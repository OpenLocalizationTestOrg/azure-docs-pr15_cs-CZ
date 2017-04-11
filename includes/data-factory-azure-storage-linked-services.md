## <a name="azure-storage-linked-service"></a>Propojené Azure úložiště služby

**Azure úložiště propojené služby** umožňuje propojení Azure úložiště klienta factory Azure dat pomocí **klíč účtu**. To nabízí factory dat globální přístup k základnímu úložišti Azure. Následující tabulka obsahuje popis prvků JSON specifické pro službu Azure úložiště propojené.

| Vlastnost | Popis | Povinné |
| :-------- | :----------- | :-------- |
| Typ | Vlastnost typu musí být nastavena na: **AzureStorage** | Ano |
| connectionString | Zadejte informace potřebné k připojení k úložišti Azure connectionString vlastnosti. | Ano |

Naleznete v následujícím článku postup zobrazit/kopii klíč účtu služby Azure úložiště: [zobrazení, kopírovat a přístupových kláves z verze regenerovat úložiště](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

**Příklad:**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Přidružení Azure úložiště zabezpečení propojené služby  
Sdílený přístup k podpisu (přidružení zabezpečení) obsahuje delegované přístupu k prostředkům ve vašem účtu úložiště. To znamená, že udělit klienta omezené oprávnění k objektům ve vašem účtu úložiště stanovený počet minut a zadaný sadu oprávnění, aniž by bylo nutné sdílet svůj účet přístupové klávesy. Přidružení zabezpečení je identifikátor URI, který zahrnuje v jeho parametry dotazu všechny informace potřebné k ověřené přístup k prostředku úložiště. Přístup k prostředkům úložiště přidružením, klienta pouze musí předat přidružení zabezpečení odpovídající konstruktor nebo metody. Podrobné informace o přidružení zabezpečení najdete v tématu [sdílené podpisů přístup: Principy modelem přidružení zabezpečení](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
Služba Azure úložiště přidružení zabezpečení propojené umožňuje propojení úložiště klienta Azure factory Azure dat pomocí sdílených přístup podpis SAS (). To factory dat nabízí omezení/čas mez přístup k prostředkům všechny/konkrétní (objektů blob/kontejneru) v úložišti. Následující tabulka obsahuje popis prvků JSON specifické pro službu Azure úložiště přidružení zabezpečení propojené. 

| Vlastnost | Popis | Povinné |
| :-------- | :----------- | :-------- |
| Typ | Vlastnost typu musí být nastavena na: **AzureStorageSas**  | Ano |
| sasUri | Zadejte sdílené URI podpis přístup k úložišti Azure zdroje, jako jsou objektů blob, kontejneru nebo tabulku. Dole najdete pár poznámek podrobnosti. | Ano | 


**Příklad:**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

Při vytváření **URI přidružení zabezpečení**, rozhodování takto:  

- Azure Data Factory podporuje pouze **Přidružení zabezpečení služby**, není přidružení zabezpečení účtu. Podrobné informace o těchto dvou typů naleznete v tématu [Typy s sdílených přístup podpisů](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) .
- Vhodné pro čtení i zápis **oprávnění** se musí nastavit u objektů podle použití propojené služby (pro čtení, zapsat, pro čtení i zápis) u výrobce data.
- **Čas vypršení** je třeba nastavit řádně podporovat. Ujistěte se, že přístup k úložišti Azure objekty nevyprší v rámci doby aktivního kanálu.
- Identifikátor URI se vytvoří na pravém kontejneru/objektů blob nebo na úrovni tabulky podle potřeby. Identifikátor Uri přidružení zabezpečení na objektů blob Azure umožňuje Data Factory služby pro přístup k této konkrétní objektů blob. Identifikátor Uri přidružení zabezpečení na kontejner objektů blob Azure umožňuje služby Data Factory iteraci objektů BLOB v tomto kontejneru. Pokud potřebujete větší nebo menší počet objektů později poskytnutí přístupu nebo aktualizovat URI přidružení zabezpečení, nezapomeňte aktualizovat službu propojené s novou URI.   
