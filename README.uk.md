# Windows у Docker

**Мови:** [English](README.md) · [Українська](README.uk.md)

> Автор: [Boryslav Deputat](https://github.com/boryslavdeputat) - Cloud / SRE / Platform.
> Зроблено з **KLAV (UA AI) / КЛАВ (УКР ШІ)**.
> Сайти: [Portfolio](https://boryslavdeputat.com/) · [ClawDBot](https://clawdbot.llc/) · [AI hub](https://boryslavdeputat.github.io/boryslavdeputat/)

Запускає повноцінну інсталяцію Windows усередині Docker-контейнера, з
апаратним прискоренням KVM і переглядом прямо в браузері (noVNC). Побудовано
на основі [dockur/windows](https://github.com/dockur/windows).

При першому запуску контейнер сам завантажує обрану версію Windows напряму
з серверів Microsoft і встановлює її без участі користувача — ISO-образ чи
ліцензійний ключ не потрібні для старту.

## Вимоги

- Linux-хост з доступним `/dev/kvm` (перевірити: `ls -la /dev/kvm`)
  - Якщо це віртуальна машина (Hyper-V, Proxmox тощо) — потрібно увімкнути
    **nested virtualization** саме для цієї VM.
- Docker + Docker Compose

## Використання

```bash
cp .env.example .env
# відредагуй .env, встанови WINDOWS_PASSWORD (обов'язково) та інше за бажанням
docker compose up -d
```

Далі відкрий:
- **Перегляд у браузері:** `http://<host>:8006`
- **RDP:** `<host>:3389` (користувач: `Docker`, або той, що вказав у `.env`)

## Налаштування

Усі параметри в `.env` (скопіюй з `.env.example`):

| Змінна | За замовчуванням | Опис |
|---|---|---|
| `WINDOWS_VERSION` | `11` | Версія Windows — див. [версії dockur/windows](https://github.com/dockur/windows#how-do-i-select-a-windows-version) |
| `WINDOWS_USERNAME` | `Docker` | Ім'я користувача Windows |
| `WINDOWS_PASSWORD` | *(обов'язково)* | Пароль користувача Windows |
| `WINDOWS_LANGUAGE` | `English` | Мова встановлення Windows |
| `WINDOWS_DISK_SIZE` | `64G` | Розмір віртуального диска |
| `WINDOWS_RAM_SIZE` | `4G` | Обсяг RAM для VM |
| `WINDOWS_CPU_CORES` | `2` | Кількість CPU-ядер для VM |

Без `/dev/kvm` контейнер все одно запуститься, але через повільну програмну
емуляцію (приблизно у 10 разів повільніше) — про це буде попередження в
логах.

## Збереження даних

Диск VM і стан зберігаються в теці `./data` на хості, тож
`docker compose down` / `up` нічого не втрачає.

## Ліцензія

Цей репозиторій — лише docker-compose-обгортка для
[dockur/windows](https://github.com/dockur/windows) — дивись той проєкт
щодо ліцензії та деталей роботи.
