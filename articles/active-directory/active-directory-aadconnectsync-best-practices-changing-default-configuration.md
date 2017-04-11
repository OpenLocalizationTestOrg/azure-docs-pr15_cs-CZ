<properties
    pageTitle="Azure AD Connect synchronizace: doporučené postupy pro změnu výchozí konfigurace | Microsoft Azure"
    description="Poskytuje doporučené postupy pro změnu výchozí konfigurace Azure AD Connect synchronizace."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect synchronizace: doporučené postupy pro změnu výchozí konfigurace
Účelem Toto téma je popisují podporované a nepodporované změny Azure AD Connect synchronizace.

Konfigurace vytvořil Azure AD Connect funguje ve většině prostředí, které se synchronizují místní služby Active Directory s Azure AD "je". V některých případech je nutné použít některé změny konfigurace splňují určité nebo požadavek.

## <a name="changes-to-the-service-account"></a>Změny účet služby
Azure AD Connect synchronizace běží v části účet služby vytvořené pomocí Průvodce instalací. Tento účet služby obsahuje šifrovacího klíče k databázi používá synchronizace. Je vytvořená pomocí dlouhé heslo 127 znaků a je nastavená neomezená hesla.

- Je **nepodporované** změna nebo resetování hesla účtu služby. Tím odstraní šifrovacího klíče a služba není mít přístup k databázi a není možné spustit.

## <a name="changes-to-the-scheduler"></a>Změny Plánovač
Od vydání z sestavení 1.1 (únor 2016) můžete nakonfigurovat [Plánovač](active-directory-aadconnectsync-feature-scheduler.md) mít různých synchronizace obrázku než výchozí 30 minut.

## <a name="changes-to-synchronization-rules"></a>Změny v pravidlech synchronizace
Průvodce instalací poskytuje konfigurace, který má fungovat obvyklé scénáře. V případě, že potřebujete udělat změny konfigurace, je třeba dodržovat tato pravidla přesto podporované konfigurace.

- Můžete [změnit atribut toků](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) Pokud toků přímé atribut výchozí není vhodný pro vaši organizaci.
- Pokud chcete [toku atribut](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) a odeberte všechny existující hodnoty atributu v Azure AD, je potřeba vytvořit pravidlo pro tento scénář.
- [Zakažte pravidlo nežádoucí synchronizace](#disable-an-unwanted-sync-rule) namísto jeho odstranění. Odstraněné pravidla znovu vytvoří během upgradu.
- Chcete-li [Změnit pravidlo mimo pole](#change-an-out-of-box-rule)by měl vytvořit kopii původního pravidla a zakažte pravidlo mimo pole. Editor pravidel synchronizace zobrazí výzvu a pomůže vám.
- Exportujte pravidel vlastní synchronizace pomocí editoru pravidla synchronizace. Editor poskytuje skript Powershellu, které můžete snadno znovu je vytvořte v případě havárie obnovení.

>[AZURE.WARNING] Synchronizace s krabice pravidla mají miniaturu. Pokud změníte tato pravidla, miniaturou již odpovídající. V budoucnu, když se pokusíte použít novou verzi Azure AD Connect může dojít k problémům. Pouze měnit způsob, jakým je popsán v tomto článku.

### <a name="disable-an-unwanted-sync-rule"></a>Zakažte pravidlo nežádoucích synchronizace
Neodstraňujte pravidlo mimo pole synchronizace. Ho znovu vytvoří během upgradu Další.

V některých případech má Průvodce instalací vyrobeno konfigurace, které nefunguje k topologii. Například pokud máte topologie doménové účtu zdroje, ale obsahují rozšířené schéma ve struktuře účet s schématu serveru Exchange, potom pravidla pro Exchange vytvořené pro struktuře účtu a struktuře zdroje. V takovém případě budete muset zakázání synchronizace pravidlo pro Exchange.

![Vypnutí synchronizace pravidla](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Na obrázku nahoře Průvodce instalací zjistil schématem staré Exchange 2003 ve struktuře účtu. Toto schéma rozšíření jsme přidali před struktuře zdroje byla zavedená v prostředí společnosti Fabrikam. Abyste měli jistotu, že jsou synchronizovány žádné atributy ze starého implementaci Exchange, by měl pravidlo synchronizace vypnutá, jak je vidět.

### <a name="change-an-out-of-box-rule"></a>Změnit pravidlo mimo pole
Pokud potřebujete ke změnám pravidlo mimo pole, zkopírujte pravidlo mimo pole a zakažte původní pravidlo. Proveďte požadované změny klonovaný pravidla. Editor pravidel synchronizace vám pomáhá s těmito kroky. Při otevření pravidlo mimo pole se zobrazí v tomto dialogovém:  
![Upozornění mimo pole pravidla](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Výběrem možnosti **Ano** vytvořit kopii pravidla. Otevře se pak klonovaný pravidlo.  
![Klonovaný pravidla](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Na tomto klonovaný pravidle proveďte potřebné změny obor, připojení a transformací.

## <a name="next-steps"></a>Další kroky

**Přehled témat**

- [Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace](active-directory-aadconnectsync-whatis.md)
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
