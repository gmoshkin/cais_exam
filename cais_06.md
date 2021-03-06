## 6. Выполнение произвольного кода на неисполнимом стеке.  Return-to-libc.h Противодействие return-to-libc: ASLR. Return-oriented programming (ROP). Аудит передач управления.

### Обход DEP: return-to-libc

Вместо передачи управления на код внутри буфера можно заменить адрес возврата на
адрес известной библиотечной функции, например `system` из стандартной
библиотеки Си.

```
  stack:
│ .......... │↖
│ .......... │← local variables
│ system     │ return address
│ *"/bin/sh" │ parameter
```

### Противодействие return-to-libc

**Рандомизация адресного пространства (ASLR)** — изменение карты памяти процесса
при каждом запуске (процесса или системы).

* Без дополнительных действий (PIE) адрес загрузки основного исполняемого файла
  не рандомизируется.

* Можно попытаться обойти защиту, используя только постоянные адреса.

* Return-oriented programming (ROP).

### Return-oriented programming (ROP).

**Гаджет** — последовательность команд в исполняемых секциях программы,
заканчивающаяся командой RET.

**Возвратно-ориентированное программирование (ROP)** — способ построения
эксплоита, при котором полезная нагрузка формируется в виде цепочки гаджетов с
известными постоянными адресами.

* Как из набора гаджетов «собрать» эксплоит?

* Где найти необходимые гаджеты?

* Какие гаджеты могут быть полезны?

* Как найти необходимые гаджеты?

### Запись числа в память

#### Обычный способ:

```
   stack:
rsp + 0 : │ v1 │
rsp + 8 : │    │
rsp +16 : │ v2 │
rsp +24 : │    │
...
mov rax, qword [rsp]
mov rcx, qword [rsp + 8]
mov qword [rcx], rax
```

#### ROP:

Управление передаётся на `g1`, последовательно обрабатывается вся цепочка
гаджетов

```
   stack:
rsp + 0 : │ v1 │
rsp + 8 : │ g2 │
rsp +16 : │ v2 │
rsp +24 : │ g3 │
...
g1: pop rax
    ret

g2: pop rcx
    ret

g3: mov qword [rcx], rax
    ret
```

### Поверхность атаки

* Linux:

    - рандомизированный стек;

    - рандомизированная куча (mmap / brk);

    - рандомизированные адреса загрузки динамических библиотек;

    - рандомизированные адреса загрузки PIE-программ;

    - нерандомизированные адреса загрузки остальных программ.

* Windows

    - по умолчанию рандомизация включена только для программ, которые явно её
      запрашивают (opt-in);

    - рандомизированный стек;

    - рандомизированная куча;

    - рандомизированные адреса служебных структур данных;

    - рандомизированные адреса загрузки динамических библиотек и программ.


### Варианты полезной нагрузки

1. Выполнить необходимые действия, полностью организовав их в виде цепочки
   гаджетов.

2. Разрешить выполнение кода на стеке, изменив права доступа к соответствующим
   страницам:

    - mprotect;

    - VirtualProtect.

3. “Stack pivot”: изменить значение ESP, выбрав в качестве новой области
   контролируемый регион в куче.


### Автоматизация ROP: Q

[Q: Exploit Hardening Made Easy]

#### Автоматическая генерация полезной нагрузки по:

* исполняемым модулям (с известными адресами загрузки);

* описанию функциональности полезной нагрузки (QooL).

#### Выделение гаджетов:

* дизассемблирование со всех байтовых смещений;

* рандомизированное тестирование семантики;

* доказательство семантики;

* получение базы гаджетов.

### Генерация полезной нагрузки

* Трансляция входной QooL-программы в последовательность Q-операций с локально
  минимальной длиной.

* Q-операции набираются из тех гаджетов, которые обнаружены в атакуемом коде.

* Проблемы:

    - побочные эффекты в гаджетах;

    - распределение регистров для исключения конфликтов.


### Ограничения Q

* Набор гаджетов Q не Тьюринг-полон:

    - но это и не требуется, т.к. достаточно вызвать mprotect или system с
      нужными параметрами;

    - вручную подготовленный набор гаджетов из стандартной библиотеки Си
      Тьюринг-полон.

* Какова вероятность успешной атаки?

    - 80% вероятность вызова уже используемой в программе функции — начиная с
      объёма кода в 20 KiB;

    - 80% вероятность вызова произвольной внешней функции — начиная с объёма
      кода в 100 KiB.


### Аудит передач управления

**CFI (Control Flow Integrity)** — средства, обеспечивающие проверку
корректности потоков данных в программе.

[kBouncer: Efficient and Transparent ROP Mitigation]

**kBouncer** проверяет последние передачи управления перед выполнением
системного вызова. Информация о передачах управления сохраняется в специальных
регистрах процессором автоматически.

[Q: Exploit Hardening Made Easy]: https://users.ece.cmu.edu/~dbrumley/pdf/Schwartz,%20Avgerinos,%20Brumley_2011_Q%20Exploit%20Hardening%20Made%20Easy.pdf
[kBouncer: Efficient and Transparent ROP Mitigation]: http://www.cs.columbia.edu/~vpappas/papers/kbouncer.pdf
