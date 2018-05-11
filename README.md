# TLog
Библиотека для логирования работы скриптов

## Описание
Библиотека на OneScript для организации логирования различных операций, выполняемых в скриптах.

## Установка 

Установка через пакетный менеджер **opm** командой:

``` cmd
opm install tlog
```

## Использование библиотеки

Библиотека подключается как отдельный класс. Экземпляр класса используется для ведения текущего лога.

Подключение библиотеки:
``` bsl
#Использовать TLog
```

Создание класса:
``` bsl
Логирование = Новый ТУправлениеЛогированием();
```

Параметры класса:
* **ИмяФайлаЛога** - Имя файла в который будет записываться лог
* **ТекстОшибки** - Переменная для возврата ошибки, если таковая имела место быть
* **РазделительУровней** - Строка для разделением уровней (отступ), по умолчанию - Табуляция
* **Уровень** - Количество строк разделения уровноей, добавленных в текущую строку лога
* **ДатаВремяВКаждойСтроке** - При установки в значение Истина, добавляет текущую дату и время в начало каждой строки, по умолчани - ЛОЖЬ
* **ВыводитьСообщенияПриЗаписи** - Вывод сообщений при записи в лог, при установке в значение Истина, все строки лога будут выведены в сообщении, по умолчанию - ИСТИНА

Экспортные процедуры класса:

**Функция СоздатьФайлЛога(ИмяЗадания = "Log", Знач ИмяКаталога = ".")**

    В указанном каталоге создает каталог с имененм задания и файл лога, имя файла сохраняется в глобальной переменной ИмяФайлаЛога. 
    
    Параметры:
     * ИмяЗадания - Строка - ИмяЗадания или идентификатор лога
     * ИмяКаталога - Строка - Конечный каталог для хранения логов
 
***

* **Функция ЗаписатьСтрокуЛога(СтрокаЛога="", ДобавитьУровеней=0)**

Записывает строку лога в файл лога (Путь к файлу хранится в глобальной переменной ИмяФайлаЛога). Запись текста инициируется и закрывается при каждом вызове процедуры.
Параметры:
    СтрокаЛога - Строка - Строка лога для записи
    ДобавитьУровеней - Число - Добавляет указанное число уровней в текущаю строку лога. По умочанию на 0.

* **Процедура УвеличитьУровень(ДобавитьУровеней=1)**

Добавляет количество уровней. Параметры:

*ДобавитьУровеней - Число - Увеличиват значение переменной Уровень на указанное число. По умочанию на 1.

* **УменьшитьУровень(ВычестьУровеней=1) Экспорт**

Уменьшает количество уровней 
Параметры:
    ВычестьУровеней - Число - Уменьшает значение переменной Уровень на указанное число. По умочанию на 1.

## ПРИМЕР ИСПОЛЬЗОВАНИЯ

``` bsl
#Использовать Tlog 

// Создадим объект
Логирование = Новый ТУправлениеЛогированием();
Логирование.ДатаВремяВКаждойСтроке = Истина; 

// Флаг наличия ошибок
БылиОшибки = Ложь;

// Создадим необходимые файлы
Если РабочийКаталог = "" Тогда
    РабочийКаталог = ".\";
КонецЕсли;
РабочийКаталог = ОбъединитьПути(РабочийКаталог,"Test_TLog");
СоздатьКаталог(РабочийКаталог);

// Создадим файл лога
Если Логирование.СоздатьФайлЛога("ТестовоеЗадание",РабочийКаталог) Тогда
    Сообщить("СоздатьФайлЛога: УСПЕШНО");
Иначе
    Сообщить("СоздатьФайлЛога: " + Логирование.ТекстОшибки);
    БылиОшибки = Истина;
КонецЕсли;

// Запишем строку
Если Логирование.ЗаписатьСтрокуЛога("Начало процедуры") Тогда
    Сообщить("ЗаписатьСтрокуЛога: УСПЕШНО");
Иначе
    Сообщить("ЗаписатьСтрокуЛога: " + Логирование.ТекстОшибки);
    БылиОшибки = Истина;
КонецЕсли;
Приостановить(1000);

// Запишем строку с отступом
Логирование.УвеличитьУровень();
Если Логирование.ЗаписатьСтрокуЛога("Строка второго уровня") Тогда
    Сообщить("ЗаписатьСтрокуЛога: УСПЕШНО");
Иначе
    Сообщить("ЗаписатьСтрокуЛога: " + Логирование.ТекстОшибки);
    БылиОшибки = Истина;
КонецЕсли;
Логирование.УменьшитьУровень();

// Запишем строку
Если Логирование.ЗаписатьСтрокуЛога("Окончание процедуры") Тогда
    Сообщить("ЗаписатьСтрокуЛога: УСПЕШНО");
Иначе
    Сообщить("ЗаписатьСтрокуЛога: " + Логирование.ТекстОшибки);
    БылиОшибки = Истина;
КонецЕсли;

//Удалим каталог
УдалитьФайлы(РабочийКаталог,"*.*");
УдалитьФайлы(РабочийКаталог);

```

Результат:

<img src="https://github.com/Tavalik/TLog/blob/master/Screenshots/TLog1.png" alt="Скриншот1">

