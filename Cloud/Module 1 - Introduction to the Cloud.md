
**Cloud computing** — это **on-demand delivery of IT resources over the internet with pay-as-you-go pricing**. AWS строится на модели **client-server**, глобальной инфраструктуре (**AWS Regions** / **Availability Zones**) и **AWS Shared Responsibility Model** — разделении ответственности за безопасность между AWS и клиентом.

**🔑 Ключевые концепции**

**☁️ Cloud Computing (определение AWS).** **Cloud computing** — **on-demand delivery of IT resources over the internet with pay-as-you-go pricing**. 
Четыре элемента: 
**On-demand delivery** (доступ к ресурсам за секунды, без длительного provisioning), **IT resources** (серверы, storage, базы данных, networking, AI/ML), **Over the internet** (доступ через веб-сервисы, без локального железа), **Pay-as-you-go pricing** (оплата только за потреблённые ресурсы, без фиксированных контрактов).

**🖥️ Client-Server Model.** **Client-server model** — клиент отправляет запрос, сервер проверяет права и возвращает ответ. В AWS «сервер» — это **virtual server** (например, **Amazon Elastic Compute Cloud (Amazon EC2)**), а не физическая машина. **Permissions** — сервер проверяет легитимность запроса перед ответом.

**🏗️ Cloud Deployment Types.** **Cloud-based deployment** — всё в облаке: миграция существующих ресурсов, новые приложения или комбинация. **On-premises deployment** — **legacy IT infrastructure** плюс **virtualization** и **resource management tools**; не даёт облачных выгод, но даёт выделенные ресурсы и низкую задержку. **Hybrid deployment** — комбинация cloud-based и on-premises.

**🎯 Six Key Benefits of the AWS Cloud.** **Trade fixed expense for variable expense** (фиксированные затраты становятся переменными). **Benefit from massive economies of scale** (экономия за счёт масштаба снижает цены). **Stop guessing capacity** (не нужно угадывать ёмкость, масштабирование по требованию). **Increase speed and agility** (ресурсы за минуты, а не недели). **Stop spending money running and maintaining data centers** (фокус на бизнесе, а не на стойках). **Go global in minutes** (развёртывание в нескольких **AWS Regions** за клики).

**🌍 AWS Global Infrastructure.** **AWS Region** — физическая географическая область, содержит минимум **3 Availability Zones**. **Availability Zone (AZ)** — один или несколько **data centers** с независимыми **power**, **networking** и **connectivity**; **AZ** физически изолированы друг от друга. Рекомендуется распределять ресурсы по нескольким **AZ** для **High Availability (HA)** и **Fault Tolerance (FT)**: если одна **AZ** падает, приложение продолжает работать.

**🔐 AWS Shared Responsibility Model.** **AWS Shared Responsibility Model** — разделение обязанностей по безопасности между AWS и клиентом. **AWS** отвечает за **Security OF the cloud**: физическая инфраструктура, **host operating system**, **virtualization layer**, **network**, **facilities**. **Customer** отвечает за **Security IN the cloud**: данные, доступ (**AWS Identity and Access Management (IAM)**), конфигурация приложений, патчи **guest operating system** (для **Infrastructure as a Service (IaaS)**). **Shared responsibilities** зависят от сервиса. Для **Amazon EC2** клиент отвечает за **guest OS** и приложения. Для **Amazon Simple Storage Service (Amazon S3)** и **Amazon DynamoDB** AWS берёт на себя **OS** и платформу, клиент отвечает только за данные и доступ. Правило: **managed services** дают меньше ответственности клиенту, **IaaS** — больше.

**📦 Service Models.** **Infrastructure as a Service (IaaS)** — клиент управляет **guest OS**, приложениями, **security groups**; AWS управляет физикой и **hypervisor**. Пример: **Amazon EC2**. **Platform as a Service (PaaS)** — AWS управляет **OS** и платформой, клиент управляет приложениями и данными. Пример: **AWS Elastic Beanstalk**. **Software as a Service (SaaS)** — AWS управляет всем, клиент только использует. Пример: **Amazon WorkSpaces**. Правило: чем абстрактнее сервис, тем меньше ответственности у клиента.

## ⚙️ AWS-сервисы и элементы

| Элемент/Сервис | 🎯 Зачем | 🔁 Отличие от соседнего |
|---|---|---|
| ⚙️ **Amazon Elastic Compute Cloud (Amazon EC2)** | Виртуальные серверы (**IaaS**) | Полный контроль над **guest OS**, больше ответственности клиента |
| 🌍 **AWS Region** | Географическая область | Содержит минимум **3 Availability Zones** |
| 🌍 **Availability Zone (AZ)** | Один или несколько **data centers** | Изолированы по **power**, **networking**, **connectivity** |
| 🌍 **Edge Location** | Точка присутствия для **content delivery** | Не для **compute**, а для **Amazon CloudFront** |
| 🔐 **AWS Identity and Access Management (IAM)** | Управление доступом | Ответственность клиента (**security IN the cloud**) |
| 📦 **Amazon Simple Storage Service (Amazon S3)** | Object storage | AWS управляет **OS**, клиент отвечает за данные и доступ |

## ⚠️ Экзаменационные ловушки

**AWS Shared Responsibility Model — главная путаница.** Запомни направление: **Security OF the cloud** — это AWS, **Security IN the cloud** — это Customer. Физическая безопасность дата-центра — AWS. Данные и **IAM-политики** — Customer. Патчинг **guest operating system** для **Amazon EC2** — Customer. Патчинг **OS** для **Amazon RDS** и **Amazon S3** — AWS.

**«Least operational overhead».** Выбирай **managed services** и **serverless**: чем абстрактнее сервис, тем меньше ответственности у клиента.

**Availability Zone (AZ) не равно Data Center.** **AZ** — это один или несколько **data centers**, **Region** — группа **AZ**. Вопрос «Сколько минимум **AZ** в **Region**?» имеет ответ **3**.

**On-premises deployment не даёт облачных выгод.** Если вопрос про **benefit of cloud**, **on-premises** всегда неправильный ответ.

**Pay-as-you-go pricing не значит бесплатно.** Это модель оплаты за потребление, а не **AWS Free Tier**.

**Six Key Benefits of the AWS Cloud** могут спрашивать в формулировке «Which is NOT a benefit?». **On-premises**-подобные ответы (фиксированная ёмкость, ручное обслуживание стоек) не являются преимуществами.

**IaaS vs PaaS vs SaaS.** **IaaS** — клиент управляет **guest OS** (**Amazon EC2**). **PaaS** — AWS управляет **OS**, клиент управляет приложениями (**AWS Elastic Beanstalk**). **SaaS** — клиент только использует (**Amazon WorkSpaces**).

## ❓ Экзаменационные вопросы

**❓ Q1.** A company wants to run its application in the AWS Cloud and needs to ensure the application remains available if a single data center fails. Which AWS Global Infrastructure component should the company use to meet this requirement?
A. Multiple AWS Regions
B. Multiple Availability Zones within a Region
C. A single Availability Zone with redundant power
D. AWS Edge Locations

**Ответ: B.** Несколько **Availability Zones (AZ)** в одном **Region** обеспечивают изоляцию отказов на уровне дата-центров. Multiple AWS Regions избыточно для отказа одного **data center** и увеличивает задержку и стоимость. **AWS Edge Locations** предназначены для **content delivery** (**Amazon CloudFront**), не для **compute**.

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