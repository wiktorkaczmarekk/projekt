## CI/CD - GitHub Actions

Ten projekt korzysta z GitHub Actions do automatyzacji testów, dbania o jakość kodu oraz wdrażania aplikacji na serwer. 

### Dostępne workflowy:
* **Django CI (`ci.yml`)**: Uruchamia się przy push/pull request na gałąź `main`. Instaluje środowisko, wykonuje migracje i odpala testy na bazie PostgreSQL.
* **Linting (`lint.yml`)**: Sprawdza jakość i formatowanie kodu za pomocą `flake8`, `black` i `isort`.
* **Security Scan (`security.yml`)**: Wykrywa luki w bezpieczeństwie kodu (`bandit`) oraz sprawdza zależności pod kątem podatności (`safety`).
* **Celery and Redis Tests (`celery.yml`)**: Uruchamia usługi Redis i Celery worker w celu przetestowania zadań asynchronicznych.
* **Deploy Application (`deploy.yml`)**: Buduje obraz Docker i wysyła aplikację na serwer.
* **Scheduled Tasks (`scheduled.yml`)**: Cykliczne zadania uruchamiane o 3:00 w nocy.

### Proces Deployu
Wdrożenie nowej wersji aplikacji odbywa się w 100% automatycznie. Wystarczy zmergować zmiany do gałęzi `main`. Pipeline sam zbuduje nowy obraz Docker i zaktualizuje go na serwerze.

### Wymagane Sekrety (GitHub Secrets)
Aby pipeline deploymentu działał poprawnie, w ustawieniach repozytorium (Settings -> Secrets and variables -> Actions) należy dodać następujące zmienne:
* `DOCKER_USERNAME` - login do rejestru Docker
* `DOCKER_PASSWORD` - hasło/token do rejestru Docker

### Debugowanie Błędów
Jeśli przy Twoim commicie pojawi się czerwony krzyżyk:
1. Przejdź do zakładki **Actions** w GitHubie.
2. Kliknij w workflow, który zakończył się błędem.
3. Wybierz konkretny "Job" (np. *build* albo *lint*) z menu po lewej stronie.
4. Rozwiń krok, przy którym widnieje ikona błędu, aby przeczytać logi z konsoli i zlokalizować problem.
