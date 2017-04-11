<properties
    pageTitle="Sestava nelicencovaný využití | Microsoft Azure"
    description="Pomáhá sestavy není licencovaný použití identifikovat nelicencovaných uživatelů, kteří používají zaplacený Azure AD funkce."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="unlicensed-usage-report"></a>Sestava využití bez licence

Pomáhá sestavy není licencovaný použití identifikovat nelicencovaných uživatelů, kteří používají zaplacený Azure AD funkce. Díky lze zlepšit pomocí licencí, které jste zakoupili a k identifikaci vás upozorní na budete potřebovat další licence. 

Sestava zobrazuje aktivní použití placené funkcí v posledních 30 dní. 

## <a name="report-structure"></a>Struktura sestavy
 
| Název sloupce          |    Popis |
| :--                  | :--         |
| Bez licence uživateli      |    Jméno uživatele |
| Funkce              | Název funkce. Příklad: podmíněné přístup |
| Aplikace k nim získat přístup | Název aplikace, která pracuje s funkcí. Příklad: Sharepointu Online v Office 365 |

 
> [AZURE.NOTE] Pokud byl odstraněn uživatelského účtu "Nelicencovaný uživatelem" sloupce se zobrazí s ID, jako je 1003000090D8B285


## <a name="conditional-access-feature"></a>Funkce podmíněné přístup

Při přístupu k služba, která má použít, pokud nemají licenci Azure AD Premium zásady podmíněné přístupu budou označeny příznakem uživatelů bez licence. 

Týká se MFA / zásady umístění a také zařízení zásady využívající Intune.
 

## <a name="see-also"></a>Viz taky

- [Použití podmíněného Accessu s Office 365 a další služby Azure Active Directory připojení aplikace](active-directory-conditional-access.md)
- [Začínáme s podmíněným přístup k Azure AD](active-directory-conditional-access-azuread-connected-apps.md) 


