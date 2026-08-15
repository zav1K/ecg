# Автодеплой на хостинг

Push у `main` автоматично заливає файли на хостинг по FTP через GitHub Actions (`.github/workflows/deploy.yml`). Пароль від хостингу ніколи не потрапляє в чат чи до Claude — тільки в зашифровані GitHub Secrets.

## Налаштування для InfinityFree (один раз)

InfinityFree офіційно дозволяє деплой через GitHub Actions ("using GitHub actions is no problem, just don't cause unreasonable load on the FTP server" — відповідь адміна на форумі).

1. **Знайди FTP-дані:**
   Client Area (client.infinityfree.com / app.infinityfree.com) → твій хостинг-акаунт → розділ **FTP Details** (не файлменеджер).
   Там будуть: FTP username, FTP password (прихований за замовчуванням, треба натиснути "показати" — це той самий пароль, що й від панелі керування), FTP hostname (зазвичай виду `ftpupload.net`), порт (зазвичай 21).

2. **Директорія:** в InfinityFree файли сайту заливаються у папку `htdocs/` — тобто `FTP_TARGET_DIR` = `/htdocs/`.

3. **Додай ці дані в GitHub Secrets** (не сюди в чат!):
   Репозиторій на GitHub → Settings → Secrets and variables → Actions → New repository secret.
   Створи чотири секрети:
   - `FTP_SERVER` — хост із FTP Details (напр. `ftpupload.net`)
   - `FTP_USERNAME` — з FTP Details
   - `FTP_PASSWORD` — з FTP Details
   - `FTP_TARGET_DIR` — `/htdocs/`

4. Змерджи цю гілку (`claude/ecg-simulator-repo-s9m3yy`) в `main` — після цього кожен push у `main` сам оновлює сайт.

## Якщо перший деплой падає з помилкою підключення

Деякі безкоштовні хостинги інколи блокують діапазон IP-адрес GitHub Actions. Якщо перший запуск у вкладці Actions на GitHub впаде з таймаутом/connection refused (а не з помилкою логіна) — напиши в підтримку InfinityFree з проханням розблокувати підключення, або скажи мені лог помилки, розберемось разом.

## Якщо хостинг не приймає звичайний FTP

Деякі провайдери дають лише SFTP (порт 22) або FTPS. Скажи мені тип з'єднання — підправлю `deploy.yml` під нього (є окремий екшн для SFTP).
