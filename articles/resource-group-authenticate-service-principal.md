<properties
   pageTitle="Vytvoření Azure služby základní pomocí prostředí PowerShell | Microsoft Azure"
   description="Popisuje, jak pomocí Powershellu Azure vytvořit aplikaci služby Active Directory a služby jistinu a udělení přístupu k prostředkům pomocí řízení přístupu na základě rolí. Ukazuje, jak ověřit aplikace pomocí hesla nebo certifikát."
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
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Vytvoření služby jistinu při přístupu k prostředkům pomocí prostředí PowerShell Azure

> [AZURE.SELECTOR]
- [Prostředí PowerShell](resource-group-authenticate-service-principal.md)
- [Azure rozhraní příkazového řádku](resource-group-authenticate-service-principal-cli.md)
- [Portál](resource-group-create-service-principal-portal.md)

Jakmile aplikace nebo skript, který potřebuje k přístupu k prostředkům, pravděpodobně nechcete spustit tento proces v části vlastní přihlašovací údaje. Máte jiná oprávnění, které chcete použít pro aplikaci a nechcete, aby aplikaci pokračovat, pokud zodpovědnosti změnit pomocí svých přihlašovacích údajů. Místo toho vytvoříte identitu aplikace, která obsahuje přihlašovacími údaji a přiřazování rolí. Při každém spuštění aplikace ověří samotné pomocí těchto přihlašovacích údajů. V tomto tématu se dozvíte, jak pomocí [Powershellu Azure](powershell-install-configure.md) nastavit všechno, co potřebujete k spuštění v části vlastní přihlašovací údaje a identity aplikace.

Pomocí Powershellu máte dvě možnosti pro ověřování aplikace AD:

 - heslo
 - certifikát

Toto téma ukazuje, jak používat obou možnostech v prostředí PowerShell. Pokud chcete, aby byla k přihlášení k Azure z programovací rámec (například Python skutečné a Node.js), může být nejlepší možností ověřování hesla. Než se rozhodnete, jestli používat heslo nebo certifikát, naleznete v části [ukázkové aplikace](#sample-applications) příklady ověřování v různých rámce.

## <a name="active-directory-concepts"></a>Active Directory koncepty

V tomto článku vytvoříte dva objekty - Active Directory (AD) aplikace a služby základní. AD aplikace je globální znázornění aplikace. Obsahuje přihlašovacích údajů (id aplikace a heslo nebo certifikát). Služby základní je místní zastoupení aplikace ve službě Active Directory. Přiřazování rolí v ní. Toto téma se zaměřuje na jednoho klienta aplikace místo, kam se má aplikace spustit v rámci jedinou organizace. Obvykle používáte jednoho klienta aplikace pro řádek obchodní aplikace, které pracují ve vaší organizaci. V aplikaci jednoho klienta máte jedné aplikaci AD a jistinu jedna služba.

Budete možná vás zajímá, - proč je potřeba oba objekty? Tento přístup větší smysl při zvažte více klienta aplikace. Obvykle se používá více klienta aplikací pro aplikace software jako služeb (SaaS), kde běží aplikace v mnoha různých předplatných. V aplikacích více klienta budete mít jedné aplikaci AD a více objektů služby (jedno v každé služby Active Directory, která zajišťuje přístup k aplikaci). Nastavení aplikace více klienta, najdete v článku [vývojáře průvodce se tak mohli ověřovat pomocí rozhraní API Azure správce prostředků](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Požadované oprávnění

Toto téma, dokončete musí mít dostatečná oprávnění v Azure Active Directory a Azure předplatné. Konkrétně musíte mít k vytvoření aplikace ve službě Active Directory a přiřadit roli hlavního uživatele služby. 

Váš účet služby Active Directory, musíte být správcem (jako **Globální správce** nebo **Správce uživatelů**). Pokud váš účet je přiřazen k roli **uživatele** , musíte mít zvýšení oprávnění správce.

Váš účet předplatného, musí mít `Microsoft.Authorization/*/Write` aplikace access, která se poskytuje prostřednictvím [vlastník](./active-directory/role-based-access-built-in-roles.md#owner) nebo [Správce přístup uživatelů](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) role. Pokud váš účet je přiřazené role **přispěvatele** , chybová při pokusu o jistinu služby přiřadit roli. Správce předplatného se musí znovu udělit dostatečná oprávnění k přístupu.

Teď přejděte k části pro ověřování [hesla](#create-service-principal-with-password) nebo [certifikátu](#create-service-principal-with-certificate) .

## <a name="create-service-principal-with-password"></a>Vytvoření služby základní pomocí hesla

V této části proveďte kroky:

- Vytvoření aplikace AD pomocí hesla
- Vytvoření hlavního uživatele služby
- přiřazení role Čtenář hlavního uživatele služby

Abyste mohli rychle provést tyto kroky, najdete v článku tyto tři rutiny. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Pojďme si tyto kroky podrobněji aby zkontrolovala, jestli že porozumět procesu.

1. Přihlaste se ke svému účtu.

        Add-AzureRmAccount

1. Vytvořte novou aplikaci služby Active Directory zadáním zobrazované jméno, identifikátor URI, který popisuje aplikace, URI, které určují aplikace a heslo pro vaši identitu aplikace.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     Pro jednoho klienta aplikace nebyly ověřeny URI.
     
     Pokud váš účet nemá [potřebná oprávnění](#required-permissions) ve službě Active Directory, se zobrazí chybová zpráva označující "Authentication_Unauthorized" nebo "žádný odběr nebyl nalezen v kontextu".

1. Prozkoumejte nové aplikace objektu. 

        $app
        
     Všimněte si, zejména **ApplicationId** vlastnost, která je potřebná k vytváření služby objekty přiřazování rolí a získáním přístupový token.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Vytvořte hlavní název služby pro aplikaci předáním id aplikace služby Active Directory.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Udělení oprávnění služby základní vašeho předplatného. V tomto příkladu přidejte jistinu služby k roli **Čtenář** , která poskytuje oprávnění ke čtení všech zdrojů v předplatné. Ostatních rolí v tématu [RBAC: předdefinované role](./active-directory/role-based-access-built-in-roles.md). Pro parametr **ServicePrincipalName** poskytují **ApplicationId** , který jste použili při vytváření aplikace. 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Pokud váš účet nemá dostatečná oprávnění k přiřadit roli, se zobrazí chybová zpráva. Zpráva informuje o účet **nemá oprávnění k provedení akce "Microsoft.Authorization/roleAssignments/write" v oboru "/ předplatná / {guid}"**. 

Je to! AD aplikace a služby jistinu se nastavují. Následující části se dozvíte, jak k přihlášení pomocí přihlašovacích údajů prostřednictvím Powershellu. Pokud chcete použít přihlašovacích údajů v aplikaci kód, můžete přejít na [ukázkové aplikace](#sample-applications). 

### <a name="provide-credentials-through-powershell"></a>Zadejte přihlašovací údaje pomocí prostředí PowerShell

Teď musíte se přihlásit jako aplikaci k provádění operací.

1. Vytvoření **PSCredential** objekt, který obsahuje vaše přihlašovací údaje pomocí příkazu **Get-pověření** . Potřebujete **ApplicationId** před spuštěním tohoto příkazu proto se ujistěte, že máte to k dispozici pro vložení.

        $creds = Get-Credential

2. Zobrazí se výzva k zadání přihlašovacích údajů. Uživatelské jméno když použijete **ApplicationId** , který jste použili při vytváření aplikace. Heslo vyberte tu, kterou jste zadali při vytváření účtu.

     ![Zadejte přihlašovací údaje](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Pokaždé, když se přihlásíte jako hlavní název služby, budete muset zadat id klienta v adresáři aplikace AD. Klienta, kterého je instancí služby Active Directory. Pokud máte jenom jedno předplatné, můžete:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Pokud máte víc předplatných, zadejte předplatné, kde jsou uložená vaše služby Active Directory. Další informace najdete v tématu [jak Azure předplatná souvisí s Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Přihlaste se jako hlavní služby tím, že zadáte, že je tento účet služby jistinu a zadáním objekt přihlašovací údaje. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Nyní máte oprávnění jako služby základní pro aplikaci služby Active Directory, která jste vytvořili.

### <a name="save-access-token-to-simplify-log-in"></a>Uložení přístupový token zjednodušit přihlášení

Chcete-li předejít poskytování služby základní pověření pokaždé, když je nutné přihlásit, můžete uložit přístupový token.

1. Pokud chcete použít aktuální přístupový token na pozdější relaci, uložte profil.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Otevřít profil a zkontrolujte jeho obsah. Všimněte si, že obsahuje přístupový token. 
        
2. Místo ručně přihlásit, jednoduše načtěte profil.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Přístupový token vyprší jejich platnost, tak pomocí uloženého profilu jde použít pouze pro, dokud tokenu je platný.
        
## <a name="create-service-principal-with-certificate"></a>Vytvoření služby základní pomocí certifikátu

V této části proveďte kroky:

- Vytvoření certifikátu podepsaného svým držitelem
- Vytvoření aplikace AD pomocí certifikátu
- Vytvoření hlavního uživatele služby
- přiřazení role Čtenář hlavního uživatele služby

Umožňuje rychle zadávat tyto kroky s Azure PowerShell 2.0 na Windows 10 nebo Windows Server 2016 Technical Preview, najdete v článku tyto rutiny. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Pojďme si tyto kroky podrobněji aby zkontrolovala, jestli že porozumět procesu. Tento článek také ukazuje, jak provádět úkoly při použití předchozích verzí Azure PowerShell nebo operační systémy.

### <a name="create-the-self-signed-certificate"></a>Vytvoření certifikátu podepsaného svým držitelem

Verze Powershellu součástí Windows 10 a Windows Server 2016 Technical Preview má aktualizované rutinu **New-SelfSignedCertificate** pro generování certifikátu podepsaného svým držitelem. Dřívější operační systémy mají rutinu New-SelfSignedCertificate ale nenabízí potřebné pro toto téma Parametry. Místo toho budete muset importovat modulu generovat certifikát. Toto téma ukazuje oba přístupy pro generování certifikát založený na operačním systému, jestli že máte. 

- Pokud máte **Windows 10 nebo Windows Server 2016 Technical Preview**, spusťte tento příkaz k vytvoření certifikátu podepsaného svým držitelem: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Pokud **nemáte Windows 10 nebo Windows Server 2016 Technical Preview**, budete muset stahování [Automatické přihlášení generátor certifikát](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) Microsoft Script Center. Rozbalit a importujte rutinu, které potřebujete.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Pak vytvořte certifikát.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

Máte certifikát a můžete pokračovat ve vytváření AD aplikace.

### <a name="create-the-active-directory-app-and-service-principal"></a>Vytvoření aplikace služby Active Directory a služby základní

1. Načtení hodnoty klíče z certifikátu.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Přihlaste se k účtu Azure.

        Add-AzureRmAccount

3. Vytvořte novou aplikaci služby Active Directory zadáním zobrazované jméno, identifikátor URI, který popisuje aplikace, URI, které určují aplikace a heslo pro vaši identitu aplikace.

     Pokud máte Azure PowerShell 2.0 (srpen 2016 nebo novější), použijte následující rutinu:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Pokud máte Azure PowerShell 1.0, použijte následující rutinu:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Pro jednoho klienta aplikace nebyly ověřeny URI.
    
    Pokud váš účet nemá [potřebná oprávnění](#required-permissions) ve službě Active Directory, se zobrazí chybová zpráva označující "Authentication_Unauthorized" nebo "žádný odběr nebyl nalezen v kontextu".
        
    Prozkoumejte nové aplikace objektu. 

        $app

    Všimněte si **ApplicationId** vlastnost, která je potřebná pro vytváření služby objekty přiřazování rolí a získávání tokeny přístupu.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Vytvořte hlavní název služby pro aplikaci předáním id aplikace služby Active Directory.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Udělení oprávnění služby základní vašeho předplatného. V tomto příkladu přidejte jistinu služby k roli **Čtenář** , která poskytuje oprávnění ke čtení všech zdrojů v předplatné. Ostatních rolí v tématu [RBAC: předdefinované role](./active-directory/role-based-access-built-in-roles.md). Pro parametr **ServicePrincipalName** poskytují **ApplicationId** , který jste použili při vytváření aplikace.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Pokud váš účet nemá dostatečná oprávnění k přiřadit roli, se zobrazí chybová zpráva. Zpráva informuje o účet **nemá oprávnění k provedení akce "Microsoft.Authorization/roleAssignments/write" v oboru "/ předplatná / {guid}"**.

Je to! AD aplikace a služby jistinu se nastavují. Následující části se dozvíte, jak k přihlášení pomocí certifikátu prostřednictvím Powershellu.

### <a name="provide-certificate-through-automated-powershell-script"></a>Zadejte certifikátu prostřednictvím automatické skript Powershellu

Pokaždé, když se přihlásíte jako hlavní název služby, budete muset zadat id klienta v adresáři aplikace AD. Klienta, kterého je instancí služby Active Directory. Pokud máte jenom jedno předplatné, můžete:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Pokud máte víc předplatných, zadejte předplatné, kde jsou uložená vaše služby Active Directory. Další informace najdete v tématu [Správa adresáři Azure AD](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Ověření skriptu, zadejte účet je služba hlavní a poskytují miniaturu certifikátu, id aplikace a id klienta. K automatizaci skript, můžete uložit tyto hodnoty jako proměnné a načíst je při provádění nebo můžete je zahrnout do skriptu.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

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
- Podrobnější vysvětlení aplikací a služeb objekty najdete v článku [aplikace a služby jistinu objekty](./active-directory/active-directory-application-objects.md). 
- Další informace o ověřování služby Active Directory najdete v tématu [Ověřování scénáře Azure AD](./active-directory/active-directory-authentication-scenarios.md).



