Це чудове запитання, яке показує, що ти думаєш про архітектуру рішення.

Давай розберемося з **API** проти **SSH** у твоєму випадку, а потім я розпишу покроковий план реалізації на Ansible.

### 1. API чи SSH? (Чому ми обираємо SSH)

Ти спитав: *"Чи можливо це через API?"*

**Коротка відповідь:** Для твоїх образів (Cisco IOSv) — **НІ, краще SSH.**

**Детальна відповідь:**
*   **Сучасний світ (RESTCONF/NETCONF):** Нові пристрої (Cisco Nexus, IOS-XE, Catalyst 9000) мають API. Ти можеш надсилати їм JSON-запити. Це круто і швидко.
*   **Твоя реальність (Cisco IOSv):** Образи, які ми використовуємо в GNS3 (`vios-adventerprise...`), базуються на класичному монолітному IOS. Вони **не мають** вбудованого HTTP API або він працює дуже криво і вимагає гігабайти оперативки, якої у нас немає.

**Висновок:** Ми будемо використовувати **Ansible через SSH**.
Це називається **Screen Scraping** (або `network_cli`). Ansible заходить на роутер, як людина, вводить команди і читає відповідь. Це найнадійніший метод для твоєї топології.

---

### 2. Архітектура: Як це працює (Ansible + SSH)

У нас є "Центр керування" і "Керовані вузли".

1.  **Control Node (Мозок):** Твій сервер `osboxes` (Zabbix Proxy).
    *   На ньому стоїть Python і Ansible.
    *   Там лежать текстові файли (YAML), де написано "Що зробити".
2.  **Managed Nodes (Руки):** Твої R1, R2, Core, Access.
    *   На них **нічого не треба ставити** (це називається Agentless).
    *   Головне, щоб на них був відкритий 22 порт (SSH).

**Процес:**
`osboxes` $\xrightarrow{\text{SSH (TCP 22)}}$ `Cisco R1` $\rightarrow$ Вводить команди $\rightarrow$ Отримує результат $\rightarrow$ Показує тобі зелений напис "OK".

---

### 3. Реалізація: Крок за кроком

Тобі знадобиться один вечір, щоб це налаштувати.

#### Етап А: Підготовка Cisco (Bootstrap)
Ansible не може підключитися до "порожнього" роутера. Тобі треба один раз зайти руками на **кожен** пристрій (R1, R2, Core, Acc) і увімкнути SSH.

Введи це на кожному пристрої:
```cisco
conf t
! 1. Домен (без нього криптографія не працює)
ip domain-name diploma.local

! 2. Генеруємо ключі (натисни Enter, обери 1024)
crypto key generate rsa

! 3. Створюємо юзера для робота
username ansible privilege 15 secret cisco123

! 4. Вмикаємо SSH на вхід
line vty 0 4
 transport input ssh
 login local
exit

! 5. Дозволяємо версію 2 (безпечніша)
ip ssh version 2
wr
```

#### Етап Б: Налаштування `osboxes` (Control Node)

Зайди на свій Ubuntu Server (`osboxes`).

1.  **Встанови Ansible:**
    ```bash
    sudo apt update
    sudo apt install software-properties-common
    sudo add-apt-repository --yes --update ppa:ansible/ansible
    sudo apt install ansible -y
    ```

2.  **Створи папку проекту:**
    ```bash
    mkdir ~/network_automation
    cd ~/network_automation
    ```

3.  **Створи файл інвентаризації (`hosts`):**
    Тут ми пишемо IP-адреси (з нашого Management VLAN 10 і Transit VLAN 100).
    `nano hosts`

    ```ini
    [routers]
    R1 ansible_host=192.168.100.2
    R2 ansible_host=192.168.100.3

    [switches]
    CORE1 ansible_host=192.168.10.2
    CORE2 ansible_host=192.168.10.3
    ACC1  ansible_host=192.168.10.11
    ACC2  ansible_host=192.168.10.12

    [cisco:children]
    routers
    switches

    [cisco:vars]
    ansible_user=ansible
    ansible_password=cisco123
    ansible_connection=network_cli
    ansible_network_os=cisco.ios.ios
    # Вимикаємо перевірку ключів SSH (щоб не питав yes/no)
    ansible_ssh_common_args='-o StrictHostKeyChecking=no'
    ```

#### Етап В: Перший тест (Ping)

Перевіримо, чи Ansible бачить мережу. Запусти команду:
```bash
ansible cisco -i hosts -m ping
```
*   Якщо побачиш зелене **"pong"** — ти переміг. Зв'язок є.
*   Якщо червоне — перевіряй пінг з `osboxes` до пристроїв.

---

### 4. Твій перший Playbook (Сценарій)

Давай зробимо щось корисне для диплома. Наприклад, **автоматичний бекап конфігурації**.
Це "вбивча фіча" для захисту.

Створи файл `backup.yml`:
```yaml
---
- name: BACKUP NETWORK CONFIGURATIONS
  hosts: cisco
  gather_facts: no

  tasks:
    - name: Get Running Config
      cisco.ios.ios_command:
        commands: show run
      register: config_output

    - name: Save Config to File
      copy:
        content: "{{ config_output.stdout[0] }}"
        dest: "./backups/{{ inventory_hostname }}_config.txt"

    - name: Show Success Message
      debug:
        msg: "Backup for {{ inventory_hostname }} completed successfully!"
```

**Запуск:**
1.  Створи папку для бекапів: `mkdir backups`
2.  Запусти: `ansible-playbook -i hosts backup.yml`

**Результат:**
У папці `backups` з'являться файли `R1_config.txt`, `CORE1_config.txt` тощо.

---

### Чому це "Рівень 1", а не просто скрипт?

1.  **Ідемпотентність:** Ansible розумний. Якщо ти скажеш "створи юзера Admin", а він вже є, Ansible скаже "OK" (нічого не змінив), а не видасть помилку.
2.  **Масштабованість:** Ти пишеш один файл, а він застосовується на 100 пристроїв одночасно.
3.  **Human Readable:** YAML читається як англійська мова. Комісія зрозуміє, що робить твій код, навіть не знаючи програмування.

**Готовий спробувати налаштувати SSH на роутерах і встановити Ansible?** Це база для GitOps.
