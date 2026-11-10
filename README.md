# Planer Gabinetu (NewWay)

Planer gabinetu psychologicznego na tablet z rysikiem. Pacjenci mają tylko kody, notatki są szyfrowane na urządzeniu.

## Zasada bezpieczeństwa nr 1
**W tym repozytorium jest wyłącznie kod.** Dane pacjentów nigdy nie trafiają do GitHuba. Są zapisane w przeglądarce tabletu i zaszyfrowane Twoim hasłem (AES-GCM 256, klucz z hasła przez PBKDF2, 310 000 iteracji). Nie wrzucaj do repozytorium kopii zapasowych `.json`.

## Wdrożenie na GitHub Pages (około 5 minut)
1. Na github.com kliknij **New repository**, np. `planer-gabinetu`. Repozytorium może być publiczne, bo zawiera sam kod.
2. Kliknij **Add file → Upload files** i wgraj: `index.html`, `sw.js`, `manifest.webmanifest`, `icon.svg`, `icon-192.png`, `icon-512.png`, `README.md`.
3. Wejdź w **Settings → Pages → Branch: main / root → Save**.
4. Po około minucie aplikacja będzie dostępna pod adresem `https://<login>.github.io/planer-gabinetu/`.
5. Na tablecie otwórz ten adres w Chrome lub Safari i wybierz **Dodaj do ekranu głównego**. Planer będzie działał jak aplikacja, także offline.

## Pierwsze uruchomienie
- Ustal hasło (min. 10 znaków). **Nie da się go odzyskać.**
- W zakładce **Ustawienia** wpisz numer wpisu do Rejestru psychologów.
- Raz w tygodniu kliknij **Kopia**. Pobrany plik jest zaszyfrowany. Trzymaj go w dwóch miejscach.

## Kalendarz Google (opcjonalnie)
Do Kalendarza trafiają tylko: kod pacjenta, rodzaj sesji, miejsce i godzina.
1. Otwórz https://console.cloud.google.com i utwórz projekt „Planer Gabinetu”.
2. Wejdź w **APIs & Services → Library**, wyszukaj **Google Calendar API** i kliknij **Enable**.
3. W **OAuth consent screen** wybierz typ **External**, dodaj swój e-mail jako *Test user* i zakres `.../auth/calendar.events`.
4. W **Credentials** wybierz **Create credentials → OAuth client ID → Web application**.
   W polu *Authorized JavaScript origins* wpisz `https://<login>.github.io`.
5. Skopiuj **Client ID** do Ustawień planera i kliknij **Połącz z Google**.
6. Zalecane: utwórz w Kalendarzu Google osobny kalendarz „Gabinet”. Jego ID (Ustawienia kalendarza → *Integruj kalendarz*) wpisz w planerze.

Funkcje:
- **Wyślij przyszłe sesje** – zaplanowane sesje trafiają do Kalendarza.
- **Pobierz z Kalendarza** – planer dodaje wydarzenia, których tytuł zaczyna się od kodu (np. „P-014 sesja”), i aktualizuje terminy przesunięte w Google.

## Dobry Gabinet
Dobry Gabinet nie udostępnia publicznego API, dlatego wizyty przenosi się plikiem CSV:
Dobry Gabinet → eksport wizyt do CSV → Planer → Ustawienia → **Wczytaj plik CSV** → wskaż kolumny → przypisz kody.
Przypisanie „osoba z DG → kod” zapisuje się w zaszyfrowanym sejfie, więc kolejne importy rozpoznają pacjentów same.

## Dokumentacja (ustawa o zawodzie psychologa z 23.01.2026, art. 28)
- Dane tożsamości (imiona, nazwisko, data urodzenia, adres, PESEL, przedstawiciel ustawowy) są w zakładce **Sejf tożsamości**. Otwiera się ją hasłem na 5 minut.
- **Opis świadczenia** należy do dokumentacji. **Notatka robocza** jest krótkotrwała i wyłączona z udostępniania pacjentowi.
- Przycisk **Drukuj dokumentację (art. 28)** tworzy wydruk z danymi gabinetu, tożsamością, listą świadczeń, datą i miejscem na podpis.
- Termin przechowywania jest liczony automatycznie: 5 lat od końca roku zakończenia świadczeń, a u małoletnich 5 lat od końca roku uzyskania pełnoletności. Lista dokumentacji po terminie jest w Ustawieniach.

## Opinie psychologiczne z pomocą Claude
Karta pacjenta → **Opinie** → uzupełnij pola → **Kopiuj dla Claude**. Polecenie zawiera tylko kod pacjenta. Gotowy tekst wklej w pole „Tekst końcowy” i wydrukuj. Jeśli sejf jest otwarty, dane osobowe zostaną wstawione dopiero na wydruku.

## Pierwsza wizyta, wnioski i kody (moduł kliniczny – plik `kliniczne.js`)
- Karta pacjenta → **Pierwsza wizyta**: szablon „Dorosły” albo „Dziecko / nastolatek – wywiad z rodzicem”, z listami objawów, oceną bezpieczeństwa oraz skalami PHQ-9 i GAD-7. Brzmienie pozycji skal porównaj z oficjalną polską wersją na phqscreeners.com.
- Po prawej na bieżąco liczą się **wnioski wstępne (tylko dla Ciebie)**: wyniki skal, sygnały ostrzegawcze, hipotezy z kodami ICD-10 / ICD-11 / DSM-5-TR, kierunki pracy i proponowane cele. To hipotezy z reguł, nie rozpoznanie.
- **Drukuj zalecenia dla pacjenta**: lista do zrobienia bez nazw zaburzeń, z numerami pomocowymi (112, 800 70 2222, 116 123, 116 111, 800 120 002). Przed drukiem możesz odznaczyć punkty i dopisać własne.
- **Kopiuj dla Claude**: wywiad i wyniki z samym kodem pacjenta, do pogłębionej analizy według aktualnych badań.
- Zakładka **Rozpoznania ICD/DSM**: wyszukiwarka kodów i statusy: hipoteza / robocze / potwierdzone / wykluczone. Rozpoznania robocze i potwierdzone trafiają na wydruk dokumentacji.
- Kody: ICD-10 obejmuje rozdział V (F) i wybrane Z. ICD-11 obejmuje rozdział 06, a także 7A00/7A01, 8A05, QD85 i QE84; polskie nazwy są robocze. DSM-5-TR to nazwy rozpoznań z kodami ICD-10-CM, bez kryteriów.
