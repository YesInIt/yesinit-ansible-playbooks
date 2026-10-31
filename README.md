# yesinit-ansible-playbooks

Публичный репозиторий с набором моих плейбуков, которые я часто использую.

## Плейбуки

| Плейбук | Что делает | Группа хостов | ОС | Запуск |
|---|---|---|---|---|
| [setup-docker-ce.yaml](playbooks/setup-docker-ce.yaml) | Ставит Docker CE + compose/buildx из официального репозитория Docker | `all` | Debian, Ubuntu | [варианты](#setup-docker-ce) |
| [setup-zabbix-agent.yml](playbooks/setup-zabbix-agent.yml) | Ставит/обновляет Zabbix Agent 2 (7.4) ролью `community.zabbix.zabbix_agent`, удаляет старый `zabbix-agent` (v1) | `all` | ОС, поддерживаемые ролью | [варианты](#setup-zabbix-agent) |

## Структура

```
├── ansible.cfg
├── requirements.yml
├── inventory/
│   └── example/          # пример, замените на реальные хосты
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


2. Установить коллекции из `requirements.yml` (нужны плагинам инвентори и плейбуку `setup-zabbix-agent.yml`):

   ```bash
   ansible-galaxy collection install -r requirements.yml
   ```

3. Запустить плейбук:

   ```bash
   ansible-playbook playbooks/setup-docker-ce.yaml --limit <ваш хост> -u <user>
   ```

   Пользователь по умолчанию не задан, поэтому `-u <user>` обязателен — либо пропишите `ansible_user` в `group_vars` своего инвентори.

   Проверить без изменений: добавить `--check --diff`.

## Варианты запуска

<a id="setup-docker-ce"></a>
### setup-docker-ce.yaml

Быстрый запуск (без прокси):

```bash
ansible-playbook ./playbooks/setup-docker-ce.yaml --limit <host> -u <user>
```

Расширенный запуск с указанием инвентори:

```bash
ansible-playbook ./playbooks/setup-docker-ce.yaml --limit <host> -u <user> -i inventory/<inventory>
```

Расширенный запуск с прокси через `-e`:

```bash
ansible-playbook ./playbooks/setup-docker-ce.yaml --limit <host> -u <user> -i inventory/<inventory> -e proxy_url="http://<proxy>:3128"
```

Прокси (`proxy_url`) можно задать и в инвентори — в `group_vars`/`host_vars`. Если `proxy_url` не задан, используется системный прокси хоста (`http_proxy`/`https_proxy` из окружения, например из `/etc/environment`) — так же и в `setup-zabbix-agent.yml`.

<a id="setup-zabbix-agent"></a>
### setup-zabbix-agent.yml

Документация роли: [community.zabbix — Zabbix Agent role](https://galaxy.ansible.com/ui/repo/published/community/zabbix/docs/ZABBIX_AGENT_ROLE/#zabbix-agent).

Нужна коллекция `community.zabbix` (см. `requirements.yml`):

```bash
ansible-galaxy collection install -r requirements.yml
```

Перед запуском поменяйте в `vars` плейбука `zabbix_agent_server` (он же используется как `ServerActive`), `zabbix_agent_version` и `zabbix_agent_hostmetadata` (метаданные для авторегистрации).
Агент слушает на основном IPv4 хоста (`ansible_default_ipv4`). Старый `zabbix-agent` (v1) удаляется, установленный `zabbix-agent2` обновляется до последней версии из репозитория выбранной версии.

Быстрый запуск (без прокси):

```bash
ansible-playbook ./playbooks/setup-zabbix-agent.yml --limit <host> -u <user>
```

Расширенный запуск с указанием инвентори:

```bash
ansible-playbook ./playbooks/setup-zabbix-agent.yml --limit <host> -u <user> -i inventory/<inventory>
```

Расширенный запуск с прокси через `-e` (прокси используется для ключа и пакетов Zabbix):

```bash
ansible-playbook ./playbooks/setup-zabbix-agent.yml --limit <host> -u <user> -i inventory/<inventory> -e proxy_url="http://<proxy>:3128"
```

## Лицензия

[LICENSE](LICENSE)
