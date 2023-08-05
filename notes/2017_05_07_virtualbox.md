# Первоначальная настройка Debian/Ubuntu в виртуальной машине VirtualBox [set-debian-ubuntu-in-virtualbox]

Дата: 7 мая 2017 г.

## Установка расширений гостевой ОС [install]

Дополнения гостевой ОС VirtualBox предоставляют графические драйвера, позволяют создавать общие папки и использовать общий буфер обмена.

В начале необходимо обновить систему и перезагрузить её:

<pre class="terminal"><span class="user"></span>sudo apt update
sudo apt full-upgrade
sudo reboot
</pre>

Установить dkms и основные компиляторы для сборки модулей dkms:

<pre class="terminal"><span class="user"></span>sudo apt install build-essential dkms
</pre>

В меню VirtualBox выбираем Устройства → "Установить Дополнения гостевой ОС". Затем в директории подключенного образа выполняем команду:

<pre class="terminal"><span class="user"></span>sudo sh ./VBoxLinuxAdditions.run
</pre>

Перезагружаем систему (можно сделать после следующего шага)


## Общая папка [common-dir]

Для обмена файлами с основной системой можно создать общую папку. Чтобы нормально пользоваться этой папкой необходимо добавить пользователя в группу `vboxsf`.

<pre class="terminal"><span class="user"></span>sudo usermod -a -G vboxsf `whoami`
</pre>

И перезагружаем систему


### Ссылки [links]

- [записки для себя: Установка дополнения в гостевую ОС Debian в Virtualbox](http://vr-77.blogspot.ru/2012/08/debian-virtualbox.html) ([Копия](2017_05_07_virtualbox-debian.html))

    Или [Эникеинг различной «фигни»: Установка Virtualbox Guest additions в Ubuntu Server](http://stuff-coding.blogspot.ru/2011/07/virtualbox-guest-additions-ubuntu.html)


----------

Вернуться  [на главную страницу](../index.html)
