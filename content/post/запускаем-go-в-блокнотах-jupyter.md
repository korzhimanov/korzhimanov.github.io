+++
date = "2017-01-21T21:00:00.000+03:00"
draft = true
image = ""
math = false
tags = ["linux", "ubuntu", "go", "jupyter", "atom"]
title = "Запускаем go в блокнотах jupyter"
+++

Люблю блокноты [Jupyter](http://jupyter.org/) за возможность удобно и быстро писать небольшие скрипты и прототипы. Расскажу, как подружить их с языком [Go](https://golang.org/).

Воспользуемся ядром [gophernotes](https://github.com/gopherds/gophernotes), разработанным специально для этого. Я буду предполагать, что [Go уже установлен и правильно настроен](https://golang.org/doc/install).

## Устанавливаем goimports

Первым делом устанавливаем `goimports`, если он ещё не установлен:
~~~bash
go get golang.org/x/tools/cmd/goimports
~~~

## Устанавливаем ZeroMQ

Далее нам потребуется библиотека [`ZeroMQ`](http://zeromq.org/) версии 4.x. К сожалению, у меня Ubuntu 14.04, и в неё установлена более старая версия этой библиотеки. Репозиториев с новой версией тоже нет, поэтому пришлось устанавливать её вручную.

Загружаем архив с [официального сайта](http://zeromq.org/intro:get-the-software):
~~~bash
wget https://github.com/zeromq/libzmq/releases/download/v4.2.0/zeromq-4.2.0.tar.gz
~~~

Распаковываем:
~~~bash
tar -xzvf zeromq-4.2.0.tar.gz
~~~

Переходим в распакованную директорию и собираем (но пока не устанавливаем!) библиотеку:
~~~bash
cd zeromq-4.2.0
./configure
make
~~~

Установку библиотеки доверим программе [`checkinstall`](http://www.asic-linux.com.mx/~izto/checkinstall/), которая создаст deb-пакет, что облегчит нам в случае чего удаление библиотеки и её апдейт:
~~~bash
checkinstall
~~~

Обновляем линки к библиотекам:
~~~bash
ldconfig
~~~

Теперь можем продолжить установку `gophernotes`.

## Устанавливаем gophernotes

Скачиваем:
~~~bash
go get -tags zmq_4_x github.com/gopherds/gophernotes
~~~

Узнаём, где Jupyter будет смотреть установленные ядра:
~~~bash
jupyter --data-dir
~~~

У меня это стандартный `$HOME/.local/share/jupyter`, так что копируем скаченное ядро туда:
~~~bash
mkdir ~/.local/share/jupyter/kernels/gophernotes
cp -r $GOPATH/src/github.com/gopherds/gophernotes/kernel/* ~/.local/share/jupyter/kernels/gophernotes
~~~

Собственно, всё. Теперь при запуске Jupyter Notebook в списке доступных ядер должен появиться язык Go!
