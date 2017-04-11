## <a name="what-is-reverse-dns"></a>Co je obráceném DNS?

Běžných záznamy DNS na IP adresu (například 64.4.6.100) povolit mapování pod názvem DNS (třeba "www.contoso.com").  Zpětné DNS umožňuje překlad IP adresy (64.4.6.100) zpět název (www.contoso.com).

Zpětné záznamy DNS se používají v celé řadě situací. Zpětné záznamy DNS jsou například rozšířený v boji proti nevyžádané e-mailu ověřením odesílatele e-mailové zprávy.  Přijímání poštovního serveru bude načtení obráceném záznam DNS Server odchozí pošty IP adresy a ověřte, pokud, které hostují oprávněním odesílat e-mailu z původní doménu. (Poznámka však této [služby Azure výpočet nepodporují odesílání e-mailů s externími doménami](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Jak obráceném DNS funguje

Zpětné záznamy DNS jsou umístěny v zvláštní zóny DNS jmenoval "ARPA" zóny.  Tyto zóny formuláře samostatné hierarchie DNS souběžně s normálním hierarchií hostingu domény, například "contoso.com".

Například DNS record "www.contoso.com" je implementovaná pomocí u záznamu DNS "A" název "www" na zónu contoso.com.  Tento záznam odkazuje na odpovídající IP adresu, v tomto případě 64.4.6.100.  Zpětné vyhledávání prováděna samostatně, pomocí "PTR" záznam s názvem "100" v zóně "6.4.64.in-addr.arpa" (Všimněte si, že IP adresy obrácené v oblastech ARPA.)  Tento záznam PTR, pokud je nakonfigurované správně, odkazuje na název www.contoso.com.

Když se organizace přiřazuje bloku IP adresy, získají také právo spravovat odpovídající ARPA zóny. Zóny ARPA odpovídající bloky IP adresy používané Azure hostované se spravuje Microsoft. Poskytovatele internetových služeb hostitelském zónu ARPA pro IP adres pro vás nebo může umožňují hostovat zónu ARPA ve službě DNS podle svého výběru, například Azure DNS.

>[AZURE.NOTE] Dopředná DNS vyhledávání a zpětné vyhledávání DNS jsou implementovaná v samostatných, paralelní hierarchie DNS. Zpětné vyhledávání "www.contoso.com" **není** umístěn v zóně "contoso.com", ale je hostovaný v zóně ARPA pro odpovídající blok adresy IP.

Další informace o zpětném DNS najdete v článku [Zpětné vyhledávání DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure podpora zpětné DNS

Azure podporuje dvou samostatných scénáře týkající se chcete-li převrátit DNS:

1. Hostitelské zónu ARPA odpovídající blok adresy IP.
2. Umožňuje konfigurovat obráceném záznam DNS na IP adresu přiřazená služby Azure.

K podpoře DNS bývalého, Azure mohou sloužit k hostiteli ARPA zón a spravovat záznamy PTR pro každou dopředného DNS.  Proces vytváření zónu ARPA, nastavení delegování a konfiguraci záznamy PTR je stejná jako běžný zóny DNS.  Pouze rozdíl se, že delegování musí být nakonfigurované prostřednictvím poskytovatele internetových služeb spíše než svého registrátora DNS a bude použito pouze PTR typ záznamu.

K podpoře ten, Azure umožňují konfigurovat obráceném vyhledávání pro IP adresy přidělit služby.  Toto obráceném vyhledávání nakonfigurovaný tak, že Azure jako záznam PTR odpovídajícího ARPA pásma.  Tyto zóny ARPA odpovídající všech rozsahů IP používaný Azure, což jsou hostovány společnost Microsoft. **Zbývající Tento článek popisuje tento scénář podrobně.**
