# 🪟 Otwarte okno wyłącza urządzenie climate

> Automatyczne oszczędzanie energii: gdy okno zostaje otwarte, aktywny klimatyzator lub ogrzewanie przechodzi w stan wstrzymania, a po zamknięciu wraca do poprzedniego trybu.

## 🧾 Szybki podgląd
- **Typ**: `automation`
- **Steruje**: urządzeniem `climate` (HVAC)
- **Główne zadanie**: wyłączenie HVAC podczas wietrzenia i przywrócenie ustawień po zamknięciu okna

## 🔌 Wejścia blueprintu

### Wymagane
| Parametr | Typ | Domyślnie | Opis |
| --- | --- | --- | --- |
| `window_entity` | `binary_sensor` (`window`, `door`) | – | Czujnik wykrywający otwarte okno lub drzwi. |
| `climate_target` | `climate` | – | Urządzenie, które ma być włączane i wyłączane. |

### Parametry czasowe
| Parametr | Domyślnie | Opis |
| --- | --- | --- |
| `minimum_open_time` | 12 s | Minimalny czas otwarcia okna wymagany do aktywacji automatyzacji. |
| `minimum_close_time` | 12 s | Czas oczekiwania po zamknięciu okna przed ponownym uruchomieniem HVAC. |

### Akcje dodatkowe
| Parametr | Domyślnie | Opis |
| --- | --- | --- |
| `open_action` | brak | Sekwencja akcji wykonywana tuż po wyłączeniu HVAC (np. powiadomienie TTS). |
| `close_action` | brak | Sekwencja akcji przed ponownym uruchomieniem urządzenia po zamknięciu okna. |

## 🧠 Jak to działa
1. Okno musi pozostawać otwarte przez `minimum_open_time`. Dopiero wtedy trigger rozpoczyna sekwencję.
2. Blueprint sprawdza, czy urządzenie `climate` jest w jednym z obsługiwanych trybów (`cool`, `heat_cool`, `heat`, `automatic`, `auto`, `dry`, `fan_only`). Jeżeli tak, zapamiętuje bieżący tryb.
3. Urządzenie zostaje wyłączone (`climate.turn_off`). Jeśli zdefiniowano `open_action`, ta sekwencja jest wykonywana.
4. Automatyzacja czeka na zamknięcie okna. Po zmianie stanu na `off` odczekuje `minimum_close_time`, aby uniknąć gwałtownego przełączania.
5. Tuż przed ponownym uruchomieniem HVAC wykonywany jest `close_action` (jeśli podany).
6. Blueprint przywraca wcześniej zanotowany tryb urządzenia (`climate.set_hvac_mode`), dzięki czemu HVAC wraca do normalnej pracy.

## 💡 Przykładowe scenariusze
- Pauzowanie ogrzewania w salonie podczas wietrzenia zimą.
- Automatyczne wyłączanie klimatyzacji w sypialni, gdy zostaną uchylone drzwi balkonowe.
- Integracja z powiadomieniami głosowymi: informacja o wstrzymaniu ogrzewania po otwarciu okna.

## 🔗 Import blueprintu
`https://github.com/amuamurawski/homeassistant/blob/main/open_window_climate_off/open_window_climet_off.yaml`
