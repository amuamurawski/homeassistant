# Otwarte okno wyłącza urządzenie climate – dokumentacja

## Przeznaczenie
- Automatyzacja wyłącza aktywny system grzewczy lub chłodzący (`climate`) po wykryciu otwartego okna.
- Przywraca poprzedni tryb HVAC, gdy okno zostanie zamknięte i upłynie zadany czas bezczynności.
- Opcjonalnie uruchamia dodatkowe akcje przy otwarciu i zamknięciu okna (np. komunikat TTS, sterowanie roletą).

## Wymagane wejścia
- `window_entity` – binarny czujnik okna/drzwi (klasa urządzenia `window` lub `door`).
- `climate_target` – urządzenie typu `climate`, które ma być włączane/wyłączane.

## Parametry czasowe
- `minimum_open_time` – minimalny czas (s), przez jaki okno musi pozostawać otwarte, zanim automatyzacja wyłączy urządzenie (domyślnie 12 s).
- `minimum_close_time` – opóźnienie (s) po zamknięciu okna przed ponownym uruchomieniem urządzenia (domyślnie 12 s).

## Akcje opcjonalne
- `open_action` – dodatkowe działania wykonywane tuż po otwarciu i wyłączeniu urządzenia (domyślnie brak).
- `close_action` – działania wykonywane przed ponownym włączeniem urządzenia po zamknięciu okna (domyślnie brak).

## Logika działania
1. Trigger uruchamia się po zmianie sensora okna na `on` i utrzymaniu tego stanu przez `minimum_open_time`.
2. Blueprint sprawdza, czy urządzenie `climate` nie jest już wyłączone (`off`). Jeśli jest aktywne w jednym z obsługiwanych trybów (`cool`, `heat_cool`, `heat`, `automatic`, `auto`, `dry`, `fan_only`), zapisuje bieżący tryb.
3. Urządzenie climate zostaje wyłączone (`climate.turn_off`), a następnie opcjonalnie wykonywany jest `open_action`.
4. Automatyzacja czeka, aż sensor okna wróci do stanu `off`, po czym odczekuje `minimum_close_time`.
5. Wykonuje `close_action` (jeżeli zdefiniowano) i przywraca poprzedni tryb HVAC (`climate.set_hvac_mode`) odpowiadający wcześniej wykrytemu stanowi.

## Przykładowe zastosowania
- Automatyczne wyłączanie ogrzewania podczas wietrzenia pomieszczenia.
- Kontrola klimatyzacji w biurze, aby uniknąć strat energii przy otwartych oknach.
- Integracja z innymi akcjami, np. sterowanie wentylacją nawiewną, powiadomienia głosowe.

## Import blueprintu
`https://github.com/amuamurawski/homeassistant/blob/main/open_window_climate_off/open_window_climet_off.yaml`
