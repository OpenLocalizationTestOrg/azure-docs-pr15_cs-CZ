<properties
   pageTitle="Základní informace o zabezpečení v úložišti jezera | Microsoft Azure"
   description="Porozumět tomu, jak je úložiště jezera dat Azure bezpečnější velký datový úložiště"
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
   ms.date="08/02/2016"
   ms.author="nitinme"/>

# <a name="security-in-azure-data-lake-store"></a>Zabezpečení v úložišti jezera dat Azure

Mnoho podniky prováděnou výhod analýz velký dat pro přehledy firmy pomůžou inteligentní rozhodovat. Organizace může mít prostředí složité a upraveny s rostoucí počet různých uživatelů. Je důležité pro svou společnost, abyste měli jistotu, že kritické obchodní data bezpečnější, uložena s správnou úroveň přístupu k jednotlivým uživatelům. Úložiště jezera dat Azure slouží splňovat tyto požadavky na zabezpečení. V tomto článku informace o možnostech zabezpečení úložiště jezera dat, včetně:

* Ověřování
* Povolení
* Izolace sítě
* Ochrana dat
* Sestavy auditování

## <a name="authentication-and-identity-management"></a>Správa ověřování a identity

Ověřování je proces, kterým je ověřit identitu uživatele při interakci uživatele s úložiště jezera dat nebo jiné služby, který se připojuje k úložišti jezera. Správa identit a ověřováním úložiště jezera dat využívá [Azure Active Directory](../active-directory/active-directory-whatis.md), úplný identity přístup správy cloudové řešení, které zjednodušení správy uživatelů a skupin.

Každý Azure předplatné může být přidružené k instanci služby Azure Active Directory. Pouze uživatelů a identit služby, které jsou definovány služby Azure Active Directory měli přístup k účtu úložiště jezera dat pomocí portálu Azure nástroje příkazového řádku nebo prostřednictvím klientské aplikace vytvoří pomocí Azure Data jezera úložiště SDK vaší organizace. Klíčové výhody používání služby Azure Active Directory jako mechanismus ovládací prvek centralizované přístupu jsou:

* Zjednodušená správa životního cyklu identity. Identita uživatele nebo službu (služby základní identita) můžete rychle vytvoříte a rychle odvolán jednoduše odstranění nebo zákaz účtu v adresáři.

* Vícefaktorové ověřování. [Vícefaktorové ověřování](../multi-factor-authentication/multi-factor-authentication.md) poskytuje další úroveň zabezpečení pro uživatele přihlášení, začátky transakce.

* Ověřování z libovolného klienta pomocí standardní otevřených protokolu, například OAuth nebo OpenID.

* Federaci s enterprise adresářovými službami a zprostředkovatelé identit jiní cloudu.

## <a name="authorization-and-access-control"></a>Povolení a řízení přístupu

Po Azure Active Directory ověřuje uživatele, aby uživatel přístup úložiště jezera dat Azure, povolení ovládacích prvků přístupová oprávnění pro úložiště jezera dat. Úložiště jezera dat odděluje se tak mohli ověřovat za týkající se účtu a souvisejících dat v následujícím způsobem:

* [Řízení přístupu na základě rolí](../active-directory/role-based-access-control-what-is.md) (RBAC) poskytovanou Azure pro správu účtů
* POSIX ACL pro přístup k datům v úložišti


### <a name="rbac-for-account-management"></a>RBAC pro správu účtů

Čtyři základní role jsou definované pro úložiště jezera dat ve výchozím nastavení. Role povolit různých operacích účtu úložiště jezera dat pomocí Azure portál, rutiny prostředí PowerShell a rozhraní REST API. Vlastník a Přispěvatel role může provádět různé funkce správy účtu. Můžete přiřadit role Čtenář uživatelům, kteří pouze práci s daty.

![Role RBAC] (./media/data-lake-store-security-overview/rbac-roles.png "Role RBAC")

Všimněte si, že i když role přiřazené pro správu účtů, některé role vliv na přístup k datům. Je potřeba použít ACL pro řízení přístupu k operacích, které můžete dělat uživatel v systému souborů. Následující tabulka obsahuje přehled správy přístupových práv a data oprávnění pro výchozí role.

| Role                    | Správa práv               | Přístupová práva | Vysvětlení |
| ------------------------ | ------------------------------- | ------------------ | ----------- |
| Žádné přiřazenou roli         | Žádná                            | Řídí ACL    | Umožňuje přecházet úložiště jezera dat nemůže používat Azure portál nebo rutiny prostředí PowerShell Azure. Uživatel může použít jenom nástroje příkazového řádku.
| Vlastník  | Všechny  | Všechny  | Role vlastníka je úroveň superuser. Tato role může spravovat všechno, co a má úplný přístup k datům.
| Čtečky   | Jen pro čtení  | Řídí ACL    | Role Čtenář Zobrazit vše, co týkající se správy účtu, například které uživateli přiřazena jaké role. Role Čtenář nelze proveďte požadované změny.   |
| Skupiny přispěvatelů              | Vše kromě přidat a odebrat role | Řídí ACL    | Role přispěvatele zvládne některé aspekty účtem, třeba nasazení a vytváření a správě upozornění. Role přispěvatele nelze přidat nebo odebrat role.
| Správce přístup uživatelů | Přidávání a odebírání rolí            | Řídí ACL    | Role správce přístup uživatelů můžete spravovat přístup uživatelů k účtům. |

Pokyny najdete v tématu [Přiřazení uživatele nebo skupiny zabezpečení účtům jezera úložiště](data-lake-store-secure-data.md#assign-users-or-security-groups-to-azure-data-lake-store-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Pomocí seznamů ACL pro operace v systému souborů

Úložiště jezera dat je systém souborů HFS jako Hadoop Distributed soubor systému (HDFS) a podporuje [POSIX ACL](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Ovládací prvky pro čtení (r), zápis (w) a spustit (oprávnění k prostředkům rolí vlastník skupiny Vlastníci a pro další uživatelé a skupiny x). V dat jezera úložiště veřejného náhledu (ální) jsou povoleny ACL na kořenová složka, podsložky a jednotlivé soubory. ACL, které použijete ke kořenové složce také použít u všech podřízených složek a souborů.

Doporučujeme definovat ACL pro víc uživatelů pomocí [skupiny zabezpečení](../active-directory/active-directory-accessmanagement-manage-groups.md). Přidání uživatelů do skupiny zabezpečení a potom do této skupiny zabezpečení přiřaďte ACL pro souboru nebo složky. To je užitečné, pokud chcete zadat vlastní přístup, protože jsou omezené pro přidání maximálně devět položek vlastní přístup pomocí protokolu. Další informace o tom, jak lépe zabezpečené dat uložených v úložišti jezera dat pomocí skupin zabezpečení Azure Active Directory najdete v tématu [Přiřazení uživatele nebo skupiny zabezpečení jako ACL k systému souborů úložiště jezera dat Azure](data-lake-store-secure-data.md#filepermissions).

![Přístup k seznamu standardních a vlastních] (./media/data-lake-store-security-overview/adl.acl.2.png "Přístup k seznamu standardních a vlastních")

## <a name="network-isolation"></a>Izolace sítě

Použití úložiště jezera řízení přístupu k úložišti dat na úrovni sítě pomůže. Můžete vytvořit bránách firewall a definování rozsahu IP adres pro klienty důvěryhodný. S rozsah IP adres můžete jenom klientech IP adresu v rámci definovaný rozsah připojení k úložišti jezera.

![Nastavení brány firewall a IP přístup] (./media/data-lake-store-security-overview/firewall-ip-access.png "Nastavení brány firewall a IP adresa")

## <a name="data-protection"></a>Ochrana dat

Organizace má zajištění zabezpečené během svého životního cyklu své firmy důležitá data. Pro data při přenosu šifrovaná úložiště jezera dat využívá standardní protokol zabezpečení TLS (Transport Layer) k zabezpečení dat, která slouží k přesunutí mezi klientem a jezera úložiště.

Ochrana dat dat u ostatních budou k dispozici v budoucích verzích.

## <a name="auditing-and-diagnostic-logs"></a>Auditování a diagnostické protokolování

Můžete použít auditování nebo diagnostické protokoly, podle toho, jestli jste hledali v protokolech týkajících se správy aktivity a související data aktivity.

*  Činnosti spojené správy pomocí rozhraní API Správce prostředků Azure a jsou umístěny na Azure portálu prostřednictvím protokolů auditování.
*  Činnosti spojené dat pomocí WebHDFS REST API a jsou umístěny na portálu Azure přes protokoly pro diagnostiku.

### <a name="auditing-logs"></a>Protokolů auditování

Dodržovat předpisy organizace může vyžadovat záznamy odpovídající auditu Pokud potřebuje a dostanete se do zvláštní případy. Úložiště jezera dat obsahuje integrované sledování a auditování a protokoly všechny činnosti správy účtu.

Pro účet správy audit zobrazení a vyberte sloupce, které chcete zaznamenat. Můžete taky exportovat protokolů auditování k základnímu úložišti Azure.

![Protokolů auditování] (./media/data-lake-store-security-overview/audit-logs.png "Protokolů auditování")

### <a name="diagnostic-logs"></a>Protokoly pro diagnostiku

Můžete nastavit přístup k datům záznamy auditu Azure portálu (v diagnostické nastavení) a vytvořit účet úložiště objektů Blob Azure, kde jsou uložené protokoly.

![Protokoly pro diagnostiku] (./media/data-lake-store-security-overview/diagnostic-logs.png "Protokoly pro diagnostiku")

Po konfiguraci diagnostiky nastavení můžete zobrazit v protokolech na kartě **Protokoly pro diagnostiku** .

Další informace o práci s protokoly pro diagnostiku s úložiště jezera dat Azure najdete v článku [protokoly pro diagnostiku přístup pro jezera úložiště](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Souhrn

Podnikoví uživatelé požadovat platformu cloudu technologie pro analýzu dat, která je zabezpečené a snadno se použije. Úložiště jezera dat Azure usnadňuje adresu, na kterou tyto požadavky pomocí správy identit a ověřovat pomocí integrace služby Azure Active Directory, ověřování na základě ACL, izolace sítě, šifrování data při přenosu šifrovaná a při přesunu (už v budoucnu) a auditování.

Pokud chcete zobrazit nové funkce v úložišti jezera dat, pošlete nám svůj názor [Fórum komunity UserVoice úložiště jezera Data](https://feedback.azure.com/forums/327234-data-lake).

## <a name="see-also"></a>Viz taky

- [Základní informace o úložiště jezera dat Azure](data-lake-store-overview.md)
- [Začínáme s jezera úložiště dat](data-lake-store-get-started-portal.md)
- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
