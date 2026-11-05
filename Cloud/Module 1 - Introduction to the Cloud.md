🧠**Cloud computing** — это **on-demand delivery of IT resources over the internet with pay-as-you-go pricing**. Простыми словами: ты больше не покупаешь серверы и не строишь дата-центр. Ты через интернет **арендуешь** вычислительные ресурсы у AWS и платишь **только за то, что реально использовал**. AWS — это платформа, которая сдаёт эти ресурсы в аренду.

**🔑 Ключевые концепции**

**Шаг 1. Client-Server Model — с чего всё начинается.** Когда ты открываешь приложение или сайт, твоё устройство — это **Client**. Оно отправляет **Request** (запрос). На другой стороне есть машина, которая этот запрос обрабатывает и возвращает **Response** (ответ) — это **Server**. В видео AWS это объясняют через кофейню: ты (Client) заказываешь кофе (Request), бариста (Server) проверяет, что ты заплатил (**Permissions** — проверка прав), и отдаёт напиток (Response). В AWS роль бариста играет не физический сервер, а **virtual server** — например, **Amazon Elastic Compute Cloud (Amazon EC2)**. Ты не думаешь о том, где стоит эта машина и кто её обслуживает — ты просто просишь её у AWS, и она появляется за секунды.

**Шаг 2. Почему это вообще стало возможно — on-demand и pay-as-you-go.** В старом мире (on-premises) ты покупал железо заранее, ждал доставку, ставил в стойку, настраивал. Это недели и большие деньги вперёд. В AWS ты **provision** (выделяешь) ресурсы за секунды. Когда ресурс больше не нужен — **deprovision** (освобождаешь), и оплата **прекращается сразу**. Ключевая мысль: в AWS нет предоплаты и нет фиксированных контрактов. Ты платишь за потребление. В видео это сравнивают с сотрудниками кофейни: бариста получают деньги только за отработанные часы, а не за то, что просто сидят на случай, если придёт гость.

**Шаг 3. Определение Cloud Computing (четыре элемента).** AWS даёт формальное определение: **on-demand delivery of IT resources over the internet with pay-as-you-go pricing**. Разберём по частям. **On-demand delivery** — ресурсы доступны за секунды, без долгого provisioning. **IT resources** — это не только серверы, но и storage, базы данных, networking, AI/ML инструменты. **Over the internet** — ты получаешь доступ через веб-сервисы, а не через локальное железо. **Pay-as-you-go pricing** — оплата только за фактически потреблённые ресурсы, без долгосрочных обязательств. Все четыре элемента вместе и есть облако.

**Шаг 4. Cloud Deployment Types — где именно ты разворачиваешь ресурсы.** Есть три модели. 
**Cloud-based deployment** — всё живёт в облаке: ты либо переносишь старые ресурсы в AWS, либо строишь новые приложения целиком в облаке, либо комбинируешь. 
**On-premises deployment** — это твой собственный дата-центр с **legacy IT infrastructure**, где ты используешь **virtualization** и **resource management tools**, чтобы хоть как-то повысить утилизацию. Важно: on-premises **не даёт** облачных выгод (эластичности, pay-as-you-go), но даёт выделенные ресурсы и низкую задержку. **Hybrid deployment** — комбинация: часть в облаке, часть on-premises. На экзамене спрашивают: «какой тип даёт dedicated resources и low latency, но не даёт cloud benefits?» — ответ **on-premises**.

**Шаг 5. Six Key Benefits of the AWS Cloud — шесть причин, зачем идти в облако.** Это официальный список AWS, его спрашивают. 
1. **Trade fixed expense for variable expense** — ты меняешь фиксированные затраты (купил сервер на 5 лет) на переменные (платишь за часы использования). 
2. **Benefit from massive economies of scale** — AWS покупает железо в огромных объёмах, поэтому цена за единицу ниже, и эта экономия передаётся тебе.
3. **Stop guessing capacity** — ты больше не гадаешь, сколько серверов купить «на будущее». Ты масштабируешься по фактическому спросу. 
4. **Increase speed and agility** — ресурсы появляются за минуты, а не за недели, поэтому команда быстрее выпускает продукт.  
5. **Stop spending money running and maintaining data centers** — ты не тратишь деньги и людей на обслуживание стоек, электричество, охлаждение. 
6. **Go global in minutes** — ты разворачиваешь приложение в нескольких **AWS Regions** за клики, а не за месяцы строительства.

**Шаг 6. AWS Global Infrastructure — физическая основа облака.** **AWS Region** — это географическая область (например, Ирландия, Сингапур). В каждом **Region** минимум **3 Availability Zones**. **Availability Zone (AZ)** — это один или несколько **data centers** с независимыми **power** (питание), **networking** (сеть) и **connectivity** (подключение). AZ физически изолированы друг от друга — если одна AZ выходит из строя, остальные продолжают работать. Поэтому AWS рекомендует распределять ресурсы по **нескольким AZ**: это даёт **High Availability (HA)** и **Fault Tolerance (FT)**. На экзамене любят вопрос: «Сколько минимум AZ в Region?» — ответ **3**. И ещё: **AZ не равно data center**. AZ — это один *или несколько* дата-центров.

**Шаг 7. AWS Shared Responsibility Model — кто за что отвечает.** Это, пожалуй, самая частая тема на экзамене. Модель делит ответственность на две части. **AWS** отвечает за **Security OF the cloud** — то есть за физическую инфраструктуру: дата-центры, железо, **host operating system**, **virtualization layer**, сеть, facilities. **Customer** отвечает за **Security IN the cloud** — то есть за то, что ты кладёшь *внутрь*: данные, доступ (**AWS Identity and Access Management (IAM)**), конфигурацию приложений, патчи **guest operating system** (для **Infrastructure as a Service (IaaS)**). Есть ещё **shared responsibilities**, которые зависят от сервиса. Для **Amazon EC2** клиент отвечает за **guest OS** и приложения. Для **Amazon Simple Storage Service (Amazon S3)** и **Amazon DynamoDB** AWS берёт на себя OS и платформу, а клиент отвечает только за данные и доступ. Запомни направление: **OF the cloud — AWS**, **IN the cloud — Customer**.

**Шаг 8. Service Models — IaaS, PaaS, SaaS.** Это про то, насколько глубоко ты управляешь сервисом. **Infrastructure as a Service (IaaS)** — ты управляешь **guest OS**, приложениями, **security groups**; AWS управляет физикой и **hypervisor**. Пример: **Amazon EC2**. **Platform as a Service (PaaS)** — AWS управляет OS и платформой, ты управляешь приложениями и данными. Пример: **AWS Elastic Beanstalk**. **Software as a Service (SaaS)** — AWS управляет всем, ты просто пользуешься. Пример: **Amazon WorkSpaces**. Правило простое: чем абстрактнее сервис, тем меньше ответственности у клиента.

## ⚙️ AWS-сервисы и элементы

| Элемент/Сервис | 🎯 Зачем | 🔁 Отличие от соседнего |
|---|---|---|
| ⚙️ **Amazon Elastic Compute Cloud (Amazon EC2)** | Виртуальные серверы (**IaaS**) | Полный контроль над **guest OS**, поэтому больше ответственности у клиента |
| 🌍 **AWS Region** | Географическая область | Содержит минимум **3 Availability Zones** |
| 🌍 **Availability Zone (AZ)** | Один или несколько **data centers** | Изолированы по **power**, **networking**, **connectivity** |
| 🌍 **Edge Location** | Точка присутствия для **content delivery** | Не для **compute**, а для **Amazon CloudFront** |
| 🔐 **AWS Identity and Access Management (IAM)** | Управление доступом | Ответственность клиента (**security IN the cloud**) |
| 📦 **Amazon Simple Storage Service (Amazon S3)** | Object storage | AWS управляет OS, клиент отвечает за данные и доступ |

## ⚠️ Экзаменационные ловушки

**AWS Shared Responsibility Model — главная путаница.** Запомни направление: **Security OF the cloud** — это AWS, **Security IN the cloud** — это Customer. Физическая безопасность дата-центра — AWS. Данные и **IAM-политики** — Customer. Патчинг **guest operating system** для **Amazon EC2** — Customer. Патчинг OS для **Amazon RDS** и **Amazon S3** — AWS.

**«Least operational overhead».** Когда в вопросе просят «наименьшие операционные усилия», выбирай **managed services** и **serverless**. Чем абстрактнее сервис, тем меньше ответственности у клиента.

**Availability Zone (AZ) не равно Data Center.** AZ — это один *или несколько* data centers. Region — группа AZ. Вопрос «Сколько минимум AZ в Region?» — ответ **3**.

**On-premises deployment не даёт облачных выгод.** Если вопрос про **benefit of cloud**, on-premises всегда неправильный ответ.

**Pay-as-you-go pricing не значит бесплатно.** Это модель оплаты за потребление, а не **AWS Free Tier**.

**Six Key Benefits of the AWS Cloud** могут спрашивать в формулировке «Which is NOT a benefit?». On-premises-подобные ответы (фиксированная ёмкость, ручное обслуживание стоек) не являются преимуществами.

**IaaS vs PaaS vs SaaS.** IaaS — клиент управляет **guest OS** (**Amazon EC2**). PaaS — AWS управляет OS, клиент управляет приложениями (**AWS Elastic Beanstalk**). SaaS — клиент только использует (**Amazon WorkSpaces**).

## ❓ Экзаменационные вопросы

**❓ Q1.** A company wants to run its application in the AWS Cloud and needs to ensure the application remains available if a single data center fails. Which AWS Global Infrastructure component should the company use to meet this requirement?
A. Multiple AWS Regions
B. Multiple Availability Zones within a Region
C. A single Availability Zone with redundant power
D. AWS Edge Locations

**Ответ: B.** Несколько **Availability Zones (AZ)** в одном **Region** обеспечивают изоляцию отказов на уровне дата-центров. Multiple AWS Regions избыточно для отказа одного data center и увеличивает задержку и стоимость. **AWS Edge Locations** предназначены для **content delivery** (**Amazon CloudFront**), не для **compute**.

**❓ Q2.** According to the AWS Shared Responsibility Model, which task is the customer's responsibility when using Amazon EC2?
A. Patching the underlying hypervisor
B. Securing the physical data center
C. Patching the guest operating system
D. Maintaining the network infrastructure

**Ответ: C.** **Amazon EC2** — это **IaaS**. Клиент отвечает за **guest operating system**, приложения и **security groups**. **Hypervisor**, физика и сеть — ответственность AWS.

**❓ Q3.** Which AWS Cloud benefit describes the ability to deploy applications to multiple geographic locations with minimal effort?
A. Stop guessing capacity
B. Trade fixed expense for variable expense
C. Go global in minutes
D. Increase speed and agility

**Ответ: C.** **Go global in minutes** — именно про глобальное развёртывание через **AWS Regions**. **Increase speed and agility** — про скорость выделения ресурсов внутри одного окружения.

**❓ Q4.** (Multiple Response) Which of the following are components of the AWS Shared Responsibility Model? (Choose two.)
A. AWS is responsible for security of the cloud.
B. The customer is responsible for security in the cloud.
C. AWS is responsible for encrypting all customer data by default.
D. The customer is responsible for physical security of AWS data centers.
E. AWS is responsible for customer IAM policy design.

**Ответ: A, B.** AWS отвечает за **Security OF the cloud** (инфраструктура), клиент — за **Security IN the cloud** (данные, доступ, конфигурация). Шифрование данных — ответственность клиента, хотя AWS предоставляет инструменты. Физическая безопасность — AWS. Дизайн **IAM-политик** — клиент.

**❓ Q5.** A startup wants to avoid large upfront infrastructure investments and pay only for the compute resources it consumes. Which cloud computing characteristic supports this?
A. High availability
B. Pay-as-you-go pricing
C. Global reach
D. Fault tolerance

**Ответ: B.** **Pay-as-you-go pricing** — оплата за потребление, без предоплаты. Это прямое определение из модели облачных вычислений.

**❓ Q6.** Which cloud deployment type provides dedicated resources and low latency but does NOT provide most cloud computing benefits?
A. Cloud-based deployment
B. On-premises deployment
C. Hybrid deployment
D. Multi-Region deployment

**Ответ: B.** **On-premises deployment** даёт выделенные ресурсы и низкую задержку, но не даёт облачных выгод (эластичность, pay-as-you-go). **Cloud-based** и **Hybrid** дают облачные преимущества.

________________________________________________________________________
Paths: [[Cloud]]
Tags: #Cloud 