<properties 
    pageTitle="Použití Azure úložiště s řešením průběžné integrace Jenkins | Microsoft Azure" 
    description="Tento kurz ukazují, jak používat službu objektů blob Azure jako úložiště pro vytvoření artefakty vytvořil Jenkins nepřetržitý integrace řešení." 
    services="storage" 
    documentationCenter="java" 
    authors="dineshmurthy" 
    manager="jahogg" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="dinesh"/>

# <a name="using-azure-storage-with-a-jenkins-continuous-integration-solution"></a>Použití úložiště Azure s řešením Jenkins nepřetržitý integrace

## <a name="overview"></a>Základní informace

Tyto informace ukazuje, jak používat úložiště objektů Blob jako úložiště sestavení artefaktů vytvořil řešení Jenkins nepřetržitý integrace (odvětví) nebo jako zdroj soubory ke stažení pro použití v proces vytváření. Jednou scénáře, které by vás to užitečné je, když jste kódování v aktivní vývojové prostředí (pomocí jazyka Java nebo jiných jazyků), buildy založené na běží nepřetržitý integrace a potřebujete úložiště pro vaše artefakty vytvořit tak, aby mohl, například je sdílet s ostatními členy organizace vaši zákazníci nebo udržovat archivu. Jiný scénář je, když práce sestavení samotné vyžaduje jiných souborů, například závislosti ke stažení jako součást sestavení při zadávání.

V tomto kurzu budete používat modul plug-in Azure úložiště pro Jenkins odvětví k dispozici microsoftem.

## <a name="overview-of-jenkins"></a>Základní informace o Jenkins ##

Jenkins umožňuje vývojářům snadno integrovat jejich změny kódu nepřetržitý integraci projektu software a buildy vyrobit automaticky často, a tím zvýšení produktivity vývojáři. Sestavení jsou verzí a artefakty Tvůrce dotazů je možné uložit do různých úložištích. Toto téma se předvedení použití úložišti objektů blob Azure jako úložiště artefakty Tvůrce dotazů. Zobrazí také stažení závislosti z úložiště objektů blob Azure.

Další informace o Jenkins můžete najít na [Zahájit Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins).

## <a name="benefits-of-using-the-blob-service"></a>Výhody používání služby objektů Blob ##

Výhody používání služby objektů Blob k hostování vašeho artefakty sestavení aktivní vývoj patří:

- Dostupnost vaší artefakty Tvůrce dotazů a/nebo ke stažení závislosti.
- Výkon při řešení Jenkins odvětví nahraje artefakty Tvůrce dotazů.
- Výkon při vašich zákazníků a partnerů stáhnout artefakty Tvůrce dotazů.
- Zásady přístupu uživatele v volba mezi anonymní přístup, na základě vypršení platnosti sdílené podpis přístupu, soukromé ovládat přístup, atd.

## <a name="prerequisites"></a>Zjistit předpoklady pro ##

Postupujte podle následujících pokynů používat službu objektů Blob s Jenkins odvětví řešení:

- Nepřetržitý integrace Jenkins řešení.

    Pokud aktuálně nemáte Jenkins odvětví řešení, můžete spustit Jenkins odvětví řešení pomocí následující postup:

    1. Na počítači, u kterých je povolené Java stahování jenkins.war <http://jenkins-ci.org>.
    2. Na příkazovém řádku, který je otevřen do složky obsahující jenkins.war spusťte:

        `java -jar jenkins.war`

    3. V prohlížeči otevřete `http://localhost:8080/`. Otevře se Jenkins řídicí panel, který budete používat můžete nainstalovat a nakonfigurovat modul plug-in Azure úložiště.

        Pro účely tohoto návodu bude postačovat během typické řešení Jenkins odvětví by nastavit tak, aby spustili služba Jenkins war na příkazovém řádku.

- Účet Azure. Můžete zaregistrovat Azure účet u <http://www.azure.com>.

- Účet Azure úložiště. Pokud ještě nemáte účet úložiště, můžete vytvořit jednu pomocí kroků při [Vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account).

- Znalost řešení Jenkins odvětví doporučujeme ale nejsou nutné, protože následující obsah bude používat příklad základní k zobrazení, která kroky potřebné při používání služby objektů Blob jako úložiště pro Jenkins odvětví sestavit artefakty.

## <a name="how-to-use-the-blob-service-with-jenkins-ci"></a>Jak používat službu objektů Blob s Jenkins odvětví ##

Používat službu objektů Blob Jenkins, budete potřebovat instalovat modul plug-in Azure úložiště, nakonfigurovat modul plug-in používat účet úložiště a vytvořte po sestavení akci, která odešle sestavení artefakty ke svému účtu úložiště. V následujících částech jsou popsány tyto kroky.

## <a name="how-to-install-the-azure-storage-plugin"></a>Jak nainstalovat plug-in úložišti Azure ##

1. V řídicím panelu Jenkins klikněte na **Spravovat Jenkins**.
2. Na stránce **Spravovat Jenkins** klepněte na položku **Spravovat doplňky**.
3. Klikněte na kartu **k dispozici** .
4. V části **Artefakt Uploaders** zaškrtněte **modul plug-in úložišti tabulek Microsoft Azure**.
5. Klikněte na **nainstalovat bez restart** nebo **Stáhnout a nainstalovat po restartování**.
6. Restartujte Jenkins.

## <a name="how-to-configure-the-azure-storage-plugin-to-use-your-storage-account"></a>Jak nakonfigurovat modul plug-in úložišti Azure používat účet úložiště ##

1. V řídicím panelu Jenkins klikněte na **Spravovat Jenkins**.
2. Na stránce **Spravovat Jenkins** klikněte na **Konfigurovat systém**.
3. V části **Konfigurace účtu Microsoft Azure úložiště** :
    1. Zadejte název účtu úložiště, kterou můžete získat z [Portálu Azure](https://portal.azure.com).
    2. Zadejte svůj úložiště klíč účtu, taky získat z [Portálu Azure](https://portal.azure.com).
    3. Pokud používáte veřejný Azure cloudu, použijte výchozí hodnotu pro **Objektů Blob adresy URL koncového bodu služby** . Pokud používáte jiný Azure obláčkem, použijte koncový bod určené [Azure portál](https://portal.azure.com) účtu úložiště. 
    4. Klikněte na **Ověřit úložiště pověření** k ověření účtu úložiště. 
    5. [Volitelné] Pokud jste ještě další úložiště účty, které se má k dispozici Jenkins odvětví, klikněte na **Přidat další účty úložiště**.
    6. Klepnutím na tlačítko **Uložit** uložte nastavení.

## <a name="how-to-create-a-post-build-action-that-uploads-your-build-artifacts-to-your-storage-account"></a>Jak vytvořit po sestavení akci, která odešle sestavení artefakty ke svému účtu úložiště ##

Pro účely instrukční nejdřív jsme muset vytvořit projekt, vytvořit více souborů a poté přidejte po sestavení akci, kterou chcete odeslat soubory ke svému účtu úložiště.

1. V řídicím panelu Jenkins klikněte na **Nová položka**.
2. Název úlohy **MyJob**, klikněte na **vytvořit projekt software zase uvolnit stylu**a klikněte na **OK**.
3. V části **Vytvoření** konfigurace projektu klikněte na **Přidat sestavit kroku** a zvolte **příkaz dávky spuštění Windows**.
4. V **příkazu**použijte následující příkazy:

        md text
        cd text
        echo Hello Azure Storage from Jenkins > hello.txt
        date /t > date.txt
        time /t >> date.txt
 
5. V části **Akce po sestavení** konfigurace projektu klikněte na **Přidat po sestavení akce** a zvolte **Odeslat artefakty k úložišti objektů Blob Azure**.
6. **Název účtu úložiště**vyberte úložiště účet, který chcete použít.
7. **Jméno Container**zadejte jméno container. (Kontejneru bude vytvořen, pokud ho ještě neexistuje po odeslání artefakty sestavení.) Můžete použít proměnné, takže v tomto příkladu zadejte **${JOB_NAME}** jako jméno container.

    **Tip:**
    
    **Příkaz** oddílu místo, kam jste zadali skriptu pro **spuštění Windows dávkové příkaz** následuje odkaz proměnné rozpozná Jenkins. Klikněte na tento odkaz zobrazíte v názvech proměnných prostředí a popisy. Všimněte si, že nejsou povoleny proměnné, které obsahují speciální znaky, například proměnnou prostředí **BUILD_URL** jako jméno container nebo běžné virtuální cestu.

8. V tomto příkladu klikněte na **zveřejnit nového kontejneru ve výchozím nastavení** . (Pokud chcete použít kontejner soukromé, musíte vytvořit sdílený přístup podpis, který chcete povolit přístup. To je nad rámec v tomto tématu. Je další informace o sdílený přístup podpisů v [Používání sdílené přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Volitelné] Klepnutím na tlačítko **Vyčistit kontejneru před nahráním** kontejneru vymazat obsah před sestavení artefakty odeslány (ponechat zrušené zaškrtnutí políčka Pokud nebudete chtít vyčistit obsah kontejneru).
10. **Seznam artefakty nahrát**, zadejte * *textu /*txt**.
11. **Běžné virtuální cesty pro nahrané artefakty**pro účely tohoto kurzu, zadejte **${SESTAVIT\_ID} / ${SESTAVIT\_číslo}**.
12. Klepnutím na tlačítko **Uložit** uložte nastavení.
13. Na řídicím panelu Jenkins klikněte na **Vytvořit** spusťte **MyJob**. Prohlédněte si konzoly výstup pro stav. Zprávy o stavu služby Azure úložiště bude obsahovat výstup konzoly při spuštění akce po sestavení nahrát artefakty Tvůrce dotazů.
14. Po úspěšném projektu můžete zkontrolovat artefakty sestavení otevřením veřejné objektů blob.
    1. Přihlaste se k [portálu Azure](https://portal.azure.com).
    2. Klikněte na **úložiště**.
    3. Klikněte na název účtu úložiště, který jste použili pro Jenkins.
    4. Klikněte na **kontejnery**.
    5. Klikněte na kontejner **myjob**, což je malá verze název úlohy přiřazené při vytvoření Jenkins projektu. Kontejner názvy a názvy objektů blob se malá písmena (malá a velká písmena) v Azure úložiště. V seznamu objektů blob pro kontejner **myjob** byste měli vidět **hello.txt** a **date.txt**. Zkopírujte adresu URL pro jednu z těchto položek a otevřete ho v prohlížeči. Zobrazí se textový soubor, který byl odeslán jako artefaktem Tvůrce dotazů.

Pouze jeden po sestavení akci, která odešle artefakty k úložišti objektů blob Azure se dají vytvořit na projekt. Všimněte si, že jeden po sestavení akci, kterou chcete odeslat artefakty k úložišti objektů blob Azure můžete zadat různých souborů (včetně zástupné znaky) a cest k souborům v **Seznamu artefakty nahrát** pomocí středníku jako oddělovač. Například pokud Jenkins sestavení vytvoří SKLENICE soubory a soubory ve formátu TXT ve složce **vytvořit** pracovní prostor a chcete nahrát k úložišti objektů blob Azure, použijte následující hodnoty **Seznamu artefakty nahrát** : **Sestavit /\*.jar; vytvořit /\*txt**. Syntaxe dvojité dvojtečky můžete také zadat cestu k použití v názvu objektů blob. Například chcete sklenic po g k získání nahráli získat nahráli pomocí **oznámení** v parametru path objektů blob pomocí **binární soubory** v objektů blob cesty a soubory TXT, použijte následující hodnoty **Seznamu artefakty nahrát** : **Sestavit /\*. jar::binaries; vytvořit /\*. txt::notices**.

## <a name="how-to-create-a-build-step-that-downloads-from-azure-blob-storage"></a>Jak vytvořit sestavení krok, který stáhne úložišti objektů blob Azure ##

Podle těchto kroků ukazují, jak nakonfigurovat krok sestavení stahování položek v úložišti objektů blob Azure. To by být užitečné, pokud chcete zahrnout do svého vytvořit položky, například sklenic po g, které uchováváte v Azure objektů blob úložiště.

1. V části **Vytvoření** konfigurace projektu klikněte na **Přidat sestavit kroku** a vyberte **stáhnout z úložiště objektů Blob Azure**.
2. **Název účtu úložiště**vyberte úložiště účet, který chcete použít.
3. **Jméno Container**zadejte jméno container s objekty BLOB, které chcete stáhnout. Můžete použít proměnné.
4. **Kulatý název**zadejte název objektů blob. Můžete použít proměnné. Také můžete hvězdičku, jako zástupný znak po zadání počáteční písmena názvu objektů blob. Například **projektu\* ** vyberete všechny objekty BLOB jejichž názvy začínají jiným **projektu**.
5. [Volitelné] **Stáhněte si cestu**zadejte cestu Jenkins počítače, ve které chcete stahovat soubory v úložišti objektů blob Azure. Proměnné lze také. (Pokud nezadáte hodnoty pro **stažení cestu**, soubory v úložišti objektů blob Azure bude stahovat do pracovního prostoru projektu.)

Pokud jste ještě další položky, které chcete stáhnout z úložiště objektů blob Azure, můžete vytvořit další kroky.

Po spuštění sestavení můžete zkontrolovat výstup konzoly historie sestavení nebo podívejte se na svého umístění pro stažení, najdete v článku zda byly úspěšně stáhli objektů BLOB, kterou hledáte.  

## <a name="components-used-by-the-blob-service"></a>Součásti používaný službou objektů Blob ##

Následující přehled součástí objektů Blob služby.

- **Úložiště účtu**: dokončení všech přístup k základnímu úložišti Azure pomocí účtu úložiště. Toto je nejvyšší úroveň názvů pro přístup k objektů BLOB. Účet může obsahovat neomezený počet kontejnerů, dokud jejich celková velikost v části 100TB.
- **Kontejner**: kontejneru poskytuje seskupení sadu objektů BLOB. Všechny objekty BLOB musí být v kontejneru. Účet může obsahovat neomezený počet kontejnery. Kontejner mohou být uloženy neomezený počet objektů BLOB.
- **Kulatý**: soubor typu a velikosti. Existují dva typy objektů BLOB uložené v úložišti Azure: objektů BLOB bloku a stránku. Většina soubory jsou objekty BLOB blokovat. Jeden blok objektů blob může být až do velikosti 200GB. Tento kurz používá objektů BLOB blokovat. Objekty BLOB stránky, jiný typ objektů blob, může být až 1TB v velikost a efektivnější změně rozsah bajtů v souboru jsou často. Další informace o objektů BLOB najdete v článku [Principy blok objektů BLOB, přidat objektů BLOB a objekty BLOB stránky](http://msdn.microsoft.com/library/azure/ee691964.aspx).
- **Formát adresy URL**: objektů BLOB jsou s možností zadání pomocí následující formát adresy URL:

    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
    
    (Formát výše platí pro veřejný Azure cloudu. Pokud používáte jiný Azure cloudu pomocí koncového bodu v [Portálu Azure](https://portal.azure.com) zjistíte koncový bod adresy URL.)

    Ve formátu výše uvedená `storageaccount` představuje název účtu úložiště `container_name` představuje název svého kontejneru a `blob_name` odpovídá názvu vaší objektů blob v tomto pořadí. V rámci jeho jméno container, může mít více cest odděleni lomítko, **/**. Jméno container příklad v tomto kurzu se **MyJob**a **${SESTAVIT\_ID} / ${SESTAVIT\_číslo}** byla použita pro běžné virtuální cestu výsledkem objektů blob s URL následující formulář:

    `http://example.blob.core.windows.net/myjob/2014-04-14_23-57-00/1/hello.txt`

## <a name="next-steps"></a>Další kroky

- [Zahájit Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Meet+Jenkins)
- [Azure úložiště SDK jazyka Java](https://github.com/azure/azure-storage-java)
- [Azure úložiště klienta SDK Reference](http://dl.windowsazure.com/storage/javadoc/)
- [Služby Azure úložiště rozhraní REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx )
- [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage/)

Další informace najdete taky v [Středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).