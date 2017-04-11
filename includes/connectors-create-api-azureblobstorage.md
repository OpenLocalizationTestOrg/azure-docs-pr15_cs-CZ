### <a name="prerequisites"></a>Zjistit předpoklady pro
- Účet Azure. můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free)
- [Úložiště objektů Blob Azure účtu](../articles/storage/storage-create-storage-account.md) včetně název účtu úložiště a jeho přístupové klávesy. Tyto informace jsou uvedeny v okně vlastností účtu úložiště na portálu Azure. Další informace o [Úložišti Azure](../articles/storage/storage-introduction.md).

Před použitím účtu úložiště objektů Blob Azure v aplikaci použití logických operátorů, připojení k úložišti objektů Blob Azure účtu. Lze provést jednoduše v rámci logiky aplikace na portálu Azure.  

Připojení k úložišti objektů Blob Azure účtu pomocí následujících kroků:  

1. Vytvoření aplikace logiky. V Návrháři logiky aplikace přidat aktivační událost a pak přidejte akce. V rozevíracím seznamu vyberte **zobrazení Microsoft spravované rozhraní API** a do vyhledávacího pole zadejte "objektů blob". Vyberte jednu z akcí:  

    ![Azure krok vytvoření připojení úložiště objektů Blob](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. Pokud jste ještě nevytvořili všechna připojení k úložišti Azure, zobrazí se výzva k podrobnosti o připojení:   

    ![Azure krok vytvoření připojení úložiště objektů Blob](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. Zadejte podrobnosti o účtu úložiště. Vlastnosti hvězdičkou jsou potřeba.

    | Vlastnost | Podrobnosti |
|---|---|
| Název připojení * | Zadejte libovolnou název připojení. |
| Název účtu úložiště Azure * | Zadejte název účtu úložiště. Název účtu úložiště se zobrazí ve vlastnostech úložiště na portálu Azure. |
| Azure úložiště účtu přístupová klávesa * | Zadejte klíč účtu úložiště. Přístupové klávesy se zobrazují ve vlastnostech úložiště na portálu Azure. |

    Těchto přihlašovacích údajů se používají k povolit aplikaci použití logických operátorů k připojení a přístup ke svým datům. 

4. Vyberte možnost **vytvořit**.

5. Všimněte si, že byl vytvořen připojení. Teď pokračujte dalšími kroky v aplikaci použití logických operátorů: 

    ![Azure krok vytvoření připojení úložiště objektů Blob](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
