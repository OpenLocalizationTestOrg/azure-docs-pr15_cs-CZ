<properties
   pageTitle="Vytvoření služby základní portálu | Microsoft Azure"
   description="Popisuje, jak vytvořit novou aplikaci služby Active Directory a jistinu služby, který lze pomocí řízení přístupu na základě rolí správce prostředků v Azure Správa přístupu k prostředkům."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>Umožňuje vytvořit aplikaci služby Active Directory a jistinu služby, které mají přístup k prostředkům portal

> [AZURE.SELECTOR]
- [Prostředí PowerShell](resource-group-authenticate-service-principal.md)
- [Azure rozhraní příkazového řádku](resource-group-authenticate-service-principal-cli.md)
- [Portál](resource-group-create-service-principal-portal.md)


Pokud máte aplikaci, kterou je potřeba přístup nebo upravit zdroje, musíte nastavit aplikace Active Directory (AD) a přiřadit potřebná oprávnění. V tomto tématu se dozvíte, jak provádět tyto kroky prostřednictvím portálu. Aktuálně musíte se pomocí portálu klasické vytvořit novou aplikaci služby Active Directory a přejděte na portál Azure přiřazení role do aplikace. 

> [AZURE.NOTE] Postup v tomto tématu platí jenom při vytváření AD aplikace pomocí **klasického portálu** . **Pokud používáte Azure portálu pro vytváření AD aplikace, tento postup se nezdaří.** 
>
> Můžete zjistit ho snadněji nastavení AD aplikace a služby základní pomocí [prostředí PowerShell](resource-group-authenticate-service-principal.md) nebo [Azure rozhraní příkazového řádku](resource-group-authenticate-service-principal-cli.md), zejména pokud chcete použít certifikát pro ověřování. Toto téma nezobrazuje používání certifikát.

Vysvětlení služby Active Directory koncepty najdete v článku [aplikace a služby jistinu objekty](./active-directory/active-directory-application-objects.md). Další informace o ověřování služby Active Directory najdete v tématu [Ověřování scénáře Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Podrobný postup na integraci aplikace do Azure pro správu zdrojů, najdete v článku [vývojáře průvodce se tak mohli ověřovat pomocí rozhraní API Azure správce prostředků](resource-manager-api-authentication.md).

## <a name="create-an-active-directory-application"></a>Vytvoření aplikace služby Active Directory

1. Přihlaste se ke svému účtu Azure pomocí [klasické portálu](https://manage.windowsazure.com/). 

2. Zkontrolujte, že znáte výchozí služby Active Directory pro vaše předplatné. Pouze můžete udělit přístup k aplikacím v adresáři stejný jako předplatné. Klikněte na **Nastavení** a vyhledejte název adresáře přidružený k předplatnému.  Další informace najdete v tématu [jak Azure předplatná souvisí s Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).
   
     ![Vyhledání výchozí adresář](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. V levém podokně vyberte **Služby Active Directory** .

     ![Vyberte služby Active Directory](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Vyberte Active Directory, která chcete použít při vytváření aplikace. Pokud máte víc než jeden služby Active Directory, vytvoření aplikace v adresáři výchozí pro vaše předplatné.   

     ![Zvolte adresář](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. Zobrazení aplikace v adresáři, vyberte **aplikace**.

     ![zobrazení aplikace](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Pokud jste nevytvořili aplikace v adresáři před, byste měli vidět něco vidíte na následujícím obrázku. Vyberte **Přidat aplikaci**

     ![Přidat aplikaci](./media/resource-group-create-service-principal-portal/create-application.png)

     Nebo klikněte na **Přidat** v dolní části okna.

     ![Přidání](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Vyberte typ aplikace, kterou chcete vytvořit. Pro účely tohoto návodu vyberte **Přidat aplikaci, které vyvíjí mé organizaci**. 

     ![nové aplikace](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Zadejte název aplikace a vyberte požadovaný typ aplikace, kterou chcete vytvořit. Pro tento kurz vytvoření **Rozhraní API webových aplikací a či nebo WEB** a kliknutím na tlačítko Další. Pokud vyberete možnost **NATIVNÍ klientské aplikaci**, zbývajících kroků v tomto článku neodpovídají uživatelského prostředí.

     ![název aplikace](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Vyplnění vlastností aplikace. **Na PŘIHLAŠOVACÍ adresa URL**zadejte identifikátor URI na web, který popisuje aplikace. Není ověřit existenci na webu. **Identifikátor URI ID aplikace**zadejte identifikátor URI, který identifikuje aplikace.

     ![vlastnosti aplikace](./media/resource-group-create-service-principal-portal/app-properties.png)

Vytvořili jste aplikaci.

## <a name="get-client-id-and-authentication-key"></a>Získání klienta id a ověřování klíče

Když programově přihlášení, musíte id pro aplikaci. Pokud spuštění aplikace v části vlastní přihlašovací údaje, musíte taky ověřovací klíč.

1. Vyberte kartu **Konfigurovat** pro nastavení vaší aplikace heslo.

     ![Konfigurace aplikace](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Zkopírujte **Kód klienta**.
  
     ![id klienta](./media/resource-group-create-service-principal-portal/client-id.png)

3. Pokud spuštění aplikace v části vlastní přihlašovací údaje, přejděte dolů do části **klíče** a vyberte, jak dlouho heslo platit.

     ![klíče](./media/resource-group-create-service-principal-portal/create-key.png)

4. Vyberte **Uložit** a vytvořte kód.

     ![uložení](./media/resource-group-create-service-principal-portal/save-icon.png)

     Uložený klíč zobrazený a lze jej zkopírujete. Nejste schopni později vyvolat klávesu tak zkopírováním nyní.

     ![uložení klíč](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>Získání id klienta

Když programově přihlášení, budete muset předat id klienta s žádostí o ověření. Web Apps a webové rozhraní API aplikace můžete načtete id klienta výběrem **zobrazení koncové body** v dolní části obrazovky a načítání id, jak je znázorněno na následujícím obrázku.  

   ![id klienta](./media/resource-group-create-service-principal-portal/save-tenant.png)

Můžete také zadat id klienta prostřednictvím Powershellu:

    Get-AzureRmSubscription

Nebo Azure rozhraní příkazového řádku:

    azure account show --json

## <a name="set-delegated-permissions"></a>Nastavení delegovat oprávnění

Pokud vaše aplikace přistupuje prostředky k jménem přihlášený uživatel, je nutné delegované oprávnění pro přístup k aplikacím udělit aplikace. Udělit přístup v části **oprávnění do jiných aplikací** na kartě **Konfigurovat** . Ve výchozím nastavení je už povoleno delegované oprávnění pro službu Azure Active Directory. Nechte toto oprávnění beze změny.

1. Vyberte **Přidat aplikaci**.

2. V seznamu vyberte **Rozhraní API správy služby Windows Azure**. Vyberte ikonu dokončeno.

      ![Vyberte aplikaci](./media/resource-group-create-service-principal-portal/select-app.png)

3. V rozevíracím seznamu pro delegovanou oprávnění klikněte na **Správa služby Azure Access jako organizace**.

      ![Vyberte oprávnění](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Uložte změny.

## <a name="assign-application-to-role"></a>Aplikace k roli přiřazení

Pokud aplikace v části vlastní přihlašovací údaje, musíte přiřadit aplikace k roli. Rozhodněte, jaké role představuje oprávnění pro aplikaci. Další informace o dostupných rolí najdete v tématu [RBAC: integrované role](./active-directory/role-based-access-built-in-roles.md). 

Přiřazení role do aplikace, musí mít odpovídající oprávnění. Konkrétně, musíte mít `Microsoft.Authorization/*/Write` přístup, který se poskytuje prostřednictvím [vlastník](./active-directory/role-based-access-built-in-roles.md#owner) nebo [Správce přístup uživatelů](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) role. Role přispěvatele nemá správný přístup.

Nastavit obor na úrovni předplatného, skupina zdroje nebo zdroje. Oprávnění se dědí nižších úrovní obor. Například přidání aplikace k roli Čtenář pro skupinu prostředků znamená, že můžete číst skupina zdroje a materiály, které obsahuje.

1. Přiřadit aplikace k roli, přejděte na portálu klasické [Azure portálu](https://portal.azure.com).

1. Zkontroloval vaše oprávnění, aby zkontrolovala, jestli že jistinu service můžete přiřadit role. Vyberte **Moje oprávnění** pro váš účet.

    ![Vyberte Moje oprávnění](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. Zobrazení přiřazená oprávnění pro váš účet. Jak je uvedeno dříve, musí patřit k rolím vlastník nebo správce přístup uživatelů nebo mít vlastní roli, která zajišťuje přístup pro zápis pro Microsoft.Authorization. Následující obrázek znázorňuje účtu, který je přiřazena role přispěvatele pro předplatné, které není odpovídající oprávnění přiřazení aplikace k roli.

    ![Zobrazit moje oprávnění](./media/resource-group-create-service-principal-portal/show-permissions.png)

     Pokud není správná oprávnění udělujete přístup k aplikaci, že musíte buď žádost, že správce předplatného se přidá do roli správce přístup uživatelů nebo požádat uděluje správce přístup k aplikaci.

1. Přejděte na úrovni obor, které chcete přiřadit aplikace. Přiřadit roli na obor předplatné vyberte **předplatné**.

     ![Vyberte předplatné](./media/resource-group-create-service-principal-portal/select-subscription.png)

     Vyberte konkrétní předplatné přiřazení aplikace.

     ![Vyberte předplatné pro přiřazení](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     V pravém horním rohu vyberte ikonu **aplikace Access** .

     ![Vyberte přístup](./media/resource-group-create-service-principal-portal/select-access.png)
     
     Nebo pokud chcete přiřadit roli v rozsahu skupiny zdroje, přejděte na skupina zdroje. Ze skupiny zásuvné zdroje vyberte **řízení přístupu**.

     ![Vyberte uživatelé](./media/resource-group-create-service-principal-portal/select-users.png)

     Tyto kroky jsou stejná pro všechny obor.

2. Vyberte možnost **Přidat**.

     ![Vyberte Přidat](./media/resource-group-create-service-principal-portal/select-add.png)

3. Vyberte roli **Čtenář** (nebo jakéhokoliv rolí, kterou chcete přiřadit aplikaci).

     ![Vyberte roli](./media/resource-group-create-service-principal-portal/select-role.png)

4. Až uvidíte nejdřív seznam uživatelů, které můžete přidat k roli, neuvidíte aplikací. Zobrazí se pouze uživatelé a skupiny.

     ![Zobrazení uživatelů](./media/resource-group-create-service-principal-portal/show-users.png)

5. Najít aplikace, musí vyhledejte jej. Začněte psát název aplikace a se změní seznam dostupných možností. Pokud se zobrazí v seznamu vyberte aplikace.

     ![přiřazení role](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Vyberte **OK** dokončete přiřazení role. Teď byste měli vidět aplikace v seznamu použití přiřazenou roli skupiny prostředků.


Další informace o přidávání uživatelů a aplikací k rolím prostřednictvím portálu přiřazování najdete v článku [přiřazení rolí použít ke správě přístupu k prostředkům Azure předplatného](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

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
- [Správa Azure zdrojů a zdroje skupiny s Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Nasazení SSH povolené OM šablony v Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Správa Azure materiály a zdroje skupiny s Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Skutečné**

- [Nasazení SSH povolené OM šablony v skutečné](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Správa Azure zdroje a skupin zdroje s skutečné](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>Další kroky

- Další informace o určení zásady zabezpečení najdete v tématu [Řízení přístupu na základě rolí Azure](./active-directory/role-based-access-control-configure.md).  
- Video ukázku z těchto kroků najdete v tématu [Povolení programový správy zdroje Azure pomocí služby Azure Active Directory](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

