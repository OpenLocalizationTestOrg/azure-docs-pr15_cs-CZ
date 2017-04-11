<properties
    pageTitle="Jak používat knihovnu klient aplikace Android mobilní telefon"
    description="Jak používat Android klienta SDK Azure mobilních aplikací."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-the-android-client-library-for-mobile-apps"></a>Jak používat knihovnu Android klienta pro mobilní aplikace

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tato příručka ukazuje, jak používat Android klienta SDK k aplikacím Mobile provádět běžné scénáře, jako například:

- Dotaz na data (vkládání, aktualizace a odstranění).
- Ověřování.
- Zpracování chyb.
- Vlastní nastavení klienta. 

Slouží také důkladné postupy do běžné kód klienta použitý v Většina mobilních aplikací.

Tento průvodce se zaměřuje na Android SDK straně klienta.  Další informace o SDK serverovou k aplikacím Mobile, najdete v článku [práce s .NET back-end SDK] [ 10] nebo [používání back-end Node.js SDK][11].

## <a name="reference-documentation"></a>Si přečtěte následující dokumentaci odkaz

[Rozhraní API Javadocs odkaz] můžete najít[ 12] pro knihovnu Android klienta na GitHub.

## <a name="supported-platforms"></a>Podporované platformy

Android SDK Azure mobilní aplikace podporuje rozhraní API úrovně 19 až 24 (KitKat prostřednictvím Nougat).  

"Server toku" ověřování používá webového zobrazení pro prezentovaný uživatelského rozhraní. Pokud zařízení není možné zobrazení webového zobrazení uživatelského rozhraní, je třeba jiné metody ověřování, které je mimo rozsah produktu.  Tento SDK není vhodná pro typ kukátka nebo podobně zamčeném zařízení.

## <a name="setup-and-prerequisites"></a>Instalační program a požadavky

[Rychlý úvod mobilní aplikace](app-service-mobile-android-get-started.md) výukového.  Tento úkol zaručuje, zda jsou splněny předpoklady k vývoji aplikací Mobile Azure.  Rychlý úvod umožňuje konfigurace účtu a vytvořte svůj první back-end mobilní aplikaci.

Pokud se rozhodnete výukového rychlý úvod, proveďte následující úkoly:

- [Vytvoření back-end mobilní aplikaci] [ 13] pomocí aplikace pro Android.
- V Android Studio [aktualizace souborů sestavení Gradle](#gradle-build).
- [Povolte internet oprávnění](#enable-internet).

###<a name="gradle-build"></a>Aktualizace souboru buildu Gradle

Změna oba soubory **build.gradle** :

1. Přidáte tento kód do souboru *projektu* úrovně **build.gradle** uvnitř značku *buildscript* :

        buildscript {
            repositories {
                jcenter()
            }
        }

2. Přidáte tento kód úrovně **build.gradle** soubor *modulu aplikace* uvnitř značku *závislosti* :

        compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    Je v současné době nejnovější verzi 3.1.0. Podporované verze najdete [tady][14].

###<a name="enable-internet"></a>Povolte internet oprávnění

Přístup k Azure, musí mít aplikace INTERNET povolení. Pokud už není aktivní, přidejte do souboru **AndroidManifest.xml** následující řádek kódu:

    <uses-permission android:name="android.permission.INTERNET" />

## <a name="the-basics-deep-dive"></a>Základní informace o hloubkové postupy

Tato část popisuje některé kódu v aplikaci rychlý úvod, která se vztahuje k používání aplikací Mobile Azure.  

###<a name="data-object"></a>Definování klienta datových tříd

Přístup k datům z tabulky databáze SQL Azure, definujte datových tříd klienta, které odpovídají k tabulkám v mobilní aplikaci back-end. Příklady v tomto tématu se předpokládá v tabulce s názvem **ToDoItem**, který má následující sloupce:

- ID
- text
- dokončení

Odpovídající zadali objektu na straně klienta:

    public class ToDoItem {
        private String id;
        private String text;
        private Boolean complete;
    }

Kód je umístěn v souboru s názvem **ToDoItem.java**.

Pokud tabulky databáze SQL Azure obsahuje více sloupců, přidejte do této třídy odpovídající pole.  Například pokud DTO bylo sloupec Priorita celé číslo (objekt přenos dat) a pak můžete přidat toto pole spolu s jeho příjemce a nastavení metod:

    private Integer priority;

    /**
    * Returns the item priority
    */
    public Integer getPriority() {
        return mPriority;
    }
    
    /**
    * Sets the item priority
    *
    * @param priority
    *            priority to set
    */
    public final void setPriority(Integer priority) {
        mPriority = priority;
    }

Naučte se vytvářet další tabulky v serverové části aplikace Mobile, najdete v článku [Postup: definování řadiči tabulky] [ 15] (.NET back-end) nebo [definovat tabulky pomocí dynamického schématu] [ 16] (Node.js back-end). Node.js back-end můžete taky použít nastavení **snadno tabulek** [Azure portálu].

###<a name="create-client"></a>Postup: vytvoření kontext klienta

Tento kód vytvoří **MobileServiceClient** objekt, který se používá pro přístup k vaší back-end mobilní aplikaci. Kód jsou uvedeny v `onCreate` metoda třídy **aktivity** podle *AndroidManifest.xml* jako **hlavní** akce a **SPOUŠTĚČ** kategorie. Rychlý úvod kód přejde do souboru **ToDoActivity.java** .

        MobileServiceClient mClient = new MobileServiceClient(
            "MobileAppUrl", // Replace with the Site URL
            this)

V tomto kódu nahradit `MobileAppUrl` s adresou URL svého back-end mobilní aplikaci, kterou najdete v [Azure portál] v zásuvné pro mobilní aplikaci back. Pro tento řádek kódu sestavíte bude potřeba přidat následující příkaz **import** :

    import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Postup: vytvoření odkaz na tabulku

Nejjednodušší způsob, jak dotazu nebo upravte data v back-end je s použitím *zadali programovací model*, protože je silného typu jazyka Java. Tento model obsahuje bezproblémové JSON serializace a rekonstrukce pomocí [gson] [ 3] knihovny při odesílání dat mezi objekty klienta a tabulek v back-end Azure SQL.

Přístup k tabulky, nejprve vytvořit [MobileServiceTable] [ 8] objekt tak, že zavoláte **jít** metoda na [MobileServiceClient][9].  Tento způsob zahrnuje dva přetížení:

    public class MobileServiceClient {
        public <E> MobileServiceTable<E> getTable(Class<E> clazz);
        public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
    }

Následující kód **mClient** je odkaz na objekt MobileServiceClient.  První přetížení se používá název třídy a název tabulky jsou stejná, kde je použito v rychlý úvod:

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

Druhá přetížení se používá při název tabulky se liší od název třídy: první parametr je název tabulky.

    MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Postup: svázat data do uživatelského rozhraní

Vazby dat zahrnuje tři součásti:

- Zdroje dat
- Rozvržení obrazovky
- Adaptér spojuje dva společně.

V našem ukázkovém kódu vrácených data z tabulky databáze SQL Azure mobilní aplikace **ToDoItem** do matice. Tato akce se řídí určitým vzorem běžných aplikací data.  Databázových dotazů často vrací kolekce řádky, které získá klienta v seznamu nebo pole. V tomto příkladu je pole zdroj dat.

Kód určuje rozvržení obrazovky, který definuje zobrazení data, která se zobrazí na zařízení.  Jsou dva vázaná společně s adaptér, které tento kód rozšíření z **ArrayAdapter&lt;ToDoItem&gt; ** předmětu.

#### <a name="layout"></a>Postup: definování rozložení

Rozložení je definován několik fragmenty kódu XML. Možnost některé ze stávajících rozložení, následující kód představuje **ListView** chceme k naplnění s daty serveru.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

V předchozím kódu atribut *listitem* identifikátor rozložení pro jednotlivé řádky v seznamu. Tento kód určuje zaškrtávacího políčka a příslušný text a získá jednou vytvořena pro každou položku v seznamu. Toto rozložení nezobrazuje pole **id** a složitější rozložení by zadat další pole v zobrazení. V souboru **row_list_to_do.xml** je tento kód.

    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
        <CheckBox
            android:id="@+id/checkToDoItem"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/checkbox_text" />
    </LinearLayout>


#### <a name="adapter"></a>Postup: definování adaptér

Protože zdroji dat naše zobrazení je maticových **ToDoItem**jsme podtřídy naše adaptér ze **ArrayAdapter&lt;ToDoItem&gt; ** předmětu. Tento podtřídy vytvoří zobrazení pro každý **ToDoItem** pomocí **row_list_to_do** rozložení.

V našem kódu je definována následujícím třídy, která je rozšíření **ArrayAdapter&lt;E&gt; ** třídy:

    public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

Přepište metodu **getView** adaptéry. Příklad:

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }

        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


        return row;
    }

Vytvoříme instance tohoto třídy v naší aktivitě následujícím způsobem:

    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

Druhý parametr konstruktoru ToDoItemAdapter je odkaz na požadované rozložení. Teď můžeme vytvořit instanci **ListView** a přiřadit **ListView**adaptér.

    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>Struktura rozhraní API

Operace s tabulkou mobilní aplikace a vlastní volání rozhraní API jsou asynchronní. Použití objektů [budoucí] a [AsyncTask] asynchronní metod zahrnující dotazů, vloží, aktualizace a odstraní. Použití termínu usnadňuje provést více operací na pozadí vlákna aniž byste museli zabývat více vnořené zpětná.

Prohlédněte si soubor **ToDoActivity.java** v projektu Android rychlý úvod z [Azure portál] příklad.

#### <a name="use-adapter"></a>Postup: pomocí adaptér

Teď jste připraveni použít datovou vazbu. Následující kód ukazuje, jak získat položek v tabulce a vyplní místní adaptér vrácených položek.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }

Zavolejte adaptér kdykoli změnit **ToDoItem** tabulku. Protože změn hotovi na základě záznamu podle, zpracovat do jednoho řádku místo kolekce. Po vložení položky **Přidat** metodu volejte adaptér; Při odstraňování, zavolejte metodu **Odebrat** .

##<a name="querying"></a>Postup: načítat data z vaší back-end mobilní aplikaci

Tato část popisuje dotazy vydat backendovou mobilní aplikaci, která zahrnuje tyto věci:

- [Vrátí všechny položky]
- [Filtrování vrácených dat.]
- [Řazení vrátil data]
- [Vrátí data na stránkách]
- [Vyberte konkrétní sloupce]
- [CONCATENATE metod zadávání dotazů](#chaining)

### <a name="showAll"></a>Postup: vrácení všech položek z tabulky

Následující dotaz vrátí všechny položky v tabulce **ToDoItem** .

    List<ToDoItem> results = mToDoTable.execute().get();

*Výsledky* proměnné Umocní nastavení z dotazu jako seznam.

### <a name="filtering"></a>Postup: Filtr vrátil data

Následující spuštění dotazu vrací všechny položky z tabulky **ToDoItem** kde **Dokončeno** – výsledek je **Nepravda**.

    List<ToDoItem> result = mToDoTable.where()
                                .field("complete").eq(false)
                                .execute().get();

**mToDoTable** je odkaz na tabulku služba mobile, který jsme vytvořili v předchozích krocích.

Definování filtru pomocí metody volání **kde** na odkaz tabulka. Metoda **kde** následuje **pole** metoda následovaný metodu, která určuje predikát logické. Možné způsoby predikátu patří **eq** (rovná se), **ne** (není rovno), **gt** (větší než), **stránku** (větší než nebo rovno), **lt** (menší než), **le** (menší než nebo rovno). Tyto metody umožňují porovnání pole typu číslo a řetězec do určité hodnoty.

Můžete filtrovat podle data. Následující metody umožňují porovnání pole celý kalendářního data nebo částí data: **rok**, **měsíc**, **den**, **hodiny**, **minuty**a **druhý**. Následující příklad přidává filtru u položky, jejichž *Termín splnění* je rovno 2013.

    mToDoTable.where().year("due").eq(2013).execute().get();

Následující metody podporují složité filtry na základě řetězce polí: **startsWith** **endsWith**, **propojit**, **podřetězec**, **indexOf**, **Nahradit**, **toLower**, **toUpper**, **střih**a **Délka**. Následující příklad filtrů pro řádky tabulky, kde má *textový* sloupec začíná na "PRI0."

    mToDoTable.where().startsWith("text", "PRI0").execute().get();

U číselných polí jsou podporovány následující metody operátor: **Přidání** **sub**, **mul**, **div**, **mod**, **prostorového uspořádání**, **ceiling**a **Zaokrouhlení**. Následující příklad filtry pro řádky tabulky, kde je **Doba trvání** sudé číslo.

    mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Predikáty můžete kombinovat s těchto logické metod: **a**, **nebo** a **Ne**. Následující příklad spojuje dva z předchozích příkladů.

    mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
                .execute().get();

Seskupení a zabydlete logických operátorů:

    mToDoTable.where()
                .year("due").eq(2013)
                    .and
                (startsWith("text", "PRI0").or().field("duration").gt(10))
                .execute().get();

Podrobnější informace a příklady filtrování najdete v tématu [Seznámení bohatou modelu dotazu Android klienta](http://hashtagfail.com/post/46493261719/mobile-services-android-querying).

### <a name="sorting"></a>Postup: řazení vrátil data

Následující kód vrátí že všechny položky z tabulky **ToDoItems** seřazené vzestupně podle *textového* pole. *mToDoTable* je odkaz na tabulku back-end, který jste vytvořili v předchozích krocích:

    mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

První parametr metodu **Řadit podle** je rovno název pole, podle kterého chcete řadit řetězec. Druhý parametr používá výčet **QueryOrder** můžete určit, zda chcete seřadit vzestupně nebo sestupně.  Pokud filtrujete metodou ***místo, kam*** , musíte metodu ***kde*** vyvolat před metodu ***Řadit podle*** .

### <a name="paging"></a>Postup: vrácení dat na stránkách

První příklad ukazuje, jak chcete-li vybrat prvních pět položek z tabulky. Dotaz vrátí položky z tabulky **ToDoItems**. **mToDoTable** je odkaz na tabulku back-end, který jste vytvořili v předchozích krocích:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Tady je dotaz, který přeskočí prvních pět položky a potom vrátí další pět:

    mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Postup: vyberte konkrétní sloupce

Následující kód ukazuje, jak k vrácení všech položek z tabulky **ToDoItems**, ale **úplný** a **textová** pole se zobrazí pouze. **mToDoTable** je odkaz na tabulku back-end, který jsme vytvořili v předchozích krocích.

    List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

Parametry funkce vyberte jsou řetězce názvy sloupců v tabulce, které chcete vrátit.

**Vyberte** metodu musí dodržovat metody jako **místo, kam** a **Řadit podle**. Může být zahájen metodami stránkování jako **horní**.

### <a name="chaining"></a>Postup: Concatenate metod zadávání dotazů

Mohou být spojeny metodách dotazování back-end tabulek. Zřetězení metod zadávání dotazů umožňuje vybrat konkrétní sloupce filtrované řádky, které jsou seřazená a stránkování. Můžete vytvořit složité logické filtry.
Obou metod dotaz vrátí objekt dotazu. Ukončit řadu metod a skutečně spuštění dotazu, zavolejte na **Spustit** metodu. Příklad:

    mToDoTable.where()
        .year("due").eq(2013)
        .and().startsWith("text", "PRI0")
        .or().field("duration").gt(10)
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
        .execute().get();

Metody zřetězené dotazu musí objednali následujícím způsobem:

1. Filtrování (**kde**) metody.
2. Řazení metod (**Řadit podle**).
3. Výběr (**Vyberte**) metody.
4. stránkovací metody (**Přeskočit** a **Nahoře**).

##<a name="inserting"></a>Postup: vložte data do back-end

Instanci třídy *ToDoItem* a nastavte jeho vlastnosti.

    ToDoItem item = new ToDoItem();
    item.text = "Test Program";
    item.complete = false;

Chcete-li vložit objekt použijte **insert()** :

    ToDoItem entity = mToDoTable.insert(item).get();

Vrácené entity shody data vložená do back-end tabulky zahrnuté ID a dalších hodnot nastavena na back-end.

Mobilní aplikace tabulky musí mít **název – ID**sloupec primárního klíče. Ve výchozím nastavení je v tomto sloupci řetězec. Identifikátor GUID je výchozí hodnota ve sloupci ID.  Můžete zadat další jedinečné hodnoty, například uživatelská jména nebo e-mailové adresy. Není-li řetězcovou hodnotu ID vloženého záznamu, back-end vygeneruje nové GUID.

Hodnoty ID řetězec přináší následující výhody:

+ ID lze vytvořit kalendáře bez vytvoření doby odezvy v databázi.
+ Záznamy budou snadněji sloučit z různých tabulek nebo databáze.
+ Hodnoty ID lepší integrace s logiky aplikace.

Hodnoty ID řetězec jsou **REQUIRED** podpora offline synchronizace.

##<a name="updating"></a>Postup: aktualizace dat v mobilní aplikaci

Aktualizace dat v tabulce, nový objekt **update()** metodě předána.

    mToDoTable.update(item).get();

V tomto příkladu je *Položka* odkaz na jeden řádek v tabulce *ToDoItem* , která byla nějaký provedené změny.
Na řádek se stejným **id** se aktualizuje.

##<a name="deleting"></a>Postup: odstranění dat v mobilní aplikaci

Následující kód ukazuje, jak se odstraní data z tabulky zadáním dat objektu.

    mToDoTable.delete(item);

Můžete taky odstranit položky zadáním pole **id** řádek pro odstranění.

    String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
    mToDoTable.delete(myRowId);

##<a name="lookup"></a>Postup: vyhledat určitou položku

Vyhledání položky s polem specifické **id** **lookUp()** způsobem:

    ToDoItem result = mToDoTable
                        .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
                        .get();

##<a name="untyped"></a>Postup: práce s daty bez typu

Bez typu model programování vám přesné publikum nemůže ovládat serializaci JSON.  Existují některé běžné scénáře, které můžete chtít použít bez typu model programování. Pokud například back-end tabulka obsahuje hodně sloupců a potřebujete odkazovat podmnožinu sloupce.  Zadaný modelu vyžaduje definovat všechny mobilních aplikací sloupce tabulky ve svojí třídě data.  

Většina volání rozhraní API pro přístup k datům je podobný zadaný programovací volání. Hlavní rozdíl je, že v modelu, bez typu vyvoláte metody **MobileServiceJsonTable** objektu místo **MobileServiceTable** objektu.

### <a name="json_instance"></a>Postup: vytvoření instance bez typu obsahu

Podobně jako zadaný modelu začnete získáním odkaz na tabulku v tomto případě je však **MobileServicesJsonTable** objektu. Získat odkaz tak, že zavoláte **jít** metoda na instanci klienta:

    private MobileServiceJsonTable mJsonToDoTable;
    //...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Po vytvoření instance **MobileServiceJsonTable**má prakticky stejné rozhraní API jsou k dispozici jako se zadaným programovací model. V některých případech trvat metody bez typu parametr místo zadaný parametr.

### <a name="json_insert"></a>Postup: vložte do bez typu tabulky

Následující kód ukazuje školá vložení. Cílem prvního kroku je vytvořit [JsonObject][1], která je součástí [gson] [ 3] knihovny.

    JsonObject jsonItem = new JsonObject();
    jsonItem.addProperty("text", "Wake up");
    jsonItem.addProperty("complete", false);

Potom umožňuje **insert()** vložení bez typu objektu do tabulky.

    mJsonToDoTable.insert(jsonItem).get();

Pokud potřebujete k získání ID vloženého objektu, použijte metodu **getAsJsonPrimitive()** .

    jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Postup: odstranit z bez typu tabulky

Následující kód ukazuje, jak odstranit instanci, v tomto případě stejné instanci aplikace **JsonObject** vytvořený v příkladu předchozí *Vložit* . Kód je shodný s zadaná písmena, ale metoda má jiného podpisu, protože odkazuje **JsonObject**.

         mToDoTable.delete(jsonItem);

Můžete taky odstranit instanci přímo pomocí své ID:

         mToDoTable.delete(ID);

### <a name="json_get"></a>Postup: vrácení všech řádků z tabulky bez typu

Následující kód ukazuje, jak získat celou tabulku. Vzhledem k tomu, že používáte JSON tabulku, můžete jenom některé sloupce v tabulce selektivně načíst.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Stejné filtrování, filtrování a stránkování metody, které jsou k dispozici pro zadaný modelu jsou dostupné pro bez typu model.

##<a name="custom-api"></a>Postup: volání vlastní rozhraní API

Vlastní rozhraní API umožňuje definovat vlastní koncové body zaznamenávající serveru funkce, které nejsou mapování pro vložení, aktualizovat, odstranit nebo operace čtení. Pomocí vlastní rozhraní API můžete mít větší kontrolu nad zpráv, včetně čtení a nastavení protokolu HTTP záhlaví zpráv a definování formát textu zprávy než JSON.

V klientovi Android zavoláte metodu **invokeApi** volání vlastní koncový bod rozhraní API. Následující příklad ukazuje, jak volání koncový bod rozhraní API s názvem **completeAll**, která vrací třídu kolekce s názvem **MarkAllResult**.

    public void completeItem(View view) {

        ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

            Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }

                @Override
                public void onSuccess(MarkAllResult result) {
                    createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
                    refreshItemsFromTable();
                }
            });
        }

Metoda **invokeApi** se nazývá na straně klienta, která odešle žádost o příspěvek na nové vlastní rozhraní API. Vlastní rozhraním API vrácený výsledek se zobrazí v dialogu zprávy, jako jsou všechny chyby. Další verze **invokeApi** vám umožní volitelně odeslat objekt v hlavním textu žádosti o, určete způsob HTTP a poslat parametrů dotazu s žádostí o. Bez typu verze **invokeApi** jsou k dispozici taky.

##<a name="authentication"></a>Postup: přidání ověřování aplikace

Výukové programy pro už popisují podrobně přidat tyto funkce.

Služba aplikace podporuje [ověřování uživatelů aplikace](app-service-mobile-android-get-started-users.md) pomocí různých poskytovatelů identit externí: Facebook Google, Account Microsoft, Twitteru a Azure Active Directory. Nastavení oprávnění pro tabulky, které chcete omezit přístup pro konkrétní operace pouze pro ověřeného uživatele. Můžete také identitu ověřených uživatelů ve vaší back-end implementovat ověřovacích pravidel.

Dva toků ověřování podporují: na **serveru** a **klientských** tok. Tok serveru poskytuje možnosti nejjednodušší ověřování závisí na webového rozhraní poskytovatelů identit.  Žádná další SDK vyžadované pro implementaci tok ověřování serveru. Tok ověřování serveru Microsoft neposkytuje žádnou rozsáhlá integrace do mobilního zařízení a doporučujeme použít pouze pro doklad koncept scénáře.

Závisí na SDK poskytovanou zprostředkovatele identit tok klienta umožňuje hlubší integrace s funkce specifické pro zařízení, například jednotného přihlašování.  Můžete třeba rozšiřte mobilní aplikace Facebook SDK.  Mobilních klientů swapy do aplikace Facebook a potvrdí vaše přihlášení před výměna zpátky na svůj mobilní aplikace.

Čtyři kroky nutné povolit ověřování v aplikaci:

- Registrace aplikace pro ověření se zprostředkovatelem identit.
- Nakonfigurujte aplikaci služby backendovou.
- Omezení oprávnění tabulky pro ověřeného uživatele pouze v back-end aplikaci služby.
- Přidání kódu pro ověřování aplikace.

Nastavení oprávnění pro tabulky, které chcete omezit přístup pro konkrétní operace pouze pro ověřeného uživatele. Můžete také ID zabezpečení ověřeného uživatele k úpravám žádosti.  Další informace: kontrolujte potvrzení [Začínáme s ověřováním] a dokumentaci serveru SDK postupy.

### <a name="caching"></a>Postup: Přidání kódu pro ověřování aplikace

Následující kód spustí proces přihlášení toku serveru pomocí zprostředkovatele Google:

    MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

Získání ID přihlášeného uživatele z **MobileServiceUser** metodou **getUserId** . Příklad použití termínu volání asynchronní přihlášení rozhraní API naleznete v tématu [Začínáme s ověřováním].

### <a name="caching"></a>Postup: mezipaměti tokeny ověřování

Ukládání do mezipaměti tokeny ověřování vyžaduje, abyste ukládání ID uživatele a ověřovací token místně do zařízení. Při příštím spuštění aplikace zaškrtněte mezipaměti, a pokud existují tyto hodnoty, můžete přeskočit protokol postupu a rehydrate klient s těmito daty. Ale tato data se rozlišují malá písmena a by měly být uloženy zašifrovaných pro zabezpečení v případě, že získá krádeže telefonu.

Zobrazí úplný příklad toho, jak do mezipaměti ověřovacích tokenů v [mezipaměti ověřování tokeny části][7].

Když se pokusíte použít token vypršela platnost, dostanete odpověď *401 neověřené* . Můžete obsloužení chyb ověřování pomocí filtrů.  Filtry zachytit žádosti o aplikaci služby back-end. Kód filtru testuje odpověď 401, spustí proces přihlášení a pak obnoví žádost, které vygenerovalo 401. 

## <a name="adal"></a>Postup: ověření uživatelé, kteří mají Active Directory Authentication Library

Můžete Active Directory ověřování knihovny (ADAL) a přihlaste se do aplikace pomocí služby Azure Active Directory uživatele. Použití přihlášení toku klienta je často výhodnější `loginAsync()` metody ZobrazitFormulář.aspx obsahuje více nativní chování činnosti koncového uživatele a umožňuje dalších úprav.

1. Konfigurace back-end vaše mobilní aplikace pro AAD přihlásit pomocí následujících kurz [jak nakonfigurovat aplikaci služby Active Directory přihlášení](app-service-mobile-how-to-configure-active-directory-authentication.md) . Zkontrolujte, že uděláte volitelné registrace aplikace native client –.

2. Nainstalujte ADAL změnou zahrnout tyto definice v souboru build.gradle:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. Přidání kódu pro následující aplikace, díky následující nahrazení:

* **Vložení AUTORITA TADY** nahraďte názvem klienta, ve kterém zřízení aplikace. Formát by měl být https://login.windows.net/contoso.onmicrosoft.com. Tato hodnota můžete zkopírovat z karty Domain Azure Active Directory [Azure klasické portálu].

* **Vložení zdroje ID TADY** nahraďte ID klienta pro mobilní aplikaci backendovou. Na kartě **Upřesnit** v části **Nastavení služby Azure Active Directory** v portálu můžete získat kód klienta.

* Nahraďte **Vložení klienta ID TADY** jste zkopírovali z aplikace native client – ID klienta.

* Nahraďte **Vložení PŘESMĚROVÁNÍ URI TADY** svého webu _/.auth/login/done_ koncový bod, pomocí schématu HTTPS. Tato hodnota by měl být stejný _https://contoso.azurewebsites.net/.auth/login/done_.

        private AuthenticationContext mContext;

        private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
        }

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
        };

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
        }

## <a name="how-to-add-push-notification-to-your-app"></a>Postup: Přidání nabízená oznámení pro aplikace

Můžete [Číst přehled] [ 6] , který popisuje jak Microsoft Azure oznámení rozbočovače podporuje širokou škálu nabízená oznámení.  V [tomto kurzu][5], nabízená oznámení odeslaný na všech zařízeních pokaždé, když je vložen záznamu.

## <a name="how-to-add-offline-sync-to-your-app"></a>Postup: Přidání offline synchronizace do aplikace

Rychlý úvod kurz obsahuje kód implementující offline synchronizace. Najděte kód předponou poznámky:

    // Offline Sync

Uncommenting následující řádky kódu můžete zavést offline synchronizace a podobný kód můžete přidat do jiných aplikací Mobile kódu.

##<a name="customizing"></a>Jak se: přizpůsobení klienta

Existuje několik způsobů můžete přizpůsobit výchozí chování klienta.

### <a name="headers"></a>Jak se: přizpůsobení požádat o záhlaví

Konfigurace **ServiceFilter** přidáte vlastní záhlaví HTTP na každou žádost:

    private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                    ServiceFilterRequest request,
                    NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
                    request.addHeader("My-Header", "Value");                    }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Jak se: přizpůsobení serializace

Klient předpokládá, že názvy tabulek, názvy sloupců a datové typy v back-end všechny odpovídají přesně datové objekty definované v klientovi. Může být libovolný počet důvodů, proč nemusí název serveru a klientských neodpovídá. Nefunguje můžete chtít udělat následující typy vlastní nastavení:

- Názvy sloupců použít v tabulce aplikace služeb není shodovat s názvy, které používáte v klientovi.
- Použijte tabulku služba aplikací, která má jiný název než předmětu, který odpovídá v klientovi.
- Zapněte automatické vlastnost velkými a malými písmeny.
- Přidání komplexní vlastnosti k objektu.

### <a name="columns"></a>Postup: mapování soukromou klienta a serveru

Předpokládejme, že kód klienta Java používá standardní názvy Java styl pro vlastnosti objektu **ToDoItem** , jako jsou třeba následující vlastnosti:

- Funkce mId
- mText
- mComplete
- mDuration

Serializuje názvům klienta do JSON názvy, které shodovat s názvy sloupců v tabulce **ToDoItem** na serveru. Následující kód používá [gson] [ 3] knihovny opatřit vlastnosti:

    @com.google.gson.annotations.SerializedName("text")
    private String mText;

    @com.google.gson.annotations.SerializedName("id")
    private int mId;

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;

    @com.google.gson.annotations.SerializedName("duration")
    private String mDuration;

### <a name="table"></a>Postup: mapování názvů jiné tabulky mezi klientem a back-end

Mapování názvu tabulky klienta na název tabulky různých mobilních služeb pomocí přepsání [getTable()] [ 4] metodu:

    mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Postup: automatizovat mapování názvů sloupců

Můžete zadat převodu strategie, která platí pro všechny sloupce pomocí [gson] [ 3] rozhraní API. Knihovna Android klienta používá [gson] [ 3] pozadí serializovat Java objektů k JSON dat pro aplikaci služby Azure se odesílá data.  Následující kód používá metodu **setFieldNamingStrategy()** nastavovaná strategie. V tomto příkladě se odstraní počáteční znak ("m") a potom malá písmena na následující znak pro každý název pole. Například ho by přeměna "část" "id."

    client.setGsonBuilder(
        MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(new FieldNamingStrategy() {
            public String translateName(Field field) {
                String name = field.getName();
                return Character.toLowerCase(name.charAt(1))
                    + name.substring(2);
                }
            });

Před použitím **MobileServiceClient**se provedou tento kód.

### <a name="complex"></a>Postup: ukládání vlastnosti objektu nebo pole do tabulky

Zatím naše serializace příklady zahrnovaly základní typy například řetězce a celá čísla.  Základní typy snadno serializuje do formátu JSON.  Pokud chceme složité objekt, který není serializovat automaticky doplnit JSON, potřebujeme Poskytněte metodu serializace JSON.  Příklad toho, jak zajistit vlastní serializaci JSON, můžete prohlížet příspěvku na blogu [přizpůsobení serializaci pomocí knihovnu gson v klientovi mobilních služeb Android][2].

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Vrátí všechny položky]: #showAll
[Filtrování vrácených dat.]: #filtering
[Řazení vrátil data]: #sorting
[Vrátí data na stránkách]: #paging
[Vyberte konkrétní sloupce]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portálu]: https://portal.azure.com
[Začínáme s ověřování]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Budoucnosti]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html