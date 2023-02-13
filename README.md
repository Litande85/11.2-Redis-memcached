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

---

### Задание 3. Удаление по TTL в Memcached

Запишите в memcached несколько ключей с любыми именами и значениями, для которых выставлен TTL 5. 

*Приведите скриншот, на котором видно, что спустя 5 секунд ключи удалились из базы.*

### *<a name="3"> Ответ к Заданию 3</a>*

---

### Задание 4. Запись данных в Redis

Запишите в Redis несколько ключей с любыми именами и значениями. 

*Через redis-cli достаньте все записанные ключи и значения из базы, приведите скриншот этой операции.*

### *<a name="4"> Ответ к заданию 4</a>


---

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже разобраться в материале.

### Задание 5*. Работа с числами 

Запишите в Redis ключ key5 со значением типа "int" равным числу 5. Увеличьте его на 5, чтобы в итоге в значении лежало число 10.  

*Приведите скриншот, где будут проделаны все операции и будет видно, что значение key5 стало равно 10.*

### *<a name="5"> Ответ к Заданию 5*</a>*
