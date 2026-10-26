# yesinit-ansible-playbooks

Публичный репозиторий с набором моих плейбуков, которые я часто использую.

## Плейбуки

| Плейбук | Что делает | Группа хостов | ОС | Запуск |
|---|---|---|---|---|
| [setup-docker-ce.yaml](playbooks/setup-docker-ce.yaml) | Ставит Docker CE + compose/buildx из официального репозитория Docker | `all` | Debian, Ubuntu | [варианты](#setup-docker-ce) |

## Структура

```
├── ansible.cfg
├── requirements.yml
├── inventory/
│   └── example/          # только пример, без реальных хостов
│       ├── hosts.yaml
│       └── group_vars/
├── playbooks/
└── .github/workflows/lint.yml
```

## Использование

1. Скопировать пример инвентори и вписать свои хосты и переменные:

   ```bash
   cp -r inventory/example inventory/prod
   ```

   Всё, кроме `inventory/example`, игнорируется git'ом — реальные инвентори в репозиторий не попадут.

2. Установить зависимости (если появятся в `requirements.yml`):

   ```bash
   ansible-galaxy collection install -r requirements.yml
   ```

3. Запустить плейбук:

   ```bash
   ansible-playbook playbooks/setup-docker-ce.yaml --limit <ваш хост>
   ```

   Проверить без изменений: добавить `--check --diff`.

## Варианты запуска

<a id="setup-docker-ce"></a>
### setup-docker-ce.yaml

Быстрый запуск (без прокси):

```bash
ansible-playbook ./playbooks/setup-docker-ce.yaml --limit <host>
```

Расширенный запуск с указанием пользователя и инвентори:

```bash
ansible-playbook ./playbooks/setup-docker-ce.yaml --limit <host> -u <user> -i inventory/<inventory>
```

Расширенный запуск с прокси через `-e`:

```bash
ansible-playbook ./playbooks/setup-docker-ce.yaml --limit <host> -u <user> -i inventory/<inventory> -e proxy_url="http://<proxy>:3128"
```

Прокси (`proxy_url`) можно задать и в инвентори — в `group_vars`/`host_vars`.

## Лицензия

[LICENSE](LICENSE)
