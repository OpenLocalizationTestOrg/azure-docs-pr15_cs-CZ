<properties
    pageTitle="Vytvoření prošetřovat interaktivním ověřování .NET HDInsight žádosti | Microsoft Azure"
    description="Naučte se vytvářet interaktivním ověřování aplikace .NET HDInsight."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Vytvoření interaktivním ověřování aplikace .NET HDInsight

Můžete provést .NET Azure HDInsight aplikace v části vlastní identitu aplikace (interaktivním) nebo identitou přihlášený uživatel aplikace (interaktivní). Příklad interaktivní aplikace naleznete v tématu [odeslání podregistru/Prasátko/Sqoop úlohy pomocí HDInsight .NET SDK](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Tento článek popisuje, jak vytvořit interaktivním ověřování aplikace .NET pro připojení k Azure HDInsight a odeslání podregistru úlohy.

Z aplikace .NET budete potřebovat:

- ID klienta Azure předplatného
- ID klienta aplikace Azure adresáře
- Azure Directory tajné klávesu application.  

Hlavní proces zahrnuje následující kroky:

2. Vytvoření aplikace Azure adresář.
2. Přiřazování rolí AD aplikace.
3. Můžete vyvíjet aplikace klienta.


##<a name="prerequisites"></a>Zjistit předpoklady pro

- HDInsight obrázku. Můžete vytvořit jednu postupujte podle pokynů najdete v [příručce Začínáme kurz](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 




## <a name="create-azure-directory-application"></a>Vytvoření aplikace Azure Directory 
Při vytváření aplikace služby Active Directory skutečně vytvoří aplikace a služby základní. Můžete spustit aplikaci identitou aplikace.

V současné době musí používat portál Azure klasické k vytvoření nové aplikace služby Active Directory. Tato možnost se přidají do portálu Azure v pozdější verzi. Můžete taky provedení těchto kroků pomocí prostředí PowerShell Azure nebo Azure rozhraní příkazového řádku. Další informace o pomocí prostředí PowerShell nebo rozhraní příkazového řádku pomocí služby základní najdete v tématu [Ověřit služby základní pomocí Správce prostředků Azure](../resource-group-authenticate-service-principal.md).

**Vytvoření aplikace Azure adresáře**

1.  Přihlaste se k [Azure klasické portálu]( https://manage.windowsazure.com/).
2.  V levém podokně vyberte **Služby Active Directory** .

    ![Azure klasické portálu služby active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Vyberte adresář, který chcete použít při vytváření nové aplikace. Musí být existující.
4.  Klikněte na **aplikace** od začátku do seznamu existujících aplikací.
5.  Klikněte na **Přidat** v dolní části přidáte novou aplikaci.
6.  Zadejte **název**, vyberte **webovou aplikaci a rozhraní API webových**a klikněte na tlačítko **Další**.

    ![novou aplikaci služby azure active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Zadejte **přihlašovací adresu URL** a **Identifikátor URI aplikace**. **Na PŘIHLAŠOVACÍ adresa URL**zadejte identifikátor URI na web, který popisuje aplikace. Není ověřit existenci na webu. URI ID aplikace zadejte identifikátor URI, který identifikuje aplikace. A potom klikněte na **Dokončit**.
Na krátkou chvíli vytvořit aplikaci trvá.  Po vytvoření aplikace portál zobrazuje snadné Glace stránku nové aplikace. Nechejte na portálu. 

    ![nové vlastnosti aplikace služby azure active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Získat aplikaci klienta ID a tajná klíč**

1.  Na stránce aplikace nově vytvořený AD **Konfigurovat** v nabídce klikněte na horní.
2.  Vytvoření kopie **ID klienta**. Je nutné ho v aplikaci .NET.
3.  **Klávesy**klikněte na rozevírací seznam **vyberte dobu trvání** a vyberte **rok 1** nebo **2 let**. Hodnoty klíče nezobrazí, dokud uložit konfiguraci.
4.  Klikněte na **Uložit** v dolní části stránky. Po zobrazení klávesu skrytou kopii klávesu. Je nutné ho v aplikaci .NET.

##<a name="assign-ad-application-to-role"></a>AD aplikace k roli přiřazení

Musíte přiřadit aplikace k [roli](../active-directory/role-based-access-built-in-roles.md) chcete udělit oprávnění k provádění akcí. Nastavit obor na úrovni předplatného, skupina zdroje nebo zdroje. Oprávnění se dědí nižší úrovně oboru (například přidání aplikace k roli Čtenář pro skupinu prostředků znamená, že můžete přečíst skupina zdroje a zdroje, který ho obsahuje). V tomto kurzu nastavíte obor na úrovni skupiny zdrojů.  Protože portálu Azure klasické, které nejsou podporovány skupiny zdrojů, tato část obsahuje provádět z portálu Microsoft Azure. 

**Chcete-li přidat role vlastníka AD aplikace**

1.  Přihlaste se k [portálu Azure](https://portal.azure.com).
2.  V levém podokně klikněte na **Pole Skupina zdroje** .
3.  Klikněte na skupina zdroje, který obsahuje HDInsight obrázku, které se spustí dotaz podregistru dál v tomto kurzu. Pokud jsou moc skupiny zdrojů, můžete použít filtr.
4.  Klikněte na **přístup** z zásuvné obrázku.

    ![Ikona cloudu a thunderbolt = rychlý úvod](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Klikněte na **Přidat** ze zásuvné **uživatelů** .
6.  Postupujte podle pokynů přidejte role **vlastníka** do aplikace AD, že kterou jste vytvořili v posledním postupu. Po dokončení je úspěšně, zobrazí se aplikace uvedené v zásuvné uživatelé s rolí vlastník.


##<a name="develop-hdinsight-client-application"></a>Vývoj HDInsight klientské aplikace

Vytvoření aplikace konzoly C# .net postupujte podle pokynů v [Odeslat Hadoop úlohy v HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Nahradíte metodu GetTokenCloudCredentials takto:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

K načtení ID klienta prostřednictvím Powershellu:

    Get-AzureRmSubscription

Nebo Azure rozhraní příkazového řádku:

    azure account show --json

      
## <a name="see-also"></a>Viz taky

- [Odeslat Hadoop úlohy v HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Vytvoření aplikace služby Active Directory a služby jistinu portálu](../resource-group-create-service-principal-portal.md)
- [Ověření služby jistinu pomocí Správce prostředků Azure](../resource-group-authenticate-service-principal.md)
- [Řízení přístupu na základě rolí Azure](../active-directory/role-based-access-control-configure.md)
