## OpenIPC для Xiaomi MJSXJ03HL (В РАЗРАБОТКЕ!!!) 
![Изображение](https://user-images.githubusercontent.com/88727968/222164240-66044bf1-16da-4ea2-af38-6fd3d3fb1b92.png)


Внимание! Любые вносимые изменения лишают вас гарантии на данное устройство! Ответственность за любой ущерб, возникший в результате любых действий пользователя, автор не несет!
_________

### Подключение UART переходнка

Нам понадобятся:

- Компьютер с USB портом 
- Тестер или вольтметр
- UART переходник на 3,3V , подобный этому

![изображение](https://user-images.githubusercontent.com/88727968/222174358-5203eb83-14ce-4f55-89bd-24775af82599.png)

Все действия будут показаны на ОС Linux (Kubuntu), если Ваша ОС отличается, пожалуйста обратитесь к специализированным инструкциям по настройке UART в вашей ОС.

Порядок работы:
1) Внимательно изучите UART переходник. Найдите контакты `GND`,`TX`,`RX`. Если ваш переходник поддерживает функцию выбора рабочего напряжения, убедитесь, что он переведен в режим работы 3,3V! 
2) При помощи тестера убедитесь в том, что Ваш UART переходник выдает напряжение 3.3V. Для этого выполните замеры между контактами `GND`и`TX`, а также `GND`и`RX`. Не используйте переходник, если величина напряжения составляет 5V! Это убьет Вашу камеру!
3) Подключите UART переходник к порту Вашего компьютера. Необходимо узнать точку монтирования переходника в Вашей операционной системе.
4) Используйте терминал. Выполните команду `lsusb` и внимательно изучите вывод. Найдите Ваш UART переходник.
5) Используйте терминал. Выполните команду `dmesg | grep attached` от суперпользователя. Сравните выводы обеих команд и найдите точку монтирования переходника. Для примера воспользуйтесь скриншотом ниже: ![Screenshot_20230301_210433](https://user-images.githubusercontent.com/88727968/222186693-932e241c-5f92-4876-b4de-e51271ea6ae9.png)
6) Итак, мы получили, что наше устройство монтируется по адресу `/dev/ttyUSB0`. В вашем случае адрес монтирования может отличаться. Этот адрес мы будем использовать при подключении через UART. Будьте внимательны, при подключении нескольких подобных устройств, а также при некорректном отключении, адрес монтирования может меняться.
________

### Разборка

В настоящее время камера поддерживает только прошивку через UART адаптер, поэтому для выполнения операций нам придется разобрать камеру.
**Помните, что это лишит вас гарантии производителя!** 

Нам понадобятся:
- Само устройство
- Фен или другое нагревательное устройство
- Плоский тонкий предмет, например канцелярский нож
- Длинная тонкая отвертка x-type
- Аккуратность

Итак, преступим:
1) Аккуратно прогрейте лицевую часть камеры (там, где объектив)
2) При помощи канцелярского ножа или иного заостренного предмета акуратно подденьте лицевую часть. Помните, под лицевой частью находятся важные провода, ***не повредите их!***
3) Двигаясь по кругу, аккуратно отделите лицевую чать от корпуса камеры. ***Помните о том, что она соединена проводами с главной платой камеры!***
4) Под низом вы найдете два болта, котроые необходимо открутить. После этого аккуратно разъедините половинки корпуса камеры. Будьте осторожны, не причините себе вреда и не повредите соединительные провода, а также детали камеры!
5) Отверните еще несколько винтов, освободив главную плату устройства. Также освободите type С порт
 ______________

### Изучение
Внимательно изучите камеру, найдите необходимые элементы, потому что на следующих этапах нам придется физически взаимодействовать с некоторыми из них.
Производитель может менять некоторые компоненты устройства без уведомления потребителей. По этой причине две камеры, выпущенные в разное время, могут иметь разную начинку и отличающееся программное обеспечение. Поэтому еще раз внимательно изучите компоненты, убедитесь что они соответствуют тем, что указаны в данном руководстве. При возникновении вопросов, пожалуйста обратитесь в наш [Телеграм-канал](https://t.me/openipc_modding)

##### Главная плата (вид с лицевой стороны)
![IMG_20210904_194002](https://user-images.githubusercontent.com/88727968/222473262-913af0d1-0fee-4ae6-843f-256784381163.jpg)Главная плата (вид с лицевой стороны). 

Самая важная для нас часть на ней - чип памяти.
##### Чип памяти на ней
![IMG_20210904_194034](https://user-images.githubusercontent.com/88727968/222473270-dfb08412-0019-4a57-aecc-3820421263e8.jpg)Чип памяти на ней.

Он имеет численно-буквенную маркировку. Убедитесь что чип на  вашей плате имеет подобную маркировку! Число 128 означает число бит памяти. 128/8=16, значит наша память 16 Мб. Выбирать прошивку нужно под этот тип памяти!
##### Главная плата (вид с тыльной стороны)
![IMG_20210904_194151](https://user-images.githubusercontent.com/88727968/222473288-c3efcdc6-2691-452f-aebb-9ab1789d4d2d.jpg)Главная плата (вид с тыльной стороны). 

Здесь расположен беспроводной модуль, центральный процессор и различные другие компоненты

##### Центральный процессор
![IMG_20210904_194132](https://user-images.githubusercontent.com/88727968/222473285-9c00e6d9-f585-4481-a48b-867b0c1f3a85.jpg)Центральный процессор. 

В нашем случае он имеет численно-буквенную маркировку. Ingenic T31N. Буква N - серия. Она указана во втором ряду. [Подробнее](https://wiki.openipc.org/en/installation.html#step-1-determine-the-system-on-chip)
_______________