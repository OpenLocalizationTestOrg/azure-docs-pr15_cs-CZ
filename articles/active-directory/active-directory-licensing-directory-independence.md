<properties
   pageTitle="Přidání a Správa více adresářů Azure Active Directory | Microsoft Azure"
   description="Pokyny a doporučené postupy pro přidání a Správa adresářů služby Azure Active Directory vysvětlující adresáře jako nezávislým zdroje"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Přidání a Správa více adresářů Azure Active Directory

V Azure Active Directory (Azure AD), je každý adresář nezávislým zdroje: partnera, plně funkční a logicky nezávisle na ostatních adresářů, které spravujete. Neexistuje žádný vztah nadřazenosti a podřízenosti mezi adresáře. Tento nezávislosti mezi adresáři obsahuje nezávislosti zdroje, pro správu nezávislosti a synchronizace nezávislosti.

##<a name="resource-independence"></a>Nezávislosti zdroje

Pokud vytváříte nebo odstranění zdroje v jednom adresáři nemá žádný vliv na všechny zdroje v jiném adresáři, s výjimkou částečné externích uživatelů, píše níže. Pokud používáte vlastní doménu "contoso.com" s jednoho adresáře, nelze použít s jiné adresáře.

##<a name="administrative-independence"></a>Pro správu nezávislosti

Pokud není pro správu uživatelů adresáře "Contoso" vytvoří adresář test "Zkouška" potom:
- Ve výchozím nastavení je uživatel, který vytváří adresář přidali jako externího uživatele do tohoto nového adresáře a přiřazenou roli globálního správce v adresáři.
- Správci adresáře "Contoso" oprávnění žádné přímé správce do adresáře "Zkouška" Pokud správce "Testu" konkrétně uděluje je tato oprávnění. Správci "Contoso" můžete řídit přístup k adresáři "Zkouška"-li určit uživatelský účet, který vytvořené Test.
- Pokud změníte (Přidat nebo odebrat) roli správce pro uživatele v jednom adresáři změna nemá vliv na všechny roli správce, který získá uživatel může do jiné složky.

##<a name="synchronization-independence"></a>Synchronizace nezávislosti

Můžete nakonfigurovat každý Azure AD adresář nezávisle na sobě k načtení dat synchronizují z jednoho instanci buď:
  - Nástroj synchronizace adresářů (DirSync) synchronizace dat s strukturu jednoho AD.
  - Azure Active Directory konektoru pro Forefront Identity správce, které synchronizace dat s jeden nebo více místní strukturami, a/nebo zdroje dat není Azure AD.

##<a name="add-an-azure-ad-directory"></a>Přidání Azure AD adresáře

Přidat adresář Azure AD na portálu Azure klasické, vyberte koncovku Azure Active Directory na levé straně a klepněte na **Přidat**.

> [AZURE.NOTE]   Na rozdíl od jiných Azure zdroje vašeho adresáře nejsou podřízené zdroji Azure předplatného. Je-li zrušit nebo povolení Azure předplatné vyprší máte pořád přístup k datům directory pomocí prostředí PowerShell Azure, rozhraní API grafu Azure nebo jiná rozhraní například Centrum pro správu Office 365. Jiné předplatné můžete také propojit s adresářem.

Obecných Přehled licencování problémů Azure AD a osvědčené postupy naleznete v tématu [Co je Azure Active Directory licencování?](active-directory-licensing-what-is.md).
