## 2. Технологии и принципы разработки безопасного ПО. Microsoft Secure Development Lifecycle, Cisco Secure Development Lifecycle. Поверхность атаки. Классификация угроз.

### Жизненный цикл ПО, направленный на обеспечение безопасности

* Дополнение-модификация к классическим моделям жизненного цикла.

* Параллельно разработке ПО осуществляются мероприятия для проверки и повышения
  безопасности ПО.

* Для каждой стадии разработки добавляются требования и шаги, только после
  осуществления которых можно переходить к следующей стадии.

* План реагирования на инциденты.

### Предпосылки возникновения SDL

Создание группы реагирования на инциденты

Microsoft. Сбор и анализ статистики по ошибкам и исправлениям. Выводы:

1. О безопасности нужно думать до и во время создания продукта, а не после.

2. Большинство ошибок являются следствием небольшого числа причин.

3. Любые внешние данные потенциально опасны.

### Принципы безопасной разработки

* Безопасный дизайн – при разработке архитектуры и дизайна нужно учитывать, что
  ПО должно защищать себя, информацию, которую оно обрабатывает и противостоять
атакам.

* Безопасность по умолчанию – программа почти наверняка содержит уязвимости,
  поэтому требуется минимизировать последствия их эксплуатации – минимизация
привилегий, ограничение числа пользователей с высоким уровнем доступа.

* Безопасность при внедрении – инструменты и описания для облегчения
  конфигурирования ПО и установки обновлений.

* Взаимодействие – реагирование на обнаруженные уязвимости и помощь для
  противодействия угрозам.

### Известные циклы безопасной разработки

* **Microsoft SDL** – системное ПО

* **Cisco SDL** – ПО для сетевого оборудования

* **PA-DSS** – рекомендации к процессам разработки платёжных систем

* **РС БР ИББС-2.6-2014** – рекомендации банка России для Обеспечение
  информационной безопасности банковских систем

===

### Microsoft SDL

### Жизненный цикл с требованиями по безопасности на основе каскадной модели

1. **Обучение**: обучение основам безопасности

2. **Требования**: задание требований, создание контрольных условий качества и
   панелей ошибок, оценка рисков безопасности и конфиденциальности

3. **Проектирование**: задание требований проектирования, анализ возможных
   направлений для злоумышленных атак, моделирование рисков

4. **Реализация**: применение утверждённых инструментов, отказ от небезопасных
   функций, статический анализ

5. **Проверка**: динамический анализ, нечёткое тестирование, проверка возможных
   направлений для злоумышленных атак

6. **Выпуск**: планирование реагирования на инциденты, окончательная проверка
   безопасности, архивация выпуска

7. **Реагирование**: выполнение плана реагирования на инциденты

В каждой фазе используются свои практики

### Практики

#### Обучение безопасности:

* Все сотрудники

* Не реже 1 раза в год

* Знания для выполнение остальных фаз

#### Разработка требований:

* Определение требований ИБ и ответственных

    - По безопасности

    - По конфиденциальности

* Оценка рисков

    - Выделение участков кода

    - Назначение приоритетов

#### Проектирование

* Архитектурные требования безопасности

    - Используемые крипто-алгоритмы

    - Настройки межсетевого экрана

* Анализ/сокращение поверхности атаки

    - Все каналы получения данных извне

* Моделирование угроз (STRIDE)

    - Кто? Что? Как? может испортить

#### Реализация

* Использование доверенных средств

* Отказ от небезопасных функций (strcpy)

* Статический анализ кода

* Перекрёстная проверка кода (code-review)

#### Проверка

* **Динамический анализ** – мониторинг выполнения, обнаружение повреждений памяти и др. проблем

* **Фаззинг** – многократный запуск на намеренно повреждённых данных

* Проверка поверхности атаки

#### Выпуск

* Планы реагирования на инциденты

* Финальный анализ безопасности

* Доверенный выпуск

#### Реагирование

* Выполнение планов реагирования на инциденты


По результатам использования SDL количество критических ошибок выявленных за год
использования Windows 2003 уменьшился больше чем в 2 раза по сравнению с Windows
2000.

===

### Cisco SDL

Жизненный цикл с требованиями по безопасности на основе спиральной модели

* Требования безопасности

* Безопасность сторонних компонент

* Проектирование

* Реализация

* Статический анализ

* Тестирование

### Стадии Cisco SDL

#### Требования безопасности:

* Внутренние требования ИБ

* Внешние требования (требования рынка)

* Безопасность сторонних компонент:

* Реестр используемого стороннего ПО

* Уведомление об уязвимостях и централизованное исправление

#### Проектирование:

* Проектирование с учетом ИБ (обучение, лучшие практики, безопасные компоненты)

* Моделирование угроз (STRIDE)

#### Реализация:

* Обучение

* Использование безопасных компонент

* Проверка кода, статический анализ

* Использование стандартов безопасного программирования

#### Статический анализ:

* Переполнение буфера, отслеживание внешних данных

#### Тестирование:

* Анализ поверхности атаки

* Тесты для всех протоколов

* Тестирование на проникновение (пентестинг)

===

### Поверхность атаки

* **Сетевая** – открытые сетевые соединения/именованные каналы/RPC.

* **Локальная** – файлы, переменные окружения

* **Пользовательская** – учётные записи:

    - гостевые (анонимные);

    - пользовательские;

    - администраторские.

* **Программная** – запущенные сервисы (обработчики Web-запросов):

    - с пользовательскими правами;

    - с повышенными правами.

#### Уменьшение поверхности атаки

* Уменьшение количества открытых сетевых соединений

* Отключение ненужных сервисов

* Запуск по требованию вместо постоянной работы

* Авторизованный доступ вместо анонимного

* Доступ к критическим компонентам только из локальной сети (не из Интернета)

* Изменение настроек по умолчанию

* Уменьшение количества кода обработчиков внешних данных

===

### Классификация угроз Модель STRIDE

* **S** (Spoofing of identity) – возможность аутентифиткации под чужим аккаунтом

* **T** (Tampering with Data) – возможность изменения данных, на которые нет прав

* **R** (Repudiation) – возможность отрицания совершённых действий

* **I** (Information disclosure) – возможность раскрытия информации, на которую
  нет прав

* **D** (Denial of service) – отказ в обслуживании

* **E** (Elevation of privilege) – возможность авторизации с повышенными правами

Для визуализации используются диаграммы потоков данных.

![image](file:///space/gmoshkin/cmc/cais/exam/data_flow_diagram.png)
