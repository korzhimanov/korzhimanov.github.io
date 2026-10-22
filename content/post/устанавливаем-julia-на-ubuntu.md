+++
date = "2017-01-21T18:00:00.000+03:00"
draft = true
image = ""
math = false
tags = ["linux", "ubuntu", "julia", "jupyter", "atom"]
title = "Устанавливаем Julia на Ubuntu"
+++

[Julia](http://julialang.org/) — это появившийся недавно язык программирования с динамической типизацией, но с упором на производительность. Его создавали специально для научных вычислений. О преимуществах языка я расскажу в другом посте, а здесь опишу процедуру установки Julia и сопутствующих компонентов — [IJulia](https://github.com/JuliaLang/IJulia.jl) и [Juno IDE](http://www.junolab.org/) на Ubuntu. У меня стоит Ubuntu версии 14.04, но думаю, для более поздних версий отличий нет.

## Устанавливаем Julia

Итак, правило первое — не устанавливайте Julia из репозиториев. Язык новый, развивающийся, и репозитории за этим развитием не поспевают. Поэтому устанавливаем язык самостоятельно. Можно напрямую с сайта, но лучше из [специально созданного PPA-репозитория](https://launchpad.net/~staticfloat/+archive/ubuntu/juliareleases), в котором всегда подерживается последняя версия Julia.

Для начала добавим в источники наших приложений [репозиторий, в котором содержатся необходимые для работы Julia библиотеки](https://launchpad.net/~staticfloat/+archive/ubuntu/julia-deps):
~~~bash
sudo add-apt-repository ppa:staticfloat/julia-deps
~~~

Теперь добавляем репозиторий с самой Julia:
~~~bash
sudo add-apt-repository ppa:staticfloat/juliareleases
~~~

Обновляем наши базы:
~~~bash
sudo apt-get update
~~~

Обновляем версии библиотек для корректной работы Julia:
~~~bash
sudo apt-get upgrade
~~~

И наконец, устанавливаем интерпретатор языка и документацию:
~~~bash
sudo apt-get install julia julia-docs
~~~

После этого мы можем запустить интерактивную среду:
~~~bash
julia
~~~

В принципе, этого уже достаточно, чтобы начать работу с языком Julia, но для большего удобства установим ещё пару вещей.

## Устанавливаем Juno IDE

Во-первых, если мы планируем работать над относительно большими проектами, то хорошо бы иметь удобную среду разработки. К сожалению, нативной IDE для Julia на данный момент не существует, но есть надстройка над [редактором Atom](https://atom.io/), которая позволяет в известной степени имитировать IDE. Эта надстройка носит название [Juno](http://junolab.org/).

Для начала надо установить редактор Atom. Как и в случае с Julia, можно сделать это с официального сайта, но лучше подключить [уже готовый репозиторий](https://launchpad.net/~webupd8team/+archive/ubuntu/atom):
~~~bash
sudo add-apt-repository ppa:webupd8team/atom
sudo apt-get update
sudo apt-get install atom
~~~

Теперь запускаем редактор и открываем его настройки (Ctrl + ,). Идём во вкладку `install` и ищем пакет `uber-juno`. Устанавливаем его — он автоматически установит ещё ряд пакетов, необходимых для работы. Всё, Juno IDE готово к работе.

## Устанавливаем IJulia

Но мы установим ещё один инструмент, который удобен для разработки небольших скриптов, а также для быстрого прототипирования. Это [IJulia](https://github.com/JuliaLang/IJulia.jl) — ядро для блокнотов [Jupyter Notebook](http://jupyter.org/), позволяющее писать программы на языке Julia в интерактивном режиме.

Поскольку я использую Jupyter и для других языков, он у меня уже установлен отдельно. В этом случае порядок действий следующий. Узнаём путь к запускающему файлу Jupyter Notebook:
~~~bash
which jupyter-notebook
~~~

Запускаем Julia:
~~~bash
julia
~~~

В открывшейся интерактивной среде определяем новую переменную окружения:
~~~julia
ENV["JUPYTER"]="/путь/к/jupyter-notebook"
~~~
И устанавливаем пакет IJulia (это надо будет выполнить один раз, после чего он уже будет в системе):
~~~julia
Pkg.add("IJulia")
~~~

Импортируем установленный пакет и запускаем блокнот:
~~~julia
using IJulia
notebook()
~~~

В браузере должна открыться страница по адресу `localhost:8888` (или что-то аналогичное в зависимости от ваших настроек Jupyter), на которой будет доступен для создания notebook на языке Julia.

Теперь можно всё закрыть и выйти из Julia. Её ядро будет доступно в Jupyter Notebook и при его запуске из терминала.

Всё минимально необходимое для начала работы с языком Julia  у нас теперь есть!

## Источники с подробностями:

- [Официальная страница загрузки Julia](http://julialang.org/downloads/)
- [Julia/IJulia/JuMP Installation Guide](http://www.juliaopt.org/install.pdf) от JuliaOpt
- [Julia for Numerical Computation in MIT Courses](https://github.com/stevengj/julia-mit)
- [IJulia](https://github.com/JuliaLang/IJulia.jl) на GitHub
- [Julia Releases](https://launchpad.net/~staticfloat/+archive/ubuntu/juliareleases) на Launchpad
- [Introducing Julia/Getting started](https://en.wikibooks.org/wiki/Introducing_Julia/Getting_started) на Wikibooks
- [The Juno IDE Documentation](http://docs.junolab.org/latest/#The-Juno-IDE-Documentation-1)
