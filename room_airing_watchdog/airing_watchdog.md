# Strażnik wietrzenia pokoju – dokumentacja

## Przeznaczenie
- Automatyzacja pilnuje, aby okno lub drzwi nie pozostawały zbyt długo otwarte przy dużej różnicy temperatur między wnętrzem a zewnątrz.
- Może wstrzymać alarm, jeżeli w pomieszczeniu nadal przekroczony jest zadany poziom CO₂ (wymaga wskazania czujnika).
- Wysyła powiadomienia na wskazany kanał i opcjonalnie uruchamia dodatkowe akcje po alarmie.

## Wymagane wejścia
- `window_contact` – binarny czujnik okna/drzwi (klasa urządzenia `door`, `window` lub `opening`).
- `room_temperature` – czujnik temperatury wewnętrznej (sensor z klasą `temperature`).
- `outdoor_temperature` – czujnik temperatury zewnętrznej (również klasa `temperature`).
- `notify_target` – usługa powiadomień, np. `notify.mobile_app_moj_telefon`.

## Opcjonalne elementy
- `co2_sensor` – dodatkowy czujnik CO₂; umożliwia wstrzymanie alarmu do czasu spadku stężenia poniżej progu.
- `follow_up_entity` – automatyzacja lub skrypt do uruchomienia po wysłaniu alarmu.
- `repeat_reminder_minutes` – interwał kolejnych przypomnień; ustaw `0`, aby je wyłączyć.
- `use_persistent_notification` – tworzy także powiadomienia w interfejsie Home Assistant.

## Kluczowe parametry czasowe i progi
- `temp_diff_threshold` (domyślnie 6 °C) – minimalna różnica temperatur, od której blueprint zaczyna odliczać czas.
- `base_allowed_minutes` (domyślnie 15 min) – początkowy limit czasu wietrzenia po przekroczeniu progu.
- `per_degree_penalty` (domyślnie 2 min/°C) – skracanie limitu za każdy dodatkowy stopień powyżej progu.
- `min_allowed_minutes` (domyślnie 5 min) – gwarantowany minimalny czas, poniżej którego limit nie spadnie.
- `co2_target` (domyślnie 900 ppm) – docelowy poziom CO₂; poniżej tej wartości alarm nie zostanie wyzwolony.
- `co2_grace_minutes` (domyślnie 10 min) – dodatkowy czas na wietrzenie, gdy CO₂ wciąż jest powyżej celu.

Limit czasu dla otwartego okna obliczany jest według wzoru:

```text
allowed_minutes = max(
    min_allowed_minutes,
    base_allowed_minutes - max(0, diff - temp_diff_threshold) * per_degree_penalty
)
```

gdzie `diff` to bezwzględna różnica między temperaturą w pomieszczeniu i na zewnątrz.

## Logika działania
1. Automatyzacja uruchamia się, gdy okno/drzwi pozostają otwarte dłużej niż 60 s.
2. Jeśli różnica temperatur jest mniejsza niż `temp_diff_threshold`, wietrzenie nie jest nadzorowane.
3. Po przekroczeniu progu rozpoczyna się odliczanie limitu czasu (z uwzględnieniem ewentualnych `co2_grace_minutes`).
4. Blueprint oczekuje na jedno z dwóch wydarzeń:
   - okno zostanie zamknięte;
   - wskazany czujnik CO₂ spadnie do wartości `co2_target` lub niższej.
5. Jeżeli limit upłynie i okno nadal jest otwarte, wysyłane jest powiadomienie z informacją o pokoju (na podstawie `area_name`), różnicy temperatur i przekroczeniu czasu.
6. Podczas otwartego okna mogą być wysyłane cykliczne przypomnienia (`repeat_reminder_minutes`), a po alarmie można uruchomić dodatkową automatyzację/skrypt (`follow_up_entity`).
7. Przy włączonej opcji `use_persistent_notification` w interfejsie HA pojawiają się dwa powiadomienia: pierwsze przy alarmie i kolejne po zamknięciu okna.

## Sposoby wykorzystania
- Przypomnienie o zamknięciu okna zimą, aby nie dogrzewać pomieszczenia.
- Kontrola wietrzenia w pokoju dziecięcym lub sypialni, z dodatkowym warunkiem obniżenia CO₂.
- Integracja z ogrzewaniem: `follow_up_entity` może wstrzymać wznowienie ogrzewania do czasu zamknięcia okna.

## Import blueprintu
Link do importu w Home Assistant:  
`https://raw.githubusercontent.com/amuamurawski/homeassistant/main/room_airing_watchdog/airing_watchdog.yaml`
