# OTClient dla CrystalServer 15.25 — gotowa paczka pod hellgift.online

Przygotowanie kompletnej, gotowej do uruchomienia paczki klienta OTClient (zimbadev/gameclient, protokół 15.25) łączącej się z serwerem hellgift.online przez HTTP.
Paczka zawiera oryginalny, skompilowany client.exe z wbudowanym botem — wystarczy podmieniona konfiguracja, bez kompilacji. Dodatkowo dołączony jest przygotowany config.lua serwera.

## Dla kogo
Dla właściciela/administratora serwera OTS "hellgift.online" opartego na CrystalServer 15.25, który chce mieć (i rozdać graczom) gotowego klienta na Windows z wbudowanym botem, łączącego się od razu z jego serwerem.

## Co zostanie zrobione i jak to działa
- Pobranie oficjalnego release'u klienta z GitHub (zimbadev/gameclient 15.25) zawierającego skompilowany `client.exe` z wbudowanym botem oraz pliki `.lua`.
- Pobranie CrystalServer (zimbadev/crystalserver) w celu przygotowania pliku konfiguracyjnego serwera zgodnego z klientem.
- Ustawienie hellgift.online jako domyślnego serwera klienta:
  - Sposób logowania: HTTP login (endpoint `http://hellgift.online/login.php`) — bez SSL.
  - Domyślne porty CrystalServer: login 7171, gra 7172.
  - Domyślny klucz OpenTibia RSA (bez zmian — zgodny z serwerem).
- Wpisanie hellgift.online jako gotowej pozycji na liście serwerów w oknie logowania oraz jako wartości domyślnej, aby gracz nie musiał nic wpisywać; usunięcie lokalnego 127.0.0.1 z paczki.
- Zachowanie wbudowanego bota bez zmian (moduł bota już jest w kliencie).
- Przygotowanie `config.lua` serwera pod hellgift.online (profilaktycznie): ustawienie adresu/hostu serwera oraz adresów URL (m.in. `url`, `coinImagesURL`) na `http://hellgift.online/...`, przy zachowaniu domyślnych portów 7171/7172 i pozostałych domyślnych ustawień CrystalServer. Dane wrażliwe (MySQL itd.) pozostają jako placeholdery do uzupełnienia przez administratora.
- Spakowanie całości do kompletnego folderu gotowego do uruchomienia na Windows + krótkie README (jak odpalić, gdzie ustawić adres, uwagi z release'u np. rozpakowanie Qt6WebEngineCore, folder cache jako read-only) oraz osobno dołączony `config.lua` serwera.

## Przepływ użytkownika (gracza)
1. Gracz rozpakowuje paczkę i uruchamia client.exe na Windows.
2. W oknie logowania hellgift.online jest już wybrany jako serwer (adres, porty, sposób logowania ustawione, HTTP).
3. Gracz podaje login i hasło do konta i klika "Login".
4. Klient łączy się z hellgift.online, pokazuje listę postaci, gracz wchodzi do gry.
5. Wbudowany bot jest dostępny w kliencie tak jak w oryginalnym release.

## Wygląd / odczucie
Standardowy ekran logowania OTClient 15.25 z paczki zimbadev — z tą różnicą, że serwerem domyślnym jest "hellgift.online" zamiast lokalnego adresu. Bez zmian graficznych; celem jest poprawne połączenie i działający bot, nie przeróbka wyglądu.

## Fazy realizacji

### Faza 1 (realizowana teraz) — MVP
- Pobranie release'u klienta (z client.exe i botem) oraz repo serwera.
- Modyfikacja plików konfiguracyjnych klienta (m.in. `init.lua` i moduł ekranu logowania) tak, aby domyślnie łączył się z hellgift.online: HTTP login `http://hellgift.online/login.php`, porty 7171/7172, domyślny RSA.
- Przygotowanie `config.lua` serwera pod hellgift.online (adres/hosty/URL) z zachowaniem domyślnych portów i placeholderów dla danych wrażliwych.
- Zbudowanie kompletnego, spakowanego folderu klienta gotowego do uruchomienia na Windows + README + dołączony config.lua serwera.

### Faza 2 (później)
- Dostrojenie pod realny endpoint logowania hellgift.online, jeśli różni się od domyślnego (inny adres login.php / API, inne porty).
- Przejście na HTTPS/SSL, gdy domena będzie miała certyfikat.
- Ewentualne ustawienie własnego klucza RSA po stronie klienta, jeśli serwer go używa.

### Faza 3 (później / poza tym środowiskiem)
- Świeża kompilacja własnego client.exe (vcpkg/CMake) — jako opcja, gdyby potrzebna była własna binarka; instrukcja + skrypt do uruchomienia na maszynie Windows.

## Założenia
- Wybrany wariant dostarczenia: gotowa, spakowana paczka klienta (oparta o oficjalny client.exe z release'u, z wbudowanym botem) + przygotowany config.lua serwera. Kompilacja od zera nie jest wykonywana — nie jest potrzebna, bo bot jest już w binarce, a adres serwera czytany jest z plików `.lua` przy starcie.
- Klient to `zimbadev/gameclient` 15.25 (zgodny z CrystalServer 15.25).
- Sposób logowania: HTTP login pod `http://hellgift.online/login.php` (HTTP, bez SSL — zgodnie z decyzją). Jeśli hellgift.online używa innego adresu/endpointu — poprawimy w Fazie 2.
- Porty domyślne CrystalServer: login 7171, gra 7172.
- Klucz RSA: domyślny OpenTibia (klient i serwer zgodne, bez zmian).
- `config.lua` serwera przygotowany profilaktycznie pod hellgift.online; dane MySQL i inne wrażliwe pozostają jako placeholdery do uzupełnienia przez administratora.
- Modyfikowane są pliki klienta; dodatkowo dostarczany jest przygotowany config.lua serwera (do samodzielnego wgrania na serwer).
- Ograniczenie środowiska: to kontener Linux bez windowsowego toolchainu, więc nie kompiluję ani nie uruchamiam tu client.exe/serwera; dostarczam gotowe pliki i paczkę, którą uruchamiasz na Windows/serwerze.
