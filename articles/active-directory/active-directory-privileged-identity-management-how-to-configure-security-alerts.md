<properties
   pageTitle="Postup při konfiguraci výstrah zabezpečení | Microsoft Azure"
   description="Zjistěte, jak nakonfigurovat výstrah zabezpečení pro rozšíření privilegovaných Správa identit Azure."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/02/2016"
   ms.author="kgremban"/>

# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Postup při konfiguraci výstrah zabezpečení v Azure AD privilegovaných Správa identit

## <a name="security-alerts"></a>Upozornění zabezpečení
Azure privilegovaných Identity Správa osobních vygeneruje upozornění, když podezřelé nebo nebezpečných aktivity v prostředí. Pokud se výstraha, se zobrazí na řídicím panelu Správce osobních informací. Vyberte upozornění zobrazíte sestavu obsahující seznam uživatelů nebo role, které spuštěná výstrahu.

![Výstrahy zabezpečení řídicího panelu Správce osobních informací – snímek][1]



| Upozornit | Aktivační událost | Doporučení |
| ----- | ------- | -------------- |
| **Role se přiřazují mimo správce osobních informací** | Správce přidělené trvale roli mimo rozhraní správce osobních informací. | Kontrola nových přiřazování rolí. Od jiných služeb, můžete přiřadit pouze trvalé správci, měl přiřazení podle potřeby změňte. |
| **Role jsou aktivované příliš často** | V rámci nastavení nejsou příliš mnoho opětovných aktivací stejné role. | Obraťte se na uživatele, aby najdete v článku proč budou aktivovali roli tolik časy. Možná časový limit je příliš krátká pro je k dokončení úkolů, případně možná jste pomocí skriptů automatickou aktivaci roli. |
| **Role nevyžadují vícefaktorové ověřování pro aktivaci** | Existují role bez MFA povolené v nastavení. | Jsme vyžadují MFA pro většinu vysoce privilegovaných role, ale důrazně vyzývá povolit MFA pro aktivaci všech rolí. |
| **Správci nepoužíváte jejich privilegovaných role** | Existuje měl správci, které nebyly aktivovali jejich role naposledy. | Spusťte access revize k určení uživatelů, které už nepotřebujete přístup. |
| **Existuje příliš mnoho globální správce** | Existuje více globální správci než doporučené. | Pokud máte velké množství globální správci, je pravděpodobné, že uživatelé se zobrazují více oprávnění než potřebují. Přesunutí uživatelů do méně privilegovaných role nebo proveďte některé z nich nárok na využití namísto trvale přiřazené. |

## <a name="configure-security-alert-settings"></a>Konfigurace nastavení upozornění zabezpečení

Můžete upravit některá upozornění zabezpečení v osobních pro práci s prostředím a cíle zabezpečení. Tímto postupem dosáhla zásuvné nastavení:

1. Přihlaste se k [portálu Azure](https://portal.azure.com/) a vyberte dlaždici **Azure AD privilegovaných Správa identit** na řídicím panelu.
2. Vyberte **Spravovat privilegovaných role** > **Nastavení** > **Nastavení upozornění**.

    ![Přejděte na nastavení upozornění zabezpečení][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>"Role jsou aktivované příliš často" upozornění

Toto oznámení aktivuje, pokud uživatel aktivuje stejnou privilegovaných roli tisknutím v rámci za určité období. Můžete nakonfigurovat časového období a počet jejích aktivací.

- **Aktivace prodloužení určeném časovém rozmezí**: Zadejte dní, hodin, minut a druhé časové období, který chcete použít ke sledování podezřelé obnovení.

- **Počet obnovení aktivace**: nastavte počet aktivací 2 až 100, které považujete za worthy upozornění v určeném časovém rozmezí jste zvolili. Je toto nastavení můžete změnit tak, že posunete jezdec nebo zadáním čísla do textového pole.


### <a name="there-are-too-many-global-administrators-alert"></a>"Nejsou příliš mnoho globální správci" upozornění

Správce osobních informací spustí toto oznámení, pokud jsou splněny dvou různých kritérií a můžete nakonfigurovat oba. Nejdřív budete muset kontaktovat určitou prahovou hodnotu globální správci. Za druhé určité procento celkové rolemi musí být globální správce. Pokud splňujete pouze jednu z těchto měrné jednotky, upozornění se nezobrazí.  

- **Minimální počet globální správce**: nastavte počet globální správci, 2 až 100, zvažte nebezpečných částky.

- **Procento globální správce**: Zadejte procentuální hodnotu správce, kteří jsou globální správci, 0 % na 100 %, který není z nebezpečných ve vašem prostředí.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>"Správci nepoužíváte jejich privilegovaných role" upozornění

Toto oznámení aktivuje, když uživatel přejde určitou dobu bez aktivace roli.

- **Spočítá počet dnů**: Zadejte počet dní, v rozsahu 0 až 100, osám uživatele bez aktivace roli.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
