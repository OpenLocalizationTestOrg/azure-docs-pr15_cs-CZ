<BR> 
## <a name="faq---reverse-dns-for-your-azure-assigned-ip-address"></a>Časté otázky – obráceném DNS přiřazené Azure IP adresa

### <a name="how-much-do-reverse-dns-records-cost"></a>Kolik Převrátit náklady záznamy DNS?
Budou zdarma!  Neexistuje žádná další náklady obráceném záznamy DNS nebo dotazů.

### <a name="will-my-reverse-dns-records-resolve-from-the-internet"></a>Odstraní z Internetu obráceném záznamy DNS?
Ano. Po nastavení vlastnost obráceném DNS pro vaše cloudové služby Azure má na starosti delegování DNS a k zajištění, aby zpětné záznam DNS odstraňuje pro všechny uživatele internet zóny DNS.

### <a name="will-a-default-reverse-dns-record-be-created-for-my-cloud-services"></a>Výchozí obráceném záznam DNS se vytvoří Moje služby cloudu?
Ne. Zpětné DNS je funkce vyjádření výslovného se změnami. Pokud se rozhodnete nastavovat vytvoří žádné výchozí zpětné DNS záznamy.

### <a name="what-is-the-format-for-the-fully-qualified-domain-name-fqdn"></a>Jaký je formát pro plně kvalifikovaný název domény (FQDN)?
FQDN jsou uvedeny v pořadí, přeposílání a musí být ukončena tečkou (například "app1.contoso.com.").

### <a name="what-happens-if-the-validation-checks-for-the-reverse-dns-ive-specified-fail"></a>Co se stane, pokud se nezdaří ověření obráceném DNS jste zadali?
Kde kontroly ověření obráceném DNS nezdaří, služba správy se nezdaří. Opravte hodnotu obráceném DNS podle potřeby a opakování.

### <a name="can-i-manage-reverse-dns-for-my-azure-website"></a>Můžete spravovat obráceném DNS pro svůj web Azure?
Zpětné DNS nepodporuje Azure weby. Zpětné DNS je podporovaný pro Azure PaaS rolích a IaaS virtuálních počítačích.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-cloud-service"></a>Můžete nakonfigurovat více obráceném záznamů DNS pro moje Cloudová služba?
Ne. Azure podporuje jeden zpětně záznam DNS pro každou službu Azure cloudu. Každý cloudové služby Azure však může mít vlastní obráceném záznam DNS.

### <a name="can-i-send-emails-to-external-domains-from-my-azure-compute-services"></a>Můžete poslat e-mailů externími doménami z Moje služby výpočet Azure?
Ne. [Výpočet azure služby nepodporují odesílání e-mailů na externí domény](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).
