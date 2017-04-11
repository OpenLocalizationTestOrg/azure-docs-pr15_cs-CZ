<properties
    pageTitle="Integrace s účtem úložiště s CDN | Microsoft Azure"
    description="Naučte se používat síť pro doručování obsahu Azure (CDN) dodat vysokorychlostní obsah tak, že ukládání do mezipaměti objektů blob z Azure úložiště."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="integrate-a-storage-account-with-cdn"></a>Integrace s účtem úložiště s CDN

CDN může být užitečné obsah do mezipaměti z Azure úložiště. Vývojáři nabízí globální řešení pro doručování obsahu vysokorychlostní tak, že ukládání do mezipaměti objektů BLOB a instancí výpočetním na fyzický uzlů v USA, Evropa, Asie, Austrálie a Jižní Amerika statický obsah.


## <a name="step-1-create-a-storage-account"></a>Krok 1: Vytvoření účtu úložiště

Pomocí následujícího postupu vytvořte nový účet úložiště pro předplatné Azure. Účet úložiště umožňuje přístup k služby Azure úložiště. Účet úložiště představuje nejvyšší úroveň názvů pro všechny komponenty služby Azure úložiště přístup: objektů Blob služby, fronty služby a služby tabulky. Další informace najdete v příručce [Úvod k úložišti tabulek Microsoft Azure](../storage/storage-introduction.md).

Vytvoření účtu úložiště, musí být správce služeb nebo spolu správce pro přidružené předplatné.

> [AZURE.NOTE] Existuje několik způsobů, které vám pomohou vytvořit účet úložiště, včetně Azure portálem a Powershellu.  U tohoto kurzu budeme používat portál Azure.  

**Vytvoření účtu úložiště pro předplatné Azure**

1.  Přihlaste se k [portálu Azure](https://portal.azure.com).
2.  V levém horním rohu vyberte **Nový**. V dialogovém okně **Nový** vyberte **Data + úložiště**a klikněte na **úložiště účtu**.

    Zobrazí se zásuvné **vytvořit úložiště účtu** .

    ![Vytvoření účtu úložiště][create-new-storage-account]

4. Do pole **název** zadejte název subdomain (subdoména). Tento údaj může obsahovat 3 24 malá písmena a číslice.

    Tato hodnota bude název hostitele v rámci identifikátor URI, který slouží k řešení objektů Blob, fronty nebo tabulku zdroje pro předplatné. Adresa zdroj kontejneru ve službě objektů Blob, použijete identifikátor URI ve formátu následující kde * &lt;StorageAccountLabel&gt; * odkazuje na hodnotu zadanou do pole **Zadejte adresu URL**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Důležité:** Adresa URL popisek formulářů subdoménu účtu úložiště URI a musí být jedinečný mezi všemi službami hostovanou v Azure.

    Tato hodnota taky slouží jako název tohoto účtu úložiště na portálu nebo při přístupu k tomuto účtu programově.

5. Ponechejte výchozí hodnoty pro **nasazení modelu**, **účet identifikovat**, **výkon**a **replikace**. 

6. Vyberte **předplatné** , které se budou používat účet úložiště v.

7. Vyberte nebo vytvořte **Pole Skupina zdroje**.  Další informace o skupinách zdroje najdete v článku [Přehled Správce prostředků Azure](azure-resource-manager/resource-group-overview.md#resource-groups).

8. Vyberte umístění pro váš účet úložiště.

8. Klikněte na **vytvořit**. Proces vytvoření účtu úložiště může trvat několik minut.


## <a name="step-2-create-a-new-cdn-profile"></a>Krok 2: Vytvoření nového profilu CDN

Profil CDN je kolekce CDN koncové body.  Každý profil obsahuje jeden nebo více CDN koncové body.  Můžete chtít používat více profily uspořádání koncových bodů CDN domény internet, webové aplikace nebo jiných kritérií.

> [AZURE.TIP] Pokud už máte CDN profil, který chcete použít pro účely tohoto návodu, přejděte ke [kroku 3](#step-3-create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="step-3-create-a-new-cdn-endpoint"></a>Krok 3: Vytvoření nového koncový bod CDN

**Vytvoření nového koncového bodu CDN pro váš účet úložiště**

1. Na [Portálu Správa Azure](https://portal.azure.com)přejděte do vlastního profilu CDN.  Se může mít připnuté ho na řídicí panel v předchozím kroku.  Pokud můžete, najdete ji kliknutím na **Procházet**a pak **CDN profily**a kliknete na profil chcete přidat koncový bod k.

    Zobrazí se zásuvné CDN profilu.

    ![CDN profilu][cdn-profile-settings]

2. Klikněte na tlačítko **Přidat koncový bod** .

    ![Přidání tlačítka koncový bod][cdn-new-endpoint-button]

    Zobrazí se zásuvné **Přidat koncový bod** .

    ![Přidání zásuvné koncový bod][cdn-add-endpoint]

3. Zadejte **název** pro tento CDN koncový bod.  Tento název se použije pro přístup k režim cached zdrojů v doméně `<endpointname>.azureedge.net`.

4. V rozevíracím seznamu **Typ Origin** vyberte *úložiště*.  

5. V rozevíracím seznamu **Origin hostname** vyberte svůj účet úložiště.

6. Ponechejte výchozí hodnoty pro **Origin cestu**, **záhlaví hostitele Origin**a **port protokol/Origin**.  Je třeba určit minimálně jeden protokol (HTTP nebo HTTPS).

    > [AZURE.NOTE] Toto nastavení umožňuje všechny veřejně viditelné kontejnerů ve vašem účtu úložiště pro ukládání do mezipaměti v CDN.  Pokud chcete omezit obor jednoho kontejneru, použijte **cesty Origin**.  Poznámka: kontejneru musí mít viditelnost nastavit veřejným.

7. Klikněte na tlačítko **Přidat** k vytvoření nové koncový bod.

8. Po vytvoření koncového bodu se zobrazí v seznamu koncové body profilu. Zobrazení seznamu zobrazuje URL použít pro přístup k obsahu mezipaměti, jakož i původní doméně.

    ![Koncový bod CDN][cdn-endpoint-success]

    > [AZURE.NOTE] Koncový bod není možné okamžitě používat.  Může trvat až 90 minut registrace se rozšíří prostřednictvím sítě CDN. Uživatelé, kteří pokusíte použít název domény CDN okamžitě může zobrazit stavový kód 404, dokud obsah je k dispozici prostřednictvím CDN.


## <a name="step-4-access-cdn-content"></a>Krok 4: Access CDN obsahu

Pokud chcete mít přístup k CDN režim cached obsahu, použijte adresu URL CDN uvedený na portálu. Adresu mezipaměti objektů blob bude vypadat podobně jako takto:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [AZURE.NOTE] Po povolení CDN přístupu k účtu úložiště nebo hostované služby všechny veřejně dostupné objekty se dá použít pro ukládání do mezipaměti CDN okraje. Pokud upravíte objekt, který je aktuálně uložených v mezipaměti v CDN, nový obsah není možné prostřednictvím CDN, dokud CDN aktualizuje jejího obsahu, když režim cached obsahu time-to-live doby platnosti.

## <a name="step-5-remove-content-from-the-cdn"></a>Krok 5: Odebrání obsahu ze zprávy CDN

Pokud chcete už mezipaměti objektu v Azure obsahu doručení Network (CDN), můžete provést jednu z následujících kroků:

-   Může být kontejneru soukromé namísto prsty. Další informace najdete v tématu [Správa anonymní přístup pro čtení kontejnerů a objektů BLOB](../storage/storage-manage-access-to-resources.md) .
-   Můžete zakázat nebo odstranění koncového bodu CDN pomocí portálu pro správu.
-   Můžete změnit hostovanou službu už odpovědět na požadavky objektu.

Objekt již uložené v CDN zůstane režim cached, dokud doby time-to-live objektu nebo odstraněné koncový bod. Když skončí platnost time-to-live období, CDN bude zkontrolujte, jestli je pořád platná CDN koncového bodu a pořád anonymně přístupných osobám s postižením objektu. Pokud není, bude mezipaměti už tento objekt.


## <a name="additional-resources"></a>Další zdroje informací

-   [Postup mapování CDN obsahu na vlastní doméně](cdn-map-content-to-custom-domain.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png

[cdn-profile-settings]: ./media/cdn-create-a-storage-account-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-a-storage-account-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-a-storage-account-with-cdn/cdn-endpoint-success.png
