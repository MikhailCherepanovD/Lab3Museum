# Задание:

## Написать многопоточное приложение, эмулирующее следующую модель:

Музей. Музейщик позволяет посетителям входить через восточный вход и выходить через западный выход. Прибытие и уход посетителей определяются в музее с помощью турникетов на входе и выходе. Во время открытия директор музея сигнализирует контроллеру, что музей открыт, и тогда контроллер разрешает вход и выход в музей. Во время закрытия директор сигнализирует что музей закрыт, на этот момент прибытие посетителей запрещено контроллером. Должно быть четыре процесса: East, West, Control and Director.

## Основные моменты реализации: 

### 1. Все взаимодействие между потоками происходит через класс Museum, в котором хранятся основные параметры музея:

Количество человек, находящихся внутри;

Состояние музея(Открыт или закрыт);

Значение команды, которую ввел пользователь с клавиатуры.

## В зависимости от команды, введенной пользователем, один из потоков выполняет соответствующие действия:

### Пользователь ввел число 1:

Поток East(вход) - запускает посетителя(увеличивает количество человек внутри на 1), если музей открыт.


### Пользователь ввел число 2:

Поток West(вsход) - выпускает посетителя(уменьшает количество человек внутри на 1).

### Пользователь ввел число 3:

Музей закрывается. Поток Director посылает сигнал о закрытии музея контролеру (поток Control), тот закрывает вход в музей.

### Пользователь ввел число 4:

Музей открывается. Поток Director посылает сигнал об открытии музея контролеру (поток Control), тот открывает вход в музей.

Каждый раз, когда пользователь вводит новую команду с клавиатуры, вызывается метод setCommand(int command) класса Museum:

 public void setCommand(int command) {
        synchronized (this) {                      
            this.command = command;
            notifyAll();                          
        }
    }

notifyAll() - уведомляет все потоки о добавлении новой команды.


### Функция main:

Функция main содержит инициализацию всех потоков и метода start() для каждого из них. 

  Museum storage = new Museum();
        East thread1 = new East(storage);
        West thread2 = new West(storage);
        Control thread3 = new Control(storage);
        Director thread4 = new Director(storage);
        thread1.start();
        thread2.start();
        thread3.start();
        thread4.start();

Затем в бесконечном цикле происходит ожидание ввода с клавиатуры для выполнения перечисленных действий.

## Тестирование:

Были написаны тесты, покрывающие запуск потоков в разной последовательности и с различной задержкой. Все тесты успешно пройдены. Состояния race condition и deadLock не обнаружены.

