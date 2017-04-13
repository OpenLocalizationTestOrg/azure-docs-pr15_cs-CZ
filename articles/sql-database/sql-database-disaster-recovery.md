<properties
   pageTitle="Obnovení databáze SQL havárie | Microsoft Azure"
   description="Informace o obnovení databáze ze regionálního datacentra výpadku nebo selhání Azure SQL aktivní Geo-replikace databáze a obnovení Geo schopnosti."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Obnovení databáze SQL Azure nebo převzetí sekundární

Databáze SQL Azure nabízí tyto možnosti obnovení z výpadku:

- [Aktivní Geo replikace](sql-database-geo-replication-overview.md)
- [Obnovení GEO](sql-database-recovery-using-backups.md#point-in-time-restore)

Další informace o pokračování scénáře pro firmy a funkce podobnému sledu podpůrné najdete v tématu [nepřerušený](sql-database-business-continuity.md).

### <a name="prepare-for-the-event-of-an-outage"></a>Příprava na událost výpadek

K úspěchu s využitím do jiné oblasti dat pomocí aktivní Geo replikace nebo geo nadbytečné zálohy, je nutné připravit serveru v jiné datovém centru výpadku osvobozením od nového primárního serveru potřeba vznikají, jakož i definovali kroků uvedených a testováno k zajištění hladkého obnovení. Těchto přípravných kroků patří:

- Určení logické serveru v jiné oblasti osvobozením od nového primárního serveru. S aktivní Geo replikace, bude alespoň jedno a případně všech sekundárním serverů. U Geo-obnovení bude obecně serveru [párových oblast](../best-practices-availability-paired-regions.md) pro oblast, ve kterém je databáze.
- Identifikace a volitelně definovat pravidla brány firewall úrovni serveru potřeba na uživatelům přístup k databázi nové primární.
- Zjistěte, jak chcete přesměrovat uživatelům nového primárního serveru, jako je třeba změnou připojovací řetězec nebo změnou položky DNS.
- Identifikace a pokud chcete vytvořit přihlášení, které musí být součástí hlavní databází v nového primárního serveru a zajistěte, aby tyto přihlášení příslušná oprávnění v hlavní databázi, pokud existuje. Další informace najdete v tématu [zabezpečení databáze SQL po obnovení havárie](sql-database-geo-replication-security-config.md)
- Určení upozornění pravidla, která bude potřeba aktualizovat mapovat na nové primární databáze.
- Konfigurace auditování v aktuální databázi primární dokladu
- Umožňuje proveďte [havárie obnovení procházení](sql-database-disaster-recovery-drills.md). Tak, aby napodobily výpadku Geo obnovení, můžete odstranit nebo přejmenovat zdrojové databáze způsobit selhání připojení aplikace. Tak, aby napodobily výpadku pro aktivní Geo replikace, můžete zakázat webovou aplikaci nebo virtuální počítač připojen k databázi nebo převzetí databázi vést k chybám connectity aplikace.

## <a name="when-to-initiate-recovery"></a>Kdy se má zahájit obnovení

Operace obnovení ovlivní aplikaci. Vyžaduje připojovací řetězec SQL nebo přesměrování pomocí DNS a může dojít ke ztrátě trvalé data. Proto, měli byste to provést pouze v případě výpadku je pravděpodobně trvat déle než cíle času pro obnovení aplikace. Při nasazení aplikace výroby by měl provést pravidelné kontroly stavu aplikace a pomocí následujících datových bodů uplatnit, že je oprávněné obnovení:

1.  Trvalé připojení selhání z vrstvy aplikace do databáze.
2.  Portál Azure zobrazuje upozornění o incident v oblasti s obecných vliv.
3.  Databázový server Azure SQL označen sníženou kvalitu.

V závislosti na odchylky aplikace výpadek služeb a možné firmy odpovědnost byste zvážit následující možnosti obnovení.

Poslední bod replikovat Geo obnovení pomocí [Načíst obnovitelné databáze](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*).

## <a name="wait-for-service-recovery"></a>Počkejte obnovení služby

Práce Azure týmy pečlivě obnovíte dostupnost služby jako rychle co nejdřív, ale v závislosti na kořenovém způsobit, že ho může trvat hodin nebo dnů.  Pokud aplikace nevadí vám významné prostoje můžete jednoduše počkat obnovení dokončete. V tomto případě není nutná žádná akce na druhé straně. Zobrazí se aktuální stav služby na naše [Řídicí panel stavu služby Azure](https://azure.microsoft.com/status/). Po obnovení oblasti se obnoví dostupnosti aplikace.

## <a name="failover-to-geo-replicated-secondary-database"></a>Přepnutí do replikovat geo sekundární databáze

Jestli prostoje aplikace můžete mít za následek firmy odpovědnost měli používat replikovat geo databází v aplikaci. Umožní aplikace rychle obnovit dostupnost v jiné oblasti v případě výpadku. Zjistěte, jak [nakonfigurovat Geo replikace](sql-database-geo-replication-portal.md).

Chcete-li obnovit dostupnost databází, je potřeba spustit převzetí služeb replikovat geo sekundární jeden z podporovaných metod.

Použijte jeden z následujících průvodce, abyste převezme replikovat geo sekundární databáze:

- [Přepnutí do sekundární replikovat geo portálu Azure](sql-database-geo-replication-portal.md)
- [Přepnutí do vedlejší replikovat geo pomocí prostředí PowerShell](sql-database-geo-replication-powershell.md)
- [Přepnutí do sekundární replikovat geo pomocí T-SQL](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Obnovení pomocí Geo obnovení

Pokud vaše aplikace prostoje nemá za následek odpovědnost business můžete obnovit Geo jako prostředek k obnovení databáze aplikace. Vytvoří kopii databáze nejnovější geo nadbytečné záložní kopie.

Použijte jeden z následujících průvodce, abyste geo obnovení databáze do nové oblasti:

- [GEO obnovení databáze pro novou oblast portálu Azure](sql-database-geo-restore-portal.md)
- [GEO obnovení databáze pro novou oblast pomocí prostředí PowerShell](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>Konfigurace databáze po obnovení

Pokud používáte geo replikace převzetí nebo obnovení geo obnovit z výpadku, musíte podat, že je připojení k nové databáze správně nakonfigurované tak, aby funkce normální aplikaci můžete pokračovat. Toto je kontrolního seznamu úkolů připravit výrobní obnoveného databáze.

### <a name="update-connection-strings"></a>Aktualizace připojení řetězce

Protože obnoveného databázi bude nacházet v jiném serveru, budete muset aktualizovat aplikace připojovací řetězec tak, aby ukazovaly na tento server.

Další informace o změně připojovací řetězec najdete v článku jazykové odpovídající vývoj [Knihovna připojení](sql-database-libraries.md).

### <a name="configure-firewall-rules"></a>Konfigurace pravidel brány Firewall

Potřebujete, abyste měli jistotu, že pravidla brány firewall nakonfigurovaný na server a databázi shodují s názvy, které jste nakonfigurovali ve primárního serveru a primárního databáze. Další informace najdete v tématu [jak: Nakonfigurujte nastavení brány Firewall (databáze SQL Azure)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Konfigurace přihlášení a databáze uživatelů

Potřebujete udělat zajistit, že všech přihlášení používané aplikací existoval na serveru, který je hostitelem obnoveného databáze. Další informace najdete v tématu [Konfigurace zabezpečení Geo replikace](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] By měl konfigurace a otestujte brány firewall pravidel serveru a přihlášení (a jejich oprávnění) během přechodu obnovení havárie. Tyto objekty úrovni serveru a jejich konfigurace nemusí být k dispozici při výpadku.

### <a name="setup-telemetry-alerts"></a>Nastavení upozornění Telemetrie

Potřebujete Ujistěte se, že existující nastavení upozornění pravidlo se aktualizují přiřadit obnoveného databáze a jiné serveru.

Další informace o pravidlech upozornění databáze najdete v článku [Dostávat oznámení upozornění](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) a [Sledovat stav služby](../monitoring-and-diagnostics/insights-service-health.md).

### <a name="enable-auditing"></a>Povolení auditování

Pokud auditování je nutná pro přístup k databázi, musíte povolit auditování po obnovení databáze. Dobrý indikátor, že není potřeba auditování je, že klientské aplikace použít zabezpečené připojení řetězce ve vzorci z *. database.secure.windows.net. Další informace najdete v tématu [Začínáme s SQL databáze auditování](sql-database-auditing-get-started.md).


## <a name="next-steps"></a>Další kroky

- Další informace o databáze SQL Azure automatické zálohy, najdete v článku [automatické zálohy databáze SQL](sql-database-automated-backups.md)
- Další informace o pokračování návrh a obnovení v organizacích, najdete v článku [kontinuitu scénáře](sql-database-business-continuity.md)
- Další informace o použití zálohy pro automatické obnovení najdete v tématu [obnovení databáze ze zálohy spuštěná služba](sql-database-recovery-using-backups.md)
