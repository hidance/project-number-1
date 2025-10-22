# 🚀 DevOps Roadmap 2025 (СНД)

> 💡 Це суб'єктивний погляд на дорожню карту для DevOps-інженерів на 2025 рік, орієнтовану на ринок СНД. Роадмап заснований на особистому досвіді, який дозволив успішно розвиватися в професії.

---

## 📝 Крок 0: Документація — Ваш Другий Мозок

**Найважливіший крок перед початком навчання!**

### ❌ Проблема
Багато хто, особливо початківці, просто зберігають безліч закладок на статті (наприклад, на Habr), але:
- Рідко до них повертаються
- Не систематизують знання
- Гуглять одне й те саме по кілька разів

### ✅ Рішення
Почати вести **особисту документацію** — це дозволить:
- Мати під рукою надійне джерело інформації
- Зберігати знання, релевантні саме для вас
- Швидко знаходити потрібну інформацію

### 🛠️ Інструменти

#### Obsidian (основна рекомендація)
- Потужний інструмент для ведення нотаток у форматі `.md`
- Дозволяє створювати зв'язки між темами
- Допомагає будувати "другий мозок"

#### Альтернативи:
- **Memos** — простий мінімалістичний варіант
- **Notion** — повнофункціональна платформа
- **WizNote** — класичний нотатник

---

## 🐧 Крок 1: Linux — Основа Основ

**Linux є фундаментом для всієї подальшої роботи в DevOps.**

### 📚 Що вивчити

#### Теорія:
- Розуміння **bash**
- Файлова система (fs)
- Робота процесів
- Базові механізми ОС

#### Практика:
- Базові команди (5 команд з двох літер + 5 команд з трьох літер)
- Основи траблшутингу
- Робота в терміналі

### 🧪 Лабораторна робота
Встановіть **Ubuntu** у VirtualBox або VMware і почніть працювати в терміналі.

### 📖 Джерела

#### Для початківців:
- 🎥 Плейлисти Семаєва по підготовці до LPIC
- 🎥 Плейлист "GNU Linux Pro | Основы GNU/Linux"

#### Книги:
- 📘 **"Внутрішнє влаштування Linux"** by Дмитро Кетов  
  *Невелика, але змістовна база для розуміння основ*

- 📘 **"UNIX і Linux. Керівництво системного адміністратора"**  
  *Великий талмуд, який варто використовувати як довідник*

---

## 🔀 Крок 2: Git

**Найкоротший, але обов'язковий крок!**

### 📚 Що вивчити
- Основи роботи з системою контролю версій
- Branching та merging
- Rebase, cherry-pick
- Робота з віддаленими репозиторіями

### 📖 Джерела

#### Для новачків:
- 📄 Стаття **"Що таке Git"** на сайті Atlassian

#### Для всіх рівнів (junior → senior):
- 🎮 **learngitbranching.js.org** — інтерактивний тренажер
  - Візуально показує, як працюють складні команди
  - Закриває 95% щоденних потреб у роботі з Git
  - Обов'язково для middle та senior

---

## 🌐 Крок 3: Net/Sec (Мережі та Безпека)

**На початковому етапі не потрібна глибина, важливий контекст.**

### 🔌 Net (Мережі)

**Що вивчити:**
- Модель TCP/IP (OSI)
- DNS
- DHCP
- HTTP/HTTPS
- TLS

### 🔒 Sec (Безпека)

**Що вивчити:**
- iptables (або nftables)
- Розуміння chains
- Блокування трафіку
- Базові правила firewall

### 📖 Джерела

- 🎥 **Linkmeup** — курс по мережах до теми BGP
- 📄 **"Мережі для найменших"** — класична стаття для розуміння основ

### 📚 Додаткова література

- 📖 **"Проект Фенікс"** (The Phoenix Project)
  - Це не технічна література, а роман
  - Про вибудовування DevOps-процесів
  - Допомагає зрозуміти філософію та принципи DevOps

---

## 🐳 Крок 4: Контейнеризація (Docker)

**Дуже важливий крок у розвитку DevOps-інженера!**

### 📚 Що вивчити

#### Основи:
- **Dockerfile** та його оптимізація
- Container runtime, runc
- Мережа (net) в Docker
- **Docker Compose**

#### Глибше розуміння:
- **Namespaces (ns)** — ізоляція процесів
- **Cgroups** — обмеження ресурсів
- Базові механізми Linux, на яких працює Docker

### 🧪 Практика

1. Встановити Docker
2. Написати власний Dockerfile
3. Оптимізувати multi-stage build
4. Запускати контейнери через docker-compose
5. Налаштувати мережу між контейнерами

### 📖 Рекомендовані ресурси

- 📘 Офіційна документація Docker
- 📘 **"Docker Deep Dive"** by Nigel Poulton
- 🎥 Відеокурси по Docker на YouTube

---

## 📊 Крок 5: Моніторинг

**Автор ставить моніторинг раніше, ніж у більшості роадмапів, оскільки це дуже часта задача.**

### 🏛️ Три стовпи моніторингу

1. **Метрики** — числові показники системи
2. **Логи** — записи подій
3. **Трейси** — шлях запиту через систему *(на початку можна пропустити)*

### 🛠️ Інструменти для вивчення

#### Метрики:
- **VictoriaMetrics** (рекомендується) — більш ефективний аналог Prometheus
- **Prometheus** — стандарт індустрії
- **Node Exporter** — збір системних метрик

#### Візуалізація:
- **Grafana** — побудова дашбордів та графіків

#### Логи:
- **Elasticsearch/Opensearch** — для великих проектів
- **Loki** — простіший варіант для невеликих проектів

#### Збір логів:
- **Vector** — сучасний швидкий колектор
- **Fluentd/Fluent-bit** — класичні рішення

#### Алертинг:
- **Alertmanager** — система сповіщень

### 🧪 Практика

1. Налаштувати збір метрик з Node Exporter
2. Візуалізувати їх у Grafana
3. Налаштувати алерти через Alertmanager
4. Налаштувати збір логів
5. Створити дашборд для моніторингу

---

## 🔄 Крок 6: CI/CD

**Автоматизація збірки та доставки коду.**

### 📚 Що вивчити

#### GitLab CI (найпопулярніший в СНД):
- Написання `.gitlab-ci.yml`
- Runners та executors
- Pipelines та stages
- Артефакти та кеш
- CI/CD змінні та секрети

#### Альтернативи:
- **GitHub Actions** — для проектів на GitHub
- **Forgejo** — self-hosted Git з CI/CD

### 🧪 Практика

1. Встановити **self-hosted GitLab**
2. Написати простий пайплайн
3. Запускати docker-compose.yml з попереднього кроку
4. Налаштувати автоматичний деплой
5. Додати етапи тестування

### 📖 Джерела

- 📘 Офіційна документація GitLab CI/CD
- 🎥 Відеокурси по GitLab CI
- 📄 Статті на Habr про CI/CD

---

## 💻 Крок 7: Програмування (Go/Python)

**Для DevOps не потрібні глибокі знання, як для розробника, але база необхідна.**

### 🎯 Навіщо?

- Написання кастомних інструментів
- Створення власних експортерів для моніторингу
- Автоматизація там, де не вистачає готових рішень
- Розуміння коду розробників

### 📚 Що вчити

- **Go** або **Python** на базовому рівні
- Синтаксис мови
- Змінні, типи даних
- Умови та цикли
- Функції
- Робота з файлами
- Основи ООП (для Python)

### 📖 Джерела

#### Python:
- 🎓 Курси на **Stepik**
- 📘 **"Python Crash Course"**
- 📘 **"Automate the Boring Stuff with Python"**

#### Go:
- 🌐 **golangify.com**
- 📘 **"The Go Programming Language"**
- 🎓 **"Go by Example"**

---

## ☸️ Крок 8: Kubernetes (K8S)

**Найбільший і найважливіший крок!**

### 📚 Що вивчити

#### Основи:
- **Маніфести** (YAML)
- **Архітектура** Kubernetes
- Pods, Services, Deployments
- ConfigMaps та Secrets
- Ingress Controllers
- Persistent Volumes

#### Інструменти екосистеми:
- **Helm** — пакетний менеджер
- **Vault** — управління секретами
- **ArgoCD** — GitOps для Kubernetes

#### Внутрішня будова:
- **CRI** (Container Runtime Interface)
- **CNI** (Container Network Interface)
- **CSI** (Container Storage Interface)

### 📖 Джерела

#### Для початківців:
- 📘 **Slurm** — "Kubernetes для розробників"
- 🎥 Офіційні туторіали на kubernetes.io

#### Для просунутих:
- 🎥 **Канал Артура Крюкова** на YouTube  
  *(автор називає це "неймовірною годнотою")*

#### Офіційні ресурси:
- 📖 **kubernetes.io** — офіційна документація

#### Книги:
- 📘 **"Kubernetes в действии"** *(чекати на друге видання)*
- 📘 **"Kubernetes изнутри"**
- 📘 **"Kubernetes Up & Running"**
- 📘 **"The Kubernetes Book"** by Nigel Poulton

### 🧪 Практика

1. **Встановити кластер** (на вибір):
   - Kubespray
   - Kubeadm
   - K3S
   - Minikube (для локальної розробки)

2. **Розгорнути додаток** через Helm

3. **Інтегрувати:**
   - Vault для управління секретами
   - ArgoCD для GitOps

4. **Налаштувати:**
   - Ingress для доступу ззовні
   - Monitoring через Prometheus
   - Logging через Loki або EFK

---

## 🎯 Крок 9: Стратегії деплою

**Теоретичний крок, але дуже важливий для розуміння.**

### 📚 Що вивчити

#### Blue-Green Deployment:
- Два ідентичні середовища
- Миттєве перемикання
- Швидкий rollback

#### Canary Release:
- Поступове розгортання
- Тестування на невеликій частині користувачів
- Поступове збільшення трафіку

#### Інші стратегії:
- Rolling Update
- Recreate
- A/B Testing
- Shadow Deployment

### 🎯 Дія

1. Прочитати пару статей про кожну стратегію
2. Задокументувати у себе в Obsidian
3. Зрозуміти переваги та недоліки кожної
4. Визначити, коли яку використовувати

### 📖 Джерела

- 📄 Статті на Medium про deployment strategies
- 📘 Офіційна документація Kubernetes про Deployments
- 🎥 Відео про GitOps та ArgoCD

---

## ☁️ Крок 10: Хмари (Clouds)

**Актуально, але в СНД досі багато on-premise рішень.**

### 🛠️ Інструменти Infrastructure as Code

- **Terraform** — стандарт індустрії
- **OpenTofu** — open-source форк Terraform

### 🌍 Провайдери

#### Для СНД:
- **Yandex.Cloud** — найпопулярніший в регіоні
- VK Cloud (ex Mail.ru Cloud)
- Selectel

#### Міжнародні:
- **AWS** (Amazon Web Services)
- **Google Cloud Platform** (GCP)
- **Microsoft Azure**

### 🧪 Практика (ідеальна лабораторна)

#### Повний цикл DevOps:

1. **За допомогою Terraform:**
   - Підняти віртуальні машини в хмарі
   - Налаштувати мережу
   - Створити необхідні ресурси

2. **За допомогою Ansible (Kubespray):**
   - Розгорнути кластер Kubernetes на VM
   - Налаштувати компоненти

3. **За допомогою ArgoCD:**
   - Розгорнути додатки з Git
   - Налаштувати автоматичну синхронізацію
   - Реалізувати GitOps підхід

### 📚 Додаткові знання

#### Методології (на фоні):
- **Scrum** — гнучка розробка
- **Agile** — принципи
- **Kanban** — візуалізація процесів

> 💡 **Порада:** Почитати про методології, але не заглиблюватися спочатку.

---

## 📚 Рекомендовані книги

### Linux та Unix
- 📘 **"Внутрішнє влаштування Linux"** by Дмитро Кетов
- 📘 **"UNIX і Linux. Керівництво системного адміністратора"**
- 📘 **"How Linux Works"** by Brian Ward
- 📘 **"The Linux Command Line"** by William Shotts

### Мережі та безпека
- 📗 **"TCP/IP Illustrated"** by W. Richard Stevens
- 📗 **"Computer Networks"** by Andrew S. Tanenbaum
- 📗 **"Practical Packet Analysis"** by Chris Sanders

### Docker та контейнери
- 📙 **"Docker Deep Dive"** by Nigel Poulton
- 📙 **"Docker in Action"** by Jeff Nickoloff
- 📙 **"Container Security"** by Liz Rice

### Kubernetes
- 📕 **"Kubernetes в действии"** *(друге видання)*
- 📕 **"Kubernetes изнутри"**
- 📕 **"Kubernetes Up & Running"** by Kelsey Hightower
- 📕 **"The Kubernetes Book"** by Nigel Poulton
- 📕 **"Kubernetes Patterns"** by Bilgin Ibryam

### Моніторинг та спостережливість
- 📔 **"Prometheus: Up & Running"** by Brian Brazil
- 📔 **"Observability Engineering"** by Charity Majors
- 📔 **"Distributed Systems Observability"** by Cindy Sridharan

### CI/CD та автоматизація
- 📓 **"Continuous Delivery"** by Jez Humble
- 📓 **"Pipeline as Code"** by Mohamed Labouardy
- 📓 **"Learning GitHub Actions"** by Brent Laster

### Infrastructure as Code
- 📒 **"Terraform: Up & Running"** by Yevgeniy Brikman
- 📒 **"Infrastructure as Code"** by Kief Morris
- 📒 **"Ansible for DevOps"** by Jeff Geerling

### Програмування
- 📘 **"The Go Programming Language"** by Alan Donovan
- 📘 **"Python Crash Course"** by Eric Matthes
- 📘 **"Automate the Boring Stuff with Python"** by Al Sweigart
- 📘 **"Go by Example"** (онлайн ресурс)

### DevOps культура та філософія
- 📙 **"Проект Фенікс"** (The Phoenix Project) by Gene Kim
- 📙 **"The DevOps Handbook"** by Gene Kim
- 📙 **"Accelerate"** by Nicole Forsgren
- 📙 **"Site Reliability Engineering"** by Google (безкоштовно)
- 📙 **"The Unicorn Project"** by Gene Kim

### Хмарні технології
- 📗 **"Amazon Web Services in Action"** by Andreas Wittig
- 📗 **"Google Cloud Platform in Action"** by JJ Geewax
- 📗 **"Learning Azure"** by Jonah Carrio Andersson

---

## 🔗 Корисні посилання

### Офіційна документація
- 🌐 [Kubernetes Documentation](https://kubernetes.io/docs/)
- 🌐 [Docker Documentation](https://docs.docker.com/)
- 🌐 [Terraform Documentation](https://www.terraform.io/docs)
- 🌐 [GitLab CI/CD Documentation](https://docs.gitlab.com/ee/ci/)
- 🌐 [Prometheus Documentation](https://prometheus.io/docs/)
- 🌐 [Grafana Documentation](https://grafana.com/docs/)

### Інтерактивні ресурси
- 🎮 [learngitbranching.js.org](https://learngitbranching.js.org/) — Git тренажер
- 💻 [Play with Docker](https://labs.play-with-docker.com/)
- 💻 [Play with Kubernetes](https://labs.play-with-k8s.com/)
- 💻 [KillerCoda](https://killercoda.com/) — інтерактивні лабораторії

### Навчальні платформи
- 🎓 [Stepik](https://stepik.org/) — курси російською
- 🎓 [golangify.com](https://golangify.com/) — навчання Go
- 🎓 [Linkmeup](https://linkmeup.ru/) — подкасти та курси по мережах

### Блоги та статті
- 📝 [Habr](https://habr.com/) — російськомовна спільнота
- 📝 [Atlassian Git Tutorial](https://www.atlassian.com/git)
- 📝 "Мережі для найменших"

### YouTube канали
- 🎥 Плейлисти Семаєва (LPIC)
- 🎥 GNU Linux Pro
- 🎥 **Канал Артура Крюкова** (Kubernetes)
- 🎥 Slurm

### Інструменти для документації
- 📝 [Obsidian](https://obsidian.md/) — knowledge base
- 📝 [Memos](https://www.usememos.com/) — lightweight notes
- 📝 [Notion](https://www.notion.so/) — all-in-one workspace
- 📝 [WizNote](https://www.wiz.cn/wiznote) — note-taking

### Хмарні провайдери (СНД)
- ☁️ [Yandex.Cloud](https://cloud.yandex.ru/)
- ☁️ [VK Cloud](https://cloud.vk.com/)
- ☁️ [Selectel](https://selectel.ru/)

### Міжнародні хмарні провайдери
- ☁️ [AWS](https://aws.amazon.com/)
- ☁️ [Google Cloud](https://cloud.google.com/)
- ☁️ [Microsoft Azure](https://azure.microsoft.com/)

---

## ✨ Висновок

### 🎯 Ключові принципи успішного навчання:

1. **📝 Документуйте все** — ваші нотатки стануть найціннішим ресурсом
2. **🔄 Практикуйте постійно** — теорія без практики мертва
3. **🎓 Вчіться послідовно** — не перестрибуйте кроки
4. **🌱 Розвивайтеся поступово** — не намагайтеся вивчити все одразу
5. **🤝 Діліться знаннями** — навчання інших поглиблює ваше розуміння

### 💪 Пам'ятайте:

- Цей роадмап — це дорожня карта, а не жорсткий план
- Адаптуйте його під свої потреби та інтереси
- Фокусуйтеся на розумінні, а не механічному запам'ятовуванні
- Будуйте реальні проекти для практики
- Не бійтеся помилятися — це частина навчання

### 🚀 Наступні кроки:

1. ⬜ Налаштуйте Obsidian та почніть документувати
2. ⬜ Встановіть Linux у віртуальній машині
3. ⬜ Пройдіть інтерактивний курс з Git
4. ⬜ Вивчіть основи мереж
5. ⬜ Почніть працювати з Docker
6. ⬜ Налаштуйте простий моніторинг
7. ⬜ Створіть свій перший CI/CD пайплайн
8. ⬜ Вивчіть основи Python або Go
9. ⬜ Розгорніть свій перший Kubernetes кластер
10. ⬜ Реалізуйте повний DevOps цикл з хмарою

---

**Успіхів у вашій DevOps подорожі! 🎉**

*📅 Оновлено: Жовтень 2025*  
*🌍 Орієнтовано на ринок СНД*