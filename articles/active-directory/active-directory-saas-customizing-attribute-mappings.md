<properties
    pageTitle="Přizpůsobení mapování atributů | Microsoft Azure"
    description="Zjistěte, jaké mapování atributů SaaS aplikací v Azure Active Directory jak je možné je upravit řeší vaše obchodní potřeby."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="customizing-attribute-mappings"></a>Přizpůsobení mapování atributů


Microsoft Azure AD podporuje zřizování aplikacím třetích stran SaaS například Salesforce, Google Apps a ostatní uživatele. Pokud máte zřizování uživatele pro aplikace SaaS povolené jiného výrobce, řídí portálu pro správu Azure hodnoty atributu ve formuláři Konfigurace s názvem "mapování atributů".

Existuje předkonfigurované sada mapování atributů mezi Azure AD uživatele a každý SaaS aplikace uživatele objekty. Některé aplikace spravovat jiné druhy objekty, například skupiny nebo kontakty. <br> 
Výchozí mapování atributů můžete přizpůsobit podle potřeb uživatele. To znamená, můžete změnit nebo odstranit existující mapování atributů nebo vytvořte nové mapování atributů.

Na portálu Azure AD dostanete tuto funkci po kliknutí na atributy v panelu nástrojů aplikace SaaS.

> [AZURE.NOTE] Odkaz **atributy** neexistuje pouze pokud máte zřizování uživatele pro aplikaci SaaS povoleno. 


![Služby Salesforce][1] 


Když kliknete na panelu nástrojů seznamu aktuální mapování, které jsou nakonfigurovány aplikace SaaS atributy.

Následující obrázek ukazuje příklad pro takto:



![Služby Salesforce][2]  


V uvedeném příkladu, zobrazí se, že **jméno** atributu spravovaných objektu v služby Salesforce vyplněné **givenName** přínosu propojený objekt Azure AD.

Pokud chcete přizpůsobit atribut mapování nebo po Pokud se chcete vrátit vlastními nastaveními zpátky na výchozí konfigurace, musíte po kliknutí na související tlačítka na panelu nástrojů v dolní části aplikace.


![Služby Salesforce][3]  


Existuje mapování atributů, která jsou potřebná aplikací SaaS budou fungovat správně. V tabulce atributy mapování související atributů **Ano** mít jako hodnotu atributu **povinné** . Pokud je požadována mapování atribut, nelze odstranit. V tomto případě **Odstranit** funkce není dostupná.

Chcete-li upravit existující mapování atribut, jednoduše vyberte mapování a klikněte na **Upravit**. Tím se vyvolá dialogovou stránku, který umožňuje upravit vybraný atribut mapování.


![Úprava mapování atributu][4]  



## <a name="understanding-attribute-mapping-types"></a>Principy atribut mapování typů


Pomocí mapování atributů můžete určit, jak vyplněné atributy ve třetím narozeninovou párty s motivy SaaS aplikace. Existují čtyři různé mapování typy podporované:

- **Přímý** – cílový atribut je vyplněné hodnotu atributu propojený objekt v Azure AD.


- **Konstantní** – cílový atribut je vyplněn určitý řetězec, který jste definovali.


- **Výraz** - cílový atribut je vyplněné na základě výsledku výrazu skript profesionálové. Další informace najdete v tématu [Výrazy psaní pro mapování atributů v Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).


- **Žádná** – cílový atribut je vlevo smíte bez jakýchkoli úprav. Ale pokud cílový atribut je někdy prázdný, ho bude žádné s výchozí hodnotou, který určíte.



Kromě tyto čtyři typy mapování základní atribut podporují mapování vlastních atributů koncepci přiřazení **výchozí** hodnoty. Přiřazení výchozí hodnoty zaručuje, že cílový atribut je vyplněné s hodnotou, pokud není žádná hodnota v Azure AD ani v cílovém objektu.

Společnost Microsoft Azure AD poskytuje efektivní provádění proces synchronizace. V prostředí spuštění jsou zpracovány pouze objekty vyžadujících aktualizace při synchronizaci obrázku. Aktualizace mapování atributů má vliv na výkon cyklu synchronizace. Je to proto vyžaduje aktualizaci konfigurace mapování atribut všechny spravované objekty znovu vyhodnocena. Z toho důvodu je doporučený osvědčený postup zachovat počet po sobě jdoucí změny mapování atributů minimálně.


##<a name="related-articles"></a>Související články

- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
- [Automatizace uživatele zřizování/zrušení zajišťování SaaS aplikace](active-directory-saas-app-provisioning.md)
- [Psaní výrazů pro mapování atributů](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Definování rozsahu filtry pro zřizování uživatelů](active-directory-saas-scoping-filters.md)
- [Použití SCIM povolit automatické zřizování uživatelů a skupin služby Azure Active Directory pro aplikace](active-directory-scim-provisioning.md)
- [Účet zřizování oznámení](active-directory-saas-account-provisioning-notifications.md)
- [Seznam výukové programy pro o tom, jak integrovat SaaS aplikace](active-directory-saas-tutorial-list.md)


<!--Image references-->
[1]: ./media/active-directory-saas-customizing-attribute-mappings/ic765497.png
[2]: ./media/active-directory-saas-customizing-attribute-mappings/ic775419.png
[3]: ./media/active-directory-saas-customizing-attribute-mappings/ic775420.png
[4]: ./media/active-directory-saas-customizing-attribute-mappings/ic775421.png
