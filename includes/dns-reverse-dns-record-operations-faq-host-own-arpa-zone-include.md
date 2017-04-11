<BR> 
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Časté otázky – hostingu ARPA zóny DNS Azure

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Můžete hostovat ARPA zóny Moje přiřadil poskytovatel služeb Internetu IP bloků v Azure DNS?
Ano. Zóny ARPA pro vlastní rozsahy IP adres v Azure DNS hostingu je plně podporován.

Jednoduše [zóně v Azure DNS](dns-getstarted-create-dnszone.md)vytvořte pracovní u svého poskytovatele [delegáta zóny](dns-domain-delegation.md).  Potom můžete spravovat záznamy PTR pro každou dopředného stejným způsobem jako jiné typy záznamů.

Můžete taky [importovat existující zóny dopředného pomocí rozhraní příkazového řádku Azure](dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Kolik hostingu mé náklady ARPA pásma?
Hostitelské zónu ARPA pro svoji IP přiřazené poskytovatel internetových služeb bloků v Azure DNS bude účtováno [Standardní](https://azure.microsoft.com/pricing/details/dns/)sazbou Azure DNS.

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Můžete hostovat ARPA zóny adresy IPv4 a IPv6 v Azure DNS?
Ano.