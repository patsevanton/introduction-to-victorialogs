Простой запрос
Поиск всех логов с определённым словом.
error

Кавычки для слов
Использование кавычек для поиска слов, совпадающих с ключевыми словами LogsQL.
"and", "error"

Фильтр по времени
Поиск логов за определённый промежуток времени.
_time:5m error

Сортировка
Сортировка выбранных логов по полю времени.
_time:5m error | sort by (_time)

Ограничение числа логов
Ограничение количества возвращаемых логов.
_time:5m error | sort by (_time) desc | limit 10

Выбор конкретных полей
Возврат только указанных полей из логов.
error _time:5m | fields _time, _stream, _msg

Исключение логов
Исключение логов с определёнными условиями.
_time:5m error -buggy_app

Логическое И / ИЛИ
Использование логических операторов для комбинирования фильтров.
_time:5m error -(buggy_app OR foobar)

Статистика
Подсчёт количества логов, соответствующих запросу.
_time:5m error | stats count() logs_with_error

Фильтр по времени
Поиск логов за заданный промежуток времени.
_time:1h AND error

Фильтры
Использование различных фильтров для поиска по времени, слову или полю.
_time:5m, log.level:error, app:buggy_app

Day range filter
Фильтр по диапазону времени в день. Используется для получения логов в определённый промежуток времени каждый день, где start и end имеют формат hh:mm.
_time:day_range[08:00, 18:00)

Week range filter
Фильтр по диапазону дней недели. Используется для получения логов в указанные дни недели.
_time:week_range[Mon, Fri]

Stream filter
Фильтр для выбора логов по конкретному потоку логов.
{app="nginx"}

_stream_id filter
Фильтр для выбора логов по уникальному идентификатору потока.
_stream_id:0000007b000001c850d9950ea6196b1a4812081265faa1c7

Word filter
Поиск логов, содержащих определённое слово.
error

Phrase filter
Поиск логов, содержащих определённую фразу.
"ssh: login fail"

Prefix filter
Фильтр для поиска сообщений, содержащих слово или фразу с указанным префиксом.
err*

Substring filter
Фильтр для поиска логов с подстрокой.
~"ampl"

Range comparison filter
Фильтр для сравнения значений полей с числовыми значениями, строками или IPv4 адресами.
response_size:>10KiB

Empty value filter
Фильтр для поиска логов, не содержащих указанный лог-поле.
host.hostname:""

Any value filter
Фильтр для поиска логов с непустым значением для указанного лог-поля.
host.hostname:*

Exact filter
Фильтр для поиска точных значений в логах.
="fatal error: cannot find /foo/bar"

Exact prefix filter
Фильтр для поиска сообщений, начинающихся с указанного префикса.
="Processing request"*

Multi-exact filter
Фильтр для поиска логов, содержащих одно из нескольких точных значений в поле.
log.level:in("error", "fatal")

contains_all filter
Фильтр для поиска логов, содержащих все указанные слова или фразы.
contains_all(foo, "bar baz")

contains_any filter
Фильтр для поиска логов, содержащих хотя бы одно слово или фразу из множества.
contains_any(foo, "bar baz")

Subquery filter
Фильтр для поиска логов с полями, соответствующими значениям, выбранным другим запросом.
_time:5m AND user_id:in(_time:1d AND path:admin | fields user_id)

Case-insensitive filter
Фильтр для поиска логов, игнорируя регистр.
i(error)

Sequence filter
Фильтр для поиска логов с фразами в определенном порядке.
seq("error", "open file")

Regexp filter
Фильтр для поиска логов с использованием регулярных выражений.
~"err|warn"

Range filter
Фильтр для поиска логов по числовым значениям в поле.
request.duration:>4.2

IPv4 range filter
Фильтр для поиска логов с IP-адресами в заданном диапазоне.
user.ip:ipv4_range(127.0.0.0, 127.255.255.255)


String range filter
Фильтр для поиска строковых значений в поле, которые находятся в заданном диапазоне.
user.name:string_range(A, C)

Length range filter
Фильтр для поиска логов по длине строки в поле.
len_range(5, 10)

Value type filter
Фильтр для поиска логов с полями определенного типа значений.
user_id:value_type(uint64)

Eq_field filter
Фильтр для поиска логов с одинаковыми значениями в двух полях.
user_id:eq_field(customer_id)

Le_field filter
Фильтр для поиска логов, где значение одного поля не превышает значение другого поля.
duration:le_field(max_duration)

Lt_field filter
Фильтр для поиска логов, где значение одного поля меньше значения другого поля.
duration:lt_field(max_duration)

Logical filter
Фильтры для комбинирования запросов с логическими операциями AND, OR, NOT.
error AND file AND app

Pipes
Дополнительные действия, такие как подсчет статистики, сортировка или ограничение числа результатов.
_time:5m | stats by (_stream) count() per_stream_logs | sort by (per_stream_logs desc) | limit 10

Block_stats pipe
Пайп для получения статистики по блокам логов.
<q> | block_stats

Blocks_count pipe
Пайп для подсчета количества блоков логов, обработанных запросом.
<q> | blocks_count


collapse_nums pipe
Этот оператор заменяет все десятичные и шестнадцатеричные числа в указанном поле на плейсхолдер <N>. Это удобно для поиска наиболее часто встречающихся паттернов в логах с различными числами. Пример запроса:
_time:5m | collapse_nums at _msg

conditional collapse_nums
Этот оператор применяется, если нужно применить collapse_nums только к определённым записям. Пример запроса:
_time:5m | collapse_nums if (user_type:=admin) at foo

copy pipe
Этот оператор копирует значения из одного поля в другое. Пример запроса:
_time:5m | copy host as server

delete pipe
Этот оператор удаляет указанные поля из логов. Пример запроса:
_time:5m | delete host, app

drop_empty_fields pipe
Этот оператор удаляет поля с пустыми значениями и пропускает записи с нулевым количеством непустых полей. Пример запроса:
_time:5m | extract 'email: <email>,' from foo | drop_empty_fields

extract pipe
Этот оператор извлекает текст из поля в соответствии с заданным шаблоном. Пример запроса:
_time:1d error | extract "ip=<ip> " from _msg | top 10 (ip)

conditional extract
Этот оператор применяет extract только к записям, которые соответствуют указанным фильтрам. Пример запроса:
_time:5m | extract if (ip:"") "ip=<ip> "

extract_regexp pipe
Этот оператор извлекает подстроки с использованием регулярных выражений и сохраняет их в поля с именами, указанными в шаблоне. Пример запроса:
_time:5m | extract_regexp "(?P<ip>([0-9]+[.]){3}[0-9]+)" from _msg


**extract_regexp**  
Используется для извлечения данных с условием пропуска логов. Пример:  
`_time:5m | extract_regexp if (ip:"") "ip=(?P<ip>([0-9]+[.]){3}[0-9]+)"`

**facets pipe**  
Возвращает наиболее частые значения для каждого поля лога. Пример:  
`_time:1h error | facets`

**field_names pipe**  
Возвращает имена полей всех логов. Пример:  
`_time:5m | field_names`

**field_values pipe**  
Возвращает все значения для указанного поля. Пример:  
`_time:5m | field_values level`

**fields pipe**  
Выбирает только указанные поля логов. Пример:  
`_time:5m | fields host, _msg`

**filter pipe**  
Фильтрует логи по заданным условиям. Пример:  
`_time:1h error | stats by (host) count() logs_count | filter logs_count:> 1_000`

**first pipe**  
Возвращает первые N логов после сортировки. Пример:  
`_time:5m | first 10 by (request_duration)`

**format pipe**  
Комбинирует поля логов по шаблону и сохраняет результат в поле. Пример:  
`_time:5m | format "request from <ip>:<port>" as _msg`

**Conditional format**  
Применяет форматирование только к определённым записям. Пример:  
`_time:5m | format if (ip:* and host:*) "request from <ip>:<host>" as message`

**join pipe**  
Объединяет результаты двух запросов по общим полям. Пример:  
`_time:1d {app="app1"} | stats by (user) count() app1_hits | join by (user) (_time:1d {app="app2"} | stats by (user) count() app2_hits)`

**json_array_len pipe**  
Вычисляет длину массива JSON в поле. Пример:  
`_time:5m | unpack_words _msg as words | json_array_len (words) as words_count`

**hash pipe**  
Вычисляет хеш значения поля. Пример:  
`_time:5m | hash(user_id) as user_id_hash`

**last pipe**  
Возвращает последние N логов после сортировки. Пример:  
`_time:5m | last 10 by (request_duration)`

**len pipe**  
Возвращает длину поля в байтах. Пример:  
`_time:5m | len(_msg) as msg_len | sort by (msg_len desc) | limit 5`

**limit pipe**  
Ограничивает количество возвращаемых логов. Пример:  
`_time:5m | limit 100`


**math pipe**  
Выполняет математические вычисления над числовыми значениями полей логов.  
Пример запроса:  
`_time:5m | math round(duration_msecs / 1000) as duration_secs`

**offset pipe**  
Пропускает N логов после сортировки.  
Пример запроса:  
`_time:5m | sort by (_time) | offset 100`

**pack_json pipe**  
Упаковывает все поля каждого лог-записи в JSON-объект и сохраняет его в указанный поле.  
Пример запроса:  
`_time:5m | pack_json as _msg`

**pack_logfmt pipe**  
Упаковывает все поля в формате logfmt и сохраняет его в указанный поле.  
Пример запроса:  
`_time:5m | pack_logfmt as _msg`

**rename pipe**  
Переименовывает поля логов.  
Пример запроса:  
`_time:5m | rename host as server`

**replace pipe**  
Заменяет все вхождения старой подстроки на новую в указанном поле.  
Пример запроса:  
`_time:5m | replace ("secret-password", "***") at _msg`

**Conditional replace**  
Применяет замену только к логам, соответствующим фильтрам.  
Пример запроса:  
`_time:5m | replace if (user_type:=admin) ("secret", "***") at password`

**replace_regexp pipe**  
Заменяет подстроки, соответствующие регулярному выражению, в указанном поле.  
Пример запроса:  
`_time:5m | replace_regexp ("host-(.+?)-foo", "$1") at _msg`

**Conditional replace_regexp**  
Применяет замену по регулярному выражению только к логам, соответствующим фильтрам.  
Пример запроса:  
`_time:5m | replace_regexp if (user_type:=admin) ("password: [^ ]+", "") at foo`

**sort pipe**  
Сортирует логи по указанным полям.  
Пример запроса:  
`_time:5m | sort by (_stream, _time)`

**stats pipe**  
Выполняет статистические вычисления над логами.  
Пример запроса:  
`_time:5m | stats count() logs_total, count_uniq(_stream) streams_total`

**Stats by fields**  
Выполняет статистику для каждой группы логов по заданным полям.  
Пример запроса:  
`_time:5m | stats by (host, path) count() logs_total, count_uniq(ip) ips_total`

**Stats by time buckets**  
Выполняет статистику по временным интервалам.  
Пример запроса:  
`_time:5m | stats by (_time:1m) count() logs_total, count_uniq(ip) ips_total`

**Статистика по временным интервалам с учётом смещения по часовому поясу**  
Этот запрос позволяет вычислять статистику, сгруппированную по дням или неделям с учётом часового пояса, отличного от UTC.  
Пример запроса:  
`_time:1w | stats by (_time:1d offset 2h) count() logs_total`

**Статистика по полям с разделением на интервалы**  
Статистика может быть сгруппирована по любому полю, подобно _time, используя шаговое значение.  
Пример запроса:  
`_time:1h | stats by (request_size_bytes:10KB) count() requests`

**Статистика по сегментам IPv4**  
Статистика может быть разделена на сегменты сети /24, если поле содержит IPv4-адреса.  
Пример запроса:  
`_time:5m | stats by (ip:/24) count() requests_per_subnet`

**Статистика с дополнительными фильтрами**  
Можно рассчитывать статистику для различных подмножеств логов, добавляя фильтры.  
Пример запроса:  
`_time:5m | stats count() if (GET) gets, count() if (POST) posts, count() if (PUT) puts, count() total`

**Получение соседних логов**  
Запрос возвращает дополнительные логи до и после найденных логов по заданному слову.  
Пример запроса:  
`_time:5m panic | stream_context after 10`

**Получение топ-N записей по полям**  
Этот запрос выводит топ-N значений по полям с наибольшим количеством совпадений.  
Пример запроса:  
`_time:5m | top 7 by (_stream)`

**Объединение результатов из нескольких запросов**  
Запрос объединяет результаты двух запросов.  
Пример запроса:  
`_time:5m error | union (_time:1h panic)`

**Получение уникальных значений по полям**  
Запрос возвращает уникальные значения для указанных полей.  
Пример запроса:  
`_time:5m | uniq by (ip)`

**Извлечение JSON-данных из поля**  
Этот запрос извлекает и распаковывает JSON-данные из указанного поля.  
Пример запроса:  
`_time:5m | unpack_json from _msg`

**Извлечение данных в формате logfmt**  
Этот запрос извлекает и распаковывает данные в формате logfmt из указанного поля.  
Пример запроса:  
`_time:5m | unpack_logfmt from _msg`


**unpack_logfmt pipe**  
Применяется для извлечения полей из logfmt формата. Можно использовать фильтрацию, например, распаковывать только если поле ip пустое.  
Пример запроса:  
`_time:5m | unpack_logfmt if (ip:"") from foo`

**unpack_syslog pipe**  
Извлекает данные из syslog сообщения. Поддерживает форматы RFC3164 и RFC5424.  
Пример запроса:  
`_time:5m | unpack_syslog from _msg`

**Conditional unpack_syslog**  
Применяется только к определённым записям, если выполняется условие. Например, распаковка только если поле hostname пустое.  
Пример запроса:  
`_time:5m | unpack_syslog if (hostname:"") from foo`

**unpack_words pipe**  
Извлекает слова из указанного поля логов в виде массива JSON. Можно исключить дубликаты.  
Пример запроса:  
`_time:5m | unpack_words from _msg as words`

**unroll pipe**  
Применяется для развёртывания массива JSON в отдельные строки.  
Пример запроса:  
`_time:5m | unroll (timestamp, value)`

**Conditional unroll**  
Применяется к данным только при выполнении фильтра. Например, развёртывание поля value только если поле value_type равно json_array.  
Пример запроса:  
`_time:5m | unroll if (value_type:="json_array") (value)`

**avg stats**  
Вычисляет среднее значение по указанным полям.  
Пример запроса:  
`_time:5m | stats avg(duration) avg_duration`

**count stats**  
Вычисляет количество логов. Может быть использовано для подсчёта записей с непустыми полями.  
Пример запроса:  
`_time:5m | stats count() logs`

**count_empty stats**  
Подсчитывает количество записей с пустыми полями.  
Пример запроса:  
`_time:5m | stats count_empty(username) logs_with_missing_username`

**count_uniq stats**  
Подсчитывает количество уникальных непустых значений по указанным полям.  
Пример запроса:  
`_time:5m | stats count_uniq(ip) ips`

**count_uniq_hash stats**  
Вычисляет количество уникальных хешей для непустых значений, что быстрее, чем count_uniq.  
Пример запроса:  
`_time:5m | stats count_uniq_hash(ip) unique_ips_count`

**histogram stats**  
Вычисляет гистограмму для указанного поля.  
Пример запроса:  
`_time:5m | stats by (host) histogram(response_size)`

**json_values stats**  
Упаковывает данные в JSON для каждого логового поля.  
Пример запроса:  
`_time:5m | stats by (app) json_values(_time, _msg) as json_logs`


Функция max возвращает максимальное значение для указанных полей.

Пример запроса:  
_time:5m | stats max(duration) max_duration  

Функция median вычисляет медиану для указанных полей.

Пример запроса:  
_time:5m | stats median(duration) median_duration  

Функция min возвращает минимальное значение для указанных полей.

Пример запроса:  
_time:5m | stats min(duration) min_duration  

Функция quantile вычисляет указанный процентиль (phi) для указанных полей.

Пример запроса:  
_time:5m | stats quantile(0.5, request_duration_seconds) p50, quantile(0.9, request_duration_seconds) p90, quantile(0.99, request_duration_seconds) p99  

Функция rate возвращает среднюю скорость в секунду для соответствующих логов на выбранном временном интервале.

Пример запроса:  
_time:5m error | stats rate()  

Функция rate_sum возвращает среднюю скорость суммы для указанных полей.

Пример запроса:  
_time:5m | stats rate_sum(bytes_sent)  

Функция row_any возвращает произвольную запись лога для каждой выбранной группы статистики.

Пример запроса:  
_time:5m | stats by (_stream) row_any() as sample_row  

Функция row_max возвращает запись лога с максимальным значением для указанного поля.

Пример запроса:  
_time:5m | stats row_max(duration) as log_with_max_duration  

Функция row_min возвращает запись лога с минимальным значением для указанного поля.

Пример запроса:  
_time:5m | stats row_min(duration) as log_with_min_duration  

Функция sum вычисляет сумму значений для указанных полей.

Пример запроса:  
_time:5m | stats sum(duration) sum_duration  

Функция sum_len вычисляет сумму длин в байтах для значений указанных полей.

Пример запроса:  
_time:5m | stats sum_len(_msg) messages_len  

Функция uniq_values возвращает уникальные непустые значения для указанных полей.

Пример запроса:  
_time:5m | stats uniq_values(ip) unique_ips  

Функция values возвращает все значения для указанных полей.

Пример запроса:  
_time:5m | stats values(ip) ips  


