#### <a name="create-an-a-record-set-with-single-record"></a>Vytvořte záznam sada se jedna položka

Pokud chcete vytvořit sadu záznamů, použijte `azure network dns record-set create`. Určení skupina zdroje, název zóny záznamu nastavit relativní název, typ záznamu a čas live (TTL).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Po vytvoření A záznamu nastavit, přidejte adresu IPv4 záznam sada se `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Vytvoření záznamu AAAA sada se jeden záznam

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Vytvoření záznamu CNAME sada se jeden záznam

Záznamy CNAME povolit pouze jednu hodnotu jednoho řetězce.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Vytvoření záznamu MX sada se jeden záznam

V tomto příkladu jsme použít název sada záznamů "@" vytvořit záznam MX vrcholu zóny (v tomto případě "contoso.com"). To je běžné záznamy MX.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Vytvoření záznamu NS sada se jeden záznam

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Vytvořte záznam PTR sada se jeden záznam  
V tomto případě "my-arpa-server zone.com" představuje zónu ARPA představující rozsah IP.  Každý záznam PTR nastavení v této zóně odpovídá IP adresu tohoto rozsahu IP.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Vytvoření záznamu SRV s jeden záznam

Pokud vytváříte SRV záznam v kořenovém zóně, můžete určit "_service" a "_protocol" v názvu záznamu. Je nutné zadat "@" v názvu záznamu.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Vytvoření záznamu TXT sada se jedna položka

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
