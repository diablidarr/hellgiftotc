# CrystalServer 15.25 — config.lua dla hellgift.online

Ten `config.lua` powstał z oficjalnego `config.lua.dist` z repo
**zimbadev/crystalserver** i został przygotowany pod domenę **hellgift.online**.

## Co zostało ustawione
| Klucz | Wartość |
|-------|---------|
| `ip` | `hellgift.online` |
| `serverName` | `hellgift.online` |
| `coinImagesURL` | `http://hellgift.online/images/store/` |
| `url` | `http://hellgift.online/` |
| `loginProtocolPort` | `7171` (domyślnie, bez zmian) |
| `gameProtocolPort` | `7172` (domyślnie, bez zmian) |

Pozostałe ustawienia pozostawiono na domyślnych wartościach CrystalServer.

## DO UZUPEŁNIENIA PRZEZ ADMINA (dane wrażliwe — placeholdery)
W sekcji **MySQL** ustaw własne dane bazy:

```lua
mysqlHost = "127.0.0.1"      -- host bazy (zwykle localhost na serwerze gry)
mysqlUser = "root"           -- <- zmień
mysqlPass = "root"           -- <- zmień na prawdziwe hasło
mysqlDatabase = "crystalserver"
mysqlPort = 3306
```

Zalecane też ustawienie `ownerName` / `ownerEmail`.

## Instalacja
1. Skopiuj ten plik do katalogu głównego serwera CrystalServer jako **`config.lua`**.
2. Uzupełnij dane MySQL (patrz wyżej) i zaimportuj `schema.sql` do bazy.
3. Upewnij się, że porty **7171** i **7172** są otwarte/forwardowane na hoście
   hellgift.online, oraz że działa login web service (`http://hellgift.online/login.php`).
4. Uruchom serwer.

## Uwaga o `ip = "hellgift.online"`
CrystalServer rozwiązuje tę wartość do adresu IP przy starcie. Jeśli w Twoim
środowisku wystąpi problem z rozwiązaniem nazwy, wpisz tu bezpośrednio publiczne
**IP** serwera zamiast domeny.
