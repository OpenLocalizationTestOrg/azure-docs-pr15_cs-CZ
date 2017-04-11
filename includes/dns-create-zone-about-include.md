Zóny DNS slouží k hostovat záznamy DNS pro určité domény. Abyste mohli začít, který je hostitelem vaší domény, potřebujete k vytvoření zóny DNS. Všechny záznamy DNS vytvořené pro konkrétní doménu bude uvnitř zóny DNS pro doménu. 

Například doména "contoso.com" může obsahovat mnoho záznamů DNS, například "mail.contoso.com" (k poštovnímu serveru) a "www.contoso.com" (pro web). 


## <a name="names"></a>Informace o DNS zóny jmen
 
- Název pásmu musí být jedinečný v rámci skupiny zdrojů a pásmu nesmí již existuje. V opačném operace se nezdaří.

- Stejný název zóny může být znovu použít v jiné skupině zdrojů nebo jiné Azure předplatné. 

- Pokud více pásem sdílet stejný název, pokaždé přiřadíte jiné názvové servery a jenom jedna instance můžete delegovat z nadřazené domény. Další informace najdete v tématu [delegáta domain Azure DNS](../articles/dns/dns-domain-delegation.md).