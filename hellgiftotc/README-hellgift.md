# OTClient - Redemption (15.25) — hellgift.online + wbudowany bot

To jest **OTClient (fork zimbadev/otc, „Redemption for 15x")** — prawdziwy OTClient
z **wbudowanym botem** (CaveBot / TargetBot / HealBot — folder `mods/game_bot`),
obsługujący **protokół 15.25**, przygotowany pod serwer **hellgift.online**.

## Co zostało ustawione
W pliku `init.lua` ustawiłem Twój serwer jako domyślny:
```lua
Servers_init = {
    ["http://hellgift.online/login.php"] = {
        ["port"] = 80,
        ["protocol"] = 1525,
        ["httpLogin"] = true
    },
}
```
- Logowanie: **HTTP** pod `http://hellgift.online/login.php` (httpLogin = true).
- Protokół **1525** (pod CrystalServer 15.25).
- Bot jest już w środku (`mods/game_bot`), włączasz go w grze skrótem **Ctrl + B**.

## ⚠️ To są ŹRÓDŁA (C++) — trzeba skompilować do client.exe
W tym repo **nie ma gotowego .exe** (autor nie publikuje binarek). Są dwie drogi:

### Droga A (ZALECANA — kompilacja w chmurze, nic nie instalujesz)
Dodałem workflow **`.github/workflows/build-windows.yml`**, który kompiluje klienta
na serwerach GitHub i daje gotowy pakiet do pobrania:
1. Zapisz projekt na GitHub (przycisk **Save → Save to GitHub**) jako repo **publiczne**.
   Klient siedzi w podfolderze `hellgiftotc/`, a workflow jest w roocie repo
   (`.github/workflows/build-windows.yml`) — więc uruchomi się automatycznie.
2. Wejdź w zakładkę **Actions** w repo → wybierz **„Build Windows Client (hellgift.online)"** → **Run workflow**
   (albo odpali się sam po pushu zmian w `hellgiftotc/`).
3. Poczekaj aż się zbuduje (pierwszy raz bywa długo — nawet 40–90 min, bo pobiera i buduje biblioteki vcpkg).
4. Po zielonym ✔ wejdź w dany run → sekcja **Artifacts** → pobierz
   **`otclient-redemption-hellgift-windows`** → w środku jest `otclient.exe` + pliki klienta.

### Droga B (lokalnie na Windows, dla zaawansowanych)
Visual Studio 2022 + vcpkg:
```
git clone --recursive https://github.com/microsoft/vcpkg
.\vcpkg\bootstrap-vcpkg.bat
cmake --preset windows-release
cmake --build build/windows-release --config Release
```
(szczegóły w README oryginalnego repo zimbadev/otc)

## ⚠️ Potrzebne assety 15.25 (sprite'y)
OTClient potrzebuje plików graficznych gry (sprite'y / `appearances` / `catalog-content.json`)
dla wersji 15.25 — w tym repo folder `data/things` jest **pusty**. Musisz wrzucić
assety 15.25 do:
```
data/things/1525/
```
Assety 15.25 masz m.in. z paczki klienta Cipsoftu (folder `assets/` z `appearances-*.dat`,
`sprites-*`, `catalog-content.json`) — to ten sam format protobuf. Bez assetów klient
się uruchomi, ale nie wyświetli grafiki gry.

## Jak odpalić po zbudowaniu (Windows)
1. Rozpakuj pobrany artefakt.
2. Upewnij się, że obok `otclient.exe` są foldery `data`, `modules`, `mods` oraz `init.lua`
   (workflow wrzuca je automatycznie) oraz assety w `data/things/1525/`.
3. Odpal **`otclient.exe`** → serwer **hellgift.online** jest już na liście → login i hasło → graj.
4. Bota włączasz skrótem **Ctrl + B**.

## Wymagania po stronie serwera (jak wcześniej)
- Domena **hellgift.online** odwieszona i wskazująca na IP serwera.
- Działający **`http://hellgift.online/login.php`** (AAC, np. MyAAC) zwracający listę światów.
- Otwarte porty **80** (login http), **7172** (gra), **7171** (login).
- `config.lua` serwera — w folderze `server-config/` (gotowy, baza do uzupełnienia u Ciebie).

---
**Uwaga uczciwościowa:** nie jestem w stanie skompilować tego ani odpalić w tym środowisku
(kontener Linux, bez Twojego MySQL/serwera i bez Windows toolchainu). Workflow oparłem na
oficjalnym presecie `windows-release` z repo — ale sam build w chmurze **nie był przeze mnie
uruchomiony**, więc pierwszy run zweryfikujesz u siebie. Jak build coś zgłosi — wklej log, poprawię.
