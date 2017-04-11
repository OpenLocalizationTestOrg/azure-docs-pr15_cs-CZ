<properties
   pageTitle="Základní informace o řízení přístupu v úložišti jezera | Microsoft Azure"
   description="Porozumět tomu, jak získat přístup k ovládací prvek v úložišti jezera dat Azure"
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
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="access-control-in-azure-data-lake-store"></a>Řízení přístupu v úložišti jezera dat Azure

Úložiště jezera dat používá model řízení přístupu, která je odvozena z HDFS a zároveň model řízení přístupu POSIX. Tento článek shrnuje základy model řízení přístupu k úložišti jezera. Další informace o HDFS k řízení modelu příručce [HDFS oprávnění](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="access-control-lists-on-files-and-folders"></a>Seznamy řízení přístupu k souborům a složkám

Existují dva typy přístupu seznamů (ACL) - **ACL přístup** a **Výchozí ACL**.

* **ACL access** – tyto řízení přístupu k objektu. Soubory a složky mít přístup ACL.

* **Výchozí ACL** – "Šablony" ACL přidružené do složky, které určují ACL přístup pro všechny podřízené položky vytvořené v této složce. Soubory nemají výchozí ACL.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

Access ACL a výchozí ACL mají stejnou strukturu.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-2.png)

>[AZURE.NOTE] Změna výchozí ACL u nadřazené neovlivní přístupu ACL nebo výchozí ACL podřízených položek, které už existuje.

## <a name="users-and-identities"></a>Uživatelé a identity

Všechny soubory a složky obsahuje jedinečných oprávnění pro tyto identity:

* Vlastní uživatel souboru
* Vlastní skupiny
* Pojmenovaná uživatelů
* Pojmenované skupiny
* Všichni ostatní uživatelé

Identity uživatelům a skupinám jsou Azure Active Directory (AAD) identit tak pokud není uvedeno jinak "uživatel", v kontextu jezera úložiště, buď znamenat AAD uživatele nebo skupinu zabezpečení AAD.

## <a name="permissions"></a>Oprávnění

Oprávnění k objektu systému souborů se **pro čtení**a **zápis**, a **spouštět** a jejich mohou sloužit k souborům a složkám uvedeno v následující tabulce.

|            |    Soubor     |   Složka |
|------------|-------------|----------|
| **Read (R)** | Můžete číst obsah souboru | Vyžaduje **Číst** a **spouštět** zobrazit obsah složky.|
| **Zápis (W)** | Můžete psát nebo připojit k souboru | Vyžaduje **Zapisovat a spouštět** k vytvoření podřízených položek ve složce. |
| **Spouštět (X)** | Neznamená cokoli v kontextu jezera úložiště dat | Je potřebná k procházení podřízených položek do složky. |

### <a name="short-forms-for-permissions"></a>Krátké formulářů pro oprávnění

**RWX**slouží k označení **Číst + zápis spustit**. Existuje více zúžené číselné formuláře ve kterém **čtení = 4**, **napište = 2**, a **spouštění = 1** a jejich součet představuje oprávnění. Tady je několik příkladů.

| Číselné formuláře | Krátký formulář |      Co to znamená:     |
|--------------|------------|------------------------|
| 7            | RWX        | Přečtěte si + zápis spustit |
| 5            | R-X        | Čtení + spustit         |
| 4            | R –        | Pro čtení                   |
| 0            | ---        | Žádná oprávnění         |


### <a name="permissions-do-not-inherit"></a>Není dědit oprávnění

Modelu POSIX stylu používaného úložiště jezera dat jsou oprávnění pro položky uložené na samotné položky. Jinými slovy nemůžete oprávnění pro položky dědí od nadřazené položky.

## <a name="common-scenarios-related-to-permissions"></a>Obvyklé scénáře týkající se oprávnění

Tady jsou některé běžné scénáře, pokud chcete zjistit, jaká oprávnění jsou potřebné k provedení určité operace účet jezera úložiště.

### <a name="permissions-needed-to-read-a-file"></a>Oprávnění potřebná k načtení souboru

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* K souboru ke čtení - volající potřebuje oprávnění **ke čtení**
* Pro všechny složky ve struktuře složek, které obsahují soubor - volající potřebuje **spouštět** oprávnění

### <a name="permissions-needed-to-append-to-a-file"></a>Oprávnění potřebná pro připojení k souboru

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* U souboru, který má být přidán k - volající potřebuje oprávnění k **zápisu**
* Pro všechny složky, které obsahují soubor - volající potřebuje **spouštět** oprávnění

### <a name="permissions-needed-to-delete-a-file"></a>Oprávnění potřebná při odstraňování souboru

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* Pro nadřazenou složku - volající potřebuje **zápis spustit** oprávnění
* Pro všechny ostatní složky v cesty k souboru –, volající musí oprávnění **spouštění**

>[AZURE.NOTE] Napište oprávnění k souboru není potřeba odstranit soubor, dokud dvou výše uvedených podmínek.

### <a name="permissions-needed-to-enumerate-a-folder"></a>Oprávnění potřebná pro výčet složky

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* Pro složku, kterou chcete vytvořit výčet - volající potřebuje oprávnění **ke čtení + Execute**
* Pro všechny nadřazené složky - volající potřebuje **spouštět** oprávnění

## <a name="viewing-permissions-in-the-azure-portal"></a>Zobrazení oprávnění v portálu Azure

**Průzkumník dat** zásuvné účtu úložiště jezera dat klikněte na **přístup** k zobrazení seznamů ACL pro soubor nebo složku. V následující snímek klikněte na přístup k zobrazení seznamů ACL pro složku **katalogu** v části účet **mydatastore** .

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

Poté na zásuvné **aplikace Access** klikněte na **Jednoduché zobrazení** jednodušší zobrazení.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

Klikněte na **Rozšířené zobrazení** rozšířené zobrazení.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a>Superuživatelů

Super uživatel nemá nejvíce práva jiných vlastníků všechny uživatele v úložišti jezera. Superuživatelů:

* s oprávněním RWX **všech** souborů a složek
* můžete změnit oprávnění k souboru nebo složky.
* můžete změnit vlastní uživatele nebo vlastní skupiny souboru nebo složky.

V Azure účtu úložiště jezera dat obsahuje několik Azure role:

* Vlastníci
* Přispěvatelům
* Čtenáři
* Atd.

Všichni účastníci roli **Vlastníci** účtu úložiště jezera dat je automaticky výhradní právo uživatele k tomuto účtu. Další informace o Azure Role na základě přístup ovládacího prvku (RBAC) najdete v článku [řízení přístupu na základě rolí](../active-directory/role-based-access-control-configure.md).

## <a name="the-owning-user"></a>Vlastní uživatele

Uživatel, který vytvořené položky se automaticky vlastní uživatel položky. Vlastní uživatel může:

* Změna oprávnění soubor, který vlastní
* Změna vlastní skupiny soubor, který vlastní, dokud vlastní uživatel členem taky cílové skupiny.

>[AZURE.NOTE] Vlastní uživatele **není možné** změnit vlastní uživatele jiného vlastněná souboru. Pouze výhradní právo uživatelé mohou měnit vlastní uživatele souboru nebo složky.

## <a name="the-owning-group"></a>Vlastní skupiny

V ACL POSIX každý uživatel souvisí s "primární skupinu". Uživatele "alice" může například patří do skupiny "finance". Alice může patří do více skupin, ale jedné skupině vždy označen jako svou primární skupinu. POSIX když Alice vytvoří soubor vlastní skupiny tento soubor je nastavené svou primární skupinu v tomto případě je "finance".
 
Po vytvoření nové položky systému souborů úložiště jezera dat přiřadí hodnotu do vlastní skupiny. 

* **Případ 1** - kořenové složce "/". Této složky se vytvoří při vytvoření účtu jezera úložiště. V tomto případě vlastní skupiny nastavena na uživatele, kterému vytvoření účtu.
* **Případ 2** (všech ostatních případech) – po vytvoření nové položky vlastní skupiny se zkopírují z nadřazenou složku.

Vlastní skupinu můžete změnit tak, že:
* Všichni uživatelé, výhradní právo
* Vlastní uživatele, pokud vlastní uživatel členem taky cílové skupiny.

## <a name="access-check-algorithm"></a>Algoritmus zaškrtnutí přístup

Na následujícím obrázku představuje algoritmu přístup zaškrtnutí u účtů jezera úložiště.

![Algoritmus ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a>Maska a "oprávnění"

**Masky** je RWX hodnota, která se používá při provádění algoritmu kontrola přístupu k omezit přístup **s názvem uživatele**, **za další vlastnictví skupiny**a **s názvem skupiny** . Tady je nejdůležitější pojmy z oblasti vstupní maska. 

* Vstupní maska vytváří "oprávnění", tedy upraví oprávnění v době kontrola přístupu k.
* Vstupní maska může přímo upravovat soubor vlastník a výhradní právo uživatelům.
* Vstupní maska má možnost odebrat oprávnění k vytváření efektivní oprávnění. Masky **nelze** přidat oprávnění k efektivní oprávnění. 

Dejte nám podívejte se na příklady. Tady vidíte vstupní maska nastavenou **RWX**, což znamená vstupní maska neodebere všechna oprávnění. Všimněte si, že skutečná oprávnění pro pojmenované uživatele, vlastní skupinu a pojmenované skupiny nezmění při kontrole přístupu.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

V následujícím příkladu vstupní maska nastavenou **R-X**. Ano ho **vypne oprávnění k zápisu** **s názvem uživatele**, **za další vlastnictví skupiny**a **s názvem skupiny** v době přístup zaškrtněte.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

Odkaz tady je místo, kam masku souboru nebo složky se zobrazí na portálu Azure.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

>[AZURE.NOTE] K vytvoření nového účtu úložiště jezera dat jsou k RWX převezme masku přístupu ACL a výchozí ACL kořenové složky ("/").

## <a name="permissions-on-new-files-and-folders"></a>Oprávnění pro nové soubory a složky

Vytvoření nového souboru nebo složky v části existující složku, určuje výchozí ACL v nadřazené složky:

* Výchozí ACL a přístupu ACL podřízené složky
* Soubor podřízené přístupu ACL (aniž by bylo výchozí ACL)

### <a name="a-child-file-or-folders-access-acl"></a>Podřízené souboru nebo složky přístupu ACL

Vytvoření podřízené souboru nebo složky, nadřazené výchozí ACL zkopírováno jako podřízeném souboru nebo složky přístupu ACL. Také pokud **jiný** uživatel oprávnění RWX v nadřazené výchozí ACL, je úplně odebraná z podřízenou položku přístupu ACL.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

Ve většině případů výše uvedených informací je vše, co by potřeba vědět o jak podřízenou položku přístupu ACL záleží. Ale pokud se seznámíte s POSIX systémy a znát hloubkovou jak dosáhnout Tato transformace, najdete v části [pomocí možnosti Umask společnosti rolí v tématu Vytvoření přístupu ACL pro nové soubory a složky](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) dál v tomto článku.
 

### <a name="a-child-folders-default-acl"></a>Výchozí ACL podřízené složky

Po vytvoření podřízené složky ve složce nadřazené nadřazenou složku výchozí ACL zkopírována, jak se má výchozí ACL podřízené složky.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a>Další témata pro Principy ACL v úložišti jezera dat

Následuje několik Pokročilá témata vám pomůže pochopit, jak jsou určeny ACL jezera úložiště souborů nebo složek.

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a>Pomocí možnosti umask společnosti rolí v tématu Vytvoření přístupu ACL pro nové soubory a složky

Ve standardu POSIX systému obecné koncept je pomocí této možnosti umask hodnotu 9-bit nadřazenou složku používá k transformaci oprávnění pro **za další vlastnictví uživatele**, **za další vlastnictví skupiny**a **jiných** na nový podřízený souboru nebo složky přístupu ACL. Bitů pomocí možnosti umask zjistěte, jaký bitů vypnout v podřízenou položku přístupu ACL. Tím je použít k selektivně zabránit tomu, aby oprávnění pro za další vlastnictví uživatele za další vlastnictví skupiny a další.
  
V systému HDFS je pomocí možnosti umask obvykle konfigurace webu možnost, která ovládá správci. Úložiště jezera dat využívá **pomocí možnosti umask nevyžádaného účtu** , který nelze změnit. V následující tabulce jsou uvedeny úložiště jezera dat pomocí možnosti umask.

| Skupiny uživatelů  | Nastavení | Vliv na nové podřízené položky přístupu ACL |
|------------ |---------|---------------------------------------|
| Za další vlastnictví uživatele | ---     | Žádný vliv                             |
| Za další vlastnictví skupiny| ---     | Žádný vliv                             |
| Další       | RWX     | Odebrání čtení + zápis spustit         | 

Následující obrázek znázorňuje tento pomocí možnosti umask v akci. Čistý efekt je odebrat **Číst + zápis spustit** pro **ostatní** uživatele. Protože pomocí možnosti umask nezadali bitů **za další vlastnictví uživatele** a **za další vlastnictví skupiny**, tato oprávnění nepřevedou.

![ACL jezera úložiště dat](./media/data-lake-store-access-control/data-lake-store-acls-umask.png) 

### <a name="the-sticky-bit"></a>Rychlé bit

Rychlé bitu je rozšířené funkce POSIX systému souborů. V rámci úložiště jezera dat je pravděpodobné, že bude třeba rychlé bit.

Následující tabulka ukazuje, jak funguje rychlé přenosová v úložišti jezera.

| Skupiny uživatelů         | Soubor    | Složka |
|--------------------|---------|-------------------------|
| Rychlé bit **Vypnuto** | Žádný vliv   | Žádný vliv           |
| Rychlé přenosová **dál**  | Žádný vliv   | Můžete komukoli kromě **výhradní právo uživatele** a **za další vlastnictví uživatele** podřízené položky z odstraňování a přejmenovávání podřízené položky.               |

Rychlé bit není uvedený v portálu Azure.

## <a name="common-questions-for-acls-in-data-lake-store"></a>Časté otázky k ACL v úložišti jezera dat

Tady jsou některé dotazy, které přichází často týkající ACL v úložišti jezera.

### <a name="do-i-have-to-enable-support-for-acls"></a>Je třeba povolit podporu pro ACL?

Ne. Řízení přístupu přes ACL je vždy úložiště jezera dat účtu.

### <a name="what-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a>Jaká oprávnění jsou potřeba odstranit zpětně složka a její obsah?

* **Zápis provést**, musíte mít nadřazenou složku.
* Složku, kterou chcete odstranit a všechny složky v ní, vyžaduje **Další + zápis spustit**.
>[AZURE.NOTE] Odstranění souborů ve složkách není vyžaduje, aby zápisu na tyto soubory. Navíc kořenové složce "/" **nikdy** odstraněním.

### <a name="who-is-set-as-the-owner-of-a-file-or-folder"></a>Kdo je nastavena jako vlastník souboru nebo složky?

Poznámkové bloky předmětů souboru nebo složky stane vlastníkem.

### <a name="who-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a>Kdo je nastavit jako vlastní okruhem souboru nebo složky při vytvoření?

Je zkopírována z vlastní skupiny nadřazené složky, ve kterém se vytvoří nový soubor nebo složku.

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a>Jsem vlastní uživatele souboru ale nemám RWX oprávnění, které budeme potřebovat. Co mám dělat?

Vlastní uživatel jednoduše změnit oprávnění pro tento soubor a udělte tak sami RWX oprávnění, který potřebují.

### <a name="does-data-lake-store-support-inheritance-of-acls"></a>Podporuje úložiště jezera dat dědičnosti ACL?

Ne.

### <a name="what-is-the-difference-between-mask-and-umask"></a>Jaký je rozdíl mezi masky a pomocí možnosti umask?

| masky | pomocí možnosti umask|
|------|------|
| Vlastnost **masky** je k dispozici na všechny soubory a složky. | **Pomocí možnosti umask** je vlastnost jezera úložiště klienta. Ano obsahuje pouze jednoho pomocí možnosti umask jezera úložišti.    |
| Vlastnost masky na soubor nebo složku můžete změnit pomocí vlastní uživatele nebo vlastní skupiny souboru nebo výhradní právo uživatele. | Vlastnost pomocí možnosti umask nelze změnit tak, že všichni uživatelé i superuživatelů. Je neměnný konstantní hodnota.|
| Vlastnost masky lze během algoritmu kontrola přístupu k za běhu a zjistit, jestli má uživatel oprávnění provádět operace v souboru nebo složky. Role masky je vytvořit "oprávnění" v době kontrola přístupu. | Pomocí možnosti umask není používaný během kontrola přístupu k vůbec. Pomocí možnosti umask slouží k určení přístupu ACL nové podřízené položky ze složky. |
| Vstupní maska hodnota 3-bit RWX vztahující se k pojmenované uživatele, pojmenované skupiny a vlastní uživatelů v době kontrola přístupu.| Pomocí možnosti umask hodnota 9 bit vztahující se k vlastní uživatele, vlastní skupiny a další nový podřízený.| 

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a>Kde se dají najít další informace o model řízení přístupu POSIX?

* [http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.HTML](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [Příručka HDFS oprávnění](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) 

* [NEJČASTĚJŠÍ DOTAZY TÝKAJÍCÍ SE POSIX](http://www.opengroup.org/austin/papers/posix_faq.html)

* [POSIX 1003.1 2008](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [POSIX 1003.1e 1997](http://users.suse.com/~agruen/acl/posix/Posix_1003.1e-990310.pdf)

* [POSIX ACL na Linux](http://users.suse.com/~agruen/acl/linux-acls/online/)

* [Řízení přístupu pomocí seznamů řízení přístupu na Linux](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a>Viz taky

* [Základní informace o úložiště jezera dat Azure](data-lake-store-overview.md)

* [Začínáme s Azure dat jezera analýzy](../data-lake-analytics/data-lake-analytics-get-started-portal.md)





