<properties
   pageTitle="Azure Active Directory vykazování čekacích dob | Microsoft Azure"
   description="Jak dlouho trvá pro vytváření sestav události objeví v Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-latencies"></a>Čekacích Azure Active Directory Sestava dob

*Tento si přečtěte následující dokumentaci je součástí [Azure Active Directory vykazování Průvodce](active-directory-reporting-guide.md).*

Sestava                                                  | Minimální  | Průměr    | Maximální
------------------------------------------------------- | -------- | ---------- | ----------
**Zabezpečení sestavy**                                    |          |            |
Aktivity nepravidelné přihlášení                              | 2 hodin  | 4 hodiny    | 8 hodin
Přihlášení z neznámých zdrojů                           | 2 hodin  | 4 hodiny    | 8 hodin
Přihlášení za k více chybám                        | 2 hodin  | 4 hodiny    | 8 hodin
Přihlášení z různých zeměpisných                      | 2 hodin  | 4 hodiny    | 8 hodin
Přihlášení z IP adres pomocí podezřelé aktivity     | 2 hodin  | 4 hodiny    | 8 hodin
Přihlášení z možného infikovaných zařízení                 | 2 hodin  | 4 hodiny    | 8 hodin
Uživatelé, kteří mají neobvyklých aktivity přihlášení                   | 2 hodin  | 4 hodiny    | 8 hodin
Uživatelé, kteří mají prozrazený přihlašovací údaje                           | 2 hodin  | 4 hodiny    | 8 hodin
**Použití sestav**                                 |          |            |
Účet zřizování aktivity                           | 2 hodin  | 4 hodiny    | 8 hodin
Účet chyby zřizování                             | 2 hodin  | 4 hodiny    | 8 hodin
Použití aplikace                                       | 2 hodin  | 4 hodiny    | 8 hodin
Stav při přechodu myší hesla                                | 2 hodin  | 4 hodiny    | 8 hodin
**Auditování & sestavy aktivit**                            |          |            |
Sestavy auditování                                            | 1 minuta | 15 minut | 30 minut
Resetování hesla aktivity (Azure AD)                      | 2 hodin  | 4 hodiny    | 8 hodin
Resetování hesla aktivity (Správce identit)              | 2 hodin  | 4 hodiny    | 8 hodin
Resetování hesla registrace aktivity (Azure AD)         | 2 hodin  | 4 hodiny    | 8 hodin
Resetování hesla registrace aktivity (Správce identit) | 2 hodin  | 4 hodiny    | 8 hodin
Vlastní skupiny aktivita (Azure AD) služby                 | 2 hodin  | 4 hodiny    | 8 hodin
Vlastní skupiny aktivita (Správce identit) služby         | 2 hodin  | 4 hodiny    | 8 hodin
**Sestavy služby RMS**                                         |          |            |
Většina aktivní uživatelé RMS                                   | 2 hodin  | 4 hodiny    | 8 hodin
Použití RMS                                               | 2 hodin  | 4 hodiny    | 8 hodin
Použití zařízení RMS                                        | 2 hodin  | 4 hodiny    | 8 hodin
Použití aplikace RMS povolena                           | 2 hodin  | 4 hodiny    | 8 hodin
**Soukromé náhled sestavy**                             |          |            |
Všechny uživatelské přihlašovací aktivity                               | 2 hodin  | 4 hodiny    | 8 hodin
