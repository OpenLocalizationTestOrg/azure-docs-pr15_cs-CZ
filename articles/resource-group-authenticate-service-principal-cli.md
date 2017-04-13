<properties
   pageTitle="Vytvoření služby základní s Azure rozhraní příkazového řádku | Microsoft Azure"
   description="Popisuje, jak používat rozhraní příkazového řádku Azure k vytvoření aplikace služby Active Directory a služby jistinu a udělení přístupu k prostředkům pomocí řízení přístupu na základě rolí. Ukazuje, jak ověřit aplikace pomocí hesla nebo certifikát."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Umožňuje vytvořit hlavní název služby přístupu k prostředkům rozhraní příkazového řádku Azure

> [AZURE.SELECTOR]
- [Prostředí PowerShell](resource-group-authenticate-service-principal.md)
- [Azure rozhraní příkazového řádku](resource-group-authenticate-service-principal-cli.md)
- [Portál](resource-group-create-service-principal-portal.md)


Jakmile aplikace nebo skript, který potřebuje k přístupu k prostředkům, pravděpodobně nechcete spustit tento proces v části vlastní přihlašovací údaje. Máte jiná oprávnění, které chcete použít pro aplikaci a nechcete, aby aplikaci pokračovat, pokud zodpovědnosti změnit pomocí svých přihlašovacích údajů. Místo toho vytvoříte identitu aplikace, která obsahuje přihlašovacími údaji a přiřazování rolí. Při každém spuštění aplikace ověří samotné pomocí těchto přihlašovacích údajů. V tomto tématu se dozvíte, jak používat [Azure rozhraní příkazového řádku pro Mac a Linux Windows](xplat-cli-install.md) nastavení spuštění v části vlastní přihlašovací údaje a identity aplikace.

Pomocí rozhraní příkazového řádku Azure máte dvě možnosti pro ověřování aplikace AD:

 - heslo
 - certifikát

Toto téma ukazuje, jak můžete pomocí obou možnostech v Azure rozhraní příkazového řádku. Pokud chcete, aby byla k přihlášení k Azure z programovací rámec (například Python skutečné a Node.js), může být nejlepší možností ověřování hesla. Než se rozhodnete, jestli používat heslo nebo certifikát, naleznete v části [ukázkové aplikace](#sample-applications) příklady ověřování v různých rámce.

## <a name="active-directory-concepts"></a>Active Directory koncepty

V tomto článku vytvoříte dva objekty - Active Directory (AD) aplikace a služby základní. AD aplikace je globální znázornění aplikace. Obsahuje přihlašovacích údajů (id aplikace a heslo nebo certifikát). Služby základní je místní zastoupení aplikace ve službě Active Directory. Přiřazování rolí v ní. Toto téma se zaměřuje na jednoho klienta aplikace místo, kam se má aplikace spustit v rámci jedinou organizace. Obvykle používáte jednoho klienta aplikace pro řádek obchodní aplikace, které pracují ve vaší organizaci. V aplikaci jednoho klienta máte jedné aplikaci AD a jistinu jedna služba.

Budete možná vás zajímá, - proč je potřeba oba objekty? Tento přístup větší smysl při zvažte více klienta aplikace. Obvykle se používá více klienta aplikací pro aplikace software jako služeb (SaaS), kde běží aplikace v mnoha různých předplatných. V aplikacích více klienta budete mít jedné aplikaci AD a více objektů služby (jedno v každé služby Active Directory, která zajišťuje přístup k aplikaci). Nastavení aplikace více klienta, najdete v článku [vývojáře průvodce se tak mohli ověřovat pomocí rozhraní API Azure správce prostředků](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Požadované oprávnění

Toto téma, dokončete musí mít dostatečná oprávnění v Azure Active Directory a Azure předplatné. Konkrétně musíte mít k vytvoření aplikace ve službě Active Directory a přiřadit roli hlavního uživatele služby. 

Váš účet služby Active Directory, musíte být správcem (jako **Globální správce** nebo **Správce uživatelů**). Pokud váš účet je přiřazen k roli **uživatele** , musíte mít zvýšení oprávnění správce.

Váš účet předplatného, musí mít `Microsoft.Authorization/*/Write` aplikace access, která se poskytuje prostřednictvím [vlastník](./active-directory/role-based-access-built-in-roles.md#owner) nebo [Správce přístup uživatelů](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) role. Pokud váš účet je přiřazené role **přispěvatele** , chybová při pokusu o jistinu služby přiřadit roli. Správce předplatného se musí znovu udělit dostatečná oprávnění k přístupu.

Teď přejděte k části pro ověřování [hesla](#create-service-principal-with-password) nebo [certifikátu](#create-service-principal-with-certificate) .

## <a name="create-service-principal-with-password"></a>Vytvoření služby základní pomocí hesla

V této části proveďte kroky k vytvoření aplikace AD heslem a přiřadit roli Čtenář služby základní.

Pojďme si teď tyto kroky.

1. Přihlaste se ke svému účtu.

        azure login

1. Máte dvě možnosti pro vytváření AD aplikace. Můžete vytvořit AD aplikace a služby základní v jednom kroku nebo byla vytvořená samostatně. Pokud nepotřebujete zadejte domovskou stránku a identifikátor URI aplikace, vytvořte je v jednom kroku. Vytvářejí zvlášť když potřebujete tyto hodnoty pro web app nastavit. Obou možnostech najdete v tomto kroku.

     - Při tvorbě AD aplikace a služby základní v jednom kroku, zadejte název aplikace a hesla, jak ukazuje tento příkaz:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - Vytvořit aplikaci AD zvlášť, zadejte název aplikace, domovské stránky URI, identifikátor URI a hesla, jak ukazuje tento příkaz:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         Příkaz předchozí vrátí hodnotu ID aplikace. Pokud chcete vytvořit hlavní název služby, zadejte tuto hodnotu jako parametr tento příkaz:
     
            azure ad sp create -a <AppId>
     
     Pokud váš účet nemá [potřebná oprávnění](#required-permissions) ve službě Active Directory, se zobrazí chybová zpráva označující "Authentication_Unauthorized" nebo "žádný odběr nebyl nalezen v kontextu".
    
     Obou možnostech bude vrácena nového hlavního uživatele služby. **Id objektu** je potřeba při udělování oprávnění. Identifikátor guid společně s **Názvy hlavní služby** je potřeba při přihlášení. Identifikátor guid se stejnou hodnotu jako id aplikace. V ukázkové aplikace tato hodnota je nazýván **ID klienta**. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Udělení oprávnění služby základní vašeho předplatného. V tomto příkladu přidejte jistinu služby k roli **Čtenář** , která poskytuje oprávnění ke čtení všech zdrojů v předplatné. Ostatních rolí v tématu [RBAC: předdefinované role](./active-directory/role-based-access-built-in-roles.md). Pro parametr **ServicePrincipalName** poskytují **objektu** , který jste použili při vytváření aplikace. 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Pokud váš účet nemá dostatečná oprávnění k přiřadit roli, se zobrazí chybová zpráva. Zpráva informuje o účet **nemá oprávnění k provedení akce "Microsoft.Authorization/roleAssignments/write" v oboru "/ předplatná / {guid}"**. 

Je to! AD aplikace a služby jistinu se nastavují. Následující části se dozvíte, jak k přihlášení pomocí přihlašovací údaje pomocí rozhraní příkazového řádku Azure. Pokud chcete použít přihlašovacích údajů v aplikaci kód, není nutné budeme pokračovat v tomto tématu. Můžete přejít na [ukázkové aplikace](#sample-applications) příklady přihlašování pomocí aplikace id a heslo. 

### <a name="provide-credentials-through-azure-cli"></a>Zadejte přihlašovací údaje pomocí rozhraní příkazového řádku Azure

Teď musíte se přihlásit jako aplikaci k provádění operací.

1. Pokaždé, když se přihlásíte jako hlavní název služby, budete muset zadat id klienta v adresáři aplikace AD. Klienta, kterého je instancí služby Active Directory. Načtení id klienta pro aktuálně ověřeného předplatné, můžete:

        azure account show

     Vracející:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Pokud potřebujete k získání id klienta jiné předplatné, použijte tento příkaz:

        azure account show -s {subscription-id}

2. Pokud potřebujete získat kód klienta používat pro přihlášení, použijte:

        azure ad sp show -c exampleapp --json

     Pro účely přihlašováním hodnotu guid uvedené v hlavní názvy služeb.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Přihlaste se jako hlavní služby.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    Zobrazí se výzva k zadání hesla. Zadejte heslo, které jste zadali při vytváření AD aplikace.

        info:    Executing command login
        Password: ********

Nyní máte oprávnění jako hlavní služby pro službu hlavního uživatele, který jste vytvořili.

## <a name="create-service-principal-with-certificate"></a>Vytvoření služby základní pomocí certifikátu

V této části proveďte kroky:

- Vytvoření certifikátu podepsaného svým držitelem
- Vytvoření aplikace AD pomocí certifikátu a jistinu služby
- přiřazení role Čtenář hlavního uživatele služby

Tyto kroky, musíte mít [OpenSSL](http://www.openssl.org/) nainstalovaný.

1. Vytvoření certifikátu podepsaného svým držitelem.

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Kombinování veřejné a privátní klíče.

        cat privkey.pem cert.pem > examplecert.pem

3. Otevřete soubor **examplecert.pem** a vyhledejte dlouhé posloupnost znaků od **---počáteční certifikát---** **---UKONČIT certifikát---**. Zkopírujte data certifikátu. Při vytváření služby základní předáte jako parametr tato data.

1. Přihlaste se ke svému účtu.

        azure login

1. Máte dvě možnosti pro vytváření AD aplikace. Můžete vytvořit AD aplikace a služby základní v jednom kroku nebo byla vytvořená samostatně. Pokud nepotřebujete zadejte domovskou stránku a identifikátor URI aplikace, vytvořte je v jednom kroku. Vytvářejí zvlášť když potřebujete tyto hodnoty pro web app nastavit. Obou možnostech najdete v tomto kroku.

     - Vytvoříte AD aplikace a služby základní v jednom kroku zadejte název aplikace a data certifikátu, jak ukazuje tento příkaz:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - Vytvořit aplikaci AD samostatně, zadejte název aplikace, domovské stránky URI, identifikátor URI a data certifikátu, jak ukazuje tento příkaz:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         Příkaz předchozí vrátí hodnotu ID aplikace. Pokud chcete vytvořit hlavní název služby, zadejte tuto hodnotu jako parametr tento příkaz:
     
            azure ad sp create -a <AppId>
  
     Pokud váš účet nemá [potřebná oprávnění](#required-permissions) ve službě Active Directory, se zobrazí chybová zpráva označující "Authentication_Unauthorized" nebo "žádný odběr nebyl nalezen v kontextu".
    
     Obou možnostech bude vrácena nového hlavního uživatele služby. Id objektu je potřeba při udělování oprávnění. Identifikátor guid společně s **Názvy hlavní služby** je potřeba při přihlášení. Identifikátor guid se stejnou hodnotu jako id aplikace. V ukázkové aplikace tato hodnota je nazýván **ID klienta**. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Udělení oprávnění služby základní vašeho předplatného. V tomto příkladu přidejte jistinu služby k roli **Čtenář** , která poskytuje oprávnění ke čtení všech zdrojů v předplatné. Ostatních rolí v tématu [RBAC: předdefinované role](./active-directory/role-based-access-built-in-roles.md). Pro parametr **ServicePrincipalName** poskytují **objektu** , který jste použili při vytváření aplikace. 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Pokud váš účet nemá dostatečná oprávnění k přiřadit roli, se zobrazí chybová zpráva. Zpráva informuje o účet **nemá oprávnění k provedení akce "Microsoft.Authorization/roleAssignments/write" v oboru "/ předplatná / {guid}"**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Zadejte certifikátu prostřednictvím skriptu automatické rozhraní příkazového řádku Azure

Teď musíte se přihlásit jako aplikaci k provádění operací.

1. Pokaždé, když se přihlásíte jako hlavní název služby, budete muset zadat id klienta v adresáři aplikace AD. Klienta, kterého je instancí služby Active Directory. Načtení id klienta pro aktuálně ověřeného předplatné, můžete:

        azure account show

     Vracející:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Pokud potřebujete k získání id klienta jiné předplatné, použijte tento příkaz:

        azure account show -s {subscription-id}

1. Načtení miniaturu certifikátu a odstraňte nadbytečné znaky, můžete:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Které vrací hodnotu Miniatura podobně jako:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Pokud potřebujete získat kód klienta používat pro přihlášení, použijte:

        azure ad sp show -c exampleapp

     Pro účely přihlašováním hodnotu guid uvedené v hlavní názvy služeb.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Přihlaste se jako hlavní služby.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

Nyní máte oprávnění jako služby základní pro aplikaci služby Active Directory, která jste vytvořili.

## <a name="sample-applications"></a>Ukázkové aplikace

Následující ukázkové aplikace ukazují, jak se přihlásit jako hlavní služby.

**.NET**

- [Nasazení SSH povolené OM šablony s .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Správa Azure materiály a zdroje skupiny s .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Začínáme s zdrojů – nasazení pomocí šablony Azure správce prostředků – v Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Začínáme s zdrojů – Správa pole Skupina zdroje – v Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Nasazení SSH povolené OM šablony v Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Správa Azure zdroje a skupin zdroje s Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Nasazení SSH povolené OM šablony v Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Správa Azure materiály a zdroje skupiny s Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Skutečné**

- [Nasazení SSH povolené OM šablony v skutečné](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Správa Azure zdroje a skupin zdroje s skutečné](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Další kroky
  
- Podrobný postup na integraci aplikace do Azure pro správu zdrojů, najdete v článku [vývojáře průvodce se tak mohli ověřovat pomocí rozhraní API Azure správce prostředků](resource-manager-api-authentication.md).
- Chcete-li získat další informace o používání certifikáty a rozhraní příkazového řádku Azure, najdete v článku [ověřování na základě certifikátu s objekty služby Azure z příkazového řádku Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
