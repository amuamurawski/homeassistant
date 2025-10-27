# Blueprinty do Home Assistant

To repozytorium zawiera moje  **blueprinty**, które wykorzystuję w automatyzacjach w [Home Assistant](https://www.home-assistant.io/).
Znajdziesz tu gotowe szablony automatyzacji, które możesz łatwo zaimportować i dostosować do swojej instalacji.

## 📂 Zawartość

Blueprinty są podzielone według typu (np. czujniki, światła, harmonogramy) i znajdują się w odpowiednich katalogach:

- `automation/` – automatyzacje np. dla czujników ruchu, powiadomień, harmonogramów
- `script/` – blueprinty dla skryptów
- `device_automation/` – automatyzacje powiązane z konkretnymi urządzeniami

Blueprinty dostępne w repozytorium:

- [Strażnik wietrzenia pokoju](room_airing_watchdog/airing_watchdog.md) – monitoruje długość wietrzenia przy dużej różnicy temperatur i przypomina o zamknięciu okna.
- [Otwarte okno wyłącza climate](open_window_climate_off/open_window_climet_off.md) – wyłącza aktywne urządzenie climate podczas wietrzenia i przywraca poprzedni tryb po zamknięciu okna.
- [Potwierdzalne powiadomienie](notify/usr_amu_confirmable_notification.md) – wysyła interaktywne powiadomienie z przyciskami potwierdzenia i odrzucenia.

## 🛠 Jak używać

1. Przejdź do **Ustawienia → Automatyzacje i sceny → Blueprinty** w Home Assistant.
2. Kliknij **Importuj blueprinta**.
3. Wklej link do blueprinta z tego repozytorium
4. Wypełnij wymagane pola i zapisz automatyzację.

## 📢 Uwagi

- Blueprinty są przetestowane w mojej domowej instalacji, ale mogą wymagać drobnych modyfikacji w zależności od Twojej konfiguracji.
- Zachęcam do ich przeglądania, modyfikowania i zgłaszania uwag lub propozycji!

## 📄 Licencja

Ten projekt udostępniam na licencji MIT. Możesz go dowolnie kopiować, modyfikować i wykorzystywać.

---

Jeśli masz pytania lub chcesz się podzielić własnymi pomysłami – zapraszam do kontaktu lub wystawienia Pull Requesta.
