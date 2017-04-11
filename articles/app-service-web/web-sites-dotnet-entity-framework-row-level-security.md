<properties
    pageTitle="Kurz: Web app s více klienta databáze pomocí Entity Framework a zabezpečení na úrovni řádku"
    description="Zjistěte, jak se dají webovou aplikaci ASP.NET MVC 5 s více klienta SQL databáze backent, pomocí Entity Framework a zabezpečení na úrovni řádku."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Kurz: Web app s více klienta databáze pomocí Entity Framework a zabezpečení na úrovni řádku

Tento kurz ukazuje, jak vytvořit víc klienta web app s "[sdílené databázi, sdílené schéma](https://msdn.microsoft.com/library/aa479086.aspx)" nájmu modelu, pomocí Entity Framework a [Zabezpečení na úrovni řádku](https://msdn.microsoft.com/library/dn765131.aspx). V tomto modelu jedné databáze obsahuje data pro mnoho klientů a každý řádek v každé tabulce je přidružená k "klienta ID." Řádek úroveň zabezpečení (RLS), novou funkci pro databázi SQL Azure, se používá zabráníte klienti přístup k datům dalších uživatelů. Při této akci musí jenom na jedné malé změnu aplikace. Centralizací logiku přístup klientovi v rámci samotnou databázi RLS zjednodušuje kód aplikace a snižuje riziko únik náhodných dat mezi klienty.

Začněme s jednoduchou aplikaci Contact Manager z [Vytvoření aplikace pro ASP.NET MVP s auth a SQL databáze a nasazení služby Azure aplikace](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Vpravo teď aplikace všem uživatelům (klienti) Pokud chcete zobrazit všechny kontakty:

![Před povolením RLS aplikace Contact Manager](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Změnami malé pár přidáme podpora víceklientská, tak, aby uživatelé můžou zobrazit jenom kontakty, které patří k nim.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Krok 1: Přidání zachytávací modul třídy v aplikaci nastavit SESSION_CONTEXT

Je jednou změnou aplikace, které je třeba mít. Vzhledem k tomu, že všichni uživatelé aplikace připojení k databázi pomocí stejný připojovací řetězec (tedy stejný přihlášení k serveru SQL), současné době nejde žádným způsobem pro zásad RLS vědět, které by měl filtr pro uživatele. Tento přístup je velmi běžné ve webových aplikacích, protože umožňuje efektivně fond připojení, znamená to, že potřebujeme jiným způsobem, jak identifikovat aktuálního uživatele aplikace v databázi. Řešením je aplikaci nastavit pár klíč hodnota pro aktuální ID uživatele v [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) hned po otevření připojení, než provádí všechny dotazy. SESSION_CONTEXT je klíč hodnota úložiště s rozsahem relace a zásadách RLS použijete ID uložené v něm k identifikaci aktuálního uživatele.

Přidáme [zachytávací modul](https://msdn.microsoft.com/data/dn469464.aspx) (zejména [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), novou funkci Framework Entity (EF) 6, automaticky nastavit aktuální ID uživatele v SESSION_CONTEXT spuštěním příkazu T SQL vždy, když EF otevře připojení.

1.  Otevřete projekt ContactManager ve Visual Studiu.
2.  Klikněte pravým tlačítkem myši na složku modely v okně Průzkumník a zvolte Přidat > předmětu.
3.  Pojmenování nové třídy "SessionContextInterceptor.cs" a klikněte na Přidat.
4.  Obsah SessionContextInterceptor.cs nahraďte následující kód.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

To je změnit pouze aplikace povinné. Pojďte dále a vytvořit a publikovat aplikaci.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Krok 2: Přidání sloupce ID schématu databáze

Dále je třeba přidat sloupec ID v tabulce Kontakty každý řádek přidružit uživatele (klient). Společnost Microsoft, můžete změnit schématu přímo v databázi, tak, aby nemáme zahrnout toto pole náš EF datového modelu.

Připojení k databázi přímo, SQL Server Management Studio nebo Visual Studio a potom provést následující T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Tím se přidá sloupec ID uživatele k tabulce Kontakty. Datový typ nvarchar(128) používáme podle UserIds uložené v tabulce AspNetUsers a vytvoříme výchozí omezení, která automaticky nastaví ID uživatele pro nově vložená řádky jako ID obsažené v SESSION_CONTEXT.

Teď tabulka vypadat takto:

![Tabulky SSMS kontakty](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Při vytvoření nové kontakty, budou se automaticky přiřadit správné ID uživatele. Pro účely ukázky však Pojďme přiřadit několik těchto současné kontakty existujícího uživatele.

Pokud jste si vytvořili s několika uživateli v aplikaci už (například použijete místní, Google nebo Facebook účty), zobrazí se jim v tabulce AspNetUsers. Následující snímek je pouze jeden uživatel zatím.

![SSMS AspNetUsers tabulky](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Zkopírujte Id pro user1@contoso.com, a vložit ho do příkazu T-SQL. Spusťte tento příkaz tři kontaktů přidružit ID uživatele.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Krok 3: Vytvoření zásad zabezpečení na úrovni řádek v databázi

Posledním kroku je vytvořit zásady zabezpečení, která používá ID uživatele v SESSION_CONTEXT automaticky filtrovat výsledky dotazů.

Během pořád připojení k databázi, provést následující T-SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Tento kód má tři položky. Nejprve vytvoří nové schéma osvědčený centralizace a omezení přístupu k objektům RLS. Potom vytvoří predikátu funkci, která vrátí '1' při ID řádku odpovídá ID uživatele v SESSION_CONTEXT. Nakonec vytvoří zásady zabezpečení, která přidá tuto funkci jako filtr i blok predikát v tabulce Kontakty. Predikát filtr způsobí dotazů pro vrácení pouze řádky, které patří do aktuálního uživatele a predikát blok funguje jako chránit nechcete, aby aplikace z někdy omylem vložení řádku pro nesprávného uživatele.

Nyní spustit aplikaci a přihlaste se jako user1@contoso.com. Tento uživatel teď uvidí jenom kontakty jsme přiřazená ID uživatele dříve:

![Před povolením RLS aplikace Contact Manager](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Ověřit to dál, zkuste registrace nového uživatele. Protože žádná byly přiřazeny je uvidí žádné kontakty. Pokud se vytvořit nový kontakt, se jim automaticky přiřadí a pouze budou moct vidět.

## <a name="next-steps"></a>Další kroky

Je to! Jednoduchý webové aplikace Contact Manager převeden na více klienta, kterého jednu místo, kam má každý uživatel svůj vlastní seznam kontaktů. Pomocí řádku úrovně zabezpečení jsme jste vyhnout složitost vynucení logiky přístupu klienta z našich kódu aplikace. Tento průhlednost umožňuje aplikace zaměření na problém skutečné obchodní po ruce, a taky snižuje riziko omylem nevrací dat, jako je codebase aplikace roste.

Tento kurz obsahuje pouze poškrábán povrchu toho, co je možné s RLS. Například je možné více sofistikované nebo použití logických operátorů podrobného přístup a je možné ukládat větší kontrolu nad aktuálním ID uživatele v SESSION_CONTEXT. Je také možné integrovat [RLS s knihovnami klientské nástroje pružná databáze](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) pro podporu více klienta shards ve vrstvě škálování dat.

Za těchto možností taky pracujeme aby RLS ještě lépe. Pokud máte nějaké dotazy, nápadů nebo věci, které chcete zobrazit, dejte nám prosím vědět v komentářích. Děkujeme za váš názor!
