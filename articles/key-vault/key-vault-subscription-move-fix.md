<properties
    pageTitle="Po přesunutí předplatné změnit ID klienta klíčové trezoru | Microsoft Azure"
    description="Informace o možnostech přechodu ID klienta pro klíčové trezoru po přihlášení k odběru se přesune do různých klienta"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Změna ID klienta klíčové trezoru po přesunutí předplatné
### <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>Otázka: Můj předplatné byl přesunut z klienta A na klienta B. Jak změnit ID klienta pro svůj stávající klíčové trezoru a nastavit správné ACL pro objekty v klientovi B?

Při vytváření nové klíčové trezoru v předplatné je automaticky svázané se výchozí ID klienta služby Azure Active Directory pro tuto předplatné. Všechny položky zásady přístupu je také stejným k tomuto klientovi ID. Obsah přesouvaných předplatného Azure z klienta A ke klientovi B, existující klíčové trezorů nedostupné tak, že objekty (uživatele a aplikací) v klientovi B. Tento problém můžete vyřešit, musíte:

- Změna ID klienta přidružené všechny existující klíčové trezory v toto předplatné ke klientovi B.
- Odeberte všechny existující položky zásady přístupu.
- Přidání nové položky zásady přístupu, které jsou přidružené k tenantovi B.

Například pokud obsahuje klíčové trezoru myvault předplatné, které byl přesunut z klienta A na klienta B, tady jak změnit ID klienta pro trezoru klíče a odebrat původní zásady přístupu.

<pre>
$vaultResourceId = (get AzureRmKeyVault - VaultName myvault). ResourceId $vault = Get-AzureRmResource – ResourceId $vaultResourceId - ExpandProperties $vault. Properties.TenantId = (Get-AzureRmContext). Tenant.TenantId $vault. Properties.AccessPolicies = @() Set AzureRmResource - ResourceId $vaultResourceId – vlastnosti $vault. Vlastnosti
</pre>

Protože trezoru v klientovi A před move původní hodnotu **$vault. Properties.TenantId** klienta a při **(Get-AzureRmContext). Tenant.TenantId** klient B.

Trezoru souvisí s ID správné klienta a budou odebrány staré položky zásady přístupu, nastavte novou přístup zásad položky s [AzureRmKeyVaultAccessPolicy nastavení](https://msdn.microsoft.com/library/mt603625.aspx).

## <a name="next-steps"></a>Další kroky

Pokud máte dotazy k trezoru klíč Azure, navštivte [Fóra trezoru klíč Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
