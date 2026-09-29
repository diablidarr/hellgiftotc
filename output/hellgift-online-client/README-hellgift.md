# OTClient (GameClient 15.25) — hellgift.online

Gotowa paczka klienta gry oparta o **zimbadev/gameclient** (protokół **15.25**,
zgodny z **CrystalServer 15.25**), przygotowana tak, aby łączyć się z serwerem
**hellgift.online**. Zawiera oryginalny, skompilowany `client.exe` z **wbudowanym
botem** — nic nie trzeba kompilować.

## Co zostało zmienione
W pliku `bin/client.exe` podmieniono domyślny adres logowania z lokalnego
`127.0.0.1` na **hellgift.online** (edycja w miejscu, rozmiar pliku bez zmian):

```
loginWebService  = http://hellgift.online/login.php
clientWebService = http://hellgift.online/login.php
```

- Sposób logowania: **HTTP login** (endpoint `http://hellgift.online/login.php`) — bez SSL.
- Port gry (7172) klient otrzymuje automatycznie z odpowiedzi `login.php`.
- Klucz **RSA**: domyślny OpenTibia (bez zmian, zgodny z CrystalServer).
- **Wbudowany bot**: bez zmian — działa jak w oryginalnym release.

## Jak uruchomić (Windows)
1. Rozpakuj cały folder w dowolne miejsce (np. `C:\hellgift`).
2. W folderze `bin\` rozpakuj **`Qt6WebEngineCore.rar`** — inaczej klient nie wystartuje
   poprawnie (wypakuj `Qt6WebEngineCore.dll` obok pozostałych plików `bin\`).
3. Pliki w folderze `cache\` ustaw jako **tylko do odczytu (read-only)** —
   dzięki temu poprawnie działają systemy pobierane z serwera.
4. Uruchom **`bin\client.exe`**.
5. W oknie logowania serwer jest już ustawiony na **hellgift.online** (HTTP).
   Podaj login i hasło do swojego konta i kliknij **Login**.
6. Wybierz postać i wejdź do gry. Bot jest dostępny w kliencie.

## Wymagania po stronie serwera hellgift.online
Aby logowanie działało, serwer musi udostępniać endpoint **`login.php`** pod
`http://hellgift.online/login.php` (standardowy login web service dla klienta 15.x —
np. z panelu AAC typu myaac/znote skonfigurowanego pod CrystalServer). Endpoint po
poprawnym logowaniu zwraca listę światów wraz z IP i portem gry (domyślnie **7172**).

Domyślne porty CrystalServer: **login 7171**, **gra 7172** (patrz dołączony `config.lua`).

## Uwagi
- Podmiana adresu w `client.exe` unieważnia podpis cyfrowy pliku (to normalne dla
  klientów OTS). Klient nadal działa; ewentualny SmartScreen/antywirus może pytać —
  zezwól na uruchomienie.
- Przejście na **HTTPS** będzie możliwe, gdy domena będzie miała certyfikat SSL —
  wtedy adres zmieni się na `https://hellgift.online/login.php` (Faza 2).
- Jeśli hellgift.online używa innego adresu endpointu logowania (np. inna ścieżka
  niż `/login.php` albo inny port), trzeba będzie ponownie podmienić adres w
  `client.exe` — daj znać, dostosujemy.

## Plik konfiguracyjny serwera
Obok tej paczki znajduje się przygotowany **`server-config/config.lua`** (patrz jego
`README.md`) — do wgrania na serwer CrystalServer.
