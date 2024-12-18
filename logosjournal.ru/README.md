# Архив сайта журнала «Логос»

«Логос» – философско-литературный журнал, принимающий к печати статьи в любой области философии, социальных и гуманитарных наук, а также в транс- и междисциплинарных областях. На сайте опубликованы полные данные о журнале (история, редколлегия, контакты, авторы, партнеры и т.д.), регламенты работы, информация для авторов статей, архив статей и др.

## wpull

**Описание раздела**

В разделе содержится информация о результатах сбора архива сайта при помощи скрипта массовой загрузки с использованием библиотеки Python **wpull**.

**Результаты**

В ходе работы удалось успешно сохранить сайт журнала, что подтверждается при попытке открыть и просмотреть архив в сервисе [REPLAY WEBPAGE](https://replayweb.page/) (не прогружаются лишь отдельные ссылки, ведущие на внешние ресурсы, что вполне ожидаемо). Общий объем содержимого сайта составил 2,95 ГБ. 

![](/logosjournal.ru/replaywebpage.jpg)

## ArchiveReady

**Описание раздела**

В разделе содержатся данные, полученные в ходе подсчёта показателей архивируемости сайта по методике **CLEAR** с помощью сервиса **ArchiveReady**, а также анализ полученных результатов.

**Результаты**

![](/logosjournal.ru/archiveready.jpg)

Несмотря на успешный эксперимент по загрузке сайта при помощи **wpull**, он получил средний индекс архивируемости – 72%, обусловленный низкими или средними показателями отдельных аспектов: доступности (Accessibility), целостности (Cohesion), и соответствия стандартам (Standards compliance).  Изучение подробных результатов показало следующее:
1. У сайта низкий показатель доступности, который связан с неудачно организованным кодом, долгой загрузкой, а также отсутствием sitemap, который упростил бы архивацию за счет готового представления структуры сайта, и наличием команд disallow в robots.txt. Большое количество элементов защиты усложняет процесс архивации.
2. На показатель соответствия стандартам повлияли некорректные html- и css-файлы, а также отсутствие кодировки (encoding) в файлах, из-за чего кодировка определяется автоматически, что влечёт за собой определённые риски.
3. Некоторое снижение показателя целостности связано с наличием внешних файлов и скриптов, затрудняющим процесс архивации.
4. Показатель метаданных – 100%, ошибки не выявлены.

## metawarc

**Описание раздела**

В разделе содержатся результаты анализа метаданных содержимого warc-файлов с помощью утилиты командной строки **metawarc**. В работе использовались следующие команды:
 1. **analyze**
 2. **metadata**
 3. **index**
 4. **dump**

**Результаты**

 1. `metawarc analyze logosjournal.ru.warc.gz`
	  | mimes                  | files | files      | share       |
	  |------------------------|-------|------------|-------------|
	  | application/pdf        | 2490  | 4213470608 | 81.2925     |
	  | text/html              | 10001 | 761038821  | 14.6831     |
	  | image/jpeg             | 410   | 177446949  | 3.42357     |
	  | image/png              | 34    | 26749736   | 0.516096    |
	  | text/css               | 46    | 2325268    | 0.0448625   |
	  | application/javascript | 40    | 2054898    | 0.0396461   |
	  | image/gif              | 10    | 5900       | 0.000113832 |
	  | application/json       | 2     | 3338       | 6.44016e-05 |
	  | text/plain             | 1     | 1616       | 3.11783e-05 |
	  | #total                 | 13034 | 5183097134 | 100         |
	 
 	Как и ожидалось, наибольшим весом обладают pdf-файлы, поскольку это научный журнал, основным продуктом производства которого являются статьи (зачастую очень большие). Также большую часть занимают html-файлы, обеспечивающие работу сайта.
	 
 2. `metawarc metadata --output logosjournal.ru_meta.jsonl logosjournal.ru.warc.gz`

	Полученные метаданные записаны в файл **logosjournal.ru_meta.jsonl**. Сбор метаданных оказался в большей части успешным, однако в ходе работы возникли ошибки, отражённые в файле **metadata_errors.txt**.
 
 3. `metawarc index logosjournal.ru.warc.gz`
 
	 В результате работы команды на основе метаданных была создана база данных (файл **metawarc.db**), которая может использоваться для реализации других команд, например, **stats** или **dump**.
 
 4. `metawarc dump -m 'application/pdf' -o pdf`

	В результате работы команды был сформированы [под-архив](https://disk.yandex.ru/d/rJALd400O0aQXA), включающий все pdf-файлы, содержащиеся в основном архиве. Эта команда была выполнена для быстрого скачивания всех статей, представлявших основной интерес при сохранении сайта, однако стоит отметить, что в под-архив могли попасть не только тексты статей, но и дополнительные материалы, так как настройки команды не позволяют разделить файлы по их содержанию.
	В команде использовался параметр mime (content type), а не extension, поскольку при просмотре базы данных на [специальном ресурсе](https://inloop.github.io/) выяснилось, что не все метаданные загрузились корректно, в связи с чем у некоторых файлов отсутствуют данные о расширении, имени и т.д.