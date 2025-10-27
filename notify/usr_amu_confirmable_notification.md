# Potwierdzalne powiadomienie – dokumentacja

## Przeznaczenie
- Skrypt wysyła do wybranego urządzenia mobilnego interaktywne powiadomienie Home Assistant.
- Odbiorca może potwierdzić lub odrzucić komunikat; każda odpowiedź uruchamia oddzielnie zdefiniowane akcje.
- Rozwiązanie sprawdza się przy scenariuszach wymagających potwierdzenia użytkownika, np. przed uruchomieniem alarmu czy otwarciem bramy.

## Wymagane wejścia
- `notify_device` – urządzenie z integracją `mobile_app`, które odbierze powiadomienie.
- `message` – treść komunikatu wyświetlanego w powiadomieniu.

## Opcjonalne parametry
- `title` – tytuł powiadomienia (domyślnie pusty).
- `confirm_text` – etykieta przycisku potwierdzenia (domyślnie `Confirm`).
- `confirm_action` – sekwencja akcji wykonywana po potwierdzeniu.
- `dismiss_text` – etykieta przycisku odrzucenia (domyślnie `Dismiss`).
- `dismiss_action` – sekwencja akcji wykonywana po odrzuceniu.
- `timeout_seconds` – maksymalny czas oczekiwania na odpowiedź (domyślnie 60 s).

## Logika działania
1. Skrypt generuje unikalne identyfikatory przycisków opartych na `context.id`.
2. Wysyła powiadomienie `notify.notify` z dwoma akcjami (potwierdzenie i odrzucenie).
3. Oczekuje na zdarzenie `mobile_app_notification_action` z jednego z przycisków albo na upłynięcie limitu czasu.
4. W zależności od otrzymanej odpowiedzi uruchamia `confirm_action` lub `dismiss_action`. Jeśli użytkownik nie odpowie, skrypt kończy działanie bez dodatkowych akcji.

## Przykładowe zastosowania
- Potwierdzenie wyłączenia alarmu lub uzbrojenia systemu bezpieczeństwa.
- Zapytanie, czy włączyć ogrzewanie/klimatyzację przed powrotem do domu.
- Decyzja o otwarciu bramy wjazdowej lub furtki.

## Import blueprintu
`https://github.com/amuamurawski/homeassistant/blob/main/notify/usr_amu_confirmable_notification.yaml`
