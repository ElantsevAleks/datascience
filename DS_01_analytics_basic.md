# Data Scientist. Аналитика. Начальный уровень
## Оглавление
* [Основы Python: Module 1-9](#basic)
* [Аналитика. Начальный уровень. Module 10-19](#analytics)

## <a id='basic'></a>**Основы Python. Module 1-9**
### **Дополнительные материалы**
* [Python.org рекомендует: Программирование для НЕпрограммистов](https://m.habr.com/ru/company/skillfactory/blog/480898/)
* Курсы на Stepik
  * todo - дописать отзыв по курсам, тех кто их проходил
  * [Python для начинающих](https://stepik.org/course/58852/promo)
  * [Python: основы и применение](https://stepik.org/course/512/promo)
  * [Адаптивный тренажер Python](https://stepik.org/course/431/promo)
  * [Программирование на Python](https://stepik.org/course/67/promo)
* Сайты для тренировок:
  * [Codewars: Achieve mastery through challenge](https://www.codewars.com/)
  * [CheckiO - coding games and programming challenges](https://checkio.org/)
* [Конспект по модулям - «Основы Python» by Andrew](ipynb/DS_01_analytics_basic_python.ipynb)

### Module 4 - Основы Python: базовые структуры данных
Входе занятия происходит знакомство с проверкой типов данных, для проверки является ли строка числом\цифрами в python есть несколько различных методов. Статья описывающая подробности [Python’s str.isdigit vs. str.isnumeric](https://lerner.co.il/2019/02/17/pythons-str-isdigit-vs-str-isnumeric/)
  * `'1234'.isdigit()` - проверяет арабские цифры 0-9
  * `'一二三四五'.isnumeric()` - проверяет не только арабские цифры, но и цифры в другом формате - например китайские иероглефы.
  * '2²'.isnumeric() - также позволит проверять выражения такого типа.
  * `'2.2'.isdecimal()` - с плавающей точкой.

## <a id='analytics'></a>**Аналитика. Начальный уровень. Module 10-19**
### **Дополнительные материалы**
* [Практикум по математике и Python](https://stepik.org/course/3356/promo)

### Module 10 - Библиотека NumPy. Часть 1
* Мера Жакара - «На самом деле в задании коэффициент Танимото, а не мера Жаккара. (Хоть первый и является расширением второго).»
* «В качестве множеств 𝐴 и 𝐵 в данной задаче выступают множества (группы) студентов, которые получили зачёт у первого **и** второго экспертов.»
* Тут подразумевается просто отношение мощности пересечения к мощности объединения.

### Module 12 - Библиотека pandas. Часть 1
* [Дополнительная практика - pandas](https://github.com/ajcr/100-pandas-puzzles/blob/master/100-pandas-puzzles.ipynb)

#### **Проблемы в модуле**
Проблема с загрузкой данных в формате numpy `"ValueError: Object arrays cannot be loaded when allow_pickle=False"`.  
Решается добавлением ключа: `arr = np.load(file_path, allow_pickle=True)`  
Подробности об ошибке:
* Проблема возникла после версии `numpy==1.16.2`
* `pickle` - модуль сериализации и десериализации данных (Бинарый формат), для повышения безопастности было изменено поведение библиотеки по умолчанию
* [Подробности](https://stackoverflow.com/questions/55824625/how-to-fix-object-arrays-cannot-be-loaded-when-allow-pickle-false-in-the-sketc)
* [Ещё подробности](https://github.com/tensorflow/tensorflow/commit/79a8d5cdad942b9853aa70b59441983b42a8aeb3#diff-b0a029ad68170f59173eb2f6660cd8e0)

Некорректная постановка задачи номер 6. Из условия можно сделать неправильный вывод, что информацию о типе данных в столбцах DataFrame можно получить, используя метод info. Однако, метод info выдает тип данных object для всех столбцов. Это происходит потому, что изначально данные загружаются в виде массива numpy, который требует, чтобы все данные были одного типа, тогда как в столбцах присутствуют str, int и float. Они все обернуты в object, чтобы был один тип. Лечится это так: `dat = dat.infer_objects()` т.е. при помощи метода `infer_objects` приводим объекты (значения в датафрейме, импортированные ранее из numpy массива) к их настоящему виду: int, str, float. Далее уже делаем подсчёт различных типов данных при помощи метода `info`.

### Module 13 - Библиотеку pandas. Часть 2

#### **Задание 8**

По куску таблицы, который печатается, конечно же, нельзя определенно утверждать, что люди с разным доходом рыбачат по-разному. Увидеть разницу нам поможет умение строить barplot, которое нам дается только ниже в необязательных заданиях: `dat.sort_values(['price', 'income'])`
* По маленькому куску таблицы, который выводится этого сказать, конечно же, нельзя.  
  
**Чтобы проверить этот факт, сравним распределения людей по типу рыбалки с доходом меньше медианного и больше:**
<pre>
logic_choice = dat['income'] < dat['income'].median()
dat_for_bar = pd.DataFrame()
dat_for_bar['below_median'] = \
    round(dat[logic_choice].groupby('mode')['income'].agg('count') / logic_choice.sum() * 100)
dat_for_bar['above_median'] = \
    round(dat[~logic_choice].groupby('mode')['income'].agg('count') / (~logic_choice).sum() * 100)

%matplotlib inline
ax = dat_for_bar.plot.bar(rot=0)
ax.set_title("Распределение людей по типам рыбалки")
ax.set_xlabel("Тип рыбалки")
ax.set_ylabel("Процент от общего числа")
</pre>
**Видим, что люди с более низким доходом чаще рыбачат с пирса, а у людей с более высоким доходом, по-видимому, чаще есть своя собственная лодка, но в целом распределения отличаются мало.**

### Module 14 - Визуализация данных с помощью Matplotlib
* [Учебник - справочник по библиотеке](https://pyprog.pro)\
* [Практическое пособие по Matplotlib - дополняется](https://github.com/koslayn/datascience/blob/master/Matplotlib.book.ipynb)

### Module 16 - Чтение и запись данных. Часть 1

#### **Проблемы в модуле**
Домашняя работа, часть 4. Работа с данными формата HTML.
Для правильного отображения символов кириллицы потребуется создать объект класса etree.HTMLParser.

`from lxml import html, etree`
`hparser = etree.HTMLParser(encoding='utf-8')`

И добавить его в аргументы:

`html_tree = html.fromstring(page, parser=hparser)`

### Module 17 - Основы SQL
#### **Дополнительные материалы по SQL:**
[Практика в SQL - LearnDB](https://learndb.ru/courses)  

    > Александр Зенков: ... очень хорошо всё объясняется и сразу на упражнениях отрабатывается. Начальный уровень бесплатный, остальное 300р за месяц, но если сесть, можно разом всё осилить. Мой рекомендасьон👍

[Интерактивный учебник и упражнения по SQL](http://sql-tutorial.ru/)

#### **Проблемы в модуле**
Домашняя работа, часть 15.3.3 - Вывести имена трех самых молодых студентов, трех самых старых преподавателей, названия трех самых продолжительных курсов.

Каждое выражение `SELECT , GROUP BY , LIMIT` нужно включить в скобки, в противном случае будет ошибка.

### Module 18 - Чтение и запись данных. Часть 2

#### **Проблемы в модуле**
Проблема в установке `psycopg2`: 
* для Windows нужно делать `pip install psycopg2-binary`, тогда `import psycopg2` заработает.
* для OS X нужно делать `pip install psycopg2-binary`, тогда `import psycopg2` заработает (возможно не сработает в стандартном терминале, тогда можно попробовать через терминал PyCharm)
