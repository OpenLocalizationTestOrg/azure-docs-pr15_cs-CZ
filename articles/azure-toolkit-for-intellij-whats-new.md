<properties
    pageTitle="Co je nového v Azure sada nástrojů pro IntelliJ | Microsoft Azure"
    description="Informace o nejnovějších funkcích v sadu Azure pro IntelliJ."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Co je nového v Azure sada nástrojů pro IntelliJ

## <a name="azure-toolkit-for-intellij-releases"></a>Azure sada nástrojů IntelliJ verzích

Tento článek obsahuje informace o různých verzí a nejnovější aktualizace Azure sada nástrojů pro IntelliJ.

> [AZURE.NOTE] Je také Azure sada nástrojů pro integrovaném vývojovém zatmění prostředí. Další informace najdete v tématu [Azure nástrojů pro Eclipse].

### <a name="august-26-2016"></a>26 srpen 2016

Azure sada nástrojů pro IntelliJ – 2016 srpen vydání zahrnuje tyto výhody:

* **Vlastní JDK rozdělení**. Azure sada nástrojů pro IntelliJ teď podporuje určující nasazení libovolného verze JDK na váš web Appu Azure kontejner:
  - Kromě JDKs poskytovanou Azure můžete také z široké výběru zulština OpenJDK verzí k dispozici v Azure Azul systémy.
  - Můžete taky určit vlastní JDK rozdělení, pokud ho uložte jako soubor ZIP ke svému účtu úložiště.
* **Vylepšení do listu Exploreru Azure**:
  - Podpora správy virtuálního počítače pomocí nový model správce prostředků v Azure: můžete seznam, vytvářet a odstraňovat zdroje na základě Správce virtuálních počítačích aniž byste museli opustit integrovaném vývojovém prostředí.
  - Podpora správy účtu úložiště objektů blob správcem v Azure zdroje, které doplňuje existujících funkcí pro správu účtů "klasické" úložiště.
* **Ovladač Microsoft JDBC 6.0 pro systém SQL Server**. Poslední ovladači JDBC obsahuje tato aktualizace pro Microsoft SQL Server (verze 6.0), což je teď součástí knihovně, ve které můžete snadno přidat k projektům jazyka Java, čímž nahrazení starší verze.

### <a name="june-29-2016"></a>29 června 2016

Azure sada nástrojů pro IntelliJ – červen 2016 vydání zahrnuje tyto výhody:

* **Požadavek Java 8**. Azure sada nástrojů pro IntelliJ vyžaduje Java 8, i když tento požadavek se týká jen této sady - aplikace můžete dál používat všechny verze Java podporovaných službami Azure.
* **Podpora pro nejnovější JDKs Java**. Nejnovější verze Java JDKs teď podporovaných nástrojů Azure pro IntelliJ.
* **Podpora pro Azure SDK v2.9.1**. Nejnovější verzi Azure SDK je teď minimální předpokladem pro Azure sada nástrojů pro IntelliJ.
* **Integrované vzorky**. Azure sada nástrojů pro IntelliJ nabízí několik ukázkové aplikace aby vývojáři začít pracovat.
* **Integrace nástroj HDInsight**. V Azure HDInsight nástroje jsou teď součástí nástrojů Azure pro IntelliJ. Další informace najdete v tématu [HDInsight modul plug-in nástroje pro IntelliJ].
* **Vzdálené ladění Java webových aplikací**. Azure sada nástrojů pro IntelliJ nyní podporuje vzdálené ladění Java web apps na aplikaci služby Azure.

### <a name="april-12-2016"></a>12 duben 2016

Azure sada nástrojů pro IntelliJ – 2016 s dubnovou vydání zahrnuje tyto výhody:

* **Podpora pro Azure SDK v2.9.0**. Nejnovější verzi Azure SDK je teď minimální předpokladem pro Azure sada nástrojů pro IntelliJ.
* **Různé použitelnosti, rychlostí reakce nejnovější aktualizace související s podpory Azure v prohlížeči**. Celá řada optimalizaci výkonu v jak této sady informuje uživatele o s Azure výsledek ve více neodpovídá uživatelského rozhraní.
* **Možnost odstranit existující kontejner webové aplikace v Azure z v rámci IntelliJ**. Azure sada nástrojů pro IntelliJ umožňuje odstranit existující Azure Web kontejner aniž byste museli opustit IntelliJ.

## <a name="see-also"></a>Viz taky ##

Další informace o sadách Azure pro Java IDEs najdete v následujících tématech:

- [Azure sada nástrojů pro Eclipse]
  - [Instalace Azure sada nástrojů pro Eclipse]
  - [Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění]
  - [Co je nového v Azure sada nástrojů pro Eclipse]
- [Azure sada nástrojů pro IntelliJ]
  - [Instalace Azure sada nástrojů pro IntelliJ]
  - [Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ]
  - *Co je nového v Azure sada nástrojů pro IntelliJ (Tento článek)*

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure].

<!-- URL List -->

[Azure sada nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure sada nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v zatmění]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvořte Ahoj světě webovou aplikaci pro Azure v IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace Azure sada nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace Azure sada nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Co je nového v Azure sada nástrojů pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547

[Modul plug-in HDInsight nástroje pro IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
