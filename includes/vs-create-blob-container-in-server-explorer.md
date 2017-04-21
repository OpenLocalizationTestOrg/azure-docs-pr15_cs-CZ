Azure úložiště fronty můžete vytvořit pomocí aplikace Visual Studio **Průzkumník serveru**.

![Objekty BLOB Průzkumníka serveru][Image1]

1. Na **kartě zobrazení** vyberte **Průzkumník serveru**.
2. V okně Průzkumník serveru rozbalte uzel **Azure** pro vaše předplatné, rozbalte uzel **úložiště** a uzel úložiště účtu, který jste zadali v úložišti Azure připojené služby.
3. Vyberte uzel **fronty** a zvolte **Vytvořit fronty** v místní nabídce.
4. Zadejte název pro kontejner a klikněte na **OK**.   

Ve výchozím nastavení nového kontejneru je soukromá a třeba zadat kód úložiště přístup ke stažení objektů BLOB tento kontejner. Pokud budete chtít zveřejnění soubory v kontejneru, vyberte kontejner v **Průzkumníku serveru** a stiskněte `F4` zobrazte okno **Vlastnosti** . Nastavte **veřejný přístup pro čtení** na **objektů Blob**. Každý, kdo na Internetu zobrazit objektů BLOB v kontejneru veřejné, ale můžete upravit nebo odstranit jenom v případě, že máte odpovídající přístupové klávesy.


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png