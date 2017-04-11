<properties 
   pageTitle="Zabezpečení dat uložených v úložišti jezera dat Azure | Microsoft Azure" 
   description="Zjistěte, jak zajistit dat v úložišti jezera dat Azure pomocí skupin a seznamy řízení přístupu" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Zabezpečení dat uložených v úložišti jezera dat Azure

Zabezpečení dat v úložišti jezera dat Azure je třech fázích.

1. Začněte tak, že vytvoříte skupiny zabezpečení v Azure Active Directory (AAD). Tyto skupiny zabezpečení slouží k provádění řízení přístupu na základě rolí (RBAC) Azure portálu. Další informace najdete v článku [Řízení přístupu na základě rolí v Microsoft Azure](../active-directory/role-based-access-control-configure.md).

2. Skupiny zabezpečení AAD přiřadí účtu úložiště jezera dat Azure. Řídí přístup k tomuto účtu úložiště jezera dat portálu a správa operacích, které z portálu nebo rozhraní API.

3. Přiřazení skupiny zabezpečení AAD jako přístup seznamy () v systému souborů jezera úložiště.

4. Kromě toho můžete vytvořit také rozsah IP adres pro klienty, které můžete získat přístup k datům v úložišti jezera.

Tento článek obsahuje pokyny o tom, jak používat portál Azure provádět výše uvedených úkolů. Podrobnější informace o způsobu úložiště jezera dat implementuje zabezpečení na úrovni účet a dat, najdete v článku [zabezpečení v úložišti jezera dat Azure](data-lake-store-security-overview.md). Nabídnout detailní informace o implementaci ACL v úložišti jezera dat Azure najdete v článku [Základní informace o řízení přístupu v úložišti jezera](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Před zahájením tohoto kurzu, musíte mít takto:

- **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
- **Účet Azure jezera úložiště**. Další informace o tom, jak ho vytvořit najdete v článku [Začínáme s úložiště jezera dat Azure](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Vytvoření skupiny zabezpečení v Azure Active Directory

Pokyny pro vytváření skupin zabezpečení AAD a jak přidávat uživatele do skupiny najdete v článku [Správa skupin zabezpečení v Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Přiřadit úložiště jezera dat Azure účty uživatele nebo skupiny zabezpečení

Když přiřadíte uživatele nebo skupiny zabezpečení úložiště jezera dat Azure účty, můžete pomocí řízení přístupu k operace správy účtu pomocí rozhraní API Správce prostředků Azure a Azure portálu. 

1. Potřebujete založit účet Azure dat jezera úložiště přihlašovacích údajů. V levém podokně klikněte na tlačítko **Procházet**a **Jezera úložiště**na zásuvné úložiště jezera dat klikněte na název účtu, ke kterému chcete přiřadit uživatele nebo bezpečnostní skupiny.

2. Ve vaší zásuvné účtu úložiště jezera dat klikněte na **Nastavení**. Z zásuvné **Nastavení** klikněte na **uživatele**.

    ![Přiřazení skupiny zabezpečení účtu úložiště jezera dat Azure] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Přiřazení skupiny zabezpečení účtu úložiště jezera dat Azure")

3. Zásuvné **uživatele** ve výchozím nastavení uvede jako některé vlastníky skupiny **správců předplatného** . 

    ![Přidání uživatelů a rolí] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Přidání uživatelů a rolí")
 
    Existují dva způsoby přidání skupiny a přiřaďte příslušných rolí.

    * Přidání uživatele nebo skupinu k tomuto účtu a potom přiřadit roli, nebo
    * Přidání roli a potom přiřadit roli uživatele nebo skupiny.

    V tomto oddílu se podíváme na první přístup přidání skupiny a potom přiřazení rolí. Můžete provádět podobné kroky nejdřív vyberte roli a potom přiřadit roli skupiny.
    
4. V zásuvné **Uživatelé** klikněte na **Přidat** otevřete zásuvné **Přidat přístup** . V zásuvné **přístup přidat** klikněte na tlačítko **Vybrat roli**a vyberte role pro uživatele nebo skupinu.

     ![Přidat role pro uživatele] (./media/data-lake-store-secure-data/adl.add.user.1.png "Přidat role pro uživatele")

    **Vlastník** a role **přispěvatele** zpřístupnit různé funkce správy účtu jezera data. Pro uživatele, kteří budou pracovat s daty v jezera dat můžete přidat k roli **Čtenář **. Rozsah Tato role je omezen na operace správy vztahující se k účtu úložiště jezera dat Azure.

    Pro data operace jednotlivých souborů systému oprávnění definovat, co můžete dělat uživatelů. Proto uživatele s role Čtenář můžou jenom zobrazit nastavení správy přidružené k účtu, ale můžete potenciálně čtení a zápis dat podle přiřazená oprávnění systému souborů. Oprávnění k systému souborů úložiště jezera dat jsou popsané v [přiřadit skupiny zabezpečení jako ACL k systému souborů úložiště jezera dat Azure](#filepermissions).



5. V zásuvné **přístup přidat** klikněte na **Přidat uživatele** otevřete zásuvné **Přidat uživatele** . V tomto zásuvné vyhledejte skupinu zabezpečení, kterou jste vytvořili dříve v Azure Active Directory. Pokud máte před sebou hodně skupin hledání z pomocí textového pole nahoře Pokud chcete filtrovat podle názvu skupiny. Klikněte na **Výběr**.

    ![Přidání skupiny zabezpečení] (./media/data-lake-store-secure-data/adl.add.user.2.png "Přidání skupiny zabezpečení")

    Pokud chcete přidat skupiny nebo uživatele, který není uvedený, můžete je pozvat pomocí ikony **Pozvat** a zadáním e-mailovou adresu uživatele nebo skupinu.

6. Klikněte na **OK**. Měli byste vidět skupiny zabezpečení přidali, jak je ukázáno v následujícím příkladu.

    ![Přidání skupiny zabezpečení] (./media/data-lake-store-secure-data/adl.add.user.3.png "Přidání skupiny zabezpečení")

7. Skupiny zabezpečení a uživatelských teď má přístup k účtu úložiště jezera dat Azure. Pokud chcete zajistit přístup k určitým uživatelům, můžete je přidat do skupiny zabezpečení. Podobně pokud chcete odvolat přístup pro uživatele, můžete je odebrat ze skupiny zabezpečení. Můžete taky přiřadíte více skupin zabezpečení s klientem. 

## <a name="filepermissions"></a>Přiřazení uživatele nebo skupiny zabezpečení jako ACL k systému souborů úložiště jezera dat Azure

Přiřazením uživatele a které skupiny zabezpečení systému souborů jezera dat Azure nastavíte řízení přístupu z dat uložených v úložišti jezera dat Azure.

1. V svůj účet zásuvné úložiště jezera dat klikněte na položku **Průzkumník Data**.

    ![Vytvoření adresáře v úložišti jezera účtu] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Vytvoření adresářů dat jezera účtu")

2. V zásuvné **Průzkumník dat** klikněte na soubor nebo složku, u kterého chcete konfigurovat ACL a potom na možnost **přístup**. Přiřazení ACL do souboru, třeba klikněte na **přístup** z zásuvné **Náhled souboru** .

    ![Nastavení ACL systému souborů jezera dat] (./media/data-lake-store-secure-data/adl.acl.1.png "Nastavení ACL systému souborů jezera dat")

3. **Access** zásuvné seznamy standardní přístupu a vlastní již byly přiřazeny ke kořenovému. Klikněte na ikonu **Přidat** pro přidání ACL vlastní úroveň.

    ![Přístup k seznamu standardních a vlastních] (./media/data-lake-store-secure-data/adl.acl.2.png "Přístup k seznamu standardních a vlastních")

    * **Přístup k standardní** zadejte adresu formátu UNIX umístění pro čtení, psaní, provést (rwx) do tří tříd jednoznačné uživatele: vlastník skupiny a ostatní.
    * **Vlastní přístup** odpovídá ACL POSIX, která umožňuje nastavení oprávnění pro konkrétní pojmenované uživatele nebo skupiny a nejen souboru vlastník nebo skupiny. 
    
    Další informace najdete v tématu [HDFS ACL](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Další informace o implementaci ACL v úložišti jezera dat najdete v tématu [Řízení přístupu v úložišti jezera](data-lake-store-access-control.md).

4. Klikněte na ikonu **Přidat** otevřete zásuvné **Přidat vlastní přístup** . V tomto zásuvné klikněte na **Vybrat uživatele nebo skupiny**a potom **Vyberte uživatele nebo skupinu** zásuvné, vyhledejte v skupinu zabezpečení, kterou jste vytvořili dříve v Azure Active Directory. Pokud máte před sebou hodně skupin hledání z pomocí textového pole nahoře Pokud chcete filtrovat podle názvu skupiny. Klikněte na skupiny, kterou chcete přidat a potom klikněte na **Výběr**.

    ![Přidat skupinu] (./media/data-lake-store-secure-data/adl.acl.3.png "Přidat skupinu")

5. Klikněte na **Vybrat oprávnění**vyberte oprávnění a jestli chcete přiřadit oprávnění jako výchozí ACL, přístup k ACL nebo obojí. Klikněte na **OK**.

    ![Přiřazení oprávnění skupině] (./media/data-lake-store-secure-data/adl.acl.4.png "Přiřazení oprávnění skupině")

    Další informace o oprávněních v úložišti jezera a výchozí/přístup ACL naleznete v tématu [Řízení přístupu v úložišti jezera](data-lake-store-access-control.md).


6. V zásuvné **Přidat vlastní aplikace Access** klikněte na **OK**. Nově přidaná skupina s odpovídající oprávnění se teď zobrazují v zásuvné **přístup** .

    ![Přiřazení oprávnění skupině] (./media/data-lake-store-secure-data/adl.acl.5.png "Přiřazení oprávnění skupině")

    > [AZURE.IMPORTANT] V současné verzi můžete stačí 9 položky v části **Vlastní přístup**. Pokud chcete přidat více než 9 uživatele, měli byste vytvořit skupiny zabezpečení, přidání uživatelů do skupiny zabezpečení, přidejte poskytnutí přístupu k těmto skupinám zabezpečení pro účet jezera úložiště.

7. V případě potřeby můžete také změnit oprávnění k přístupu, po přidání skupiny. Nebo zrušte zaškrtnutí políčka pro každý typ oprávnění (pro čtení, zapsat, spouštět) založené na tom, zda chcete odebrat nebo toto oprávnění přiřaďte skupině zabezpečení. Klikněte na **Uložit** změny uložte, kliknutím na **Zahodit** ji vrátit zpět změny.

## <a name="set-ip-address-range-for-data-access"></a>Nastavení rozsah IP adres pro přístup k datům

Úložiště jezera dat Azure umožňuje další uzamčení přístup k úložišti dat na úrovni sítě. Můžete povolit bránu firewall, zadejte IP adresu nebo definování rozsahu IP adres pro klienty důvěryhodné. Po povolení pouze klientech IP adres v rámci definovaný rozsah můžete připojit k úložišti.

![Nastavení brány firewall a IP přístup] (./media/data-lake-store-secure-data/firewall-ip-access.png "Nastavení brány firewall a IP adresa")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Odstranění skupiny zabezpečení pro účet úložiště jezera dat Azure

Při odebrání skupiny zabezpečení z účtů úložiště jezera dat Azure mění jenom přístup k operace správy účtu pomocí rozhraní API Správce prostředků Azure a Azure portálu.

1. Ve vaší zásuvné účtu úložiště jezera dat klikněte na **Nastavení**. Z zásuvné **Nastavení** klikněte na **uživatele**.

    ![Přiřazení skupiny zabezpečení účtu jezera dat Azure] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Přiřazení skupiny zabezpečení účtu jezera dat Azure")

2. V zásuvné **Uživatelé** klikněte na skupiny zabezpečení, které chcete odebrat.

    ![Odstranění skupiny zabezpečení] (./media/data-lake-store-secure-data/adl.add.user.3.png "Odstranění skupiny zabezpečení")

3. V zásuvné skupiny zabezpečení klikněte na **Odebrat**.

    ![Odebrání skupiny zabezpečení] (./media/data-lake-store-secure-data/adl.remove.group.png "Odebrání skupiny zabezpečení")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Odstranění skupiny zabezpečení ACL ze systému souborů úložiště jezera dat Azure

Při odebrání skupiny zabezpečení ACL ze systému souborů úložiště jezera dat Azure změna přístupu k datům v úložišti jezera Data.

1. V svůj účet zásuvné úložiště jezera dat klikněte na položku **Průzkumník Data**.

    ![Vytvoření adresářů dat jezera účtu] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Vytvoření adresářů dat jezera účtu")

2. V zásuvné **Průzkumník dat** klikněte na soubor nebo složku, u kterého chcete odebrat ACL a na zásuvné svůj účet klepněte na ikonu **aplikace Access** . Pokud chcete odebrat ACL pro soubor, musí klikněte na **přístup** z zásuvné **Náhled souboru** .

    ![Nastavení ACL systému souborů jezera dat] (./media/data-lake-store-secure-data/adl.acl.1.png "Nastavení ACL systému souborů jezera dat")

3. V **Accessu** zásuvné z oddílu **Vlastní přístup** klikněte na skupiny zabezpečení, které chcete odebrat. V zásuvné **Vlastní přístup** na tlačítko **Odebrat** a klikněte na **OK**.

    ![Přiřazení oprávnění skupině] (./media/data-lake-store-secure-data/adl.remove.acl.png "Přiřazení oprávnění skupině")


## <a name="see-also"></a>Viz taky

- [Základní informace o úložiště jezera dat Azure](data-lake-store-overview.md)
- [Kopírování dat z Azure úložiště objektů BLOB jezera úložiště dat](data-lake-store-copy-data-azure-storage-blob.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Začínáme s úložiště jezera dat pomocí prostředí PowerShell](data-lake-store-get-started-powershell.md)
- [Začínáme s úložiště jezera dat pomocí .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Protokoly pro diagnostiku přístup pro jezera úložiště dat](data-lake-store-diagnostic-logs.md)
