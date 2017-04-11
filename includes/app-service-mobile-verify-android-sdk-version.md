Z důvodu pokračující vývoj nemusí verze Android SDK nainstalovaných v Android Studio odpovídat verzi v kódu. Android SDK odkazuje v tomto kurzu je verze 23 nejnovější při psaní. Číslo verze může zvětšit a zobrazit novinek SDK, doporučujeme vám použít nejnovější dostupnou verzi.

Jsou dva příznaky neshodu verze:

1. Při vytváření nebo opětovné sestavení projektu, může se Gradle chybové zprávy jako "**se nepodařilo najít cílové Google Inc.:Google APIs:n**".

2. Standardní Android objekty v kódu, který by měla vyřešit podle `import` příkazy může být generování chybové zprávy.

Pokud některý z těchto zobrazují, nemusí verze Android SDK nainstalovaných v Android Studio odpovídat cílové SDK stažený projektu.  Pokud chcete ověřit verzi, proveďte následující změny:


1. V Android Studio, klikněte na **Nástroje** => **Android** => **SDK správce**. Pokud jste nenainstalovali nejnovější verze platformy SDK, potom klikněte na nainstalovat. Poznamenejte si číslo verze.

2. Na kartě Průzkumník projektu v části **Gradle skripty**, otevřete soubor **build.gradle (modeule: aplikace)**. Ujistěte se, že **compileSdkVersion** a **buildToolsVersion** jsou nastavené na nejnovější verzi SDK nainstalovaný. Značky může vypadat takto:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. Android Studio prohlížeč projektu klikněte pravým tlačítkem myši na uzel projektu, vyberte **Vlastnosti**a v levém sloupci vyberte **Android**. Ověřte, zda **Projekt vytvořit cílovou** stejnou verzi SDK jako **targetSdkVersion**.

4. V Android Studio soubor už použili můžete určit cílový SDK a minimální verze SDK, na rozdíl od v případě zatmění.
