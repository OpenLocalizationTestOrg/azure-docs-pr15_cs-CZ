<properties
    pageTitle="Azure Active Directory rutiny pro konfiguraci nastavení skupiny | Microsoft Azure"
    description="Jak spravovat nastavení pro skupiny pomocí rutin Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory rutiny pro konfiguraci nastavení skupiny

Následující nastavení pro jednotné skupiny lze nakonfigurovat ve vašem adresáři:

1.  Klasifikace: hodnotami oddělenými čárkou seznam klasifikace, které uživatelé mohou nastavit ve skupině. Příklady bude "Klasifikovaná", "Tajná" a "Nejvyšší tajná."

2.  Použití URL pokyny: adresa URL odkazující uživatele s podmínkami použití pro používání skupin služby Unified způsobem definovaným ve vaší organizaci. Tato adresa URL se zobrazí v uživatelském rozhraní, kde uživatelé používat skupiny.

3.  Seskupení vytváření povolené: jestli žádný, některých nebo všech mohou uživatelé vytvářet Unified skupiny. Když nastavená na zapnuto, všichni uživatelé můžete vytvářet skupiny. Když nastavení na vypnuto, žádní uživatelé můžete vytvářet skupiny. Když vypnout, můžete taky určit skupinu zabezpečení u uživatelů, kteří jsou pořád moct vytvářet skupiny.

Toto nastavení se konfigurují nastavení a SettingsTemplate objektů. Nejprve nezobrazí k objektům nastavení v adresáři. To znamená, že adresáři nakonfigurovaný s výchozím nastavením. Chcete-li změnit výchozí nastavení, vytvoříte nový objekt nastavení pomocí šablony nastavení. Nastavení šablony jsou definovány Microsoft.

Můžete si stáhnout modul obsahující rutiny pro tyto operace z [webu Microsoft připojit](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).

## <a name="create-settings-at-the-directory-level"></a>Namapovat na úrovni adresáře

Tento postup namapovat na úrovni adresáře, které se vztahují ke všem skupinám Office v adresáři.

1. Pokud neznáte které SettingTemplate používat, tato rutina vrací seznam nastavení šablon:

    `Get-MsolAllSettingTemplate`

    ![Seznam šablon nastavení](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Můžete přidat adresu URL obecné zásady použití, nejprve potřebujete k získání SettingsTemplate objektu, který definuje hodnota URL obecné zásady použití; To znamená Group.Unified šablony:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Dále vytvořte nový objekt nastavení založený na této šabloně:

    `$setting = $template.CreateSettingsObject()`

4. Změňte hodnotu obecné zásady použití:

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Nakonec použijte nastavení:

    `New-MsolSettings –SettingsObject $setting`

    ![Přidání adresy URL obecné zásady použití](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Tady jsou definované v Group.Unified SettingsTemplate nastavení.

 **Nastavení**                          | **Popis**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Typ: řetězce<li>Výchozí: ""                  | Seznam oddělený středníkem platné klasifikace hodnoty, které se dají použít pro jednotné skupiny.                
 <ul><li>EnableGroupCreation<li>Typ: logické<li>Výchozí: PRAVDA              | Příznak označující, zda je povolen vytváření skupin jednotné v adresáři.                               
 <ul><li>GroupCreationAllowedGroupId<li>Typ: řetězce<li>Výchozí: ""         | Identifikátor GUID skupiny zabezpečení umožňující vytvořit jednotné skupiny i v případě EnableGroupCreation hodnotu false.
 <ul><li>UsageGuidelinesUrl<li>Typ: řetězce<li>Výchozí: ""                  | Odkaz na pokyny týkající se používání skupiny.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Nastavení pro čtení na úrovni adresáře

Tento postup číst nastavení na úrovni adresářů, které se vztahují ke všem skupinám Office v adresáři.

1. Přečtěte si všechny existující nastavení adresáře:

    `Get-MsolAllSettings`

2. Přečtěte si všechna nastavení pro určité skupiny:

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Nastavení určitých adresáři pomocí SettingId GUID čtení:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Nastavení Identifikátor GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Aktualizace nastavení na úrovni adresáře

Tento postup aktualizujte svá nastavení na úrovni adresáře, které se vztahují ke všem skupinám Office v adresáři.

1. Získání existující objekt nastavení:

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Zobrazit hodnotu, kterou chcete aktualizovat:

    `$value = $Setting.GetSettingsValue()`

3. Aktualizace hodnotu:

    `$value["AllowToAddGuests"] = "false"`

4. Aktualizace nastavení:

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Odebrání nastavení na úrovni adresáře

V tomto kroku odeberete nastavení na úrovni adresářů, které se vztahují ke všem skupinám Office v adresáři.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Rutina syntaktické reference

Následující dokumentaci pro další Azure Active Directory PowerShell můžete najít v [Azure Active Directory rutiny](http://go.microsoft.com/fwlink/p/?LinkId=808260).

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>Odkaz na objekt SettingsTemplate (Group.Unified SettingsTemplate objekt)

- "název": "EnableGroupCreation", "typ": "System.Boolean", "Výchozí": "PRAVDA", "Popis": "Příznak typu boolean označující, pokud funkce vytváření jednotné skupina zapnuté."

- "název": "GroupCreationAllowedGroupId", "typ": "System.Guid", "Výchozí": "", "Popis": "Identifikátor GUID skupiny zabezpečení, která je povolené vytvořit skupiny jednotné."

- "název": "ClassificationList", "typ": "System.String", "Výchozí": "", "Popis": "S hodnotami oddělenými čárkou seznamu platné klasifikace hodnot, které se dají použít pro jednotné skupin."

- "název": "UsageGuidelinesUrl", "typ": "System.String", "Výchozí": "", "Popis": "Odkaz na pokyny skupiny použití."

Jméno | Typ | Výchozí hodnota | Popis
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "System.Boolean"  | "true"  | "Příznak typu boolean označující, pokud funkce vytváření jednotné skupina zapnuté."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "Identifikátor GUID skupiny zabezpečení, která je povolené k vytváření skupin služby Unified."
"ClassificationList"  | "System.String"  | ""  | "Oddělenými čárkou seznam hodnot platné klasifikace použité sjednocený seskupením."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "Odkaz na pokyny skupiny použití."

## <a name="next-steps"></a>Další kroky

Následující dokumentaci pro další Azure Active Directory PowerShell můžete najít v [Azure Active Directory rutiny](http://go.microsoft.com/fwlink/p/?LinkId=808260).

Další informace od nadřízeného programu Microsoft Rudolf de Jong je k dispozici v [Jeho skupiny blogu](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad).

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)

* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
