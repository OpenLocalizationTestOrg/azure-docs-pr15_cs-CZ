<properties 
    pageTitle="Jak používat Blitline pro obrázek zpracování - Azure funkce Průvodce" 
    description="Naučte se používat službu Blitline pro zpracování obrázků v aplikaci Azure." 
    services="" 
    documentationCenter=".net" 
    authors="blitline-dev" 
    manager="jason@blitline.com" 
    editor="jason@blitline.com"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/09/2014" 
    ms.author="support@blitline.com"/>
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a>Jak používat Blitline s Azure a úložišti Azure

Tato příručka, bude popisují, jak pro přístup ke službám Blitline a způsob odeslání úlohy Blitline.

## <a name="what-is-blitline"></a>Co je Blitline?

Blitline je obrázek cloudové zpracování služba, která poskytuje enterprise úrovně obrázek zpracování na zlomek ceny, která by nákladů můžete sami vytvořit.

Skutečnosti je, že zpracování obrázků mělo pořád dokola obvykle znovu od základu navržené každého webu. Uvědomujeme to, protože jsme jste vytvořili je milionů časy příliš. Jeden den, kterou jsme se rozhodli, že možná je čas jsme jenom to udělat pro všechny uživatele. Jak na to, to udělat rychle a efektivně, a jejich ukládání všichni pracovní mezitím víme.

Další informace najdete v tématu [http://www.blitline.com](http://www.blitline.com).

## <a name="what-blitline-is-not"></a>Co Blitline není...

Pokud chcete zvýraznit, co je užitečné pro Blitline, je často jednodušší k identifikaci co Blitline nedělá před chovaly.

- Blitline není k dispozici widgety HTML na odeslání obrázků. Obrázky dostupné musí obsahovat veřejně nebo s omezenými oprávněními k dispozici pro Blitline kontaktovat.

- Blitline nedělá živý obraz zpracování jako Aviary.com

- Blitline nepřijímá odesílání obrázků, obrázky nelze použít pro Blitline přímo. Musíte nabízená úložišti Azure nebo jiných místech Blitline podporuje a sdělte Blitline jak si je.

- Blitline je datových paralelní a nedělá synchronní zpracování. Což znamená, že ho musí přidělit nám postback_url a jsme můžou obsahovat až jsme dokončí zpracování.

## <a name="create-a-blitline-account"></a>Vytvoření účtu Blitline

[AZURE.INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a>Postup vytvoření Blitline projektu

Blitline používá JSON můžete definovat akce, kterou chcete provést na obrázek. Tento JSON se skládá z několika jednoduchých pole.

Nejjednodušší příklad vypadá takto:

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

Tady máme JSON, který bude trvat obrázku "src" "... boys.jpeg" a potom změňte velikost tento obrázek 240 x 140.

ID aplikace je něco, co najdete na vaše **Informace o připojení** nebo **SPRAVOVAT** karet na Azure. Je váš tajné identifikátoru, který umožňuje spuštění úlohy na Blitline.

Parametr "uložit" identifikuje informace o místo, kam chcete vložit obrázek po jsme ho zpracovali. V tomto případě důvodu byla nebyla definována jednu. Pokud je definován žádné umístění Blitline uloží ji místně (a dočasně) v umístění jedinečné cloudu. Budou moct získat uvedeném místě z JSON vrácené Blitline, když vyberete jednotlivé Blitline. Identifikátor "obrázek" požaduje a je vrácen v případě, k identifikaci tento údaj uložený obrázek.

Můžete najít další informace o těchto *funkcích* podporujeme tady: <http://www.blitline.com/docs/functions>

Můžete zjistit taky si přečtěte následující dokumentaci informace o možnostech úlohy tady: <http://www.blitline.com/docs/api>

Až budete mít vaše JSON musíte udělat stačí **příspěvku** ho`http://api.blitline.com/job`

Zobrazí se zpět JSON, která vypadá přibližně takto:

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


Podle toho poznáte, že Blitline obdrží žádost ho byla umístění ve frontě zpracování a po dokončení bude obrázek jsou dostupné na webu: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_aplikace\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a>Jak uložit obrázek ke svému účtu úložiště Azure

Pokud máte účet Azure úložiště, můžete snadno mít Blitline nabízená zpracovaných obrázky do Azure kontejner. Přidáním "azure_destination" definujete umístění a oprávnění pro Blitline chcete použít pro.

Tady je příklad:

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


Vyplněním CAPITALIZED hodnot pomocí vlastní můžete odeslat tento JSON http://api.blitline.com/job a obrázek "src" zpracovat s filtrem Rozostření a budou pak posune můžete Azure cíl.

###<a name="please-note"></a>Poznámka:

Přidružení zabezpečení musí obsahovat celý přidružení zabezpečení adresu url, včetně názvu souboru v cílovém souboru.

Příklad:

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


Můžete taky přečíst nejnovější verzi Blitline v úložišti Azure dokumenty [tady](http://www.blitline.com/docs/azure_storage).


## <a name="next-steps"></a>Další kroky

Navštivte blitline.com podrobnější informace o všech naše dalších funkcí:

* Koncový bod rozhraní API Blitline dokumenty <http://www.blitline.com/docs/api>
* Funkce Blitline rozhraní API <http://www.blitline.com/docs/functions>
* Příklady Blitline rozhraní API <http://www.blitline.com/docs/examples>
* Třetí část Nuget knihovny <http://nuget.org/packages/Blitline.Net>
