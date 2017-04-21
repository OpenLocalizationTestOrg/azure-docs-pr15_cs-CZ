<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Použití Azure rozhraní příkazového řádku s Azure správce prostředků (ARM)

Než budete moct použít Azure rozhraní příkazového řádku s příkazy Správce prostředků a šablonami pro nasazení Azure zdrojů a úloh využití prostředků skupin, je nutné pro účet Azure (samozřejmě). Pokud nemáte účet, můžete získat [bezplatnou zkušební verzi Azure tady](https://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Pokud ještě nemáte účet Azure, musíte ale předplatné MSDN předplatného, můžete získat bezplatné Azure přeplatky aktivací své [MSDN odběratele tady výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) – nebo se pomocí bezplatného účtu. Buď fungovalo Azure přístup pomocí protokolu.

### <a name="step-1-verify-the-azure-cli-version"></a>Krok 1: Ověření verze rozhraní příkazového řádku Azure

Používat Azure rozhraní příkazového řádku pro naléhavé příkazy a ARM šablony, musíte mít minimální verze 0.8.17. Pro ověření vaší verzí, zadejte `azure --version`. Měli byste vidět nějak:

    $ azure --version
    0.8.17 (node: 0.10.25)

Pokud potřebujete aktualizovat svoji verzi Azure rozhraní příkazového řádku, přečtěte si článek [Azure rozhraní příkazového řádku](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Krok 2: Ověřte, že používáte pracovní nebo školní identit s Azure

Příkaz režimu ARM můžete použít jenom Pokud používáte [klienta služby Azure Active Directory](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) nebo [Hlavní název služby](https://msdn.microsoft.com/library/azure/dn132633.aspx). (Toto jsou zkratka *organizační ID*.)

Pokud chcete zobrazit, pokud máte, přihlaste se zadáním `azure login` a pomocí svého pracovního nebo školního uživatelské jméno a heslo po zobrazení výzvy. Pokud nějaké máte, byste měli vidět nějak takto:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Pokud to nevidíte, musíte vytvořit nového klienta (nebo služby základní) pomocí své identity pro účet Microsoft. (Toto je často v případě osobní web MSDN předplatných nebo bezplatnou zkušební předplatné.) Pokud chcete vytvořit pracovní nebo školní id z účtu Azure vytvořená pomocí id služeb Microsoft, najdete v článku [přidružení adresáře Azure AD pomocí nové předplatné Azure](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). Pokud si myslíte, že byste si měli id organizace už, budete muset komunikovat s lidmi, kteří vytvoření účtu.

### <a name="step-3-choose-your-azure-subscription"></a>Krok 3: Volba předplatného Azure

Pokud mají jenom jedno předplatné v Azure účet Azure rozhraní příkazového řádku automaticky přidružena, které předplatné ve výchozím nastavení. Pokud máte víc předplatných, budete muset vyberte předplatné, které chcete použít tak, že zadáte `azure account set <subscription id or name> true` kde _předplatné id nebo název_ je id předplatného nebo na název předplatného, které se mají pro práci s v aktuální relaci.

Měli byste vidět nějak takto:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Krok 4: Umístěte vaší Azure rozhraní příkazového řádku v režimu ARM

Režim Azure zdroje Management (ARM) pomocí služby Azure rozhraní příkazového řádku, zadejte `azure config mode arm`. Měli byste vidět nějak takto:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] Můžete přejít zpět používat příkazy pro správu služby Azure zadáním `azure config mode asm`.
