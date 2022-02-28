# lab2


## Каков размер MBR из чего он состоит?
Менее 512 байт, состоит из 3-х компонентов:
- 446 байт - главная загрузочная информация;
- 64 байта - информация о таблице раздела;
- 2 байта - проверка корректности MBR.

## Сколько разделов поддерживает MBR?
4 раздела

## описать процесс загрузки все этапы на bios и uefi
- Сначала происходит базовая система загрузки, проверки целостности  устройства происходят. Ищет и выполняет загрузчик операционной системы.
- MBR  или  GPT в свою очередь передают управление загрузкой Grub (Grub2).
- Grub подготавливает систему к загрузке ядра операционной системы. 
- Дальше выполняется загрузка ядра Linux.
- Ядро, сразу после своей загрузки, запускает главный процесс инициализации, который приводит к запуску всех необходимых служб и программ.
- После того, как операционная система загрузилась, у нас есть возможность устанавливать уровни выполнения. Всего их 7 от 0 до 6.

## описать порядок загрузки ОС на sysVinit и systemd
Есть 2 системы инициализации sysVinit и systemd.

sysVinit - запускаются определённые службы, которые соответствуют уровню выполнения. Есть определенные директории уровней в которых есть определенные службы, которые либо убиваются, либо создаются при переходе на этот уровень. Все происходит в определенной последовательности.
init просит ядро запустить некоторые начальные системные вызовы. После закрывает свой стандартный вход и выход. После делает себя лидером сеанса. Он читает и обрабатывает inittab.
Он запускает все процессы, у которых есть запись inittab.

systemd улучшенный родитель всех процессов. функции более обширные чем в sysVinit.
Он отвечает за инициализацию необходимых файловых систем, служб и драйверов, необходимых для работы системы
В системах systemd этот процесс разбивается на различные этапы, которые выставляются в качестве целевых единиц. Процесс загрузки сильно распараллелен, так что порядок достижения конкретных целевых единиц не является детерминированным.  
Сначала systemd монтирует файловые системы, определенные в /etc/fstab, включая любые файлы подкачки или разделы. В этот момент она может получить доступ к файлам конфигурации, расположенным в /etc, включая свои собственные. Он использует свой файл конфигурации, /etc/systemd/system/default.target, для определения состояния или цели, в которую он должен загружать хост.
Для сервера по умолчанию, скорее всего, будет использоваться многопользовательский.target, который аналогичен уровню выполнения 3 в SystemV. Emergency.target аналогичен однопользовательскому режиму.

## показать скриншоты вашего используемого Linux дистрибутива и объяснить на какой системе инициализации он работает
Мной используется Ubuntu 20.04 LTS, на ней используется система инициализации systemd. Поскольку начиная с  версии 15.04 ubuntu переходит с системы инициализации upstart на systemd, а 16.04 LTS становится первым LTS-выпуск Ubuntu, который переведён на систему инициализации systemd.
![!](https://github.com/blackrose198/lab2/blob/main/photo_2022-02-27_17-16-45.jpg)
