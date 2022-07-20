[![arduino-library-badge](https://www.ardu-badge.com/badge/GyverINA.svg?)](https://www.ardu-badge.com/GyverINA)
[![Foo](https://img.shields.io/badge/Website-AlexGyver.ru-blue.svg?style=flat-square)](https://alexgyver.ru/)
[![Foo](https://img.shields.io/badge/%E2%82%BD$%E2%82%AC%20%D0%9D%D0%B0%20%D0%BF%D0%B8%D0%B2%D0%BE-%D1%81%20%D1%80%D1%8B%D0%B1%D0%BA%D0%BE%D0%B9-orange.svg?style=flat-square)](https://alexgyver.ru/support_alex/)
[![Foo](https://img.shields.io/badge/README-ENGLISH-blueviolet.svg?style=flat-square)](https://github-com.translate.goog/GyverLibs/GyverINA?_x_tr_sl=ru&_x_tr_tl=en)  

[![Foo](https://img.shields.io/badge/ПОДПИСАТЬСЯ-НА%20ОБНОВЛЕНИЯ-brightgreen.svg?style=social&logo=telegram&color=blue)](https://t.me/GyverLibs)

# GyverINA
Лёгкая библиотека для модулей power-monitor'ов INA219 и INA226

### Совместимость
Совместима со всеми Arduino платформами (используются Arduino-функции)

## Содержание
- [Установка](#install)
- [Инициализация](#init)
- [Использование](#usage)
- [Пример](#example)
- [Версии](#versions)
- [Баги и обратная связь](#feedback)

<a id="install"></a>
## Установка
- Библиотеку можно найти по названию **GyverINA** и установить через менеджер библиотек в:
    - Arduino IDE
    - Arduino IDE v2
    - PlatformIO
- [Скачать библиотеку](https://github.com/GyverLibs/GyverINA/archive/refs/heads/main.zip) .zip архивом для ручной установки:
    - Распаковать и положить в *C:\Program Files (x86)\Arduino\libraries* (Windows x64)
    - Распаковать и положить в *C:\Program Files\Arduino\libraries* (Windows x32)
    - Распаковать и положить в *Документы/Arduino/libraries/*
    - (Arduino IDE) автоматическая установка из .zip: *Скетч/Подключить библиотеку/Добавить .ZIP библиотеку…* и указать скачанный архив
- Читай более подробную инструкцию по установке библиотек [здесь](https://alexgyver.ru/arduino-first/#%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0_%D0%B1%D0%B8%D0%B1%D0%BB%D0%B8%D0%BE%D1%82%D0%B5%D0%BA)
### Обновление
- Рекомендую всегда обновлять библиотеку: в новых версиях исправляются ошибки и баги, а также проводится оптимизация и добавляются новые фичи
- Через менеджер библиотек IDE: найти библиотеку как при установке и нажать "Обновить"
- Вручную: **удалить папку со старой версией**, а затем положить на её место новую. "Замену" делать нельзя: иногда в новых версиях удаляются файлы, которые останутся при замене и могут привести к ошибкам!


<a id="init"></a>
## Инициализация
### INA219
```cpp
INA219 ina219 (Сопротивление шунта, Максимальный ожидаемый ток, Адрес на шине I2c)

INA219 ina219;                  // Стандартные значения для модуля INA219 (0.1 Ом, 3.2А, адрес 0x40) - подойдет для одного модуля
INA219 ina219(0x41);            // Шунт и макс. ток по умолчанию, адрес 0x41 - подойдет для нескольких модулей
INA219 ina219(0.05);            // Шунт 0.05 Ом, макс. ток и адрес по умолчанию (3.2А, 0x40) - Допиленный модуль или голая м/с
INA219 ina219(0.05, 2.0);       // Шунт 0.05 Ома, макс. ожидаемый ток 2А, адрес по умолчанию (0x40) - Допиленный модуль или голая м/с
INA219 ina219(0.05, 2.0, 0x41); // Шунт 0.05 Ома, макс. ожидаемый ток 2А, адрес 0x41  - Допиленные модули или голые м/с
```
### INA226
```cpp
INA226 ina226 (Сопротивление шунта, Максимальный ожидаемый ток, Адрес на шине I2c)

INA226 ina226;                  // Стандартные значения для модуля INA226 (0.1 Ом, 0.8 А, адрес 0x40) - подойдет для одного модуля
INA226 ina226(0x41);            // Шунт и макс. ток по умолчанию, адрес 0x41 - подойдет для нескольких модулей
INA226 ina226(0.05);            // Шунт 0.05 Ом, макс. ток и адрес по умолчанию (0.8 А, 0x40) - Допиленный модуль или голая м/с
INA226 ina226(0.05, 1.5);       // Шунт 0.05 Ома, макс. ожидаемый ток 1.5 А, адрес по умолчанию (0x40) - Допиленный модуль или голая м/с
INA226 ina226(0.05, 1.5, 0x41); // Шунт 0.05 Ома, макс. ожидаемый ток 1.5 А, адрес 0x41  - Допиленные модули или голые м/с
```

<a id="usage"></a>
## Использование
### INA219
```cpp
bool begin();                               // Инициализация модуля и проверка присутствия, вернет false если INA219 не найдена
void sleep(true / false);                   // Включение и выключение режима низкого энергопотребления, в зависимости от аргумента

void setResolution(channel, mode);          // Установка разрешения и режима усреднения для измерения напряжения и тока
// channel - канал измерения
INA219_VBUS             // Канал АЦП, измеряющий напряжение шины (0-26в)
INA219_VSHUNT           // Канал АЦП, измеряющий напряжение на шунте

// mode - режим работы и разрешение
INA219_RES_9BIT         // 9 Бит - 84мкс
INA219_RES_10BIT        // 10 Бит - 148мкс 
INA219_RES_11BIT        // 11 Бит - 276мкс 
INA219_RES_12BIT        // 12 Бит - 532мкс
INA219_RES_12BIT_X2     // 12 Бит, среднее из 2х - 1.06 мс
INA219_RES_12BIT_X4     // 12 Бит, среднее из 4х - 2.13 мс
INA219_RES_12BIT_X8     // 12 Бит, среднее из 8х - 4.26 мс
INA219_RES_12BIT_X16    // 12 Бит, среднее из 16х - 8.51 мс
INA219_RES_12BIT_X32    // 12 Бит, среднее из 32х - 17.02 мс
INA219_RES_12BIT_X64    // 12 Бит, среднее из 64х - 34.05 мс
INA219_RES_12BIT_X128   // 12 Бит, среднее из 128х - 68.10 мс

float getShuntVoltage();                    // Прочитать напряжение на шунте
float getVoltage();                         // Прочитать напряжение
float getCurrent();                         // Прочитать ток
float getPower();                           // Прочитать мощность

uint16_t getCalibration();                  // Прочитать калибровочное значение (после старта рассчитывается автоматически)
void setCalibration(calibration value);     // Записать калибровочное значение 	(можно хранить его в EEPROM)	 		
void adjCalibration(calibration offset);    // Подкрутить калибровочное значение на указанное значение (можно менять на ходу)
```
### INA226
```cpp
bool begin();                               // Инициализация модуля и проверка присутствия, вернет false если INA226 не найдена
void sleep(true / false);                   // Включение и выключение режима низкого энергопотребления в зависимости от аргумента

void setAveraging(avg);                     // Установка количества усреднений измерений (см. таблицу ниже)
INA226_AVG_X1
INA226_AVG_X4
INA226_AVG_X16
INA226_AVG_X64
INA226_AVG_X128
INA226_AVG_X256
INA226_AVG_X512
INA226_AVG_X1024

void setSampleTime(channel, time);          // Установка времени выборки напряжения и тока (INA226_VBUS / INA226_VSHUNT), по умолчанию INA226_CONV_1100US
// channel - канал измерения
INA226_VBUS         // напряжение шины (0-36в)
INA226_VSHUNT       // напряжение на шунте

// time - время выборки (накопления сигнала для оцифровки)
INA226_CONV_140US   // 140 мкс
INA226_CONV_204US   // 204 мкс
INA226_CONV_332US   // 332 мкс
INA226_CONV_588US   // 588 мкс
INA226_CONV_1100US  // 1100 мкс
INA226_CONV_2116US  // 2116 мкс
INA226_CONV_4156US  // 4156 мкс
INA226_CONV_8244US  // 8244 мкс
    
float getShuntVoltage();                    // Прочитать напряжение на шунте
float getVoltage();                         // Прочитать напряжение
float getCurrent();                         // Прочитать ток
float getPower();                           // Прочитать мощность

uint16_t getCalibration();                  // Прочитать калибровочное значение (после старта рассчитывается автоматически)
void setCalibration(calibration value);     // Записать калибровочное значение 	(можно хранить его в EEPROM)	 		
void adjCalibration(calibration offset);    // Подкрутить калибровочное значение на указанное значение (можно менять на ходу)
```

<a id="example"></a>
## Пример
Остальные примеры смотри в **examples**!

<details>
<summary>INA219</summary>

```cpp
#include <GyverINA.h>

// Создаем обьект: INA219 ina(Сопротивление шунта, Макс. ожидаемый ток, I2c адрес);
// INA219 ina(0x41);              // Стандартные настройки для модуля, но измененный адрес
// INA219 ina(0.05f);             // Стандартный адрес и макс. ток, но другой шунт (0.05 Ом)
// INA219 ina(0.05f, 2.0f);       // Стандартный адрес, но другой шунт (0.05 Ом) и макс. ожидаемый ток (2А)
// INA219 ina(0.05f, 2.0f, 0x41); // Полностью настраиваемый вариант, ручное указание параметров
INA219 ina;                       // Стандартный набор параметров для Arduino модуля (0.1, 3.2, 0x40)

void setup() {
  // Открываем последовательный порт
  Serial.begin(9600);
  Serial.print(F("INA219..."));

  // Проверяем наличие и инициализируем INA219
  if (ina.begin()) {
    Serial.println(F("connected!"));
  } else {
    Serial.println(F("not found!"));
    while (1);
  }

  // Можно веревести в режим сна, вызвав .sleep с аргументом true, чтобы разбудить - вызываем повторно с указанием false
  // ina.sleep(true);   // Усыпить INA219
  // ina.sleep(false);  // Разбудить INA219

  // INA219 имеет возможность встроенной калибровки измерения тока, при помощи специального калибровочного значения
  // После запуска библиотека автоматически рассчитает и запишет калибровочное значение на основе введенных данных
  // Полученное значение можно прочитать, используя метод .getCalibration(); для изменения и/или сохранения в EEPROM
  Serial.print(F("Calibration value: ")); Serial.println(ina.getCalibration());
  // Далее полученное значение можно изменять для подстройки под реальное сопротивление шунта и сохранять в EEPROM
  // Чтобы записать калибровочное значение в INA219 существует метод .setCalibration(value);
  // ina.setCalibration(ina.getCalibration() + 10); // Прочитать-модифицировать-записать калибровочное значение
  // Так же, можно использовать метод .adjCalibration(offset); для подстройки калибровки без непосредственного чтения
  // ina.adjCalibration(10);  // Увеличить калибровочное значение на 10
  // ina.adjCalibration(-20); // Уменьшить калибровочное значение на 20
  // Можно хранить в EEPROM и загружать в INA219 именно смещение калибровки вместо непосредственного значения

  // Так же имеется возможность выбрать разрешение АЦП (9-12 Бит) и включить встроенное усреднение измерений
  // Выбор настроек для измерения напряжения и тока разделены и определяются константами INA219_VBUS или INA219_VSHUNT
  // Усреднение увеличивает время измерений, снижая шумы измерений, доступно только для 12ти битного режима
  // ina.setResolution(INA219_VBUS, INA219_RES_10BIT); // Измеряем напряжение в 10ти битном режиме, 12 бит по умолчанию
  // Использование пониженного разрешения ускоряет измерения (см. таблицу в INA219.h), но НЕ рекомендуется
  // Использование встроенного усреднения крайне рекомендуется для повышения стабильности показаний на шумной нагрузке
  ina.setResolution(INA219_VBUS, INA219_RES_12BIT_X4);      // Напряжение в 12ти битном режиме + 4х кратное усреднение
  ina.setResolution(INA219_VSHUNT, INA219_RES_12BIT_X128);  // Ток в 12ти битном режиме + 128х кратное усреднение
  
  Serial.println("");
}

void loop() {
  // Читаем напряжение
  Serial.print(F("Voltage: "));
  Serial.print(ina.getVoltage(), 3);
  Serial.println(F(" V"));

  // Читаем ток
  Serial.print(F("Current: "));
  Serial.print(ina.getCurrent(), 3);
  Serial.println(F(" A"));

  // Читаем мощность
  Serial.print(F("Power: "));
  Serial.print(ina.getPower(), 3);
  Serial.println(F(" W"));

  // Читаем напряжение на шунте
  Serial.print(F("Shunt voltage: "));
  Serial.print(ina.getShuntVoltage(), 6);
  Serial.println(F(" V"));

  Serial.println("");
  delay(1000);
}
```
</details>
<details>
<summary>INA226</summary>

```cpp
#include <GyverINA.h>

/*
   Внимание!!! Пределы измерения напряжения шунта у INA226 = +/- 81.92 мВ
   При использовании модуля INA226 с шунтом 0.1 Ом макс. измеряемый ток будет I ~ 820 мА
   При использовании другого шунта, рекомендуется рассчитать его так, чтобы падение напряжения не превышало 82мВ!

   Пример:
   Макс. ожидаемый ток = 5 A
   Предел падения напряжения на шунте = 80 мВ
   R шунта = 0.08 В / 5 А = 0,016 Ом
   Шунт должен иметь сопротивление 0.016 Ом (160 мОм)
*/

// Создаем обьект: INA226 ina(Сопротивление шунта, Макс. ожидаемый ток, I2c адрес);
// INA226 ina(0x41);              // Стандартные настройки для модуля, но измененный адрес
// INA226 ina(0.05f);             // Стандартный адрес и макс. ток, но другой шунт (0.05 Ом)
// INA226 ina(0.05f, 1.5f);       // Стандартный адрес, но другой шунт (0.05 Ом) и макс. ожидаемый ток (1.5 А)
// INA226 ina(0.05f, 1.5f, 0x41); // Полностью настраиваемый вариант, ручное указание параметров
INA226 ina;                       // Стандартный набор параметров для Arduino модуля (0.1, 0.8, 0x40)

void setup() {
  // Открываем последовательный порт
  Serial.begin(9600);
  Serial.print(F("INA226..."));

  // Проверяем наличие и инициализируем INA226
  if (ina.begin()) {
    Serial.println(F("connected!"));
  } else {
    Serial.println(F("not found!"));
    while (1);
  }

  // Можно веревести в режим сна, вызвав .sleep с аргументом true, чтобы разбудить - вызываем повторно с указанием false
  // ina.sleep(true);   // Усыпить INA226
  // ina.sleep(false);  // Разбудить INA226

  // INA226 имеет возможность встроенной калибровки измерения тока, при помощи специального калибровочного значения
  // После запуска библиотека автоматически рассчитает и запишет калибровочное значение на основе введенных данных
  // Полученное значение можно прочитать, используя метод .getCalibration(); для изменения и/или сохранения в EEPROM
  Serial.print(F("Calibration value: ")); Serial.println(ina.getCalibration());
  // Далее полученное значение можно изменять для подстройки под реальное сопротивление шунта и сохранять в EEPROM
  // Чтобы записать калибровочное значение в INA226 существует метод .setCalibration(value);
  // ina.setCalibration(ina.getCalibration() + 10); // Прочитать-модифицировать-записать калибровочное значение
  // Так же, можно использовать метод .adjCalibration(offset); для подстройки калибровки без непосредственного чтения
  // ina.adjCalibration(10);  // Увеличить калибровочное значение на 10
  // ina.adjCalibration(-20); // Уменьшить калибровочное значение на 20
  // Можно хранить в EEPROM и загружать в INA226 именно смещение калибровки вместо непосредственного значения

  // Для повышения помехозащищенности INA226 имеет возможность настроить время выборки напряжения и тока
  // INA226 будет захватывать "кусок" сигнала выбранной продолжительности, что повысит точность на шумном сигнале
  // По умолчанию выборка занимает 1100 мкс, но может быть изменена методом .setSampleTime(канал, время);
  // Варианты времени выборки см. в таблице (файл INA226.h)
  ina.setSampleTime(INA226_VBUS, INA226_CONV_2116US);   // Повысим время выборки напряжения вдвое
  ina.setSampleTime(INA226_VSHUNT, INA226_CONV_8244US); // Повысим время выборки тока в 8 раз

  // Так же имеется возможность использовать встроенное усреднение выборок
  // Усреднение применяется и для напряжения и для тока и пропорционально увеличивает время оцифровки
  // Рекомендуется на шумной нагрузке, устанавливается методом .setAveraging(кол-во усреднений) (см. таблицу в INA226.h)
  ina.setAveraging(INA226_AVG_X4); // Включим встроенное 4х кратное усреднение, по умолчанию усреднения нет 

  Serial.println("");
}

void loop() {
  // Читаем напряжение
  Serial.print(F("Voltage: "));
  Serial.print(ina.getVoltage(), 3);
  Serial.println(F(" V"));

  // Читаем ток
  Serial.print(F("Current: "));
  Serial.print(ina.getCurrent(), 3);
  Serial.println(F(" A"));

  // Читаем мощность
  Serial.print(F("Power: "));
  Serial.print(ina.getPower(), 3);
  Serial.println(F(" W"));

  // Читаем напряжение на шунте
  Serial.print(F("Shunt voltage: "));
  Serial.print(ina.getShuntVoltage(), 6);
  Serial.println(F(" V"));

  Serial.println("");
  delay(1000);
}

```
</details>

<a id="versions"></a>
## Версии
- v1.0
- v1.1 - пофиксил варнинги, причесал табуляцию (AlexGyver)
- v1.2 - исправлена ошибка в расчетах ina226

<a id="feedback"></a>
## Баги и обратная связь
При нахождении багов создавайте **Issue**, а лучше сразу пишите на почту [alex@alexgyver.ru](mailto:alex@alexgyver.ru)  
Библиотека открыта для доработки и ваших **Pull Request**'ов!


При сообщении о багах или некорректной работе библиотеки нужно обязательно указывать:
- Версия библиотеки
- Какой используется МК
- Версия SDK (для ESP)
- Версия Arduino IDE
- Корректно ли работают ли встроенные примеры, в которых используются функции и конструкции, приводящие к багу в вашем коде
- Какой код загружался, какая работа от него ожидалась и как он работает в реальности
- В идеале приложить минимальный код, в котором наблюдается баг. Не полотно из тысячи строк, а минимальный код
