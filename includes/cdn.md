# <a name="using-cdn-for-azure"></a>Použití CDN pro Azure

Síť pro doručování obsahu Azure (CDN) nabízí vývojáři globální řešení pro doručování obsahu vysokorychlostní tak, že ukládání do mezipaměti objektů BLOB a statický obsah instancí výpočetním na fyzické uzlů v USA, Evropa, Asie, Austrálie a Jižní Amerika. Aktuální seznam CDN uzel umístění najdete v tématu [Azure CDN uzel umístění].

Tento úkol zahrnuje tyto kroky:

* [Krok 1: Vytvoření účtu úložiště](#Step1)
* [Krok 2: Vytvořte nový koncový bod CDN pro váš účet úložiště](#Step2)
* [Krok 3: Přístup k obsahu CDN](#Step3)
* [Krok 4: Odebrání CDN obsahu](#Step4)

Výhody používání CDN do mezipaměti Azure dat patří:

-   Lepší výkon a uživatelského prostředí pro koncové uživatele, kteří jsou vzdáleném od zdroje obsahu a jsou pomocí aplikací, kde jsou mnoha internetových cest potřebný k načtení obsahu
-   Velké distribuované rozšiřitelnost pro lépe zpracování okamžité velkém zatížení, například na začátku události, například spuštění produktu

Stávajících zákazníků CDN teď můžete použít Azure CDN [Azure klasické portálu]. CDN doplněk funkcí k vašemu předplatnému a na samostatném [fakturační plán].

<a id="Step1"> </a>
<h2>Krok 1: Vytvoření účtu úložiště</h2>

Pomocí následujícího postupu vytvořte nový účet úložiště pro předplatné Azure. Účet úložiště umožňuje přístup k služby Azure úložiště. Účet úložiště představuje nejvyšší úroveň názvů pro všechny komponenty služby Azure úložiště přístup: objektů Blob služby, fronty služby a služby tabulky. Další informace o služby Azure úložiště najdete v tématu [Použití úložiště služby Azure](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Vytvoření účtu úložiště, musí být správce služeb nebo spolu správce pro přidružené předplatné.

> [AZURE.NOTE] Informace o této operace pomocí rozhraní API správy služby Azure naleznete v tématu [Vytvoření účtu úložiště](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) odkaz.

**Vytvoření účtu úložiště pro předplatné Azure**

1.  Přihlaste se k [Azure klasické portálu].
2.  V levém dolním rohu klikněte na **Nový**. V dialogovém okně **Nový** vyberte **Datové služby**a klikněte na **úložiště**, pak **Vytvořit**.

    Zobrazí se dialogové okno **Vytvořit účet úložiště** .

    ![Vytvoření účtu úložiště][create-new-storage-account]

4. Do pole **Adresa URL** zadejte název subdomain (subdoména). Tento údaj může obsahovat ze 3 24 malá písmena a číslice.

    Tato hodnota bude název hostitele v rámci identifikátor URI, který slouží k řešení objektů Blob, fronty nebo tabulku zdroje pro předplatné. Při řešení zdroj kontejneru ve službě objektů Blob, použijete identifikátor URI ve formátu následující kde * &lt;StorageAccountLabel&gt; * odkazuje na hodnotu zadanou do pole **Zadejte adresu URL**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt; *

    **Důležité:** Adresa URL popisek formulářů subdoménu účtu úložiště URI a musí být jedinečný mezi všemi službami hostovanou v Azure.

    Tato hodnota taky slouží jako název tohoto účtu úložiště na portálu nebo při přístupu k tomuto účtu programově.

5.  Ze seznamu **Oblastí/spřažení skupiny** vyberte oblast nebo skupinu spřažení účtu úložiště. Vyberte skupinu spřažení místo do oblasti, pokud mají úložiště služby byli ve stejném datovém centru s jinými službami Windows Azure, které používáte. To můžete zvýšit výkon a jsou žádné náklady vzniklé v souvislosti s výstupní.  

    **Poznámka:** Vytvořit skupinu spřažení, otevřete oblasti **Nastavení** portálu pro správu, klikněte na **skupiny spřažení**a potom klikněte na tlačítko **Přidat skupinu spřažení** nebo **Přidat**. Můžete taky vytváření a Správa skupin spřažení pomocí rozhraní API Správa služby Windows Azure. Další informace najdete v tématu [operace na spřažení skupiny].

6. V seznamu **předplatné** vyberte předplatné, které se budou používat účet úložiště v.
7.  Klikněte na **vytvořit účet úložiště**. Proces vytvoření účtu úložiště může trvat několik minut.
8.  Pokud chcete ověřit, že úložiště účet byl úspěšně, zkontrolujte, jestli účet vypadá v položkách uvedeny **úložiště** bude mít stav **Online**.

<a id="Step2"> </a>
<h2>Krok 2: Vytvořte nový koncový bod CDN pro váš účet úložiště</h2>

Po povolení CDN přístupu k účtu úložiště nebo hostované služby všechny veřejně dostupné objekty se dá použít pro ukládání do mezipaměti CDN okraje. Pokud upravíte objekt, který je aktuálně uložených v mezipaměti v CDN, nový obsah není možné prostřednictvím CDN, dokud CDN aktualizuje jejího obsahu, když režim cached obsahu time-to-live doby platnosti.

**Vytvoření nového koncového bodu CDN pro váš účet úložiště**

1. Na [portálu Azure klasické], v navigačním podokně klikněte na **CDN**.

2. Na pásu karet klikněte na **Nový**. V dialogovém okně **Nový** vyberte **Aplikaci služby**a **CDN**a potom vyberte **Vytvořit**.

3. V rozevíracím seznamu **Původní doméně** vyberte úložiště účet, který jste vytvořili v předchozím oddíle ze seznamu účtů volného. 

4. Klikněte na tlačítko **vytvořit** vytvořte nový koncový bod.

5. Po vytvoření koncového bodu se zobrazí v seznamu koncové body pro předplatné. Zobrazení seznamu zobrazuje URL použít pro přístup k obsahu mezipaměti, jakož i původní doméně. 

    Původní doméně je umístění, ze kterého CDN ukládá obsah. Původní doméně může být účet úložiště nebo do cloudové služby, úložiště účet se používá pro účely v tomto příkladu. Obsah úložiště je mezipaměti serverům okraj podle nastavení mezipaměti ovládací prvek, které určíte, nebo výchozí heuristiky mezipaměti sítě. 


    > [AZURE.NOTE] Konfigurace vytvoření koncového bodu okamžitě nebudou k dispozici. může trvat až 60 minut registrace se rozšíří prostřednictvím sítě CDN. Uživatelé, kteří pokusíte použít název domény CDN okamžitě může zobrazit stavový kód 400 (Chybný požadavek), dokud obsah je k dispozici prostřednictvím CDN.

<a id="Step3"> </a>
<h2>Krok 3: Access CDN obsahu</h2> 

Pokud chcete mít přístup k CDN režim cached obsahu, použijte adresu URL CDN uvedený na portálu. Adresu mezipaměti objektů blob bude vypadat podobně jako takto:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*myPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Krok 4: Odebrání obsahu ze zprávy CDN</h2>

Pokud chcete už mezipaměti objektu v Azure obsahu doručení Network (CDN), můžete provést jednu z následujících kroků:

-   Pro objektů blob Azure můžete odstranit objektů blob z veřejné kontejner.
-   Může být kontejneru soukromé namísto prsty. Další informace najdete v článku [Omezení přístupu k kontejnerů a objektů BLOB](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) .
-   Můžete zakázat nebo odstranění koncového bodu CDN pomocí portálu pro správu.
-   Můžete změnit hostovanou službu už odpovědět na požadavky objektu.

Objekt již uložené v CDN zůstane režim cached až do doby time-to-live objektu. Když skončí platnost time-to-live období, CDN bude zkontrolujte, jestli je pořád platná CDN koncového bodu a pořád anonymně přístupných osobám s postižením objektu. Pokud není, bude mezipaměti už tento objekt.

Na portálu Správa Azure není aktuálně podporovaná možnost okamžitě vymazat obsah. Kontaktujte prosím [podporu Azure](https://azure.microsoft.com/support/options/) v případě potřeby okamžitě vymazat obsah. 

## <a name="additional-resources"></a>Další zdroje informací

-   [Jak vytvořit skupinu spřažení v Azure]
-   [Jak: Správa účtů úložiště pro předplatné Azure]
-   [Informace o rozhraní API pro správu služby]
-   [Postup mapování CDN obsahu na vlastní doméně]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure CDN uzel umístění]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure klasické portálu]: https://manage.windowsazure.com/
  [fakturační plán]: /pricing/calculator/?scenario=full
  [Jak vytvořit skupinu spřažení v Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [Informace o rozhraní API pro správu služby]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [Postup mapování CDN obsahu na vlastní doméně]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
