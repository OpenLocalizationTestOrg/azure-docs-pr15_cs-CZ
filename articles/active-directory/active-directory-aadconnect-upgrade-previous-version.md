<properties
   pageTitle="Azure AD Connect: Upgrade z předchozí verze | Microsoft Azure"
   description="Tento článek vysvětluje různé metody upgradovat na nejnovější verzi Azure Active Directory připojení, včetně upgradu a migraci dráha."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Upgradovat na nejnovější z předchozí verze
Toto téma popisuje různé metody, které slouží k instalaci Azure AD Connect upgradovat na nejnovější verzi. Doporučujeme, abyste si aktualizujte verzích Azure AD Connect. Podle kroků popsaných v [kyvu migrace](#swing-migration) používají také po provedení změn v konfiguraci podstatné.

Pokud chcete upgradovat z DirSync, najdete v článku [Upgrade z synchronizační nástroj služby Azure AD (DirSync)](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md) místo.

Existuje několik různých strategie upgradu Azure AD Connect.

Metoda | Popis
--- | ---
[Automatické upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) | Zákazníkům s Expresní instalace je nejjednodušší způsob.
[Místní upgrade](#in-place-upgrade) | Pokud jste ještě jeden server, upgradujte instalace na místě na stejný server.
[Dráha migrace](#swing-migration) | Se dvěma servery můžete mezi servery s novou verzi nebo konfigurace připravit a změňte active server Jakmile budete připraveni.

Požadované oprávnění najdete v článku [oprávnění požadovaných pro upgrade](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade).

## <a name="in-place-upgrade"></a>Místní upgrade
V místní upgrade vyhovovat přesouvající se z Azure AD Sync nebo Azure AD Connect. Nebudou fungovat DirSync nebo řešení s FIM + Azure AD spojnice.

Tento způsob je upřednostňované, pokud jste vytvořili jeden server a asi menší než 100 000 objektů. Pokud jsou všechny změny v pravidlech mimo pole synchronizace úplný import a úplné synchronizaci se provedou po upgradu. Zajistíte tím, že nové konfigurace se použije na všechny existující objekty v systému. Může to trvat několik hodin v závislosti na počet objektů v rozsahu modul synchronizace. Plánovač synchronizace normální delta ve výchozím nastavení každých 30 minut je pozastavené, ale stále synchronizace hesel. Zvažte proveďte místní upgrade během víkend. Pokud jsou žádné změny konfigurace mimo pole s novou verzi Azure AD Connect, budou místo toho spustit normální delta importovat a synchronizovat.  
![Místní Upgrade](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Pokud provedete změny mimo pole synchronizace pravidla, tyto nastaví se zpátky na výchozí konfigurace při upgradu. Abyste měli jistotu, že bude k dispozici konfiguraci mezi upgradů, zkontrolujte, že změn podle popisu v tématu [Doporučené postupy pro změnu výchozí konfigurace](active-directory-aadconnectsync-best-practices-changing-default-configuration.md).

## <a name="swing-migration"></a>Dráha migrace
Pokud máte složitých nasazení nebo velmi velké množství objektů, může to být nepraktické proveďte místní upgrade v aktivním systému. Může to některé zákazníci trvat několik dnů a během této doby žádné změny delta zpracování. Tato metoda slouží také při plánování zásadní změny konfigurace a chcete je vyzkoušíte před tyto odesílají do cloudu.

Pro podobnému sledu doporučujeme používat dráha migrace. Potřebujete (minimálně) dva servery, aktivních a jeden pracovní server. Active server (modré čáry obrázku dole) je zodpovědný za načíst aktivní výroby. Přípravná serveru (na obrázku dole přerušované fialovou čáry) počítat s novou verzi nebo konfigurace a až plně budete připraveni, tento server je nastavená jako aktivní. Předchozí active server teď s ze starších verzí nebo konfigurace nainstalovali, je udělali pracovní server a upgradovat.

Dva servery můžete použít různé verze. Například active server, ke kterému chcete vyřadit můžete použít Azure AD Sync a nové pracovní serveru můžete použít Azure AD Connect. Pokud používáte dráha migrace se dají novou konfiguraci je dobré mít stejnou verzi na těchto serverech.  
![Pracovní serveru](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Poznámka: Je má byla poznamenat, že někteří uživatelé nastavit, aby byly tři nebo čtyři servery v tomto scénáři. Při upgradu pracovní server nemáte v případě [havárie obnovení](active-directory-aadconnectsync-operations.md#disaster-recovery)záložní server. Se třemi nebo čtyři servery jednu sadu serverů primární/úsporném režimu s novou verzí můžete připravit, zajistit, že jsou vždy serveru v testovacím chtít převzít řízení.

Tento postup funguje taky můžete přesouvat z Azure AD Sync nebo řešení s FIM + Azure AD spojnice. Tyto kroky nefungují DirSync, ale stejného dráha migrace (nazývané také paralelní nasazení) metody pomocí kroků DirSync najdete v [Upgradu Azure Active Directory (DirSync) synchronizovat](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="swing-migration-steps"></a>Dráha migračních kroků

1. Pokud používáte Azure AD Connect na serverech a plánujete pouze změny konfigurace, zkontrolujte active server a přípravu server používáte stejnou verzi. Který usnadňuje porovnání rozdílů později. Pokud upgradujete z Azure AD Sync, těchto serverech používají různé verze. Pokud upgradujete ze starší verze Azure AD Connect, je vhodné začít se dvěma servery ve stejné verzi, ale není potřeba.
2. Pokud jste provedli některé vlastní konfigurace a pracovní serveru ji nemá, postupujte podle pokynů v části [Přesunout vlastní konfigurace z aktivní pracovní server](#move-custom-configuration-from-active-to-staging-server).
3. Pokud upgradujete ze starší verzi Azure AD Connect, upgradujte na nejnovější verzi pracovní server. Pokud přesunujete z Azure AD Sync, nainstalujte na pracovní server Azure AD Connect.
4. Informujte modul synchronizace spuštění úplného importu a úplné synchronizaci na pracovní server.
5. Ověřte, že nové konfigurace nezpůsobí změny neočekávané pomocí postupu v části **ověření** ověření [Konfigurace serveru](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server). Pokud něco není jako očekávané, správný, spusťte import a synchronizace a ověření dat vypadá všechno dobře, dokud. Tento postup najdete v tématu propojené.
6. Změna pracovní serveru být active server. Jde o posledním kroku **přepněte active server** ověření [Konfigurace serveru](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Pokud upgradujete Azure AD Connect, upgradujte na server teď přípravu režimu na nejnovější verzi. Postupujte stejně jako před zobrazíte data a konfigurace upgradovat. Pokud jste upgradovali z Azure AD Sync, nyní můžete vypnout a vyřadit z provozu starý serveru.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Přesouvat vlastní konfigurace aktivní pracovní server
Pokud jste provedli změny konfigurace active server, budete muset Ujistěte se, že stejný změny budou provedeny pracovní server.

Vlastní synchronizace pravidla, která jste vytvořili přesouvat pomocí prostředí PowerShell. Další změny má být použito stejným způsobem jako v obou sítě a nelze migrovat.

Věc, kterou musíte nakonfigurovaný stejným způsobem jako na serverech:

- Připojení ke stejné strukturami.
- Domény a OU filtrování.
- Stejné volitelné funkce, například synchronizaci hesel a zpětným heslo.

**Přesunutí pravidla synchronizace**  
Přesunutí pravidla vlastní synchronizace, postupujte takto:

1. Otevřete **Editor pravidel synchronizace** na active server.
2. Vyberte vlastní pravidla. Klikněte na **Exportovat**. Tím se vyvolá okno Poznámkový blok. Uložte dočasný soubor s příponou PS1. Díky tomu skript Powershellu. Zkopírujte soubor ps1 pracovnímu serveru.  
![Export pravidla synchronizace](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Identifikátor GUID spojnice odlišuje pracovní serveru a musí změnit. Identifikátor GUID, spusťte **Synchronizaci pravidla Editor**, vyberte jednu z krabice pravidel představující stejné připojeného systému, a klikněte na **Exportovat**. Nahraďte GUID v souboru PS1 GUID z pracovního serveru.
4. Na příkazovém řádku prostředí PowerShell spusťte soubor PS1. Tím vytvoříte vlastní synchronizace pravidlo na testovacím serveru.
5. Pokud máte víc vlastní pravidla, opakujte pro všechny vlastní pravidla.

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
