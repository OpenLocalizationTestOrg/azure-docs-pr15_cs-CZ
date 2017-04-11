<properties
    pageTitle="Azure AD Connect stavu operace."
    description="Tento článek popisuje další operace, které lze provést po zavedení Azure AD stavu připojení."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure AD Connect stavu operace

Následující téma popisuje různých operacích, které lze provést pomocí Azure AD stavu připojení.

## <a name="enable-email-notifications"></a>Povolit e-mailová oznámení
Můžete nakonfigurovat Azure AD připojení stavu služby odeslat e-mailová oznámení při generování upozornění, že není v pořádku infrastrukturu identity. K tomu dojde při generování upozornění, i když je označen jako vyřešit. Postupujte podle těchto pokynů pro nastavení e-mailová oznámení.

![Azure AD Connect stavu e-mailové oznámení Seznamte se s](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] Ve výchozím nastavení jsou zakázaná e-mailová oznámení.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Chcete-li povolit Azure AD připojení stavu e-mailová oznámení

1. Otevřete zásuvné oznámení služby, u kterého chcete dostávat e-mailového oznámení.
2. Klikněte na tlačítko "Nastavení oznámení" z panelu akcí.
3. Zapnutí e-mailového oznámení přepínač na zapnuto.
4. Zaškrtněte políčko Konfigurace globálních správců pro příjem e-mailová oznámení.
5. Pokud chcete dostávat e-mailová oznámení na jiných e-mailové adresy, můžete je zadat do pole dalšího e-mailu příjemce. Z tohoto seznamu odebrat e-mailovou adresu, klikněte na položku pravým tlačítkem myši a vyberte odstranit.
6. Po dokončení změn klikněte na "Uložit". Všechny změny bude trvat efekty, jenom když vyberete "Uložit".

## <a name="delete-a-server-or-service-instance"></a>Odstranění instanci server nebo službu

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Odstranění serveru služby Azure AD připojení stavu
V některých případech můžete chtít odebrání serveru ze sledován. Postupujte podle pokynů níže a odebrání serveru ze služby Azure AD připojení stavu.

Při odstraňování serveru, mějte na paměti toto:

- Tato akce se ZASTAVÍ shromažďování žádná další data ze serveru. Tento server se odstraní z sledování služby. Po této akci nebudete moct dívat na nové upozornění, sledování a používání technologie pro analýzu dat pro tento server.
- Tato akce není odinstalovat nebo odebrat Agent stavu ze serveru. Pokud před provádění tohoto kroku jste odinstalovali Agent stavu, se zobrazí chyba události při serveru související s agentem stavu.
- Tato akce se neodstraní už shromážděné z tohoto serveru. Tato data se odstraní podle Microsoft Azure zásady uchovávání informací.
- Po provedení této akce, pokud chcete spustit akci, sledování na stejný server musíte odinstalovat a znova nainstalovat agent stavu na tomto serveru.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Chcete-li odstranit na server služby Azure AD připojení stavu

Azure AD Connect stavu služby AD FS a Azure AD připojení (synchronizace):

1. Otevřete zásuvné Server ze seznamu zásuvné Server tak, že vyberete název serveru má být odebrán.
2. Na zásuvné serveru klikněte na tlačítko "Odstranit" z panelu akcí.
3. Potvrďte odstranění serveru tak, že zadáte název serveru v potvrzovacím okně.
4. Klikněte na tlačítko "Odstranit".

Azure AD Connect stavu služby AD DS:

1. Otevřete řídicím panelu řadiče domény.
2. Vyberte řadiče domény má být odebrán.
3. Klikněte na tlačítko "Odstranit vybrané" z panelu akcí.
4. Potvrďte odstranění serveru.
5. Klikněte na tlačítko "Odstranit".

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Odstranění instanci služby služby Azure AD připojení stavu

V některých případech můžete chtít odeberte instanci služby. Postupujte podle pokynů níže a odeberte instanci služby služby Azure AD připojení stavu.

Při odstraňování instanci služby, mějte na paměti toto:

- Tato akce odebere aktuální instanci služby z monitorování služby.
- Tato akce se není odinstalovat nebo odebrat Agent stavu ze všech serverů, které byly sledovány jako součást této instance služby. Pokud jste odinstalovali Agent stavu před provádění tohoto kroku, se zobrazí chyba události při servery související s agentem stavu.
- Podle Microsoft Azure zásady uchovávání informací se odstraní všechna data v této instanci služby.
- Po provedení této akce, pokud chcete spustit sledování službu, odinstalujte a znovu nainstalujte agent stavu na všech serverech, které bude sledovat. Po provedení této akce, pokud chcete začít znova, sledování na stejný server musíte odinstalovat a znova nainstalovat agent stavu na tomto serveru.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Chcete-li odstranit instanci služby služby Azure AD připojení stavu

1. Otevřete zásuvné služby z zásuvné seznam služby vybráním identifikátor service (farmy jméno), kterou chcete odebrat.
2. Na zásuvné serveru klikněte na tlačítko "Odstranit" z panelu akcí.
3. Potvrďte název služby jeho opětovným zadáním do pole potvrzení. (například: sts.contoso.com)
4. Klikněte na tlačítko "Odstranit".
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Správa přístupu k roli na základě řízení přístupu
### <a name="overview"></a>Základní informace
[Řízení přístupu na základě rolí](role-based-access-control-configure.md) pro stav připojení Azure AD poskytuje přístup služby Azure AD stavu připojení k uživatelům nebo skupinám mimo globální správci. Probíhá zadáváním přiřazení role do skupiny nebo uživatelé, kteří a poskytuje mechanismus omezit globální správci v adresáři.

#### <a name="roles"></a>Role
Stav připojení Azure AD podporuje následující předdefinované role.

| Role | Oprávnění |
| ----------- | ---------- |
| Vlastník | Vlastníci umožňuje ***Spravovat přístup*** (například přiřadit roli uživatele nebo skupinu), ***Zobrazit všechny informace*** (například zobrazit upozornění) z portálu a ***změnit nastavení*** (například e-mailová oznámení) v rámci stavu připojení Azure AD. <br>Ve výchozím nastavení globální správci služby Azure AD přiřazené tuto roli a nejde je změnit.  |
|Skupiny přispěvatelů|  Přispěvatelům můžete ***Zobrazit všechny informace*** (například zobrazit upozornění) z portálu a ***změnit nastavení*** (například e-mailová oznámení) v rámci Azure AD stavu připojení.|
|Čtečky| Čtenáři můžete ***Zobrazit všechny informace*** (například zobrazit upozornění) z portálu Microsoft ve stavu připojení Azure AD.|

Všechny ostatní role (například "Správci přístup uživatelů" nebo "DevTest Labs Users"), i když k dispozici v portálu prostředí mít žádný vliv na přístup ve stavu připojení Azure AD.

#### <a name="access-scope"></a>Obor přístupu

Azure AD Connect podporuje řízení přístupu na dvou úrovních:

- ***Všechny instance služby***: to je doporučený pro většinu zákazníků a ovládací prvky Accessu pro všechny instance služby (například farmě služby AD FS) všech typů role, které jsou sledován Azure AD stavu připojení.

- ***Instanci služby***: V některých případech může musíte oddělit přístupu na základě rolí typy nebo instancí služby. V tomto případě se dá řídit přístup na úrovni instanci služby.  

Udělení oprávnění pokud koncový uživatel má přístup k buď na úrovni adresáře nebo instanci služby.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Jak umožnit uživatelům nebo skupinám přístup k Azure AD stavu připojení
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Postup 1: Vyberte rozsah odpovídající přístup
Pokud chcete povolit přístup uživatelů na úrovni *všechny instance služby* v rámci stavu připojení Azure AD, otevřete hlavní zásuvné v Azure AD stavu připojení.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Krok 2: Přidání uživatelů, skupin a přiřadit role
1. Klikněte na část "Users" z části konfigurace.<br>
![Azure AD Connect hlavní zásuvné RBAC stavu](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Vyberte "Přidat"
3. Vyberte "Roli", například "Vlastník"<br>
![Azure AD Connect stavu RBAC přidat uživatele](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Zadejte název nebo identifikátor cílových uživatele nebo skupiny. Můžete vybrat jeden nebo několik uživatelů a skupin ve stejnou dobu. Klikněte na "výběr".
![Azure AD Connect stavu RBAC vybrat uživatele](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Vyberte "Ok".<br>

6. Po dokončení přiřazení role uživatele nebo skupiny se zobrazí v seznamu.<br>
![Azure AD Connect stavu RBAC uživatel seznamu](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Tento postup vám umožní uvedené uživatele a skupiny přístup podle jejich přiřazené role.
>[AZURE.NOTE]
- Globální správci vždy mají úplný přístup k všechny operace ale účty globální správce nebude vyskytovat v seznamu.
- Funkce "Pozvat uživatele" není podporováno v Azure AD stavu připojení.

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Krok 3: Umístění zásuvné nasdílet uživatele nebo skupiny
1. Po přiřazení oprávnění, uživatel může dostat stavu připojení Azure AD tak, že přejdete do [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth).
2. Jednou na zásuvné, uživatel můžete připnout na zásuvné nebo různé části na řídicí panel jednoduše kliknutím na "Připnout na řídicí panel"<br>
![Zásuvné pin Azure AD RBAC stavu připojení](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Uživatel s přiřazenou roli "Čtečka" nebude moct operaci "vytvořit" k získání rozšíření stavu připojení Azure AD z Azure Marketplace. Tento uživatel dá pořád dostat zásuvné tak, že přejdete k výše uvedeného odkazu. Pro pozdější použití může uživatel připnout zásuvné na řídicí panel.

### <a name="remove-users-andor-groups"></a>Odebrání uživatelů nebo skupin
Můžete odebrat uživatele nebo skupiny přidané do části Azure AD připojení stavu Role pro řízení přístupu kliknutí pravým tlačítkem myši a vyberte odebrat.<br>
![Azure AD Connect stavu RBAC odebrat uživatele](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Související odkazy

* [Azure AD Connect stavu](active-directory-aadconnect-health.md)
* [Azure AD Connect instalaci agenta stavu](active-directory-aadconnect-health-agent-install.md)
* [Použití Azure AD stavu připojení se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Použití stavu připojení Azure AD pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Použití Azure AD stavu připojení se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect stavu časté otázky](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect historie verzí stavu](active-directory-aadconnect-health-version-history.md)
