# SFML в Ubuntu 16.04

__SFML__ (англ. Simple and Fast Multimedia Library — простая и быстрая мультимедийная библиотека) — свободная кроссплатформенная мультимедийная библиотека. Написана на C++, но доступна также для C, D, Java, Python, Ruby, OCaml, .Net и Go. Представляет собой объектно-ориентированный аналог SDL.

SFML содержит ряд модулей для простого программирования игр и мультимедиа приложений. Исходный код библиотеки предоставляется под лицензией zlib/png license.

В Ubuntu 16.04 SFML представлен модулями:

<table class="table table-bordered">
<thead>
<tr>
<th>Модуль</th>
<th>deb-пакет</th>
<th>Описание</th>
</tr>
</thead>
<tbody>
<tr>
<td>System</td>
<td><code>libsfml-system2.3v5</code></td>
<td>Управление временем и потоками, он является обязательным, так как все модули зависят от него.</td>
</tr>
<tr>
<td>Window</td>
<td><code>libsfml-window2.3v5</code></td>
<td>Управление окнами и взаимодействием с пользователем.</td>
</tr>
<tr>
<td>Graphics</td>
<td><code>libsfml-graphics2.3v5</code></td>
<td>Делает простым отображение графических примитивов и изображений, для своей работы требует модуль Window.</td>
</tr>
<tr>
<td>Audio</td>
<td><code>libsfml-audio2.3v5</code></td>
<td>Предоставляет интерфейс для управления звуком.</td>
</tr>
<tr>
<td>Network</td>
<td><code>libsfml-network2.3v5</code></td>
<td>Для сетевых приложений.</td>
</tr>
</tbody>
</table>

Установить все модули можно с помощью пакета libsfml-dev:

<pre class="terminal"><span class="user"></span>sudo apt install libsfml-dev
</pre>

Дополнительно доступны еще пакет документации `libsfml-doc` и пакет отладочных символов `libsfml2`.3-dbg.

## Компиляция программы c SFML

Рассмотрим следующий код на языке C++ демонстрирующее простейшее приложение на SFML (отображаем окно и заливаем его синим цветом):

```cpp
//Подключаем заголовок модуля '''Graphics''', а он автоматически подключает заголовок модуля '''Window'''
# include <SFML/Graphics.hpp>

int main()
{
    // Создаём окно
    sf::RenderWindow App(sf::VideoMode(800, 600, 32), "Hello World - SFML");

    // Основной цикл
    while (App.isOpen())
    {
        // Проверяем события (нажатие кнопки, закрытие окна и т.д.)
        sf::Event Event;
        while (App.pollEvent(Event))
        {
            // если событие "закрытие окна":
            if (Event.type == sf::Event::Closed)
                 //закрываем окно
                App.close();
        }

        // очищаем экран, и заливаем его синим цветом
        App.clear(sf::Color(0,0,255));

        // отображаем на экран
        App.display();
    }

    return 0;
}
```

Файл с кодом назовем `hello.cpp`. Простой `makefile` для сборки:

```make
PACKAGE = hello

CC      = g++
CLEAN   = rm -f
CFLAGS  = -Wall -g
LDADD   = `pkg-config sfml-all --libs` -lm


all: $(PACKAGE)

$(PACKAGE): hello.o
    $(CC) -o $(PACKAGE) hello.o $(LDADD)

hello.o: hello.cpp
    $(CC) $(CFLAGS) -c -o hello.o hello.cpp

clean:
    $(CLEAN) $(PACKAGE)
    $(CLEAN) hello.o
```

В Ubuntu заголовочные файлы имеют стандартное расположение, поэтому указывать путь на них не надо. Подключаются библиотеки ключами:

<table class="table table-bordered table-compact">
<thead>
<tr>
<th>Модуль</th>
<th>Ключ</th>
</tr>
</thead>
<tbody>
<tr>
<td>System</td>
<td><code>-lsfml-system</code></td>
</tr>
<tr>
<td>Window</td>
<td><code>-lsfml-window</code></td>
</tr>
<tr>
<td>Graphics</td>
<td><code>-lsfml-graphics</code></td>
</tr>
<tr>
<td>Audio</td>
<td><code>-lsfml-audio</code></td>
</tr>
<tr>
<td>Network</td>
<td><code>-lsfml-network</code></td>
</tr>
</tbody>
</table>

Указывать библиотеки можно с помощью pkg-config. Следующая команда подключит все модули:

```make
LDADD = `pkg-config sfml-all --libs` -lm
```


----------

Вернуться  [на главную страницу](../index.html)
