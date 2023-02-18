# Домашнее задание к занятию 11.2. «Кеширование Redis/memcached» - `Елена Махота`

- [Ответ к Заданию 1](#1)
- [Ответ к Заданию 2](#2)
- [Ответ к Заданию 3](#3)
- [Ответ к Заданию 4](#4)
- [Ответ к Заданию 5*](#5)

---

### Задание 1. Кеширование 

Приведите примеры проблем, которые может решить кеширование. 

*Приведите ответ в свободной форме.*


### *<a name="1"> Ответ к Заданию 1 </a>*

### *Общие проблемы, которые решает кеширование*

- **Повышение производительности** достигается за счет складывания в кэш данных, к которым чаще всего происходит обращение;
- **Увеличение скорости ответа**, повышение отзывчивости. Кэширование позволяет получать контент быстрее, потому что нет необходимости снова проделывать путь по всей сети. Кэши, поддерживаемые рядом с пользователем, например кэш браузера, могут сделать обслуживание запроса почти мгновенным.
- **Экономия ресурсов базы данных**, например, применяя кэширование тяжелых запросов;
- **Снижение сетевых затрат** - контент можно кэшировать в разных точках сетевого пути между потребителем и источником контента. Когда контент кэшируется ближе к потребителю, запросы не будут требовать значительной сетевой активности за пределами кэша. Сглаживание бустов трафика - например, во время черной пятницы онлайн-магазины используют кэш, чтобы переживать резкое увеличение трафика. Доступность контента во время сбоев сети. При использовании определенных политик кэширование может обслуживать контент пользователям даже в течение коротких сбоев.

### *Частные проблемы, которые решает кеширование*


- **DNS**. Преобразование цифровых адресов в буквенные происходит на удаленном сервере провайдера, однако ничто не мешает браузеру сохранить сопоставления IP-адресов с доменными именами уже посещенных веб-ресурсов на локальном компьютере, избавив таким образом от необходимости отправлять повторный запрос в службу DNS на сервере и тем самым ускорить загрузку запрошенной веб-страницы.
- **CDN** (Content Delivery Network). Часто процесс кэширования запускается при первом обращении к контенту. В этом случае пользователь, первым загружающий какой-нибудь статический файл, затрачивает на его загрузку больше времени. Зато у всех остальных пользователей, которые находятся в одном регионе с «первопроходцем», время загрузки значительно сокращается, поскольку файл уже будет кэширован. Узлы CDN настраивают таким образом, чтобы они выполняли обмен кэшированным контентом непосредственно между собой, без обращения к первоисточнику. При этом такой обмен пограничные узлы выполняют по запросам пользователей.
- **HTTP-кэширование**. В контексте HTTP протокола кэширование заключается в хранении HTML-страниц, изображений и других файлов в промежуточном хранилище(кэше) для уменьшения времени повторного доступа к ним. Кэшем обладают как пользовательский браузер, так и промежуточные прокси-сервера и сетевые шлюзы, которые участвуют в сообщении клиента с сервером, хранящим запрашиваемую информацию.Кеширование в HTTP не является обязательным, но чаще всего бывает используется для повторного использования сохраненных ресурсов в целях оптимизации. Стандартно в HTTP не могут быть закэшированы данные, запрошенные по безопасному протоколу HTTPS или ответы на запросы с отличным от «GET» методами.  Приватный кэш предназначен для отдельного пользователя, например, кеш браузера содержит все документы, загруженные пользователем через HTTP. Он используется, чтобы можно было пролистывать назад/вперед загруженные ранее страницы, сохранять или использовать их в качестве ресурса, не обращаясь лишний раз к серверу. Кроме того, кэш полезен при отключении от сети. Разделяемый кэш или прокси-кэш - это кеш совместного использования, который сохраняет отклики, чтобы их потом могли использовать разные пользователи. Прокси, обслуживающий множество пользователей, может установить, например, ваш провайдер или компания в своей  локальной сети,  чтобы можно было повторно использовать популярные ресурсы, сокращая тем самым сетевой трафик и время ожидания.
- **Кэш процессора** — это сверхбыстрая память, которую располагают прямо на кристалле процессора. Извлечение данных из этой памяти не занимает столько времени, сколько бы потребовалось для извлечения того же объема из оперативной памяти, следовательно, процессор молниеносно получает все необходимые данные и может тут же их обрабатывать, и не простаивает. От количества уровней кэша и объема ячеек сверхбыстрой памяти на каждом из уровней, во многом зависит скорость и производительность системы. Особенно хорошо это ощущается в компьютерах, ориентированных на гейминг или сложные вычисления.
- **Тяжелые запросы в базу данных**. Самый быстрый способ обработки запроса к БД — пропустить часть обработки и использовать предварительно вычисленный ответ. Благодаря кэшированию предварительно рассчитанные результаты запросов к базе данных сохраняются в локальном кэше. Если другой запрос может использовать эти результаты, все операции в базе данных, выполняемые для этого запроса, исключаются. Это может  значительно уменьшить среднее время отклика на запросы. Помимо повышения производительности, возможность получения ответа на запрос из локального кэша экономит сетевые ресурсы и снижает время обработки на сервере базы данных. Если запрос уже рассчитан, то такая обработка исключается, освобождая ресурсы сервера для других задач.


Использованные источники:

\- Презентация "Системы хранения и передачи данных: Кэширование, Redis/Memcached", Сергей Андрюнин 

\- [Основы кэширования: терминология, основные HTTP-заголовки и стратегии | 8HOST.COM](https://www.8host.com/blog/osnovy-keshirovaniya-terminologiya-osnovnye-http-zagolovki-i-strategii/)

\- [Для чего нужен кэш DNS и как просмотреть его содержимое | Белые окошки (white-windows.ru)](https://www.white-windows.ru/dlya-chego-nuzhen-kesh-dns-i-kak-prosmotret-ego-soderzhimoe/?ysclid=le35fcl1wu888581080)

\- [Что такое CDN - как работает сеть доставки контента и как выбрать провайдера (selectel.ru)](https://selectel.ru/blog/review-cdn/?ysclid=le35kg5jpg488759074)

\- [HTTP cache — Национальная библиотека им. Н. Э. Баумана (bmstu.wiki)](https://ru.bmstu.wiki/HTTP_cache)

\- [Что такое кэш в процессоре и зачем он нужен | Процессоры | Блог | Клуб DNS (dns-shop.ru)](https://club.dns-shop.ru/blog/t-100-protsessoryi/37338-chto-takoe-kesh-v-protsessore-i-zachem-on-nujen/?ysclid=le35z08lrs50761734&utm_referrer=https%3A%2F%2Fyandex.ru%2F)

\- [Управление кэшированием запросов (oracle.com)](https://docs.oracle.com/cloud/help/ru/analytics-cloud/ACABI/GUID-C9C7EBEE-4C49-4F7F-980E-92A1DCFE24A3.htm#ACABI-GUID-1AB66403-BA5E-4796-B8FD-FA4ECA2C7144)

\- [Кэширование и производительность веб-приложений / Хабр (habr.com)](https://habr.com/ru/company/ruvds/blog/350310/)

---

### Задание 2. Memcached

Установите и запустите memcached.

*Приведите скриншот systemctl status memcached, где будет видно, что memcached запущен.*

### *<a name="2"> Ответ к заданию 2</a>*

Установка memcached

```bash
sudo apt update && sudo apt install memcached
```

Проверка статуса сервиса

```bash
sudo systemctl status memcached
```

![status](img/img%202023-02-13%20221859.png)

---

### Задание 3. Удаление по TTL в Memcached

Запишите в memcached несколько ключей с любыми именами и значениями, для которых выставлен TTL 5. 

*Приведите скриншот, на котором видно, что спустя 5 секунд ключи удалились из базы.*

### *<a name="3"> Ответ к Заданию 3</a>*

Установка telnet

```bash
sudo apt install telnet
```

Команда для подключения к memcached серверу

```bash
# structure
# telnet hostname/ip port

telnet localhost 11211
```

Команда для записи данных в memcached

```bash
# structure
# set key_name meta_data expiry_time length_in_bytes

# example

user@makhota-server:~$ telnet localhost 11211
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
set Test 0 10 5   # expiry_time 10    
Hello
STORED
get Test 
END
set Test 0 15 5    # expiry_time 15
Hello
STORED
get Test
VALUE Test 0 5
Hello
END
get Test
VALUE Test 0 5
Hello
END
get Test
END
set Test 0 5 5  # expiry_time 5 
Hello
STORED
get Test
END
stats
STAT pid 6762
STAT uptime 203
STAT time 1676317771
STAT version 1.6.9
STAT libevent 2.1.12-stable
STAT pointer_size 64
STAT rusage_user 0.037673
STAT rusage_system 0.018831
STAT max_connections 1024
STAT curr_connections 1
STAT total_connections 2
STAT rejected_connections 0
STAT connection_structures 2
STAT response_obj_oom 0
STAT response_obj_count 1
STAT response_obj_bytes 16384
STAT read_buf_count 2
STAT read_buf_bytes 32768
STAT read_buf_bytes_free 0
STAT read_buf_oom 0
STAT reserved_fds 20
STAT cmd_get 5
STAT cmd_set 3
STAT cmd_flush 0
STAT cmd_touch 0
STAT cmd_meta 0
STAT get_hits 2
STAT get_misses 3
STAT get_expired 0
STAT get_flushed 0
STAT delete_misses 0
STAT delete_hits 0
STAT incr_misses 0
STAT incr_hits 0
STAT decr_misses 0
STAT decr_hits 0
STAT cas_misses 0
STAT cas_hits 0
STAT cas_badval 0
STAT touch_hits 0
STAT touch_misses 0
STAT auth_cmds 0
STAT auth_errors 0
STAT bytes_read 128
STAT bytes_written 95
STAT limit_maxbytes 67108864
STAT accepting_conns 1
STAT listen_disabled_num 0
STAT time_in_listen_disabled_us 0
STAT threads 4
STAT conn_yields 0
STAT hash_power_level 16
STAT hash_bytes 524288
STAT hash_is_expanding 0
STAT slab_reassign_rescues 0
STAT slab_reassign_chunk_rescues 0
STAT slab_reassign_evictions_nomem 0
STAT slab_reassign_inline_reclaim 0
STAT slab_reassign_busy_items 0
STAT slab_reassign_busy_deletes 0
STAT slab_reassign_running 0
STAT slabs_moved 0
STAT lru_crawler_running 0
STAT lru_crawler_starts 3
STAT lru_maintainer_juggles 341
STAT malloc_fails 0
STAT log_worker_dropped 0
STAT log_worker_written 0
STAT log_watcher_skipped 0
STAT log_watcher_sent 0
STAT unexpected_napi_ids 0
STAT round_robin_fallback 0
STAT bytes 0
STAT curr_items 0
STAT total_items 3
STAT slab_global_page_pool 0
STAT expired_unfetched 2
STAT evicted_unfetched 0
STAT evicted_active 0
STAT evictions 0
STAT reclaimed 3
STAT crawler_reclaimed 0
STAT crawler_items_checked 0
STAT lrutail_reflocked 0
STAT moves_to_cold 3
STAT moves_to_warm 1
STAT moves_within_lru 0
STAT direct_reclaims 0
STAT lru_bumps_dropped 0
END
gets key
END
version
VERSION 1.6.9
quit
Connection closed by foreign host.
user@makhota-server:~$ 

```

Cкриншот, на котором видно, что спустя 5 секунд ключи удалились из базы

![telnet](img/img%202023-02-13%20230308.png)

---

### Задание 4. Запись данных в Redis

Запишите в Redis несколько ключей с любыми именами и значениями. 

*Через redis-cli достаньте все записанные ключи и значения из базы, приведите скриншот этой операции.*

### *<a name="4"> Ответ к заданию 4</a>

Установка `redis`

```bash

sudo apt update
sudo apt install redis
```

Проверка статуса

```bash
systemctl status redis
```

![status redis](img/img%202023-02-18%20174036.png)


Записала в Redis несколько ключей, через redis-cli достала все записанные ключи и значения из базы.

![redis-cli](img/img%202023-02-18%20181452.png)


---

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже разобраться в материале.

### Задание 5*. Работа с числами 

Запишите в Redis ключ key5 со значением типа "int" равным числу 5. Увеличьте его на 5, чтобы в итоге в значении лежало число 10.  

*Приведите скриншот, где будут проделаны все операции и будет видно, что значение key5 стало равно 10.*

### *<a name="5"> Ответ к Заданию 5*</a>*

![incrby](img/img%202023-02-18%20182211.png)

Использованные источники:

\- [Шпаргалка по Redis / Хабр (habr.com)](https://habr.com/ru/post/204354/)