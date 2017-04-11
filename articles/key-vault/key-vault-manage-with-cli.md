<properties
    pageTitle="Správa klíč trezoru pomocí rozhraní příkazového řádku | Microsoft Azure"
    description="Tento kurz použít k automatizaci běžné úkoly v klíč trezoru pomocí rozhraní příkazového řádku"
    services="key-vault"
    documentationCenter=""
    authors="BrucePerlerMS"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="bruceper"/>

# <a name="manage-key-vault-using-cli"></a>Správa klíč trezoru pomocí rozhraní příkazového řádku #
Azure trezoru klíč je k dispozici ve většině oblastech. Další informace najdete v tématu [klíč trezoru ceny stránky](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod  
Pomocí tohoto kurzu vám pomůžou začít s Azure klíč trezoru vytvoření kontejneru zesílený (trezoru) v Azure, ukládání a správu cryptographic klíče a tajemství v Azure. Ho vás provede proces vytvoření trezoru obsahující klíče nebo heslo, které používáte s Azure aplikace pomocí rozhraní příkazového řádku platformě Azure. Ho pak se dozvíte, jak aplikace použít tento klíč nebo heslo.

**Předpokládaná doba dokončete:** 20 minut

>[AZURE.NOTE]  Tento kurz nezahrnuje pokyny, jak psát Azure aplikace, že jeden z kroků obsahuje, které ukazuje, jak povolit aplikaci pro používání klíče nebo tajná klíčové trezoru.
>
>V současné době nejde nakonfigurovat Azure klíč trezoru Azure portálu. Použijte tyto pokyny rozhraní různé platformy příkazového řádku. Nebo Azure pokyny k Powershellu, podívejte se na [Tento kurz odpovídající](key-vault-get-started.md).

Základní informace o trezoru klíč Azure, najdete v článku [Co je Azure klíč trezoru?](key-vault-whatis.md)

## <a name="prerequisites"></a>Zjistit předpoklady pro
Tento kurz musí mít takto:

- Předplatné Microsoft Azure. Pokud nemáte jeden, můžete zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial).
- Rozhraní příkazového řádku verze 0.9.1 nebo novější. Nainstalujte nejnovější verzi a připojení k předplatnému Azure naleznete v tématu [instalace a konfigurace rozhraní Azure různé platformy příkazového řádku](../xplat-cli-install.md).
- Aplikace, která bude nakonfigurovaný na používání klíč nebo heslo, které vytvoříte v tomto kurzu. Ukázka aplikace je k dispozici z [Webu služby Stažení softwaru](http://www.microsoft.com/download/details.aspx?id=45343). Pokyny najdete v tématu doprovodnou souboru Readme.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Získání nápovědy pomocí rozhraní příkazového řádku platformě Azure

Tento kurz se předpokládá, že znáte rozhraní příkazového řádku (flám, Terminálová, příkazový řádek)

Je - nápovědu nebo -h parametru lze se dá zobrazit Nápověda pro konkrétní příkazy. Můžete také azure Nápověda [příkaz], [možnosti] formátování lze také vracet stejné informace. Například následující příkazy všechny vrátí stejné informace:

    azure account set --help

    azure account set -h

    azure help account set

Pokud jste na pochybách parametry potřeby tak, že příkazu, najdete v nápovědě pomocí – Nápověda, -h nebo azure nápovědy [příkaz].

Můžete taky přečíst následující kurzy k seznámení s správce prostředků Azure v Azure různé platformy příkazového řádku rozhraní:

- [Jak nainstalovat a nakonfigurovat rozhraní Azure různé platformy příkazového řádku](../xplat-cli-install.md)
- [Správce Azure rozhraní příkazového řádku různé platformy s Azure zdroje](../xplat-cli-azure-resource-manager.md)


## <a name="connect-to-your-subscriptions"></a>Připojení k předplatného

Přihlaste se pomocí účtu organizace, použijte tento příkaz:

    azure login -u username -p password

nebo pokud si chcete přihlásit zadáním interaktivní

    azure login

>[AZURE.NOTE]  Přihlášení metodu jde použít pouze pomocí účtu organizace. Účet organizace je uživatel, která spravuje vaši organizaci a podle klienta služby Azure Active Directory vaší organizace.


Pokud zrovna nemáte účet organizace a používáte účet Microsoft k přihlášení k předplatnému Azure, můžete můžete snadno vytvořit pomocí následujících kroků.

1.  Přihlaste se k přihlášení k [Portálu Správa Azure](https://manage.windowsazure.com/)a klepněte na služby Active Directory.
2.  Pokud existuje žádný adresář, vyberte vytvořit adresář a zadejte požadované informace.
3.  Vyberte svůj adresář a přidání nového uživatele. Tomuto novému uživateli je účet organizace. Při vytváření uživatel bude dodaného s obou e-mailovou adresu uživatele a dočasného hesla. Uloží tyto informace, jak se používá v další krok.
4.  Z portálu Microsoft vyberte nastavení a potom vyberte správce. Klikněte na Přidat a přidání nového uživatele jako spolu správce. Díky účet organizace pro správu předplatného Azure.
5.  Nakonec odhlášení z portálu Azure a potom znova se přihlaste pomocí nového účtu organizace. Pokud je první čas přihlašování pomocí tohoto účtu, se výzva ke změně hesla.

Další informace o použití účet organizace s Microsoft Azure najdete v článku [registraci Microsoft Azure jako organizace](../active-directory/sign-up-organization.md).

Pokud máte víc předplatných a chcete zadat specifické pro účely trezoru klíč Azure, zadejte podle následujících pokynů v tématu předplatná pro váš účet:

    azure account list

Chcete-li předplatné pro použití, zadejte:

    azure account set <subscription name>

Další informace o konfiguraci rozhraní příkazového řádku Azure různé platformy najdete v článku [Jak nainstalovat a nakonfigurovat rozhraní Azure různé platformy příkazového řádku](../xplat-cli-install.md).


## <a name="switch-to-using-azure-resource-manager"></a>Přechod k používání správce prostředků Azure

Klíč trezoru vyžaduje správce prostředků Azure, takže zadejte následující přepněte do režimu správce prostředků Azure:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Vytvoření nové skupiny prostředků

Pokud chcete použít Správce prostředků Azure, všechny související materiály vzniká uvnitř skupiny zdrojů. Pro účely tohoto návodu vytvoříme nové skupiny prostředků "ContosoResourceGroup".

    azure group create 'ContosoResourceGroup' 'East Asia'

První parametr je název skupiny prostředků a druhý parametr je umístění. Umístění, použijte příkaz `azure location list` k identifikaci určení alternativní umístění na snímek v tomto příkladu. Pokud budete potřebovat další informace, zadejte:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Zprostředkovatele prostředků trezoru klíče registru
Zkontrolujte, jestli že je tento klíč trezoru zdroje poskytovatel registrovaná ve vašem předplatném:

`azure provider register Microsoft.KeyVault`

To stačí udělat jednou na jedno předplatné.


## <a name="create-a-key-vault"></a>Vytvoření klíčových trezoru

Použití `azure keyvault create` příkaz Vytvořit klíčové trezoru. Tento skript má tři povinné parametry: název skupiny prostředků, názvu klíčové trezoru a zeměpisné polohy.

Pokud používáte název trezoru ContosoKeyVault, název skupiny zdroje ContosoResourceGroup a umístění jihovýchodní Asie, zadejte například:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Výstup tento příkaz zobrazí vlastnosti klíčové trezoru, který jste právě vytvořili. Jsou dva nejdůležitější vlastnosti:

- **Název**: V tomto příkladu je to ContosoKeyVault. Použijete tento název pro jiné rutiny klíč trezoru.
- **vaultUri**: V tomto příkladu je to https://contosokeyvault.vault.azure.net. Aplikace, které využívají trezoru prostřednictvím jeho rozhraní REST API musíte použít tento identifikátor URI.

Váš účet Azure je teď oprávnění provádět všechny operace s trezoru klíče. Co ještě nikdo jiný je.


## <a name="add-a-key-or-secret-to-the-key-vault"></a>Přidání klíče nebo tajná klíčové trezoru

Pokud chcete Azure klíč trezoru při vytváření klíče software chráněn za vás, použijte `azure key create` příkaz a zadejte tento příkaz:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Ale máte existující klíč .pem soubor uložený jako místní soubor do souboru nazvaného softkey.pem, který chcete nahrát do trezoru klíč Azure zadejte následující příkaz Import klíče z. PEM soubor, který chrání klávesu softwarem ve službě trezoru klíč:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Teď můžete odkázat klíč, který jste vytvořili nebo nahrát trezoru klíč Azure pomocí jeho URI. Získáte pomocí **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** vždy aktuální verzi a pomocí **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** této konkrétní verzi.

Pokud chcete přidat tajná trezoru, tedy hesla s názvem SQLPassword a, která obsahuje hodnotu z Pa$ $w0rd do trezoru klíč Azure, zadejte následující:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Teď můžete odkázat toto heslo, které jste přidali do trezoru klíč Azure pomocí jeho URI. Získáte pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword** vždy aktuální verzi a pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** této konkrétní verzi.

Pojďme zobrazte klávesu tajná, který jste právě vytvořili:

- Pokud chcete zobrazit kód, zadejte:`azure keyvault key list --vault-name 'ContosoKeyVault'`
- Chcete-li zobrazit skryté, zadejte:`azure keyvault secret list --vault-name 'ContosoKeyVault'`


## <a name="register-an-application-with-azure-active-directory"></a>Registrace aplikace pomocí služby Azure Active Directory

Tento krok by provést obvykle vývojář, v samostatném počítači. Se nevztahuje na Azure klíč trezoru, ale je součástí tady pro dokončení.


>[AZURE.IMPORTANT] Dokončete kurzu, váš účet, trezoru a aplikace, která bude registrovat v tomto kroku musí být stejného Azure adresáře.

Aplikace, které využívají klíčové trezoru musí ověřovat pomocí token ze služby Azure Active Directory. K tomuto účelu vlastník aplikace nejdřív zaregistrujte aplikace v Azure Active Directory. Na konci registrace vlastníka aplikace obdrží následující hodnoty:


- **ID aplikace** (označovaná taky jako ID klienta) a **ověřování klíč** (označovaná taky jako sdílené heslo). Aplikace musí prezentovat obě tyto hodnoty Azure Active Directory, chcete-li získat token. Konfigurace aplikace k tomuto závisí na aplikace. Ukázka aplikace trezoru klíč nastaví vlastníka aplikace tyto hodnoty v konfiguračního souboru.



Registrace aplikace v Azure Active Directory:

1. Přihlaste se k portálu Azure.
2. Na levé straně klikněte na **Služby Active Directory**a vyberte v adresáři, ve kterém bude registrovat aplikace. <br> <br> Poznámka: Je nutné vybrat stejný adresář, který obsahuje Azure předplatné, ke které jste vytvořili trezoru klíče. Pokud si nejste jisti, které adresáře to je, klikněte na **Nastavení**, identifikovat předplatné, ke které jste vytvořili trezoru klíče a zapište si název adresáři zobrazené v posledním sloupci.

3. Klikněte na **aplikace**. Pokud žádná aplikace byly přidány do vašeho adresáře, tato stránka se zobrazí jenom na odkaz **Přidat aplikaci** . Klikněte na odkaz nebo případně můžete kliknout na **Přidat** na panelu s příkazy.
4.  V průvodci **Přidat aplikaci** na **Co chcete udělat?** stránky, klikněte na **Přidat aplikaci, které vyvíjí mé organizaci**.
5.  Na stránce **námi o aplikaci** zadejte název aplikace a vyberte **Rozhraní API webových aplikací a či nebo WEB** (výchozí). Klepněte na ikonu Další.
6.  Na stránce **Vlastnosti aplikace** , zadejte **Adresu URL na ODHLÁSIT** a **Identifikátor URI aplikace** webové aplikace. Pokud není aplikace tyto hodnoty, můžete jejich pro tento krok (je třeba zadat http://test1.contoso.com obou polí). Není důležité, pokud existují těchto webů; Důležité je, že se liší pro všechny aplikace v adresáři aplikace Identifikátor URI pro jednotlivé aplikace. V adresáři používá tento řetězec k identifikaci aplikace.
7.  Klikněte na ikonu dokončení a uložte provedené změny v průvodci.
8.  Na stránce Snadné spuštění klikněte na **KONFIGUROVAT**.
9.  Přejděte do oddílu **klíčů** , nastavte dobu trvání a klikněte na tlačítko **Uložit**. Na stránce aktualizuje a teď zobrazí hodnoty klíče. S touto hodnotou klíče a hodnotu **ID klienta** musíte nakonfigurovat aplikaci. (Pokyny k této konfiguraci bude specifická.)
10. Zkopírujte hodnotu ID klienta z této stránky, které budete používat v dalším kroku nastavit oprávnění pro trezoru.




## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Povolit aplikaci pro použití klávesy nebo tajná

Chcete-li povolit aplikaci pro přístup k klíč nebo tajná v trezoru, použijte `azure keyvault set-policy` příkaz.

Například pokud vaše jméno trezoru je ContosoKeyVault a aplikaci, kterou chcete povolit má klienta ID 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed a chcete-li povolit aplikaci dešifrování a přihlaste se pomocí klávesy v trezoru, spusťte následující:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '["decrypt","sign"]'

>[AZURE.NOTE] Používáte na příkazovém řádku systému Windows, mají apostrofy nahraďte dvojitých uvozovkách a také uniknout interní dvojitých uvozovkách. Příklad: "[\"dešifrování\",\"znak\"]".

Pokud chcete povolit stejné aplikace číst tajemství v trezoru, spusťte následující:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '["get"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Pokud chcete používat modul zabezpečení hardwaru (HSM) ##

Přidané assurance importovat nebo vygenerování klíčů v hardwaru zabezpečení modulech (moduly hardwarového zabezpečení), které nikdy opustit HSM okraj. Moduly hardwarového zabezpečení jsou FIPS ověřit 140-2 úrovně 2. Pokud tento požadavek neplatí pro vás, tuto část přeskočit a přejít na [Odstranit klíčové trezoru a související klíče a tajemství](#delete-the-key-vault-and-associated-keys-and-secrets).

Vytvoření klávesy chráněné heslem HSM, musíte mít trezoru předplatné, které podporuje chráněné heslem HSM klíče.

Když vytvoříte keyvault, přidejte parametr "sku":

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Můžete přiřadit software chráněn klíčů (jak je znázorněno výše) a chráněné heslem HSM trezoru. Při vytváření chráněné heslem HSM klíče tvořeného nastavte parametr cílový "HSM":

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

Tento příkaz umožňuje importovat klíč ze souboru .pem ve vašem počítači. Příkaz importuje klávesu do moduly hardwarového zabezpečení ve službě trezoru klíč:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Další příkaz importuje "přenést vlastní klíč" balíček (BYOK). Toto oprávnění umožňuje generovat kód v místním HSM a odeslat jej moduly hardwarového zabezpečení ve službě klíč trezoru bez klíče necháte HSM okraj:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Podrobnější pokyny týkající se tohoto BYOK balíček najdete v článku [jak klávesami HSM-Protected s Azure klíč trezoru](key-vault-hsm-protected-keys.md).


## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Odstranit klíčové trezoru a související klíče a tajemství

Pokud už nepotřebujete klíčové trezoru a klíč nebo tajná ní obsaženými, můžete odstranit klíčové trezoru pomocí příkazu odstranit azure keyvault:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Nebo můžete odstranit celý Azure zdroj skupinu, která obsahuje klíčové trezoru a dalších zdrojů, která jste zadali v dané skupině:

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Další příkazy Azure rozhraní příkazového řádku různé platformy

Další příkazy můžete osvědčuje při vedení Azure klíč trezoru.

Vypíše tabulkového zobrazení všech klíče a vybrané vlastnosti:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Vypíše tabulkového zobrazení všechny skryté názvy a vybrané vlastnosti:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Tady je příklad toho, jak odebrat určitého klíče:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Tady je příklad toho, jak odebrat konkrétní tajná:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Další kroky

Programování odkazy, najdete v článku [Příručka pro vývojáře Azure klíč trezoru](key-vault-developers-guide.md).
