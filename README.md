# Próbny egzamin na broń (teoria)

Serwis do próbnych egzaminów z części teoretycznej na pozwolenie na broń: 20 losowych pytań, 30 minut, zalicza 18 poprawnych odpowiedzi. Ma też tryb nauki, powtórkę błędów i historię wyników. Po zdanym egzaminie wysyła wynik e-mailem do instruktora przez EmailJS.

Całość to jeden plik `index.html`. Nie potrzebuje serwera, bazy danych ani budowania.

## 1. Publikacja na GitHub Pages

1. Załóż na GitHubie nowe repozytorium, np. `egzamin-bron`, z widocznością **Public**. Darmowe GitHub Pages działa z repozytoriami publicznymi.
2. W repozytorium kliknij **Add file → Upload files**, przeciągnij `index.html` i `README.md`, a potem kliknij **Commit changes**.
3. Wejdź w **Settings → Pages**. W sekcji **Build and deployment** wybierz **Source: Deploy from a branch**, **Branch: main**, folder **/(root)** i kliknij **Save**.
4. Po 1–2 minutach strona będzie pod adresem `https://TWOJ-LOGIN.github.io/egzamin-bron/`.

## 2. Konfiguracja wysyłki maili (EmailJS)

1. Załóż darmowe konto na [emailjs.com](https://www.emailjs.com/).
2. **Email Services → Add New Service**: podłącz skrzynkę, z której mają wychodzić maile (np. Gmail). Zanotuj **Service ID**.
3. **Email Templates → Create New Template**. Ustaw:
   - **To Email:** `{{to_email}}`
   - **Subject:** `Egzamin próbny: {{status}} – {{student_name}}`
   - **Reply To:** `{{student_email}}`
   - **Treść:**

     ```
     Kursant: {{student_name}}
     E-mail kursanta: {{student_email}}
     Wynik: {{score}}/{{total}} ({{status}})
     Czas: {{duration_min}} min
     Data: {{date}}
     ```

   Zapisz szablon i zanotuj **Template ID**.
4. **Account → General**: skopiuj **Public Key**.
5. W `index.html` znajdź blok `CONFIG` (na początku skryptów) i wpisz swoje dane:

   ```js
   const CONFIG = {
     EMAILJS_PUBLIC_KEY: "twój public key",
     EMAILJS_SERVICE_ID: "service_xxxxxxx",
     EMAILJS_TEMPLATE_ID: "template_xxxxxxx",
     INSTRUCTOR_EMAIL: "twoj.adres@example.com",
     SEND_ON_FAIL: false
   };
   ```

   Zmiany zapiszesz bezpośrednio na GitHubie: otwórz plik, kliknij ikonę ołówka, a potem **Commit changes**.

Public Key EmailJS może być widoczny w kodzie strony, tak działa ta usługa. Nie wpisuj tam żadnych haseł. W ustawieniach konta EmailJS warto włączyć limit liczby wysyłek, żeby nikt nie zużył Twojego limitu. Darmowy plan ma miesięczny limit wiadomości, aktualny sprawdzisz na stronie EmailJS.

Dopóki EmailJS nie jest skonfigurowany albo gdy wysyłka się nie uda, kursant zobaczy Twój adres, gotowy tekst wiadomości i link otwierający jego program pocztowy.

## Jak to działa dla kursanta

1. Klika **Rozpocznij egzamin** i wpisuje imię i nazwisko (e-mail opcjonalnie). Dane zostają w jego przeglądarce na kolejne podejścia.
2. Rozwiązuje test. Po zakończeniu widzi wynik i listę błędów z podstawą prawną.
3. Jeśli zdał (co najmniej 18/20), strona sama wysyła Ci maila. Po zmianie `SEND_ON_FAIL` na `true` dostaniesz też wyniki niezdane.

## Ważne

- Test działa w przeglądarce kursanta, więc mail jest informacją, a nie dowodem zaliczenia. Osoba obeznana z technologią mogłaby wysłać fałszywy wynik. Wiążący sprawdzian najlepiej robić na zajęciach.
- Baza pytań (tablica `BANK` w `index.html`) to oficjalne 200 pytań KWP w Gdańsku. Poprawne odpowiedzi ustalono na podstawie ustawy o broni i amunicji (t.j. Dz.U. 2024 poz. 485 ze zm.), rozporządzeń wykonawczych i Kodeksu karnego. Pytania, przy których odpowiedź wymaga potwierdzenia, są w pliku `do-weryfikacji.txt`. Każde pytanie ma format:
  `["treść", ["A", "B", "C"], indeks_poprawnej (0=A, 1=B, 2=C), "podstawa prawna", "poprawna odpowiedź", "dział", nr_w_bazie]`
