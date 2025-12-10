Це правильний крок. Ми перетворюємо твій **`osboxes`** (який зараз працює як Zabbix Proxy) на **Центр Автоматизації (Control Node)**.

Оскільки це **Ubuntu Server**, процес дуже простий. Тобі потрібен інтернет на цій машині (перевір `ping 8.8.8.8`).

Ось покрокова інструкція, що саме скачати і як налаштувати середовище.

---

### Крок 1. Оновлення та встановлення Ansible
Ми використовуємо офіційний репозиторій (PPA), щоб отримати найновішу версію Ansible (бо старі версії з стандартного Ubuntu можуть погано працювати з Cisco).

Вводь ці команди по черзі в консоль `osboxes`:

```bash
# 1. Оновлюємо списки пакетів
sudo apt update

# 2. Встановлюємо необхідні утиліти (щоб додавати репозиторії)
sudo apt install -y software-properties-common

# 3. Додаємо офіційний репозиторій Ansible
sudo add-apt-repository --yes --update ppa:ansible/ansible

# 4. Встановлюємо Ansible, Git та SSHpass (потрібен для паролів)
sudo apt install -y ansible git sshpass python3-pip
```

---

### Крок 2. Встановлення бібліотек для Cisco
Ansible написаний на Python. Щоб він міг "говорити" з мережевим обладнанням (Cisco IOS), йому потрібні спеціальні бібліотеки (`paramiko` або `netmiko`).

```bash
# Встановлюємо бібліотеки Python для роботи з SSH
pip3 install paramiko
```

---

### Крок 3. Налаштування робочого середовища
Не працюй в кореневій папці. Створимо окрему директорію для твого диплома.

```bash
# 1. Створюємо папку
mkdir ~/diploma-automation

# 2. Заходимо в неї
cd ~/diploma-automation
```

Тепер створимо найголовніший файл налаштувань — **`ansible.cfg`**.
Це критично для GNS3, тому що віртуальні роутери часто змінюються, і SSH може лаятися на "Host Key verification failed". Ми це вимкнемо.

Створи файл:
```bash
nano ansible.cfg
```

Встав туди цей текст:
```ini
[defaults]
# Вимикаємо перевірку ключів (щоб не писати "yes" кожного разу)
host_key_checking = False
# Вимикаємо створення .retry файлів (сміття)
retry_files_enabled = False
# Вимикаємо попередження про Python
deprecation_warnings = False
# Вказуємо шлях до інвентаря за замовчуванням
inventory = ./hosts
```
*(Натисни `Ctrl+O`, `Enter` щоб зберегти, і `Ctrl+X` щоб вийти)*.

---

### Крок 4. Створення Інвентаря (Список пристроїв)
Тепер створимо файл `hosts`, де опишемо нашу мережу (IP-адреси ми затвердили в минулих кроках).

```bash
nano hosts
```

Встав туди це (перевір IP, якщо вони відрізняються у тебе):

```ini
[routers]
R1 ansible_host=192.168.100.2
R2 ansible_host=192.168.100.3

[core_switches]
CORE1 ansible_host=192.168.100.4
CORE2 ansible_host=192.168.100.5

[access_switches]
ACC1 ansible_host=192.168.10.11
ACC2 ansible_host=192.168.10.12
ACC3 ansible_host=192.168.10.13

[cisco:children]
routers
core_switches
access_switches

[cisco:vars]
# Налаштування підключення
ansible_user=ansible
ansible_password=cisco123
ansible_connection=network_cli
ansible_network_os=cisco.ios.ios
ansible_become=yes
ansible_become_method=enable
ansible_become_password=cisco123
```
*(Збережи і вийди)*.

> **Важливо:** Цей файл розраховує, що ти вже створив юзера `ansible` з паролем `cisco123` на всіх пристроях Cisco (через консоль, як ми говорили раніше) і увімкнув там SSH.

---

### Крок 5. Фінальний Тест
Перевіримо, чи все працює. Ця команда спробує зайти на всі пристрої і просто сказати "Привіт".

```bash
ansible cisco -m ping
```

**Що ти маєш побачити:**
Зелений текст для кожного пристрою:
```text
R1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
...
```

Якщо бачиш **SUCCESS** — вітаю! Твій сервер готовий керувати всією мережею через код.
Якщо **UNREACHABLE** — перевіряй:
1.  Чи пінгується IP (`ping 192.168.100.2`).
2.  Чи увімкнено SSH на роутері (`show ip ssh`).
3.  Чи правильний пароль у файлі `hosts`.

Пробуй встановити і пиши результат!
