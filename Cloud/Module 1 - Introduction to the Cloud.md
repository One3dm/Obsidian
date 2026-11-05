**AWS Cloud Practitioner Essentials: Основы облачных концепций**

Облачные вычисления — это **поставка IT-ресурсов по требованию через интернет с оплатой по факту использования**. AWS строится на модели клиент-сервер, глобальной инфраструктуре (Regions/AZ) и разделении ответственности за безопасность между AWS и клиентом.

🔑 Ключевые концепции

- 🧠 **Cloud Computing (определение AWS)**: On-demand delivery of IT resources over the internet with pay-as-you-go pricing. Четыре элемента: по требованию, IT-ресурсы, через интернет, оплата по мере использования.
- 🧠 **Client-Server Model**: Клиент отправляет запрос → сервер проверяет права → возвращает ответ. В AWS «сервер» — это виртуальный сервер (EC2), а не физическая машина.
- 💰 **Модель оплаты**: Только за то, что используешь. Provisioning — секунды/минуты. Deprovisioning — немедленно прекращается оплата. Нет предоплаты, нет фиксированных контрактов.
- 🔁 **Типы развертывания**:
  - ☁️ **Cloud-based**: Всё в облаке (миграция или новое построение).
  - 🏢 **On-premises**: Legacy IT + виртуализация. ❌ Нет облачных выгод. ✅ Выделенные ресурсы и низкая задержка.
  - 🔀 **Hybrid**: Комбинация облака и on-premises.
- 🎯 **Шесть преимуществ AWS Cloud** (официальный список):
  1. 💰 Trade fixed expense for variable expense.
  2. 📉 Benefit from massive economies of scale.
  3. 📈 Stop guessing capacity.
  4. ⚡ Increase speed and agility.
  5. 🏢 Stop spending money running and maintaining data centers.
  6. 🌍 Go global in minutes.
- 🌍 **AWS Global Infrastructure**:
  - 🔑 **Region** — географическая область. Минимум **3 Availability Zones** в каждом Region.
  - 🔑 **Availability Zone (AZ)** — один или несколько дата-центров с независимыми питанием, сетью и подключением. AZ изолированы физически.
  - ✅ **Рекомендация**: распределять ресурсы по **нескольким AZ** для High Availability и Fault Tolerance. Если одна AZ падает — приложение продолжает работать.
- 🔐 **AWS Shared Responsibility Model**:
  - 🏢 **AWS**: Security **OF** the cloud — физическая инфраструктура, хост-ОС, виртуализация, сеть, facilities.
  - 👤 **Customer**: Security **IN** the cloud — данные, доступ (IAM), конфигурация приложений, патчи гостевой ОС (для IaaS).
  - 🔀 **Shared**: Зависит от сервиса. Для EC2 клиент отвечает за ОС и приложения. Для S3/DynamoDB AWS берёт на себя ОС и платформу, клиент — только данные и доступ.
  - 🎯 **Правило**: Managed services → меньше ответственности клиента. IaaS → больше ответственности клиента.

## ⚙️ AWS-сервисы (связи в этом модуле)

| Концепция | Сервис/элемент | Логика |
|---|---|---|
| Client-Server | ⚙️ Amazon EC2 | Виртуальный сервер в облаке |
| Global Infrastructure | 🌍 Regions, AZs, Edge Locations | База для HA/FT и низкой задержки |
| Shared Responsibility (IaaS) | 🔐 Amazon EC2 | Клиент управляет гостевой ОС и патчами |
| Shared Responsibility (абстрактный) | 📦 Amazon S3, DynamoDB | AWS управляет платформой, клиент — данными |
| High Availability | ✅ Multi-AZ deployment | Отказоустойчивость через изоляцию AZ |

## ⚠️ Экзаменационные ловушки

- 🪤 **Shared Responsibility — главная путаница**. Вопросы дают сценарий и спрашивают: «Кто отвечает за X?» Запомни направление: 🔐 физическая безопасность дата-центра → **AWS**. 👤 данные и IAM-политики → **Customer**. Патчинг гостевой ОС EC2 → **Customer**. Патчинг ОС для RDS/S3 → **AWS**.
- 🎯 **«Least operational overhead»** → выбирай managed/serverless. 📦 Чем абстрактнее сервис, тем меньше ответственности у клиента.
- 🪤 **AZ ≠ Data Center**. AZ — это *один или несколько* дата-центров. Region — группа AZ. Вопрос: «Сколько минимум AZ в Region?» → **3**.
- ❌ **On-premises не даёт облачных выгод**. Если вопрос про benefit of cloud, on-premises — всегда неправильный ответ.
- 💰 **«Pay-as-you-go» ≠ бесплатно**. Это модель оплаты за потребление, а не Free Tier.
- 🎯 **Шесть преимуществ** могут спрашивать в формулировке «Which is NOT a benefit?» — on-premises-подобные ответы (фиксированная ёмкость, ручное обслуживание стоек) не являются преимуществами.

## ❓ Экзаменационные вопросы

**❓ Q1.** A company wants to run its application in the AWS Cloud and needs to ensure the application remains available if a single data center fails. Which AWS Global Infrastructure component should the company use to meet this requirement?
A. Multiple AWS Regions
B. Multiple Availability Zones within a Region
C. A single Availability Zone with redundant power
D. AWS Edge Locations

**✅ Ответ: B.** Несколько AZ в одном Region обеспечивают изоляцию отказов на уровне дата-центров. ❌ Multiple Regions — избыточно для отказа одного DC и увеличивает задержку/стоимость. ❌ Edge Locations — для CDN, не для compute.

**❓ Q2.** According to the AWS Shared Responsibility Model, which task is the customer's responsibility when using Amazon EC2?
A. Patching the underlying hypervisor
B. Securing the physical data center
C. Patching the guest operating system
D. Maintaining the network infrastructure

**✅ Ответ: C.** EC2 — IaaS. Клиент отвечает за гостевую ОС, приложения и security groups. ❌ Гипервизор, физика и сеть — AWS.

**❓ Q3.** Which AWS Cloud benefit describes the ability to deploy applications to multiple geographic locations with minimal effort?
A. Stop guessing capacity
B. Trade fixed expense for variable expense
C. Go global in minutes
D. Increase speed and agility

**✅ Ответ: C.** «Go global in minutes» — именно про глобальное развёртывание через Regions. ❌ «Speed and agility» — про скорость выделения ресурсов внутри одного окружения.

**❓ Q4.** (Multiple Response) Which of the following are components of the AWS Shared Responsibility Model? (Choose two.)
A. AWS is responsible for security of the cloud.
B. The customer is responsible for security in the cloud.
C. AWS is responsible for encrypting all customer data by default.
D. The customer is responsible for physical security of AWS data centers.
E. AWS is responsible for customer IAM policy design.

**✅ Ответ: A, B.** 🔐 AWS — security OF the cloud (инфраструктура). 👤 Customer — security IN the cloud (данные, доступ, конфигурация). ❌ Шифрование данных — ответственность клиента (хотя AWS предоставляет инструменты). ❌ Физическая безопасность — AWS. ❌ Дизайн IAM-политик — клиент.

**❓ Q5.** A startup wants to avoid large upfront infrastructure investments and pay only for the compute resources it consumes. Which cloud computing characteristic supports this?
A. High availability
B. Pay-as-you-go pricing
C. Global reach
D. Fault tolerance

**✅ Ответ: B.** 💰 Pay-as-you-go = оплата за потребление, без предоплаты. Это прямое определение из модели облачных вычислений.

## 🔁 Связи
#aws/domain1 #aws/domain2 #aws/global-infrastructure #aws/shared-responsibility #aws/cloud-concepts #aws/regions #aws/availability-zones #aws/ec2 #aws/s3

## ❓ Мой вопрос
(вставь сюда свой вопрос или сомнение)

---

Присылай следующий модуль — оформлю в том же стиле с эмодзи-мнемоникой.
________________________________________________________________________
Paths: [[Cloud]]
Tags: #Cloud 