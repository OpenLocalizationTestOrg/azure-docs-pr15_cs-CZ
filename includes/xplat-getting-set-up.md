<properties services="virtual-machines" title="Setting up Azure CLI for service management" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli"></a>Použití Azure rozhraní příkazového řádku

Následující postup vám pomohou používat rozhraní příkazového řádku Azure snadno s velkými předplatného a nejnovější verzi. Pokud budete muset nainstalovat Azure rozhraní příkazového řádku a připojení k vašemu účtu, přečtěte si článek [rozhraní Azure příkazového řádku (Azure rozhraní příkazového řádku)](xplat-cli-install.md).

### <a name="step-1-update-azure-cli-version"></a>Krok 1: Aktualizace Azure rozhraní příkazového řádku verze

Pokud chcete používat Azure rozhraní příkazového řádku pro naléhavé příkazy pomocí služby správy režimu, měli byste mít nejnovější verzi Pokud je to možné. Pro ověření vaší verzí, zadejte `azure --version`. Měli byste vidět nějak:

    $ azure --version
    0.8.17 (node: 0.10.25)

Pokud chcete aktualizovat svoji verzi Azure rozhraní příkazového řádku, přečtěte si článek [Azure rozhraní příkazového řádku](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Krok 2: Nastavení předplatného a účet Azure

Po připojení vašeho rozhraní příkazového řádku Azure pomocí účtu, který chcete použít, můžete mít víc než jedno předplatné. Pokud neuděláte, byste měli zkontrolovat předplatná k dispozici pro váš účet zadáním `azure account list`a pak vyberte předplatné, které chcete použít tak, že zadáte `azure account set <subscription id or name> true` kde _předplatné id nebo název_ je id předplatného nebo na název předplatného, které se mají pro práci s v aktuální relaci. Měli byste vidět nějak takto:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [AZURE.NOTE] Pokud ještě nemáte účet Azure, musíte ale předplatné MSDN předplatného, můžete získat bezplatné Azure přeplatky aktivací své [MSDN odběratele tady výhod](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) – nebo se pomocí bezplatného účtu. Buď fungovalo Azure přístup pomocí protokolu.
