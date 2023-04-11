
## Порты и переферийные устройства

### /dev/ttyS0 - консоль ввода\вывода
Порт используется для отладки через TTL-USB устройство

```
Параметры порта:
1500000,8,n,1
```

### /dev/ttyS3 - порт 485
Порт используется для подклбчения любых датчиков с интерфесом RS485

```
Параметры порта задаются поделюаемым датчиком
 ```
### /dev/ttyS2 — LORA модем RAK3172. 
```
Параметры порта
115200,8,n,1
```
### /dev/ttyS1 — Датчик тока и напряжения

```
Параметры порта
9600,8,n,1
```

## Работа с датчиком тока
Датчик тока подключен к UART1 (/dev/ttyS1) и работает по
протоколу modbus rtu.

| Адрес | Значение                           |
--------|------------------------------------|
|0x49   |напряжение в Вольт*100 (целое число)|
|0x4B   |мощность в Ваттах                   |
|0x4A   |ток в Амперах*100 (целое число)     |

### Пример опроса датчика утилитой [modpoll](https://www.modbusdriver.com/modpoll.html)
```bash
rock@napi:~$ sudo ./modpoll -m rtu -b 9600 -p none -t 4 -c 1 -a 1 -r 0x4A /dev/ttyS1
modpoll 3.10 - FieldTalk(tm) Modbus(R) Master Simulator Copyright (c) 2002-2021 proconX Pty Ltd Visit https://www.modbusdriver.com for Modbus libraries and
tools. Protocol configuration: Modbus RTU, FC3
Slave configuration...: address = 1, start reference = 74, count=1
Communication.........: /dev/ttyS1, 9600, 8, 1, none, t/o 1.00 s,poll rate 1000 ms
Data type.............: 16-bit register, output (holding) registertable
-- Polling slave... (Ctrl-C to stop)
[74]: 33 - - --- Ток 0.33А
```

## Веб-интерфейс

Веб-интерфейс позволяет выполнять следующий функционал
- Настривать сеть
- Настраивать параметры LORA модема
- Настривать новые датчики в формате конфига сервиса Telegraf
- Просматривать служебную информацию о работе системы и сервисов
- Обновлять прошивку (Napi Linux) и сбрасывать устройство к заводским настройкам
- :pick_ip: Просмотривать локально графики с датчика тока 