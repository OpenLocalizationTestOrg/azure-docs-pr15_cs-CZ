## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Určení definice struktury obdélníkový datové sady
V části strukturu v datové sady JSON **volitelný** oddíl pro obdélníkový tabulky (s řádky a sloupce) a obsahuje více sloupců v tabulce. Části strukturu bude používat pro buď poskytuje informace o typu pro převody typu nebo dělá mapování sloupců. V následujících částech jsou popsány těchto funkcí najdete v podrobných. 

Každý sloupec obsahuje následující vlastnosti:

| Vlastnost | Popis | Povinné |
| -------- | ----------- | -------- |
| Jméno | Název sloupce. | Ano |
| Typ | Datový typ sloupce. V tématu typu převodu následující části Další podrobné informace o kdy by měla zadáte informace o typu | Ne |
| jazykové verze | .NET na základě jazykové verze má být použita při typ není zadán a je typ .NET data a času nebo Datetimeoffset. Výchozí hodnota je "cs-cz".  | Ne |
| Formát | Formátovací řetězec při typ není zadán a je .NET zadejte data a času nebo Datetimeoffset. | Ne |

Následující příklad ukazuje oddíl strukturu JSON tabulka, která má tři sloupce ID – název a lastlogindate.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Použijte následující pravidla platí pro kdy je vhodné zahrnout "strukturu" informace, co chcete zahrnout do části **strukturu** .

- **Pro strukturovaných zdrojů dat** , která obsahují schématu a zadejte informace o datových společně s daty vlastní (zdroje jako tabulek Azure SQL Server Oracle, atd.), měli byste určit části "strukturu" jenom v případě, že chcete, aby se mapování sloupců určitý pramen sloupců pro konkrétní sloupce v jímky a jejich jména nejsou stejné (viz informace v části mapování sloupců dole). 

    Výše uvedené, informace o typu vynechán v části "strukturu". Na strukturovaného zdroje, informace o typu neexistuje už jako součást definice datové sady v úložišti, takže Nezahrnujte informace o typu při zahrnutí části "strukturu".
- **Pro schéma na zdroje dat pro čtení (konkrétně objektů blob Azure)** , můžete si vybrat pro uložení dat bez uložení schématu nebo typ informace s daty. Pro tyto typy zdrojů dat by měl obsahovat "strukturu" v následujících případech 2:
    - Chcete-li provést mapování sloupců.
    - Po datové zdroje v aktivitu kopírovat, můžete zadat typ informací v "strukturu" a data factory použije tyto informace typ pro převod na nativní typy jímce. Článek [Přesunutí data mezi libovolnými objektů Blob Azure](../articles/data-factory/data-factory-azure-blob-connector.md) najdete v tématu Další informace.

### <a name="supported-net-based-types"></a>Podporované. Typy založené na síť 
Data factory podporuje následující CLS kompatibilní se standardem .NET na základě typ hodnoty poskytuje informace o typu "struktury" pro schéma na zdroje dat pro čtení jako objektů blob Azure.

- Int16
- Int32 
- Int64
- Jeden
- Dvojité
- Decimal
- Byte]
- Logická hodnota
- Řetězec 
- Identifikátor GUID
- Data a času
- DateTimeOffset
- Časového rozpětí 

Pro data a času a Datetimeoffset můžete taky můžete také zadat "jazykové verze" & "formát" řetězec usnadnit analýzu vlastní řetězec data a času. V tématu s příkladem použití typu převodu dole.

