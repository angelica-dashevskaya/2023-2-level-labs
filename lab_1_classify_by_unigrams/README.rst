Лабораторная работа №1
======================


.. toctree::
    :maxdepth: 1
    :titlesonly:
    :caption: Full API

    lab_1_classify_by_unigrams.api.rst


Дано
----

1. Три текста на `английском <assets/texts/en.txt>`__,
   `немецком <assets/texts/de.txt>`__ и
   `неизвестном <assets/texts/unknown.txt>`__ языках.
2. `Языковые профили <assets/profiles>`__ немецкого, английского,
   испанского, французского, итальянского, русского и турецкого языков.
   Необходимо определить, на каком языке написан неизвестный текст,
   анализируя встречаемость букв в каждом из языков.

Что надо сделать
----------------

Шаг 0. Подготовка (проделать вместе с преподавателем на практике)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Создать форк репозитория.
2. Установить необходимые инструменты для работы.
3. Изменить файлы ``main.py`` и ``start.py``.
4. Закоммитить изменения и создать Pull request.

**Важно:** в файле ``start.py`` вы должны написать код, определяющий
язык неизвестного текста.

Для этого реализуйте функции в модуле ``main.py`` и импортируйте их в
``start.py``. Весь код, выполняющий детектирование языка, должен быть
выполнен в функции ``main`` в файле ``start.py``:

.. code:: py

   def main() -> None:
       pass

Вызов функции в файле ``start.py``:

.. code:: py

   if __name__ == '__main__':
       main()

В рамках данной лабораторной работы **нельзя использовать модули
collections, itertools, а также сторонние модули.**

Обратите внимание, что желаемую оценку необходимо указать в файле
`target_score.txt <target_score.txt>`__. Возможные значения: 0, 4,
6, 8, 10. Чем большее значение выставлено, тем больше тестов будет
запущено.

Шаг 1. Токенизировать текст
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. important:: Выполнение Шага 1 соответствует 4 баллам

Реализуйте функцию :py:func:`lab_1_classify_by_unigrams.main.tokenize`.

Например, строка ``'Hey! How are you?'`` должна быть токенизирована
следующим образом:
``['h', 'e', 'y', 'h', 'o', 'w', 'a', 'r', 'e', 'y', 'o', 'u']``.

Продемонстрируйте выделяемые токены из текста на английском языке в
файле ``start.py``. Текст на английском языке сохранен в переменную
``en_text``.

Шаг 2. Получить частотный словарь по заданному тексту
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Реализуйте функцию
:py:func:`lab_1_classify_by_unigrams.main.calculate_frequencies`.

Под относительной частотой подразумевается отношение количества
вхождений токена к общему числу токенов.

Так, из последовательности токенов
``['h', 'e', 'y', 'h', 'o', 'w', 'a', 'r', 'e', 'y', 'o', 'u']``
должен получиться следующий словарь частот:

.. code:: py

    {'h': 0.16666666666666666,
     'e': 0.16666666666666666,
     'y': 0.16666666666666666,
     'o': 0.16666666666666666,
     'w': 0.08333333333333333,
     'a': 0.08333333333333333,
     'r': 0.08333333333333333,
     'u': 0.08333333333333333}

Шаг 3. Создать профиль конкретного языка
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. important:: Выполнение Шагов 1-3 соответствует 6 баллам

Профиль языка – это структура с информацией о конкретном языке. В
настоящей лабораторной работе профиль языка состоит из названия языка и
частотного словаря.

В дальнейших лабораторных работах вы будете работать с другими языковыми
профилями. Пример языковых профилей вы можете найти в
`следующем проекте <https://github.com/shuyo/language-detection>`__.
Несмотря на то что данные профили содержат информацию о n-граммах,
с которыми мы познакомимся позднее, структура этих профилей аналогична.

Пример языкового профиля, который требуется в настоящей лабораторной
работе:

.. code:: json

   {
       "name": "en",
       "freq": {
           "g": 0.8,
           "t": 0.2
        }
   }

Здесь ключу ``"freq"`` соответствует частотный словарь, ключу ``"name"``
– название языка.

В данной лабораторной работе языковой профиль **обязательно**
представляет собой словарь, который содержит два ключа – ``"freq"`` и
``"name"``.

Реализуйте функцию
:py:func:`lab_1_classify_by_unigrams.main.calculate_frequencies`.

Для токенизации необходимо использовать
функцию :py:func:`lab_1_classify_by_unigrams.main.tokenize`.
Для получения частотного словаря необходимо использовать
функцию :py:func:`lab_1_classify_by_unigrams.main.calculate_frequencies`.

Продемонстрируйте создание языкового профиля для английского языка в
файле ``start.py``.

Шаг 4. Рассчитать метрику ``MSE``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

В дальнейшем для определения близости двух языковых профилей нам
понадобится метрика среднеквадратичной ошибки (``MSE``, `Mean Squared
Error <https://en.wikipedia.org/wiki/Mean_squared_error>`__). Для начала
рассмотрим эту метрику безотносительно применения к задаче детекции
языка.

Значение ``MSE`` рассчитывается следующим образом:
:math:`MSE = \frac{\sum (y_{i} - p_{i})^{2}}{n}`

Здесь ``y`` - истинное значение, ``p`` - предсказанное значение,
``n`` - количество значений. Обратите внимание, что количество
истинных значений ``y`` и количество предсказанных значений
``p`` совпадает и равно ``n``.

Таким образом, метрика ``MSE`` - не что иное, как среднее квадратов
разности между истинными значениями и предсказанными значениями. Чем это
значение меньше, тем ближе предсказанные значения к истинным.

Реализуйте функцию :py:func:`lab_1_classify_by_unigrams.main.calculate_mse`.

Шаг 5. Сравнить два языковых профиля
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Для сравнения языковых профилей необходимо рассчитать значение метрики
``MSE``, определяющей различие между двумя языками.

Для этого необходимо выделить все токены, встречающиеся в двух языковых
профилях, а также сопоставить им частотность в каждом из языков. Иными
словами, мы находим объединение множества токенов в первом языке с
множеством токенов во втором языке. Далее, для каждого из токенов
находим его встречаемость в каждом из множеств.

Для примера рассмотрим два таких языковых профиля:

.. code:: py

   profile_1 = {
       'name': 'lang1',
       'freq': {'a': 0.5, 'b': 0.5}
   }
   profile_2 = {
       'name': 'lang2',
       'freq': {'b': 0.5, 'c': 0.5}
   }

В данных профилях встречаются следующие символы: ``a``, ``b``, ``c``.
При этом в профиле первого языка их встречаемость следующая:
``[0.5, 0.5, 0]``. В профиле второго языка встречаемость такая:
``[0, 0.5, 0.5]``.

Приняв встречаемость символов в первом языке за истинные значения и
встречаемость символов во втором языке за предсказанные, мы можем
рассчитать разницу профилей по метрике ``MSE``. Ее значение будет равно
``0.167`` (с округлением до третьего знака).

.. note:: Что изменится, если сделать наоборот и принять за истинные
          значения встречаемость токенов во втором языке и за предсказанные -
          в первом? Почему?

Реализуйте функцию сравнения двух языковых профилей
:py:func:`lab_1_classify_by_unigrams.main.compare_profiles`.

Для расчета метрики ``MSE`` необходимо обращаться к функции
:py:func:`lab_1_classify_by_unigrams.main.calculate_mse`.

Шаг 6. Определить язык неизвестного текста
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. important:: Выполнение Шагов 1-6 соответствует 8 баллам

Реализуйте функцию :py:func:`lab_1_classify_by_unigrams.main.detect_language`.

Она определяет язык текста на основе метрики ``MSE`` и
возвращает название языка с наименьшим значением ``MSE``. Название языка
находится в языковом профиле. Если у двух языков одинаковое значение
метрики, отсортируйте названия языков в алфавитном порядке и возьмите
первое.

Для нахождения значения метрики ``MSE`` необходимо использовать функцию
:py:func:`lab_1_classify_by_unigrams.main.compare_profiles`.

В файле ``start.py`` определите, к какому языку ближе текст на
неизвестном языке: к английскому или к немецкому. Текст на немецком
языке сохранен в переменной ``de_text``. Текст на неизвестном языке
сохранен в переменной ``unknown_text``.

Шаг 7. Загрузка языкового профиля
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Для определения языка может быть недостаточно двух языковых профилей. На
самом деле в данной задаче может использоваться произвольное количество
языковых профилей (например, 6).

Для дальнейшей работы нам потребуется возможность загружать языковой
профиль из файла с расширением ``json``. Узнать больше об этом типе
файлов можно `здесь <https://en.wikipedia.org/wiki/JSON>`__.

В папке `assets <assets>`__ для вас сохранены несколько языковых
профилей. Для корректной работы необходимо сформировать путь к каждому
из них в формате ``assets/profiles/<filename>.json``. Например, путь к
испанскому языковому профилю должен выглядеть так:
`assets/profiles/es.json <assets/profiles/es.json>`__. Сохраните
список таких путей в переменную в файле ``start.py``.

Для чтения и сохранения такого типа файлов часто используется
стандартный модуль ``json``, изучить его документацию можно
`здесь <https://docs.python.org/3/library/json.html>`__.

Реализуйте функцию чтения языкового профиля из файла
:py:func:`lab_1_classify_by_unigrams.main.load_profile`.

Функция должна только читать файл, никакой дополнительной обработки не
подразумевается.

Пример вызова функции:

.. code:: py

   language_profile = load_profile(filename)

Шаг 8. Обработка языкового профиля
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Языковые профили могут выглядеть по-разному. Вы можете заметить, что
языковые профили в папке `assets <assets>`__ имеют особенный формат.
В них содержатся токены не только из одного символа, но и из нескольких,
а также присутствует дополнительный ключ ``n_words``.

Необходимо привести языковой профиль к нашему единому формату.

Реализуйте функцию
:py:func:`lab_1_classify_by_unigrams.main.preprocess_profile`,
приводящую языковой профиль к формату, требуемому в
настоящей работе. Напоминаем, что языковой профиль должен содержать
только два ключа: ``name`` и ``freq``. По ключу ``freq`` содержится
частотный словарь, ключами которого выступают униграммы в нижнем
регистре, значениями - относительная встречаемость токена.

Например, мы имеем следующий необработанный языковой профиль:

.. code:: py

   profile_raw = {
       'name': 'lang1',
       'freq': {
           'ab': 3,
           '4c': 2,
           'a': 1
           'F': 2,
           'c&': 1,
           'abc': 7
       },
       'n_words': [3, 6, 7]
   }

Для приведения языкового профиля к нужному формату необходимо из
представленного набора токенов (ключ ``freq``) выбрать униграммы,
состоящие из букв, привести их к нижнему регистру, а также посчитать
относительную частоту. Поле ``n_words`` содержит в себе список из трех
чисел, которые обозначают количество униграмм, биграмм и триграмм,
соответственно. Для подсчета относительной частоты токенов используйте
первое из трех чисел поля ``n_words``, при этом ключа ``n_words`` в
обработанном профиле быть не должно. Поле ``name`` не требуется
дополнительно обрабатывать.

Таким образом, приведенный пример необработанного языкового профиля
должен быть приведен к следующему:

.. code:: py

   profile_raw = {
       'name': 'lang1',
       'freq': {
           'a': 0.33333333333,
           'f': 0.66666666666,
       },
   }

Пример вызова функции:

.. code:: py

   processed_profile = preprocess_profile(profile_raw)

Шаг 9. Сбор языковых профилей
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Поскольку нам предстоит сравнение целого ряда языковых профилей,
необходимо загрузить и предобработать сразу несколько профилей.

Реализуйте функцию :py:func:`lab_1_classify_by_unigrams.main.collect_profiles`.

Функция должна вызывать :py:func:`lab_1_classify_by_unigrams.main.load_profile`
и :py:func:`lab_1_classify_by_unigrams.main.preprocess_profile`.

Пример вызова функции:

.. code:: py

   collected_profiles = collect_profiles(paths_to_profiles)

Шаг 10. Определить язык неизвестного текста
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Теперь мы готовы определить язык неизвестного текста, рассматривая сразу
несколько возможных вариантов.

Реализуйте функцию
:py:func:`lab_1_classify_by_unigrams.main.detect_language_advanced`
Функция возвращает отсортированный список кортежей вида
``[('lang1', score), ('lang2', score)]``, где первым элементом кортежа
выступает название языка, а вторым - значение ``MSE``. Длина списка
соответствует количеству переданных профилей известных языков.
Сортировка предполагается от меньшего значения метрики к большему. Языки
с совпадающим значением ``MSE`` необходимо упорядочить
лексикографически.

.. note:: Почему в данной задаче лучше сортировать от меньшего к
          большему, а не наоборот?

Шаг 11. Сформировать отчет
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. important:: Выполнение Шагов 1-11 соответствует 10 баллам

Теперь, когда мы можем сравнивать целый ряд языков, необходимо
сформировать понятный отчет.

Для этого реализуйте функцию
:py:func:`lab_1_classify_by_unigrams.main.print_report`,
которая печатает отчет в следующей форме:

.. code:: py

   <lang1>: MSE <score>
   <lang2>: MSE <score>

Обратите внимание, что ``score`` необходимо округлить до пяти знаков
после запятой.

Например, если был получен результат
``[('en', 0.00013213), ('de', 0.00016231), ('fr', 0.00010123)]``, то при
вызове функции в консоль будет выведен следующий отчет:

.. code:: py

   en: MSE 0.00013
   de: MSE 0.00016
   fr: MSE 0.0001

Для округления можно использовать форматирование строки:
``f'{score:.5f}'``.

Продемонстрируйте детекцию неизвестного языка путём сравнения с
языковыми профилями английского, французского, итальянского, русского,
испанского и турецкого языков в файле ``start.py``. Для вывода отчета в
консоль вызовите функцию ``print_report``.

Полезные ссылки
---------------

-  `Коллекция языковых
   профилей <https://github.com/shuyo/language-detection>`__
-  `Описание метрики Mean Squared
   Error <https://en.wikipedia.org/wiki/Mean_squared_error>`__
-  `Описание формата JavaScript Object
   Notation <https://en.wikipedia.org/wiki/JSON>`__
-  `Описание стандартной библиотеки
   json <https://docs.python.org/3/library/json.html>`__

