# Автодеплой на хостинг

Push у `main` автоматично заливає файли на хостинг по FTP через GitHub Actions (`.github/workflows/deploy.yml`). Пароль від хостингу ніколи не потрапляє в чат чи до Claude — тільки в зашифровані GitHub Secrets.

## Налаштування (один раз)

1. **Створи окремий FTP-акаунт у cPanel** (не головний логін!):
   cPanel → Files → FTP Accounts → Create FTP Account.
   Обмеж його директорією, де лежить сайт (напр. `public_html/` або `public_html/ecg/`).
   Запиши: FTP-сервер (зазвичай `ftp.твійдомен.com`), логін, пароль, шлях до директорії.

2. **Додай ці дані в GitHub Secrets** (не сюди в чат!):
   Репозиторій на GitHub → Settings → Secrets and variables → Actions → New repository secret.
   Створи чотири секрети:
   - `FTP_SERVER` — напр. `ftp.твійдомен.com`
   - `FTP_USERNAME`
   - `FTP_PASSWORD`
   - `FTP_TARGET_DIR` — шлях на сервері, напр. `/public_html/` (зі слешем в кінці)

3. Змерджи цю гілку (`claude/ecg-simulator-repo-s9m3yy`) в `main` — після цього кожен push у `main` сам оновлює сайт.

## Якщо хостинг не приймає звичайний FTP

Деякі провайдери дають лише SFTP (порт 22) або FTPS. Скажи мені тип з'єднання — підправлю `deploy.yml` під нього (є окремий екшн для SFTP).
