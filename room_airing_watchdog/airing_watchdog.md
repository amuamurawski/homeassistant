# 🌬️ Strażnik wietrzenia pokoju

> Inteligentny strażnik pilnuje, aby otwarte okno nie wychłodziło domu. Gdy różnica temperatur jest zbyt duża, przypomni o zamknięciu okna i może uruchomić dodatkowe akcje.

## 🧾 Szybki podgląd
- **Typ**: `automation`
- **Nadzoruje**: czas otwartego okna przy dużej różnicy temperatur
- **Powiadomienia**: kanał `notify`, opcjonalnie powiadomienia trwałe i dodatkowe akcje
- **Wymaga**: czujnika okna, temperatury wewnętrznej i zewnętrznej

## 🔌 Wejścia blueprintu

### Wymagane
| Parametr | Typ | Domyślnie | Opis |
| --- | --- | --- | --- |
| `window_contact` | `binary_sensor` (`door`, `window`, `opening`) | – | Czujnik otwarcia okna lub drzwi. |
| `room_temperature` | `sensor` (`temperature`) | – | Czujnik temperatury w pomieszczeniu. |
| `outdoor_temperature` | `sensor` (`temperature`) | – | Czujnik temperatury na zewnątrz. |
| `notify_target` | `notify` target | – | Kanał powiadomień, np. `notify.mobile_app_moj_telefon`. |

### Opcjonalne
| Parametr | Typ | Domyślnie | Opis |
| --- | --- | --- | --- |
| `open_follow_up_action` | akcja | brak | Sekwencja akcji wykonywana razem z powiadomieniem o zbyt długim wietrzeniu (np. komunikat TTS, pauza ogrzewania). |
| `close_follow_up_action` | akcja | brak | Sekwencja akcji wykonywana po wykryciu zamknięcia okna (np. wznowienie ogrzewania, komunikat głosowy). |
| `repeat_reminder_minutes` | liczba | 10 | Interwał kolejnych przypomnień; `0` wyłącza powtórki. |
| `use_persistent_notification` | bool | `true` | Tworzy powiadomienie w interfejsie HA przy alarmie i po zamknięciu okna. |

### Parametry progów i czasu
| Parametr | Domyślnie | Opis |
| --- | --- | --- |
| `temp_diff_threshold` | 6 °C | Minimalna różnica temperatur, od której zaczyna się kontrola. |
| `base_allowed_minutes` | 15 min | Czas bazowy na wietrzenie po przekroczeniu progu. |
| `per_degree_penalty` | 2 min/°C | Skracanie limitu za każdy stopień powyżej progu. |
| `min_allowed_minutes` | 5 min | Najmniejszy możliwy limit czasu. |
## 🧠 Jak to działa
1. Okno musi pozostawać otwarte co najmniej 60 sekund, aby automatyzacja ruszyła.
2. Jeżeli różnica temperatur (`diff`) nie przekracza `temp_diff_threshold`, strażnik pozostaje w spoczynku.
3. Gdy próg jest przekroczony, obliczany jest limit wietrzenia:

   ```text
   allowed_minutes = max(
       min_allowed_minutes,
       base_allowed_minutes - max(0, diff - temp_diff_threshold) * per_degree_penalty
   )
   ```

4. Blueprint czeka na zamknięcie okna przez `allowed_minutes` (liczone w sekundach).
5. Po przekroczeniu limitu wysyłane jest powiadomienie z informacją o pokoju (`area_name`), różnicy temperatur oraz czasie otwarcia.
6. Dopóki okno pozostaje otwarte i ustawiono `repeat_reminder_minutes`, kolejne przypomnienia trafiają do wskazanego kanału.
7. `open_follow_up_action` pozwala wykonać dodatkowe akcje razem z alarmem (np. powiadomienie głosowe, sterowanie ogrzewaniem), `close_follow_up_action` zadziała po zamknięciu okna, a `use_persistent_notification` dodaje powiadomienia w interfejsie HA.

## 💡 Przykładowe scenariusze
- Zimowe przypomnienia o zamknięciu okna, aby nie dogrzewać niepotrzebnie pomieszczenia.
- Integracja z ogrzewaniem lub klimatyzacją poprzez `open_follow_up_action`/`close_follow_up_action`, np. pauza pieca podczas wietrzenia i automatyczne wznowienie po zamknięciu okna.

## 🔗 Import blueprintu
`https://raw.githubusercontent.com/amuamurawski/homeassistant/main/room_airing_watchdog/airing_watchdog.yaml`
