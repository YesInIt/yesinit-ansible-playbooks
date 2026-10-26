# yesinit-ansible-playbooks

Публичный репозиторий с набором моих плейбуков, которые я часто использую.

## Плейбуки

| Плейбук | Что делает | Группа хостов | ОС |
|---|---|---|---|
| [setup-docker-ce.yaml](playbooks/setup-docker-ce.yaml) | Ставит Docker CE + compose/buildx из официального репозитория Docker | `all` | Debian, Ubuntu |

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

## Лицензия

[LICENSE](LICENSE)
