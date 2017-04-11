<properties
   pageTitle="Správa služby Azure Analysis Services | Microsoft Azure"
   description="Naučte se spravovat serveru služby Analysis Services v Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="manage-analysis-services"></a>Správa služby Analysis Services

Po vytvoření serveru služby Analysis Services v Azure, může být některé správu a úkoly, které potřebujete provést hned nebo někdy dolů na cestách. Spusťte například zpracování aktualizovat data, určit, kdo bude můžete získat přístup k modelech na serveru nebo sledovat stav svého serveru. Některé úlohy správy lze provádět pouze v Azure portal ostatním v SQL Server Management Studio (SSMS) a některé úkoly můžete provést.

## <a name="azure-portal"></a>Azure portálu
[Azure portál](http://portal.azure.com/) je místo, kam můžete vytvořit a odstranit servery, sledovat prostředků serveru, změna velikosti a Správa uživatelů, kteří mají přístup k serverům.  Pokud máte potíže, můžete taky odeslat žádost o podporu.

![Zjištění názvu serveru v Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Připojení k serveru v Azure je stejně jako připojení k instanci serveru ve vaší organizaci. Z SSMS můžete provádějí řadu úloh například data procesu nebo vytvořit skript zpracování, spravovat role a pomocí Powershellu.

![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

 Zvětšit rozdílů reprodukujte ověřování, který používáte pro připojení k serveru. Připojení k serveru služby Analysis Services Azure, musíte vybrat **Active Directory, ověřování hesla**.

### <a name="to-connect-with-ssms"></a>Spojení s SSMS
1. Před připojením, budete muset zjištění názvu serveru. **Azure** portálu > serveru > **Přehled** > **název serveru**, zkopírujte názvu serveru.

    ![Zjištění názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

2. V SSMS > **Průzkumník objektů**, klikněte na **Připojit** > **Služby Analysis Services**.

3. V dialogovém okně **připojit k serveru** vložte do pole název serveru a pak v **ověřování**, zvolte jednu z následujících akcí:

    **Integrované ověřování služby active Directory** pomocí jednotné přihlašování ve službě Active Directory Azure Active Directory federation.

    **Active Directory, ověřování hesla** účtu organizace. Třeba při připojení z jiných domény připojen k počítači.

    Poznámka: Pokud se nezobrazuje ověřování služby Active Directory, budete muset [Povolit ověřování služby Azure Active Directory](#enable-azure-active-directory-authentication) v SSMS.

    ![Připojení v SSMS](./media/analysis-services-manage/aas-manage-connect-ssms.png)

Správa serveru v Azure pomocí SSMS je podobné jako Správa místního serveru, jsme neprojdou přejdete do podrobnosti tady. V nápovědě budete potřebovat najdete v části [Správa instanci služby Analysis](https://msdn.microsoft.com/library/hh230806.aspx) na webu MSDN.

## <a name="server-administrators"></a>Správce serveru
Můžete **Správci Analysis Services** v zásuvné ovládací prvek pro váš server Azure portál nebo SSMS ke správě správci serveru. Správci Analysis Services jsou správci serveru databáze s oprávněními pro běžné úlohy správy databáze třeba přidávání a odebírání databází a Správa uživatelů. Ve výchozím nastavení uživatele, který vytvoří server Azure portálu se automaticky přidá ve formě správce Analysis Services.

Měli byste taky vědět:

-   Windows Live ID není podporované identity typ pro služby Azure Analysis Services.  
-   Správci Analysis Services musí být platná uživatelům Azure Active Directory.
-   Při vytváření server služby Azure Analysis Services pomocí šablony Azure zdroje správce, Správce služby Analysis přijímá pole JSON uživatelů, které má být přidán jako správce.

Správce služby Analysis se může lišit od správci Azure zdroje, které můžete spravovat zdroje Azure předplatná. To udržuje kompatibility s existujícími XMLA a TSML Správa chování v služby Analysis Services a umožňují oddělit od Azure zdroje správu a analýzu služby Správa databáze.

Pokud chcete zobrazit všechny role a přístup k typy pro zdroj Azure Analysis Services, pomocí řízení přístupu (IAM) na zásuvné ovládacího prvku.

## <a name="database-users"></a>Uživatelé databáze
Uživatelům Azure databáze modelu služby Analysis Services musí být v Azure Active Directory. Organizace e-mailovou adresu nebo UPN musí být uživatelská jména určeným pro modelu databáze. Tím se liší od místního modelování databází, které podporu uživatelů tak, že uživatelská jména domény Windows.

Uživatele můžete přidat pomocí [přiřazování rolí v Azure Active Directory](../active-directory/role-based-access-control-configure.md) nebo pomocí [Jazyka skriptování tabulkového modelu](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) v SQL Server Management Studio.

**Ukázka skriptu TMSL**

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Users"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users to query the model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@contoso.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="enable-azure-active-directory-authentication"></a>Povolit ověřování služby Azure Active Directory
Chcete-li povolit funkci ověřování Azure Active Directory pro SSMS v registru, vytvořit textový soubor s názvem EnableAAD.reg, a pak zkopírujte a vložte následující:


```
Windows Registry Editor Version 5.00
[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\Microsoft Analysis Services\Settings]
"AS AAD Enabled"="True"
```

Uložte a spusťte soubor.



## <a name="next-steps"></a>Další kroky
Tabulkový model nasazený už nové server, je nyní vhodné čas. Další informace najdete v tématu [nasazení služby Azure Analysis Services](analysis-services-deploy.md).

Pokud jste nasazení modelu na serveru, budete chtít připojit pomocí klienta nebo prohlížeče. Další informace najdete v tématu [získání dat z Azure Analysis Services serveru](analysis-services-connect.md).
