# Налаштування збору та моніторингу логів з GNS3-мережі в Zabbix

## Огляд

Цей документ описує процес налаштування централізованого збору логів з локальної GNS3-мережі через `rsyslog` та їх моніторингу в системі Zabbix.

---

## 1. Збір логів в окремий файл

### Мета
Налаштувати службу `rsyslog` для збору всіх логів з GNS3-мережі (IP: `147.232.184.136`) в окремий файл `/var/log/on-premise.log`.

### Кроки налаштування

#### 1.1. Створення конфігураційного файлу
Створіть файл `/etc/rsyslog.d/15-on-premise.conf`:

```bash
sudo nano /etc/rsyslog.d/15-on-premise.conf
```

Додайте наступне правило:

```nginx
if $fromhost-ip == '147.232.184.136' then {
    action(type="omfile" file="/var/log/on-premise.log")
    stop
}
```

#### 1.2. Перевірка основної конфігурації
Переконайтеся, що в `/etc/rsyslog.conf` увімкнено прийом UDP-логів:

```conf
module(load="imudp")
input(type="imudp" port="514")
```

#### 1.3. Налаштування прав доступу
Встановіть правильні права для лог-файлу:

```bash
sudo touch /var/log/on-premise.log
sudo chown syslog:adm /var/log/on-premise.log
sudo chmod 640 /var/log/on-premise.log
```

#### 1.4. Перезапуск служби
Застосуйте зміни:

```bash
sudo systemctl restart rsyslog
sudo systemctl status rsyslog
```

---

## 2. Налаштування Zabbix Agent

### Мета
Налаштувати локального Zabbix Agent для читання файлу `/var/log/on-premise.log` і відправки даних на Zabbix Server.

### Кроки налаштування

#### 2.1. Надання прав доступу
Додайте користувача `zabbix` до групи `adm`:

```bash
sudo usermod -a -G adm zabbix
```

Перевірте членство в групі:

```bash
groups zabbix
```

#### 2.2. Конфігурація агента
Відредагуйте файл `/etc/zabbix/zabbix_agentd.conf`:

```bash
sudo nano /etc/zabbix/zabbix_agentd.conf
```

Переконайтеся, що вказано:

```conf
Server=127.0.0.1
ServerActive=127.0.0.1
Hostname=Zabbix server
```

> **Важливо:** Значення `Hostname` має точно збігатися з назвою хоста в Zabbix UI.

#### 2.3. Перезапуск агента
Застосуйте зміни:

```bash
sudo systemctl restart zabbix-agent
sudo systemctl status zabbix-agent
```

---

## 3. Налаштування Zabbix UI

### Мета
Створити елемент даних (Item) для збору логів з файлу через Zabbix Agent.

### Кроки налаштування

#### 3.1. Створення Item
1. Перейдіть: **Data collection** → **Hosts** → **Zabbix server** → **Items** → **Create item**
2. Заповніть поля:
   - **Name:** `Логи з локальної мережі (On-Premise)`
   - **Type:** `Zabbix agent (active)`
   - **Key:** `log[/var/log/on-premise.log]`
   - **Type of information:** `Log`
   - **Update interval:** `1m` (або за потребою)

#### 3.2. Додавання тегів
Додайте теги для зручної фільтрації:
- `scope` = `on-premise`
- `type` = `log`
- `source` = `gns3`

#### 3.3. Збереження
Натисніть **Add** для збереження Item.

---

## 4. Налаштування тригерів (Triggers)

### Мета
Створити правила для автоматичного виявлення критичних подій у логах.

### Як створювати тригери
1. Перейдіть: **Data collection** → **Hosts** → **Zabbix server** → **Triggers** → **Create trigger**
2. Використовуйте **Expression constructor**
3. Оберіть функцію `find` для Item `log[/var/log/on-premise.log]`

### Приклади тригерів

#### 4.1. Падіння інтерфейсу
Реагує на зміну стану інтерфейсу на "down".

```
find(/Zabbix server/log[/var/log/on-premise.log],,"like","changed state to down")=1
```

- **Severity:** `Warning` або `Average`
- **Name:** `Інтерфейс на пристрої змінив стан на DOWN`

#### 4.2. Загальна помилка
Реагує на ключові слова помилок.

```
find(/Zabbix server/log[/var/log/on-premise.log],,"regexp","ERROR|Failed|Critical")=1
```

- **Severity:** `Average` або `High`
- **Name:** `Виявлено критичну помилку в логах`

#### 4.3. Cisco System Message (High Priority)
Реагує на системні повідомлення Cisco високого рівня.

```
find(/Zabbix server/log[/var/log/on-premise.log],,"like","%SYS-2-LOGMSG")=1
```

- **Severity:** `High`
- **Name:** `Критичне системне повідомлення Cisco (SYS-2)`

#### 4.4. Невдала спроба входу
Реагує на помилки автентифікації.

```
find(/Zabbix server/log[/var/log/on-premise.log],,"regexp","Failed login|Authentication failure")=1
```

- **Severity:** `Warning`
- **Name:** `Виявлено невдалу спробу входу`

### Рекомендації щодо тригерів
- ⚠️ **Не створюйте тригер на кожен лог!**
- ✅ Створюйте тригери лише для подій, які вимагають вашої уваги
- ✅ Використовуйте відповідний рівень Severity
- ✅ Додавайте зрозумілі описи та назви

---

## 5. Перевірка роботи

### 5.1. Перевірка надходження логів
Подивіться на вміст файлу:

```bash
tail -f /var/log/on-premise.log
```

### 5.2. Перевірка в Zabbix UI
1. **Monitoring** → **Hosts** → **Zabbix server** → **Latest data**
2. Знайдіть Item `Логи з локальної мережі (On-Premise)`
3. Перевірте, чи надходять дані

### 5.3. Тестування тригерів
1. **Monitoring** → **Problems**
2. Перевірте, чи з'являються проблеми при виникненні відповідних подій у логах

---

## 6. Усунення проблем

### Логи не надходять у `/var/log/on-premise.log`
- Перевірте конфігурацію rsyslog: `sudo rsyslogd -N1`
- Перевірте статус служби: `sudo systemctl status rsyslog`
- Перевірте, чи надходять пакети на порт 514: `sudo tcpdump -i any port 514`

### Zabbix не читає логи
- Перевірте права доступу: `sudo -u zabbix cat /var/log/on-premise.log`
- Перевірте логи агента: `sudo tail -f /var/log/zabbix/zabbix_agentd.log`
- Переконайтеся, що Hostname в конфігурації збігається з UI

### Тригери не спрацьовують
- Перевірте вираз тригера через Test
- Переконайтеся, що Item активний і отримує дані
- Перевірте, чи відповідає формат логів регулярному виразу

---

## Автор і дата
Створено: 21 жовтня 2025  
Версія: 1.0