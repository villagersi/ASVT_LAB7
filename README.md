# Описание проекта
Данный проект реализует клиент-серверное взаимодействие с использованием протокола MQTT (Message Queue Telemetry Transport). Он включает в себя основные компоненты для публикации и подписки на сообщения через брокер MQTT. Ниже представлено подробное объяснение ключевых классов и методов.
# Структура проекта
## Проект состоит из следующих основных классов:
1. Mqtt - основной класс для работы с клиентом MQTT.
2. MqttConnectionSettings - класс для хранения настроек подключения к брокеру MQTT.
3. Publisher - класс для отправки сообщений на определенный топик.
4. Subscriber - класс для получения сообщений из заданного топика.

## Класс Mqtt
### Основные поля и события
- ILogger logger: Логгер для записи информации о работе приложения.
- IManagedMqttClient managedMqttClient: Управляемый клиент MQTT.
- MqttConnectionSettings mqttSettings: Настройки подключения к MQTT-брокеру.
- event EventHandler<(string Topic, object Data)> OnReceive: Событие, которое вызывается при получении сообщения.
### Конструктор
```c#
public Mqtt(ILogger logger, MqttConnectionSettings mqttSettings)
```
Конструктор принимает логгер и настройки подключения, а также инициализирует параметры сериализации JSON.
### Метод BuildAsync
```c#
public async Task BuildAsync()
```
Этот метод создает и настраивает клиент MQTT. В нем:
- Устанавливаются параметры подключения (IP-адрес сервера, идентификатор клиента).
- Настраивается TLS (если требуется) для безопасного соединения.
- Запускается управляемый клиент MQTT.
### Метод ManagedMqttClient_ApplicationMessageReceivedAsync
```c#
private Task ManagedMqttClient_ApplicationMessageReceivedAsync(MqttApplicationMessageReceivedEventArgs e)
```
Обрабатывает входящие сообщения, десериализует их из формата JSON и вызывает событие OnReceive.
### Методы Subscribe и PublishAsync
```c#
public async Task Subscribe(string topic, MqttQualityOfServiceLevel qualityOfServiceLevel)
public async Task PublishAsync(string topic, object data, string format = "json")
```
- Subscribe: Подписывается на указанный топик с заданным уровнем качества обслуживания (QoS).
- PublishAsync: Публикует сообщение в указанный топик в формате JSON, текстовом или бинарном.
## Класс MqttConnectionSettings
### Свойства
- Server: Адрес сервера MQTT.
- Port: Порт сервера (по умолчанию 1883).
- UserName и Password: Учетные данные для аутентификации.
- SslProtocol: Протокол SSL для безопасного соединения.
- QOS: Уровень качества обслуживания сообщений.
## Класс Publisher
### Основной метод Main
```c#
static async Task Main(string[] args)
```
В этом методе:
- Настраиваются зависимости (логирование, кэширование).
- Загружаются настройки из конфигурационного файла.
- Создается экземпляр класса Mqtt и запускается процесс публикации сообщений.
### Метод StartPublishAsync
```c#
private static async Task StartPublishAsync(Common.Mqtt mqttSender)
```
Этот метод позволяет пользователю вводить сообщения в консоль и отправлять их на заданный топик до тех пор, пока не будет введено слово "exit".
## Класс Subscriber
### Основной метод Main
```c#
static async Task Main(string[] args)
```
Подобно классу Publisher, здесь также настраиваются зависимости и загружаются конфигурации. Затем создается экземпляр класса Mqtt, который подписывается на сообщения.
### Метод Handler
```c#
private static void Handler(object sender, (string Topic, object Data) obj)
```
Этот метод обрабатывает полученные сообщения. Он пытается десериализовать данные как JSON или текст и выводит их в лог.
# Заключение
Данный проект демонстрирует использование протокола MQTT для обмена сообщениями между устройствами в рамках концепции Интернета вещей (IoT). Он позволяет легко интегрировать различные устройства и системы, обеспечивая надежную передачу данных даже при нестабильных соединениях.
# Результаты работы программы
![image](https://raw.githubusercontent.com/villagersi/ASVT_LAB7/b5b1ffec1bd870c7b309ef0769abce80159ede09/ScreenShot.png)
