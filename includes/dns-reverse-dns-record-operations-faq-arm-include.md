<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Časté otázky – obráceném DNS přiřazené Azure IP adresa

### <a name="how-much-do-reverse-dns-records-cost"></a>Kolik Převrátit náklady záznamy DNS?
Budou zdarma!  Neexistuje žádná další náklady obráceném záznamy DNS nebo dotazů.

### <a name="will-the-reverse-dns-records-for-my-azure-assigned-public-ip-address-resolve-from-the-internet"></a>Vyřeší obráceném záznamy DNS pro moje přiřazené Azure veřejnou IP adresu z Internetu?
Ano. Jakmile nastavíte vlastnost obráceném DNS pro veřejnou IP adresu, spravuje Azure delegování DNS a k zajištění, aby zpětné záznam DNS odstraňuje pro všechny uživatele internet zóny DNS.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-public-ip-addresses"></a>Výchozí obráceném záznam DNS se vytvoří Moje veřejnou IP adres?
Ne. Zpětné DNS je funkce vyjádření výslovného se změnami. Pokud se rozhodnete nastavovat vytvoří žádné výchozí zpětné DNS záznamy.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Jaký je formát pro plně kvalifikovaný název domény (FQDN)?
FQDN jsou uvedeny v pořadí, přeposílání a musí být ukončena tečkou (například "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Co se stane, pokud se nezdaří ověření obráceném DNS jste zadali?
Kde kontroly ověření obráceném DNS nezdaří, služba správy se nezdaří. Opravte hodnotu obráceném DNS podle potřeby a opakování.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Můžete spravovat obráceném DNS pro svůj web Azure?
Zpětné DNS nepodporuje Azure weby. Zpětné DNS je podporována pro Azure virtuálních počítačích.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-public-ip-address"></a>Můžete nakonfigurovat více obráceném záznamů DNS pro moje veřejnou IP adresu
Ne. Azure podporuje jeden zpětně záznam DNS pro každou veřejnou IP adresu. Každý veřejnou IP adresu však může mít vlastní obráceném záznam DNS.

### <a name="can-i-configure-reverse-dns-records-for-an-ipv6-public-ip-address"></a>Můžete nakonfigurovat obráceném záznamy DNS pro veřejnou IP adresu IPv6?
Ne.  V současné době obráceném záznamy DNS jsou podporované veřejnou IP adresy IPv4 pro pouze.

### <a name="can-i-configure-a-reverse-dns-record-for-my-public-ip-address-without-having-a-domainnamelabel-specified"></a>Můžete nakonfigurovat obráceném záznam DNS pro moje veřejnou IP adresu bez nutnosti DomainNameLabel zadané?
Ne. Můžete využít obráceném záznamy DNS pro veřejnou IP adresy, je nutné zadat vlastnost DomainNameLabel.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Můžete poslat e-mailů externími doménami z Moje služby výpočet Azure?
Ne. [Výpočet azure služby nepodporují odesílání e-mailů na externí domény](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
