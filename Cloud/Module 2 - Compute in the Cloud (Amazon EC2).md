**Compute** — это вычислительная мощность, нужная для запуска приложений. В AWS ты не покупаешь физические серверы, а **арендуешь виртуальные** и платишь по потреблению. Модуль 2 — это **Amazon Elastic Compute Cloud (Amazon EC2)** (виртуальные серверы), модели его оплаты, автоматическое масштабирование (**Amazon EC2 Auto Scaling**), распределение трафика (**Elastic Load Balancing, ELB**), развязка компонентов через сообщения (**Amazon SQS**, **Amazon SNS**, **Amazon EventBridge**) и дополнительные вычислительные опции без управления серверами (**AWS Lambda**, **AWS Fargate**, **Amazon ECS**, **Amazon EKS**, **AWS Batch**).

**🔑 Ключевые концепции**

**Шаг 1. Что такое Amazon EC2 и зачем он нужен.** 
**Amazon Elastic Compute Cloud (Amazon EC2)** — это сервис, который даёт тебе **virtual server** в облаке. Раньше, чтобы запустить приложение, ты покупал физический сервер, ждал доставку, ставил его в стойку, настраивал сеть и ОС — это недели и большие деньги вперёд. С EC2 ты в консоли выбираешь параметры и через минуты получаешь работающую машину. Не нужна — выключаешь, и оплата прекращается. EC2 — это **Infrastructure as a Service (IaaS)**, поэтому **клиент** отвечает за **guest operating system**, патчи, **security groups** и приложения, а **AWS** — за физику, **hypervisor** и дата-центры.

**Шаг 2. Как запустить EC2: три шага.** 
1. Выбрать **Amazon Machine Image (AMI)**. AMI — это шаблон, в котором уже есть операционная система и, возможно, нужный софт. Один AMI можно использовать, чтобы запустить много одинаковых инстансов — это даёт **repeatability** (повторяемость) и упрощает масштабирование. AMI можно создать самому, взять готовый от AWS или купить в **AWS Marketplace**. 
2. Выбрать **Instance Type**, то есть сколько CPU, памяти и сетевой пропускной способности будет у машины. 
3. Подключиться: для Linux используется **SSH**, для Windows — **Remote Desktop Protocol (RDP)**, либо **AWS Systems Manager** как более безопасный вариант.

**Шаг 3. Пять семейств Instance Types — запоминай «на что ориентирован». 
General purpose** — сбалансированные, CPU и память примерно поровну; подходят для веб-серверов, репозиториев кода, когда нагрузка ещё непонятна. 
**Compute optimized** — упор в CPU; подходят для игровых серверов, 
**High Performance Computing (HPC)**, машинного обучения, научного моделирования. 
**Memory optimized** — упор в память; подходят для больших датацентров, аналитики, баз данных. 
**Accelerated computing** — используют **Graphics Processing Unit (GPU)** и другие аппаратные ускорители; подходят для графики, машинного обучения, вычислений с плавающей точкой. 
**Storage optimized** — упор в высокую производительность локального диска; подходят для больших баз данных, **data warehousing**, I/O-интенсивных приложений.

**Шаг 4. Кто за что отвечает при работе с EC2 (Shared Responsibility).** Поскольку EC2 — это **unmanaged service** и **IaaS**, клиент выполняет всю security-настройку: конфигурирует **security groups** (это firewall), управляет **guest operating system**, ставит обновления, настраивает приложения. AWS отвечает за **Security OF the cloud** — физическую инфраструктуру, **hypervisor**, сеть и дата-центры. На экзамене это формулируют так: «патчинг **guest operating system** на EC2 — чья ответственность?» Ответ — **Customer**.

**Шаг 5. Три способа управлять AWS (все через API).** Любое действие в AWS — запуск инстанса, остановка, изменение настроек — это вызов **API**. Есть три способа его сделать. 
**AWS Management Console** — веб-интерфейс, визуально, удобно для людей.
**AWS Command Line Interface (AWS CLI)** — командная строка, удобно для скриптов и автоматизации на Windows, macOS и Linux. 
**AWS Software Development Kit (AWS SDK)** — библиотеки для языков программирования (C++, Java, .NET, Python и др.), нужны, когда ты встраиваешь вызовы AWS прямо в код приложения.

**Шаг 6. Модели оплаты EC2 — самая экзаменуемая часть.** 
**On-Demand Instances** — платишь за потребление, без обязательств; подходит для непредсказуемых нагрузок. 
**Reserved Instances (RI)** — скидка до **75%** за обязательство на 1 или 3 года; для **predictable** нагрузок; скидка гибко применяется к другим размерам в том же семействе и к другим **Availability Zones** в том же **Region**. 
**Spot Instances** — скидка до **90%**, но AWS может забрать инстанс; только для **fault-tolerant** задач, которые могут перезапуститься. 
**Savings Plans** — скидка до **72%** за обязательство по **$/час** на 1 или 3 года; гибче RI, потому что покрывает **Amazon EC2**, **AWS Fargate**, **AWS Lambda** и **Amazon SageMaker AI**. 
**Dedicated Hosts** — целый физический сервер в твоё распоряжение; нужно для **Bring Your Own License (BYOL)** и compliance. 
**Dedicated Instances** — изоляция от других аккаунтов, но ты не выбираешь, на каком именно сервере работаешь. 
**Capacity Reservations** — резерв capacity в конкретной **Availability Zone**; оплата идёт по **On-Demand** rate, **даже если инстансы не запущены**; нужно для критичных нагрузок с жёсткими требованиями к capacity.

**Шаг 7. Scalability vs Elasticity — два разных слова, которые часто путают.** **Scalability** — это способность системы **расти** со временем, долгосрочное планирование. Рост бывает двух видов: **scale up** (дать существующей машине больше мощности) и **scale out** (добавить машин). 
**Elasticity** — это **автоматическое** масштабирование вверх и вниз в ответ на **real-time demand**: при пике добавляем, при спаде убираем. На экзамене: «автоматически добавляет инстансы при пике нагрузки» — это **elasticity**.

**Шаг 8. Amazon EC2 Auto Scaling — как работает.**
**Amazon EC2 Auto Scaling** автоматически регулирует количество инстансов в группе (**Auto Scaling group**) в ответ на изменения нагрузки. Есть **dynamic scaling** (реакция в реальном времени) и **predictive scaling** (заранее по прогнозу). В группе три настройки: **minimum capacity** — нижний порог, столько инстансов запускается сразу после создания группы; **desired capacity** — идеальное количество, которое Auto Scaling старается поддерживать (если не указать, по умолчанию равно **minimum**); **maximum capacity** — верхний лимит, чтобы не разориться на масштабировании. Платишь только за фактические EC2-инстансы, сам Auto Scaling бесплатен.

**Шаг 9. Elastic Load Balancing (ELB) — зачем он нужен.**
**Elastic Load Balancing (ELB)** автоматически распределяет входящий трафик между несколькими ресурсами, чтобы ни одна машина не была перегружена. Клиент обращается к DNS-имени балансировщика — это **единая точка контакта**, а не к конкретному EC2. ELB выполняет **health checks** и отправляет трафик только в здоровые targets. Когда ELB связан с **Auto Scaling group**, новые инстансы **автоматически регистрируются** в балансировщике, а завершённые — автоматически дерегистрируются. Важно: **ELB и Auto Scaling — разные сервисы**. Auto Scaling регулирует **количество** инстансов, ELB распределяет **трафик**. Вместе они дают **High Availability** и **Fault Tolerance**.

**Шаг 10. Четыре типа ELB.** **Application Load Balancer (ALB)** работает на **Layer 7** (HTTP/HTTPS), умеет path-based routing, подходит для веб-приложений и микросервисов. 
**Network Load Balancer (NLB)** работает на **Layer 4** (TCP/UDP), даёт очень низкую задержку, сохраняет **source IP** клиента и имеет фиксированный IP — для performance-critical сценариев. 
**Gateway Load Balancer (GWLB)** работает на **Layer 3** (IP) и нужен для third-party appliances вроде firewall и IDS/IPS.
**Classic Load Balancer (CLB)** — legacy, для новых архитектур не рекомендуется.

**Шаг 11. Зачем развязывать компоненты (Decoupling).** В **monolithic** архитектуре компоненты **tightly coupled**: если один упал, падает всё приложение. В **microservices** архитектуре компоненты **loosely coupled**: если один упал, остальные работают. Для этого в AWS есть три сервиса обмена сообщениями. **Amazon SQS** — очередь, **pull**-модель: consumer сам забирает сообщения, они хранятся до 14 дней, ничего не теряется, если consumer временно недоступен. **Amazon SNS** — **publish-subscribe**, **push**-модель: publisher отправляет в topic, subscribers (email, SMS, SQS, Lambda и др.) получают сразу; сообщения не хранятся. **Amazon EventBridge** — **event-driven** маршрутизация: правила (**rules**) фильтруют события по содержимому и направляют в targets; хорошо подходит для реакций на события AWS и SaaS.

**Шаг 12. SQS: Standard vs FIFO.** **Standard queue** даёт неограниченный throughput, но **at-least-once** delivery (возможны дубликаты) и **best-effort ordering** (порядок не гарантирован). **FIFO queue** даёт **exactly-once processing** и строгий порядок First-In-First-Out, но throughput ограничен. На экзамене: «порядок транзакций критичен» — это **FIFO**; «максимальный throughput, порядок не важен» — это **Standard**.

**Шаг 13. Дополнительные вычислительные опции (Serverless и контейнеры).** **AWS Lambda** — **serverless**: ты загружаешь код, он запускается по событию (например, загрузка файла в **Amazon S3**), платишь за время выполнения в миллисекундах. Серверами управлять не нужно. **AWS Fargate** — **serverless** compute для контейнеров: работает с **Amazon ECS** и **Amazon EKS**, не нужно provisioning EC2 и управление кластером, платишь за выделенные vCPU и память. **Amazon ECS** — AWS-native оркестрация контейнеров. **Amazon EKS** — управляемый **Kubernetes**, если нужен стандартный K8s API и экосистема. **AWS Batch** — полностью управляемый batch computing: автоматически выделяет compute под объём задач, работает поверх ECS/EKS/Fargate/EC2. Ключевое различие: **Lambda** — event-driven и короткие задачи; **Fargate** — контейнеры и длительные процессы, платишь за provisioned ресурсы, даже если контейнер простаивает.

## ⚙️ AWS-сервисы и элементы

| Сервис/элемент | 🎯 Зачем | 🔁 Отличие от соседнего |
|---|---|---|
| ⚙️ **Amazon Elastic Compute Cloud (Amazon EC2)** | Виртуальные серверы (**IaaS**) | Полный контроль над **guest operating system**, поэтому больше ответственности у клиента |
| ⚙️ **Amazon EC2 Auto Scaling** | Автоматически регулирует количество инстансов | Не распределяет трафик, а создаёт и удаляет ресурсы |
| ⚙️ **Elastic Load Balancing (ELB)** | Распределяет входящий трафик между targets | Не масштабирует инстансы, а направляет трафик |
| ⚙️ **Application Load Balancer (ALB)** | HTTP/HTTPS, path-based routing | Layer 7, для веб-приложений |
| ⚙️ **Network Load Balancer (NLB)** | TCP/UDP, низкая задержка, фиксированный IP | Layer 4, для performance-critical |
| ⚙️ **Gateway Load Balancer (GWLB)** | Трафик к third-party appliances | Layer 3, для security appliances |
| ⚙️ **AWS Management Console** | Визуальное управление AWS | Для людей, не для автоматизации |
| ⚙️ **AWS Command Line Interface (AWS CLI)** | Скрипты и автоматизация | Работает из терминала, кроссплатформенно |
| ⚙️ **AWS Software Development Kit (AWS SDK)** | Интеграция AWS в код | Для разработчиков, язык-специфичные API |
| 📦 **Amazon Machine Image (AMI)** | Шаблон для запуска EC2 | Не сам сервер, а чертёж сервера |
| 💰 **Savings Plans** | Гибкая скидка на compute | Гибче RI: покрывает Fargate и Lambda, не привязан к instance family |
| 💰 **EC2 Capacity Reservations** | Гарантия capacity в Availability Zone | Не скидка, а резерв: платишь даже за простой |
| 📥 **Amazon SQS** | Очередь сообщений, pull-модель | Сообщения ждут consumer, персистентность до 14 дней |
| 📤 **Amazon SNS** | Publish-subscribe, push-модель | Мгновенная доставка подписчикам, без持久ящего хранения |
| ⚡ **Amazon EventBridge** | Event-driven маршрутизация | Фильтрация по содержимому события, интеграция с AWS и SaaS |
| ⚡ **AWS Lambda** | Serverless, код по событию | Не для длительных процессов, оплата за выполнение |
| 📦 **AWS Fargate** | Serverless-контейнеры | Платишь за provisioned ресурсы, не за запросы |
| 📦 **Amazon ECS** | Оркестрация контейнеров, AWS-native | Может работать на Fargate или EC2 |
| 📦 **Amazon EKS** | Управляемый Kubernetes | Для стандартного Kubernetes API |
| ⚡ **AWS Batch** | Пакетные вычисления | Автоматически выделяет compute под batch jobs |

## ⚠️ Экзаменационные ловушки

**ELB и Auto Scaling — не одно и то же.** **ELB** распределяет трафик, **Auto Scaling** добавляет и убирает инстансы. Вопрос «автоматически масштабирует EC2 при пике» — это **Auto Scaling**; «распределяет запросы между инстансами» — это **ELB**.

**Dedicated Hosts vs Dedicated Instances.** **Dedicated Hosts** — целый физический сервер, полный контроль, нужен для **BYOL** и compliance. **Dedicated Instances** — изоляция от других аккаунтов, но ты не выбираешь конкретный сервер; проще и дешевле. Вопрос «нужен контроль над размещением и лицензии» — это **Dedicated Hosts**.

**Spot vs Reserved.** **Spot** даёт скидку до 90%, но инстанс могут забрать; только для **fault-tolerant**. **Reserved** даёт до 75% и гарантию на срок; для **steady-state**.

**Savings Plans vs Reserved Instances.** **Savings Plans** гибче: покрывают **Amazon EC2**, **AWS Fargate**, **AWS Lambda**, **Amazon SageMaker AI**, обязательство в **$/час**. **Reserved Instances** привязаны к instance family и **Region**, хотя внутри family и по **Availability Zones** есть гибкость. Вопрос «нужна гибкость между сервисами» — это **Savings Plans**.

**Capacity Reservations — это не скидка.** Это резерв capacity в конкретной **Availability Zone** с оплатой по **On-Demand** rate, даже если инстансы не запущены. Вопрос «гарантия capacity для критичной нагрузки» — это **Capacity Reservations**; «скидка за обязательство» — это **Savings Plans** или **Reserved Instances**.

**Auto Scaling: desired capacity по умолчанию равен minimum.** Если не указать **desired capacity**, оно будет равно **minimum capacity**. Именно **minimum** запускается сразу после создания группы.

**Scalability vs Elasticity.** **Scalability** — способность расти долгосрочно. **Elasticity** — автоматическое масштабирование вверх и вниз в реальном времени. Вопрос «автоматически добавляет инстансы при пике» — это **elasticity**.

**ALB vs NLB.** **ALB** — HTTP/HTTPS, **Layer 7**, path-based routing; для веб-приложений. **NLB** — TCP/UDP, **Layer 4**, сохраняет **source IP**, фиксированный IP; для низкой задержки. Вопрос «нужен фиксированный IP балансировщика» — это **NLB**.

**ELB не跨 Region.** ELB работает в пределах одного **Region**, но может охватывать несколько **Availability Zones**. Межрегиональная маршрутизация — это **AWS Global Accelerator**.

**ELB не шифрует данные at rest и не кэширует контент.** Кэширование — это **Amazon CloudFront**; шифрование at rest — **Amazon EBS** и **Amazon S3**.

**SQS vs SNS.** **SQS** — очередь: consumer забирает сообщение, оно ждёт, пока его обработают. **SNS** — рассылка: subscriber получает сразу, и если он недоступен, сообщение может потеряться без DLQ. Вопрос «сообщения должны сохраняться, пока consumer не готов» — это **SQS**; «отправить уведомление сразу нескольким получателям» — это **SNS**.

**SQS Standard vs FIFO.** **Standard** — неограниченный throughput, **at-least-once**, **best-effort ordering**. **FIFO** — **exactly-once**, строгий порядок, но throughput ограничен. Вопрос «порядок команд критичен» — это **FIFO**; «максимальный throughput» — это **Standard**.

**EventBridge не хранит события как SQS.** EventBridge не предназначен для持久ящего хранения событий для последующей обработки; SQS хранит до 14 дней.

**SNS не гарантирует доставку, если subscriber недоступен** (без DLQ). SQS гарантирует, что сообщение будет ждать.

**Decoupling = loosely coupled.** Ключевые слова в вопросах: «independent», «failure of one component doesn't affect others», «buffer».

**Lambda vs Fargate.** **Lambda** — event-driven, короткие задачи, оплата за выполнение. **Fargate** — контейнеры, длительные процессы, оплата за provisioned CPU и memory, даже если контейнер простаивает. Вопрос «контейнеры без управления серверами» — это **Fargate**; «код по событию без серверов» — это **Lambda**.

**ECS vs EKS.** **ECS** — AWS-native оркестрация, проще, если Kubernetes не нужен. **EKS** — управляемый **Kubernetes**, нужен для стандартного K8s API и экосистемы. Вопрос «нужен Kubernetes» — это **EKS**.

**AWS Batch не равен EC2.** **AWS Batch** — управляемый сервис для пакетных вычислений, сам выделяет compute под задачи.

**EC2 — unmanaged и IaaS.** Патчинг **guest operating system** — **Customer**; патчинг **hypervisor** — **AWS**; **security groups** — **Customer**. Сравни с **Amazon S3** и **Amazon RDS**, где AWS управляет OS и платформой, а клиент отвечает только за данные и доступ.

## ❓ Экзаменационные вопросы

**❓ Q1.** A company has a steady-state workload that will run continuously for the next three years. The company wants to maximize cost savings and is willing to commit to a specific instance family in a specific Region. Which pricing model is MOST cost-effective?
A. On-Demand Instances
B. Spot Instances
C. Reserved Instances
D. Dedicated Instances

**Ответ: C.** **Reserved Instances** дают скидку до 75% за обязательство на 1–3 года для **predictable** нагрузок. **On-Demand** дороже для долгосрочных нагрузок. **Spot** может быть прерван. **Dedicated** нужен для licensing и compliance, а не для экономии.

**❓ Q2.** A startup runs batch processing jobs that can tolerate interruptions and can restart from checkpoints. Which EC2 pricing option provides the GREATEST cost savings?
A. Savings Plans
B. Reserved Instances
C. Spot Instances
D. On-Demand Instances

**Ответ: C.** **Spot Instances** дают скидку до 90%, но могут быть прерваны. Задача **fault-tolerant**, поэтому Spot — идеальный кандидат. **Savings Plans** и **Reserved Instances** — для **predictable** нагрузок, **On-Demand** дороже.

**❓ Q3.** A company needs to ensure that a specific number of EC2 instances are always available in a particular Availability Zone for a critical application, regardless of demand. Which AWS feature should the company use?
A. EC2 Auto Scaling
B. EC2 Capacity Reservations
C. Spot Instances
D. Savings Plans

**Ответ: B.** **EC2 Capacity Reservations** гарантируют capacity в конкретной **Availability Zone**; оплата идёт по **On-Demand** rate, даже за простой. **Auto Scaling** регулирует количество, но не гарантирует capacity в **Availability Zone**. **Spot** не даёт гарантии, **Savings Plans** — это скидка, а не резерв.

**❓ Q4.** Which task is the customer's responsibility under the AWS Shared Responsibility Model when running applications on Amazon EC2?
A. Patching the hypervisor
B. Configuring security groups
C. Maintaining physical network infrastructure
D. Securing the data center facilities

**Ответ: B.** **Amazon EC2** — это **IaaS**. Клиент отвечает за **security groups**, **guest operating system** и приложения. **Hypervisor**, физика и сеть — ответственность AWS.

**❓ Q5.** A company wants to automate the creation of EC2 instances and integrate AWS services directly into its Python application code. Which method should the company use?
A. AWS Management Console
B. AWS CLI
C. AWS SDK
D. AWS CloudFormation

**Ответ: C.** **AWS SDK** даёт язык-специфичные API (например, Python/boto3) для интеграции в код. **Console** — визуальный интерфейс, **CLI** — для скриптов и терминала, **AWS CloudFormation** — это Infrastructure as Code, а не SDK.

**❓ Q6.** An application experiences unpredictable traffic spikes. The operations team wants the number of EC2 instances to automatically increase during spikes and decrease when traffic drops. Which AWS service should they use?
A. Reserved Instances
B. Amazon EC2 Auto Scaling
C. Dedicated Hosts
D. Savings Plans

**Ответ: B.** **Amazon EC2 Auto Scaling** автоматически регулирует количество инстансов в ответ на demand; **dynamic scaling** работает в реальном времени. **Reserved Instances** и **Savings Plans** — это pricing, **Dedicated Hosts** — физический сервер, а не auto-scaling.

**❓ Q7.** (Multiple Response) Which of the following are benefits of using Amazon EC2 instead of managing physical on-premises servers? (Choose two.)
A. No need to patch the guest operating system
B. Ability to launch instances in minutes instead of weeks
C. Pay only for compute capacity that is consumed
D. AWS is responsible for configuring security groups
E. No responsibility for application-level security

**Ответ: B, C.** EC2 даёт быстрый provisioning и pay-per-use. Патчинг **guest operating system**, **security groups** и безопасность приложений остаются на клиенте.

**❓ Q8.** What is a benefit of using an Elastic Load Balancing (ELB) load balancer with applications running in the AWS Cloud?
A. An ELB will automatically scale resources to meet capacity needs.
B. An ELB can balance traffic across multiple compute resources.
C. An ELB can span multiple AWS Regions.
D. An ELB can balance traffic between multiple internet gateways.

**Ответ: B.** ELB распределяет трафик между несколькими targets. **Auto Scaling** масштабирует, а не ELB. ELB не跨 Region и не балансирует между internet gateways.

**❓ Q9.** A company has a website that experiences unpredictable traffic spikes. The company wants to ensure that incoming requests are distributed across multiple EC2 instances and that unhealthy instances do not receive traffic. Which AWS service provides this functionality?
A. Amazon EC2 Auto Scaling
B. Elastic Load Balancing
C. Amazon CloudFront
D. AWS Global Accelerator

**Ответ: B.** **Elastic Load Balancing** распределяет трафик и выполняет **health checks**, направляя запросы только в здоровые инстансы. **Auto Scaling** регулирует количество, **CloudFront** — это CDN, **AWS Global Accelerator** — оптимизация маршрутизации.

**❓ Q10.** (Multiple Response) Which of the following are purposes of using an AWS Elastic Load Balancer? (Choose two.)
A. Distribute incoming traffic across multiple targets in one or more Availability Zones.
B. Automatically scale EC2 instances based on traffic.
C. Provide a single point of contact for clients.
D. Encrypt data at rest in backend storage.

**Ответ: A, C.** ELB распределяет трафик по targets и даёт единую точку входа. Масштабирование — это **Auto Scaling**; шифрование at rest — это **Amazon EBS** и **Amazon S3**.

**❓ Q11.** A company needs a load balancer that can handle millions of requests per second with extremely low latency and must preserve the client's source IP address. Which ELB type should the company choose?
A. Application Load Balancer
B. Network Load Balancer
C. Gateway Load Balancer
D. Classic Load Balancer

**Ответ: B.** **Network Load Balancer** работает на **Layer 4** (TCP/UDP), сохраняет **source IP** и обеспечивает низкую задержку. **ALB** — Layer 7, **GWLB** — для appliances, **CLB** — legacy.

**❓ Q12.** How does Elastic Load Balancing integrate with Amazon EC2 Auto Scaling?
A. ELB automatically increases the maximum capacity of the Auto Scaling group.
B. Instances launched by Auto Scaling are automatically registered with the load balancer.
C. ELB replaces the need for Auto Scaling groups.
D. ELB manually registers each EC2 instance with the Auto Scaling group.

**Ответ: B.** При связке ELB с **Auto Scaling group** инстансы автоматически регистрируются и дерегистрируются в балансировщике. ELB не меняет capacity группы, не заменяет Auto Scaling, и регистрация не ручная.

**❓ Q13.** A company wants to decouple a front-end application from a backend processing service. The backend may be unavailable for short periods, and no messages should be lost during that time. Which AWS service should the company use?
A. Amazon SNS
B. Amazon SQS
C. Amazon EventBridge
D. AWS Lambda

**Ответ: B.** **Amazon SQS** — очередь с персистентностью до 14 дней; сообщения ждут, пока consumer не будет готов. **SNS** — push, сообщения не持久ящие; **EventBridge** не для持久ящего хранения; **Lambda** — это compute, а не очередь.

**❓ Q14.** A company needs to send real-time notifications to multiple subscribers, including email, SMS, and an SQS queue, when a new order is placed. Which AWS service should the company use?
A. Amazon SQS
B. Amazon SNS
C. Amazon EventBridge
D. AWS Step Functions

**Ответ: B.** **Amazon SNS** — это publish-subscribe с push-доставкой в email, SMS, SQS и Lambda. **SQS** — только очередь, **EventBridge** не для SMS и email напрямую, **Step Functions** — оркестрация.

**❓ Q15.** A company processes financial transactions that must be handled in the exact order they are received. Duplicate processing must be prevented. Which Amazon SQS queue type meets these requirements?
A. Standard queue
B. FIFO queue
C. Dead-letter queue
D. Delay queue

**Ответ: B.** **FIFO queue** обеспечивает **exactly-once processing** и строгий порядок. **Standard** даёт **best-effort ordering** и **at-least-once** (возможны дубликаты). **Dead-letter queue** — для неудачных сообщений, **delay queue** — для отложенной доставки.

**❓ Q16.** Which AWS service is BEST suited for building an event-driven architecture that reacts to changes in AWS services and routes events based on content?
A. Amazon SQS
B. Amazon SNS
C. Amazon EventBridge
D. Elastic Load Balancing

**Ответ: C.** **Amazon EventBridge** — event-driven, правила фильтруют по содержимому, есть интеграция с AWS-сервисами и SaaS. **SQS** — очередь, **SNS** фильтрует только по атрибутам, **ELB** — балансировка трафика.

**❓ Q17.** (Multiple Response) Which of the following are characteristics of a monolithic application architecture? (Choose two.)
A. Components are loosely coupled.
B. A failure in one component can bring down the entire application.
C. Components are tightly coupled.
D. Components communicate through message queues.

**Ответ: B, C.** **Monolithic** — это **tightly coupled**; отказ одного компонента приводит к каскадному отказу. **Loose coupling** — это microservices, а message queues — это decoupling, а не монолит.

**❓ Q18.** (Multiple Response) Which of the following are characteristics of an Amazon SQS Standard queue? (Choose two.)
A. Exactly-once processing
B. At-least-once delivery
C. Best-effort ordering
D. Strict first-in-first-out ordering

**Ответ: B, C.** **Standard queue** обеспечивает **at-least-once delivery** и **best-effort ordering**. **Exactly-once** и строгий FIFO — это **FIFO queue**.

**❓ Q19.** A company wants to run containerized applications without provisioning or managing any servers. The company does not need Kubernetes and wants the simplest deployment option. Which AWS service should the company use?
A. Amazon EC2
B. AWS Fargate
C. Amazon EKS
D. AWS Batch

**Ответ: B.** **AWS Fargate** — serverless compute для контейнеров, работает с **Amazon ECS**; не нужно управлять серверами или кластерами EC2. **EC2** требует управления серверами, **EKS** — это Kubernetes (избыточно, если K8s не нужен), **Batch** — для пакетных задач.

**❓ Q20.** A developer wants to run code in response to an object being uploaded to Amazon S3, without managing any infrastructure. Which AWS service should the developer use?
A. Amazon EC2
B. AWS Lambda
C. AWS Fargate
D. Amazon ECS

**Ответ: B.** **AWS Lambda** запускает код по событию (например, загрузка в **Amazon S3**) без управления серверами; AWS управляет execution environment и runtime. **EC2** требует управления инстансом, **Fargate** и **ECS** — для контейнеров, а не для event-driven кода.

**❓ Q21.** What is the primary difference between AWS Lambda and AWS Fargate?
A. Lambda is for containers; Fargate is for event-driven code.
B. Lambda runs code in response to events; Fargate runs containers with provisioned resources.
C. Lambda requires managing servers; Fargate does not.
D. Fargate is only for batch workloads; Lambda is for web applications.

**Ответ: B.** **Lambda** — event-driven код с оплатой за выполнение; **Fargate** — контейнеры с оплатой за provisioned CPU и memory. Остальные варианты переворачивают это различие или неверны.


________________________________________________________________________
Paths: [[Cloud]]
Tags: #Cloud 