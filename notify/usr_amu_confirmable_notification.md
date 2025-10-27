# 🔔 Potwierdzalne powiadomienie

> Interaktywne powiadomienia w aplikacji Home Assistant, które wymagają świadomego potwierdzenia lub odrzucenia, zanim uruchomią kolejne akcje.

## 🧾 Szybki podgląd
- **Typ**: `script`
- **Obsługiwane urządzenia**: każde z integracją `mobile_app`
- **Przeznaczenie**: sytuacje, w których użytkownik musi zatwierdzić akcję (np. wyłączenie alarmu, otwieranie bramy)

## 🔌 Wejścia skryptu

### Wymagane
| Parametr | Typ | Domyślnie | Opis |
| --- | --- | --- | --- |
| `notify_device` | `device` (`mobile_app`) | – | Urządzenie mobilne, które otrzyma powiadomienie. |
| `message` | tekst | – | Treść komunikatu wyświetlanego użytkownikowi. |

### Opcjonalne
| Parametr | Domyślnie | Opis |
| --- | --- | --- |
| `title` | pusty | Tytuł powiadomienia. |
| `confirm_text` | `Confirm` | Tekst przycisku potwierdzającego. |
| `confirm_action` | brak | Akcje wykonywane po potwierdzeniu. |
| `dismiss_text` | `Dismiss` | Tekst przycisku odrzucającego. |
| `dismiss_action` | brak | Akcje wykonywane po odrzuceniu. |
| `timeout_seconds` | 60 | Limit czasu na odpowiedź; po jego przekroczeniu skrypt kończy działanie. |

## 🧠 Jak to działa
1. Skrypt generuje unikalne identyfikatory akcji na podstawie `context.id`.
2. Wysyła powiadomienie (`notify.notify`) z dwoma przyciskami: potwierdzenia i odrzucenia.
3. Oczekuje na zdarzenie `mobile_app_notification_action` z wybraną akcją lub na upłynięcie limitu czasu (`timeout_seconds`).
4. W zależności od odpowiedzi użytkownika uruchamia `confirm_action`, `dismiss_action`, albo kończy się bez efektu po przekroczeniu czasu.

## 💡 Przykłady użycia
- Potwierdzenie wyłączenia systemu alarmowego.
- Pytanie, czy rozpocząć nagrzewanie samochodu/sauny przed powrotem do domu.
- Decyzja o otwarciu bramy wjazdowej z zachowaniem potwierdzenia.

## 🔗 Import blueprintu
`https://github.com/amuamurawski/homeassistant/blob/main/notify/usr_amu_confirmable_notification.yaml`
