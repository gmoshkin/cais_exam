## 8. Представления программы, использующиеся при поиске ошибок в исходном коде.  Потоковая и контекстная чувствительность. Типы обнаруживаемых ошибок. Путь распространения ошибки.

### Модели программы

* Исходный код, как представление программы для анализа неудобен

* Используются модели, разработанные в области компиляторов

* Лексический анализ – программа, как поток токенов

* Синтаксический анализ – абстрактное синтаксическое дерево

* Семантический анализ – проверка типов

* Построение промежуточного представления

    - Более высокоуровневого чем для компилятора

* Построение модели программы, позволяющей анализировать потоки данных и
  управления.  Используемые модели:

    - граф потока управления (ГПУ) и граф вызовов (ГВ)

    - граф зависимостей(ГЗ)

### Граф потока управления

##### Программа:
```
int n = 10;
int f = 1;
while (n > 0) {
    f = f * n;
    n = n – 1;
}
res = f;
```

##### ГПУ:
```
  ┏━━━━━━━┓
  ┃ n =10 ┃
  ┃ f = 1 ┃
  ┗━━━━━━━┛
      │
      v
  ┏━━━━━━━┓
┌─┃ n > 0 ┃‹┐
│ ┗━━━━━━━┛ │
│     │     │
│     v     │
│ ┏━━━━━━━┓ │
│ ┃f = f*n┃ │
│ ┃n = n-1┃─┘
│ ┗━━━━━━━┛
│ ┏━━━━━━━┓
└›┃res = f┃
  ┗━━━━━━━┛
```

* Анализ выполняющийся по ГПУ без использования графа вызовов –
  внутрипроцедурный

* Вершинами могут быть как базовые блоки так и отдельные инструкции (менее
  компактно)

### Граф зависимостей (ГЗ)

* Инструкция S зависит по данным от инструкции T если существует путь от T к S и
  значение переменной определяется в T используется в S (DU-зависимость).

* Зависимости по данным между узлами-инструкциями представляются в виде
  направленных рёбер

* На практике помимо рёбер DU-зависимостей (запись-чтение) также используются
  зависимости вида:

    - чтение-запись (UD)

    - запись-запись (DD)

* Зависимости по управлению

### Граф зависимостей по данным

[вставь картинку]

### Граф вызовов (ГВ)

[вставь картинку]

Анализ выполняющийся с использованием графа вызовов – межпроцедурный

Требуется проверять корректность вызовов – количество и тип параметров

### Сложности построения графа вызовов – вызов по указателю

[вставь картинку]

Требуется анализировать возможные значения указателей

### Граф указателей

[вставь картинку]

Граф, который для данной точки программы содержит рёбра между переменными p и x
если p может/должно указывать на x