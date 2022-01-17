# FilmML

Данный проект состоит из взаимосвязанных частей: парсера и самого классификатора жанров фильмов.

Парсер разработан исключительно для работы с русскоязычным сайтом ivi.ru. Для работы с ним необходимо передать в главный метод main корневую ссылку на сайт (для дальнейшей конкатенации с ссылками на страницы фильма) и ссылку на необходимый список фильмов (можно использовать любой на данном сайте)
В процессе написания парсера возникли сложности в подбором источника данных. Один из самых распространенных сайтов - kinopoisk.ru, имеет встроенную защиту от чрезмерного количества запросов. И хотя ее можно обойти, было решено подобрать другой интернет-ресурс.

Сайт ivi.ru такой защиты не имеет, однако страницы, на которых расположена база фильмов, генерируются автоматически. Стандартный запрос читает любую страницу как первую. Для решения этой проблемы для генерации запросов была использована библиотека Selenium Webdriver, позволяющая симулировать браузерные запросы. С помощью него возможно прочитать данные в том числе с динамически генерируемых страниц. Таким образом была выделена небольшая база примерно из 1300 кинофильмов, с описанием их аннотации, жанра и названием. 

В результате парсер сформирует файл в формате .tsv с хранящимся множеством фильмов. Каждый элемент списка содержит в себе информацию:
1. Название фильма
2.Год выпуска
3.Жанр
4.Краткое содержание сюжета


При работе с полученным датасетом производились следующие действия:

1. производилась обработка датасета и классификация жанров, определялось, сколько раз встречается каждый жанр в наборе.
2. производилась обработка описаний фильмов - текст чистился от лишних слов - предлогов, союзов и им подобных, а также составлялась частотная таблица встречаемости слов в тексте.
3. В первой части проекта строились модели логистической регрессии и линейных опорных векторов - производилась классификация датасета по жанрам, отсеивались жанры с малым числом включения в датасете для последующего построения моделей.
Во второй строилась обучающаяся модель - датасет был разбит на две части, где первая часть использовалась для обучения, вторая - для теста. Перед работой датасет так же был обработан, были отсеяны жанры с малым включением в датасет. Обучающий датасет был еще разбит на обучающий и валидационный, что после использовалось в реккурентной модели для классификации





Проблемы, возникающие при работе с данным проектом:

1 - необходимо очистить данные. В наборе могут содержаться часто употребляющиеся словесные конструкции, например: "Данный фильм вы можете посмотреть на нашем сайте". Их необходимо убрать. Кроме того, поскольку цель проекта - оценить, какие слова указывают на тот или иной жанр, необходимо очистить сам датасет (описания фильмов) от употребления названий этих жанров (убрать такие слова, как "драма", "комедия" и подобные.

2 - Для более точной и качественной работы необходимо осторожно определять следующие параметры:
- максимальное число встречаемости жанров в датасете, ниже которого все жанры отбрасываются - актуально для первой и второй части проекта, чтобы отбрасывать те данные, которые имеют малый набор, из-за чего общая точность модели будет ниже
- количество эпох и размер batch_size (для второй части проекта) 
- - важно выбрать оптимальные значения, при которых система обучится, но не переобучится. 


Файлы проекта:

ParserIVI - парсер сайта ivi.ru, формирование датасета

Part_1 - первая часть проекта, обработка датасета, построение модели логистической регрессии, линейных опорных векторов. На вход подается итог парсера. На выходе получаются результаты построения моделей.
Part_2 - вторая часть проекта, обработка датасета, рекуррентная модель для классификации. Обучение и анализ. На вход подается файл парсера, разделенный на две части, равные.
Результат работы - точность модели.
film.tsv - результат работы парсера, используемый датасет для остальных проектов.
