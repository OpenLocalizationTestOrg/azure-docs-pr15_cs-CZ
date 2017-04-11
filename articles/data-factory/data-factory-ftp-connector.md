<properties 
    pageTitle="Přesuňte data z serveru FTP | Microsoft Azure" 
    description="Informace o tom, jak přesunout data ze serveru FTP pomocí Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Přesunutí dat ze serveru FTP pomocí Azure Data Factory
Tento článek popisuje činnost kopie v Azure Data Factory slouží k přesunutí dat ze serveru FTP úložiště podporované jímky dat. Tento článek je založena na článek [aktivity přesun dat](data-factory-data-movement-activities.md) , který představuje obecné základní informace o přesun dat s kopírovat aktivitu a v seznamu datový úložiště podporované jako zdroje/propadů. 

Data factory v současné době podporuje pouze přesunutí dat ze serveru FTP další úložiště dat, ale ne pro přesunutí dat z jiných dat ukládají na serveru FTP. Podporuje obou místních i cloudových FTP servery. 

Pokud přesouváte data z **místního** serveru FTP serveru cloudové úložiště dat (Příklad: úložišti objektů Blob Azure), instalace a používání Brána pro správu dat. Brána pro správu dat je agent klienta, který je nainstalovaný v počítači místní, které umožňuje cloudovým službám se připojit k místní zdroje. Podrobné informace o brány najdete v článku [Brána pro správu dat](data-factory-data-management-gateway.md) . Viz článek [přesunutí dat mezi místního umístění a cloudu](data-factory-move-data-between-onprem-and-cloud.md) podrobné pokyny týkající se nastavení brány a s ním pracovat. Použití brány se připojit k serveru FTP i v případě, že server je v počítači virtuální Azure IaaS (OM). 

Brány můžete nainstalovat na stejném počítači místní nebo OM IaaS Azure na serveru FTP. Ale doporučujeme nainstalovat bránu na samostatném počítače nebo samostatné OM IaaS Azure Chcete-li předejít konflikty prostředků a lepší výkon. Při instalaci brány na samostatném počítač počítač soubory byste měli přístup k serveru FTP. 

## <a name="copy-data-wizard"></a>Kopírování dat Průvodce
Nejjednodušší způsob, jak vytvořit kanál, který slouží ke kopírování dat ze serveru FTP je použití Průvodce datovým kopírovat. V tématu [kurz: vytvoření kanálu pomocí Průvodce kopírováním](data-factory-copy-data-wizard-tutorial.md) rychlé informace o vytváření kanálů pomocí Průvodce datovým kopírovat. 

Následující příklady poskytují definice JSON vzorku, které slouží k vytvoření potrubí pomocí [Azure portál](data-factory-copy-activity-tutorial-using-azure-portal.md) nebo [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) nebo [Azure Powershellu](data-factory-copy-activity-tutorial-using-powershell.md). 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Ukázka: Zkopírování dat z serveru FTP objektů blob Azure

Tento příklad ukazuje, jak chcete zkopírovat data ze serveru FTP k úložišti objektů Blob Azure. Data lze však zkopírovaný **přímo** k některým propadů uvedená [tady](data-factory-data-movement-activities.md#supported-data-stores) pomocí kopírování aktivity v Azure Data Factory.  
 
Vzorku obsahuje následující factory entit dat:

- Propojené služba typu [Server_ftp](#ftp-linked-service-properties).
- Propojené služba typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Zadávání [datovou sadu](data-factory-create-datasets.md) typu [sdílené položce](#fileshare-dataset-type-properties).
- Objekt výstup [datovou sadu](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Kanálem k odesílání zpráv](data-factory-create-pipelines.md) s kopírovat aktivity na počítači se používá [FileSystemSource](#ftp-copy-activity-type-properties) a [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Vzorku slouží ke kopírování dat ze serveru FTP k objektů blob Azure každou hodinu. JSON vlastnosti použité v těchto vzorcích jsou popsány v části Sledování vzorky. 

**FTP propojené služby** Tento příklad používá základní ověřování pomocí uživatelského jména a hesla ve formátu prostého textu. Můžete taky použít jednu z těchto způsobů: 

- Anonymní přístup 
- Základní ověřování pomocí šifrované přihlašovacích údajů
- FTP přes protokol SSL/TLS (FTPS)

Naleznete v části různých typů ověřování můžete použít [FTP propojené služby](#ftp-linked-service-properties) . 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
        }
      }
    }

**Služba Azure úložiště propojené**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Zadávání datovou sadu FTP** Tento datovou sadu odkazuje na složku FTP `mysharedfolder` a soubor `test.csv`. Kanálu slouží ke kopírování souboru do cíle. 

Nastavení "externí": "true" informuje o tom, službu Data Factory, datové externí stránku factory dat a není vytvořené pomocí aktivity v data factory.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Výstup datovou sadu objektů Blob Azure**

Zápisu dat do nových objektů blob každou hodinu (četnost: hodiny, intervalu: 1). Cestu ke složce objektů blob dynamicky vyhodnocení podle počáteční čas dané řezu, který zpracovávání. Cestu ke složce používá rok, měsíc, den a hodin díly počáteční čas.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Příležitostí s aktivitou kopie**

Kanálu obsahuje kopírovat aktivity, který je nakonfigurovaný na používání datové sady vstupní a výstupní a je naplánováno spuštění každou hodinu. V kanálu JSON definice typ **zdroje** je nastavena na **FileSystemSource** a **jímky** typ je nastavený na **BlobSink**. 
    
    {
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>Vlastnosti propojené služby FTP

Následující tabulka obsahuje popis prvků JSON specifické pro FTP propojené služby.

| Vlastnost | Popis | Povinné | Výchozí |
| -------- | ----------- | -------- | ------- | 
| Typ | Vlastnost typu musí být nastavena na Server_ftp | Ano | &nbsp;
| Host (hostitel) | Jméno nebo IP adresu serveru FTP | Ano | &nbsp;
| authenticationType | Určete typ ověřování | Ano | Základní, anonymní |
| uživatelské jméno | Uživatele, kteří mají přístup k serveru FTP | Ne | &nbsp;
| heslo | Heslo uživatele (uživatelské jméno) | Ne | &nbsp;
| encryptedCredential | Šifrované přihlašovací údaje pro přístup k serveru FTP | Ne | &nbsp;
| názevbrány | Název brány Brána pro správu dat pro připojení k serveru FTP místní | Ne    | &nbsp;
| port | Na kterém je přijímá serveru FTP port | Ne | 21 |
| enableSsl | Určete, zda chcete dát FTP přednost před kanál SSL/TLS. | Ne | PRAVDA | 
| enableServerCertificateValidation | Určete, zda chcete povolit ověřovací certifikát SSL serveru při použití FTP kanálem SSL/TLS. | Ne | PRAVDA | 

### <a name="using-anonymous-authentication"></a>Použití anonymní přístup

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Pomocí uživatelského jména a hesla ve formátu prostého textu pro základní ověřování
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Použití portu, enableSsl, enableServerCertificateValidation

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Používat encryptedCredential ověřování a brány

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Další informace o nastavení přihlašovacích údajů pro zdroj dat FTP místní najdete v článku [Nastavení přihlašovacích údajů a zabezpečení](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Kopírovat FTP aktivity typ vlastnosti

Úplný seznam oddíly a vlastnosti jsou k dispozici pro definování aktivity naleznete v článku [Vytvoření kanály](data-factory-create-pipelines.md) . Vlastnosti jako je název, popis, vstupní a výstupní tabulky a zásady jsou dostupné pro všechny typy aktivit. 

Vlastnosti dostupné v části typeProperties aktivity na druhou stranu se liší podle jednotlivé typy aktivit. Kopírovat aktivity vlastnosti typu lišit podle toho, typy zdrojů a propadů.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Výkon a optimalizace  
V tématu [kopírování aktivity Performance optimalizace Průvodce](data-factory-copy-activity-performance.md) se naučit používat klíčové faktory, které dopad na výkon přesun dat (Kopírovat aktivita) v Azure Data Factory a různé způsoby, jak optimalizovat jeho.

## <a name="next-steps"></a>Další kroky
Naleznete v následujících článcích: 

- [Kopírovat aktivity kurz](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) podrobné pokyny k vytváření kanálů aktivitu kopírovat. 
