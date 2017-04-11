**Cíl C**: 

1. Na počítači Mac otevřete _QSTodoListViewController.m_ v Xcode a přidejte následujícího postupu. Přejděte do _google_ _microsoftaccount_, _twitter_, _facebook_nebo _windowsazureactivedirectory_ Pokud nepoužíváte Google jako poskytovatele identity. Pokud používáte Facebook, [budete muset povolených domén Facebook v aplikaci](https://developers.facebook.com/docs/ios/ios9#whitelist).

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. Nahrazení `[self refresh]` v `viewDidLoad` v _QSTodoListViewController.m_ následující:

            [self loginAndGetData];

3. Stisknutím klávesy _Spustit_ spusťte aplikaci a pak se přihlaste. Pokud jste se přihlásili, je třeba zobrazit seznam úkol a aktualizovat.

**Swift**:

1. Na počítači Mac otevřete _ToDoTableViewController.swift_ v Xcode a přidejte následujícího postupu. Přejděte do _google_ _microsoftaccount_, _twitter_, _facebook_nebo _windowsazureactivedirectory_ Pokud nepoužíváte Google jako poskytovatele identity. Pokud používáte Facebook, [budete muset Facebook povolených domén v aplikaci](https://developers.facebook.com/docs/ios/ios9#whitelist).
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. Odstranit řádky `self.refreshControl?.beginRefreshing()` a `self.onRefresh(self.refreshControl)` na konci `viewDidLoad()` v _ToDoTableViewController.swift_. Přidání hovoru do `loginAndGetData()` na jejich místo:

            loginAndGetData()

3. Stisknutím klávesy _Spustit_ spusťte aplikaci a pak se přihlaste. Pokud jste se přihlásili, je třeba zobrazit seznam úkol a aktualizovat.
