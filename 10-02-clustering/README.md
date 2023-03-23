# Домашнее задание к занятию "`10.2. Кластеризация`" - `Александр Недорезов`



### Задание 1

В чём различие между SMP- и MPP-системами?

*Приведите ответ в свободной форме.*

### Ответ

В SMP системах для всех процессоров используется одна основная память и система ввода-вывода, в MPP системах каждый процессор фактически включен в модуль, представляющий собой отдельный компьютер (память, устройства ввода-вывода, сетевой адаптер)/
Из-за особенности использования памяти SMP плохо масштабируются, хотя скорость межпроцессорного обмена выше. SMP-архитектура используется, когда много пользователей работают с одной базой данных, а MPP, например, для аналитики больших объемов данных, распределенных БД

---

### Задание 2

В чём отличие сильно связанных и слабо связанных систем?

*Приведите ответ в свободной форме.*

### Ответ

Основное различие между слабосвязанной и сильно связанной многопроцессорной системой заключается в том, что слабосвязанная система имеет распределенную память, тогда как тесно связанная система имеет общую память. Сильносвязанные системы это например современные процессоры, или кластеры в которых система аппаратно не отличается. Слабосвязанные системы это например кластерные системы из нескольких серверов которые имеют различную аппаратную составляющую

Слабосвязанная многопроцессорная система:
* Каждый процессор имеет свой собственный модуль памяти.
* Эффективен при выполнении задач, выполняемых на разных процессорах, имеет минимальное взаимодействие
* Как правило, не сталкиваются с конфликтом памяти.
* Взаимосвязь на основе Системы передачи сообщений (МТС).
* Скорость передачи данных - низкая
* Стоимость - недорогая

Сильно связанная многопроцессорная система:
* Процессоры имеют общие модули памяти.
* Эффективен для высокоскоростной обработки или обработки в реальном времени.
* Испытывают больше конфликтов памяти.
* Взаимосвязь на основе Взаимосвязанных сетей PMIN, IOPIN, ISIN.
* Скорость передачи данных - высокая
* Стоимость - дорогая

---

### Задание 3

Какие преимущества отличают кластерные системы от обычных серверов?

*Приведите ответ в свободной форме.*

### Ответ

К преимуществам кластерных систем можно отнести:
1. абсолютную масштабируемость, отсутствие ограничений на размер узлов и кластеров
2. отказоустойчивость, благодаря автономности узлов
3. высокий коэффициент готовности
5. соотношение цена/производительность
6. возможность совместного использования вычислительных ресурсов нескольких машин для решения одной задачи

---

### Задание 4

Приведите примеры типов современных кластерных систем.

*Приведите ответ в свободной форме.*

### Ответ

1. Отказоустойчивые кластеры (High-availability clusters, HA, кластеры высокой доступности): Pacemaker, Corosync, Red Hat Cluster Suite, Microsoft Windows Server, Linux - HA, VMware vSphere, Proxmox VE, XenServer, Openstack
2. Кластеры с балансировкой нагрузки (Load balancing clusters): PostgreSQL, nginx, HAProxy
3. Вычислительные кластеры (High performance computing clusters, HPC): Beowulf, Azure, NCSA NT Supercluster
4. Системы распределенных вычислений: Kafka, AWS, BOINC

---

### Задание 5

Где используют сервис Kafka, rabitMQ?

*Приведите ответ в свободной форме.*

### Ответ
Kafka и RabbitMQ - это самые распространенные брокеры сообщений, т.е.системы очередей, рассылки и обмена сообщениями. Они применяются для связи микросервисов, потоковой передачи данных. Сервисы отлично подходят для задач, в рамках которых требуется собирать, хранить и обрабатывать большие неструктурированные данные. Например, это могут быть платформы, аккумулирующие данные из множества источников, сервисы, которые занимаются стриминговой аналитикой.

RabbitMQ работает как кластер узлов, где очереди распределяются по узлам и, опционально, реплицируются в целях устойчивости к ошибкам и высокой доступности. 
Принцип работы RabbitMQ:
* Паблишеры (publishers) отправляют сообщения на exchange’и
* Exchange’и отправляют сообщения в очереди и в другие exchange’и
* RabbitMQ отправляет подтверждения паблишерам при получении сообщения
* Получатели (consumers) поддерживают постоянные TCP-соединения с RabbitMQ и объявляют, какую очередь(-и) они получают
* RabbitMQ проталкивает (push) сообщения получателям
* Получатели отправляют подтверждения успеха/ошибки
* После успешного получения, сообщения удаляются из очередей


Kafka - точно так же кластер узлов, но отличается принципом работы: 
* Приложение-продюсер создает сообщение и отправляет его на узел Kafka.
* Брокер сохраняет сообщение в топике, на который подписаны приложения-потребители.
* Потребитель при необходимости делает запрос в топик и получает из него нужные данные.

Т.е. основные отличия Apache Kafka и RabbitMQ обусловлены принципиально разными моделями доставки сообщений, реализуемыми в этих системах. В частности, Apache Kafka действует по принципу вытягивания (pull), когда получатели (consumers) сами достают из топика (topic) нужные им сообщения. RabbitMQ, напротив, реализует модель проталкивания, отправляя необходимые сообщения получателям. 

В связи с этим у Kafka есть многое для работы с высокими нагрузками ( пропускная способность, репликация, горизонтальное масштабирование, параллельная обработка логов сразу на нескольких серверах), и Kafka более масштабируема, нежели RabbitMQ, хотя редко приходится иметь дела с масштабами, при которых у RabbitMQ появляются проблемы.
