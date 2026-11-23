# Практична робота № 2

**Дисципліна:** Основи побудови інформаційних систем та мереж (ОК-13)

**Тема:** Структура повідомлень прикладного протоколу HTTP. Формування запиту вручну

| Поле | Значення |
|---|---|
| Студент (прізвище, ім'я, по батькові) | Шпак Даниїл |
| Група | ЗКН-21 |
| Номер варіанта | 19 |
| Індивідуальний домен | `sumdu.edu.ua` |
| «Чужий» домен для завдання A.3.1 | `torproject.org` |
| Середовище виконання | Windows 11; Ubuntu 24.04 (Killercoda) |
| Дата виконання | 21.11.2026 |

## Частина A. Збір експериментальних даних

### Завдання A.1. Формування запиту вручну

**Команда:**

```bash
nc -C sumdu.edu.ua 80
```

**Набраний запит:**

```http
GET / HTTP/1.1
Host: sumdu.edu.ua
Connection: close

```

**Відповідь:**

```text
HTTP/1.1 301 Moved Permanently
Date: Sat, 21 Nov 2026 14:49:53 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Server-Timing: cfEdge;dur=12,cfOrigin;dur=0
Location: https://sumdu.edu.ua/
Speculation-Rules: "/cdn-cgi/speculation"
Report-To: {"group":"cf-nel","max_age":604800,"endpoints":[{"url":"https://a.nel.cloudflare.com/report/v4?s=OWzlLT0BUCoUjHUBA76CIxcJjpYFczWtZGKFA7ZZeyHtv3fEEqJq%2Fur%2Bbv%2FueWsetumdt7cQFlPU0TdSbrAxKtjvp0Iwx3Drs68jP8qoFqfR%2BiiAKTQIVo%2BDGsuMbA%3D%3D"}]}
Nel: {"report_to":"cf-nel","success_fraction":0.0,"max_age":604800}
Server: cloudflare
CF-RAY: a5e080c62accacc1-CDG
alt-svc: h3=":443"; ma=86400

339
<html>
<head><title>301 Moved Permanently</title></head>
<body><a href="https://sumdu.edu.ua/cdn-cgi/content?id=8cEulPEbNgbsmQw6CDBwbyR1H2Fyn70D_6BSNmKwDmo-1795272593.6434267-1.2.1.1-nXAhgvZihVRFofUqaXM.M19iw7komc8PIqUiWTf5sP9MN7GJK34bP3U5KbbuxeMy" aria-hidden="true" rel="nofollow noopener" style="display: none !important; visibility: hidden !important"></a>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
<script type="module" src="https://static.cloudflareinsights.com/beacon.min.js/vce701823e78c4301a35b7e07294d87301792088814092" integrity="sha512-UKNk8GbxkABxKcXYuLkO16EleLGUkcvJWtpSNmExE0Too3ZaFAxTscNWEc1OdAv5/BikHawapSCaiD2XCmp+Bw==" data-cf-beacon='{"version":"2024.11.0","token":"b27f9af4dd7144ee8b8ea9d9f77fcd5f","spa":2}' crossorigin="anonymous"></script>
</body>
</html>

0
```

### Завдання A.2. Запит без поля `Host` у версії 1.1

**Команда:**

```bash
printf 'GET / HTTP/1.1\r\nConnection: close\r\n\r\n' | nc sumdu.edu.ua 80
```

**Вивід:**

```text
HTTP/1.1 400 Bad Request
Server: cloudflare
Date: Sat, 21 Nov 2026 14:52:15 GMT
Content-Type: text/html
Content-Length: 155
Connection: close
CF-RAY: -

<html>
<head><title>400 Bad Request</title></head>
<body>
<center><h1>400 Bad Request</h1></center>
<hr><center>cloudflare</center>
</body>
</html>
```

### Завдання A.3. Вплив поля `Host` на відповідь сервера

#### A.3.1. Чуже доменне ім'я в полі `Host`

**Команда:**

```bash
printf 'GET / HTTP/1.1\r\nHost: torproject.org\r\nConnection: close\r\n\r\n' | nc sumdu.edu.ua 80
```

**Вивід:**

```text
HTTP/1.1 409 Conflict
Date: Sat, 21 Nov 2026 14:53:36 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 17
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Referrer-Policy: same-origin
X-Frame-Options: SAMEORIGIN
Server: cloudflare
CF-RAY: a5e0865c9b71d30d-CDG

error code: 1001
```

#### A.3.2. Неіснуюче ім'я в полі `Host`

**Команда:**

```bash
printf 'GET / HTTP/1.1\r\nHost: opism-pr02.invalid\r\nConnection: close\r\n\r\n' | nc sumdu.edu.ua 80
```

**Вивід:**

```text
HTTP/1.1 409 Conflict
Date: Sat, 21 Nov 2026 14:59:23 GMT
Content-Type: text/plain; charset=UTF-8
Content-Length: 17
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Expires: Thu, 01 Jan 1970 00:00:01 GMT
Referrer-Policy: same-origin
X-Frame-Options: SAMEORIGIN
Server: cloudflare
CF-RAY: a5e08ed9dab3b82b-CDG

error code: 1001
```

#### A.3.3. Запит без поля `Host` у версії 1.0

**Команда:**

```bash
printf 'GET / HTTP/1.0\r\n\r\n' | nc sumdu.edu.ua 80
```

**Вивід (збережена частина):**

```text
HTTP/1.1 403 Forbidden
Date: Sat, 21 Nov 2026 15:03:03 GMT
Content-Length: 57
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Referrer-Policy: same-origin
Expires: Thu, 01 Jan 1970 00:00:01 GMT
CF-RAY: a5e094384cf1d10d-CDG
```

*Примітка: тіло відповіді A.3.3 у збереженому фрагменті відсутнє.*

### Завдання A.4. Два запити в одному з'єднанні

**Команда:**

```bash
printf 'GET /opism-pr02-12345 HTTP/1.1\r\nHost: sumdu.edu.ua\r\n\r\nGET / HTTP/1.1\r\nHost: sumdu.edu.ua\r\nConnection: close\r\n\r\n' | nc -C sumdu.edu.ua 80
```

**Вивід:**

```text
HTTP/1.1 301 Moved Permanently
Date: Sat, 21 Nov 2026 15:15:15 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Server-Timing: cfEdge;dur=6,cfOrigin;dur=0
Location: https://sumdu.edu.ua/opism-pr02-12345
Speculation-Rules: "/cdn-cgi/speculation"
Report-To: {"group":"cf-nel","max_age":604800,"endpoints":[{"url":"https://a.nel.cloudflare.com/report/v4?s=12GoBcToc1YnzqtGcXFsNezJ0Tb5trsy%2FTmB5pB93iBlPGbGpvUGrEJSjN8d788uiAePovHi8%2B7lCnU5oZCV0lVVMJK4r1Kb6fRBZ2c%2Fj92K4%2F1amrX6PJ59%2BayiBQ%3D%3D"}]}
Nel: {"report_to":"cf-nel","success_fraction":0.0,"max_age":604800}
Server: cloudflare
CF-RAY: a5e0a615deff029a-CDG
alt-svc: h3=":443"; ma=86400

338
<html>
<head><title>301 Moved Permanently</title></head>
<body><a href="https://sumdu.edu.ua/cdn-cgi/content?id=QBvGkfHQFQGvhbcxqEPpAACfKf8Lidqk5l_L.5sY.XI-1795274115.508198-1.2.1.1-XHRvXpFRPpx7lEIzbgohX3YzU9Np8JOT2c0Kea_9xZGBNKNXJAIDO7NgAK5X9GMg" aria-hidden="true" rel="nofollow noopener" style="display: none !important; visibility: hidden !important"></a>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
<script type="module" src="https://static.cloudflareinsights.com/beacon.min.js/vce701823e78c4301a35b7e07294d87301792088814092" integrity="sha512-UKNk8GbxkABxKcXYuLkO16EleLGUkcvJWtpSNmExE0Too3ZaFAxTscNWEc1OdAv5/BikHawapSCaiD2XCmp+Bw==" data-cf-beacon='{"version":"2024.11.0","token":"b27f9af4dd7144ee8b8ea9d9f77fcd5f","spa":2}' crossorigin="anonymous"></script>
</body>
</html>

0

HTTP/1.1 301 Moved Permanently
Date: Sat, 21 Nov 2026 15:15:15 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: close
Server-Timing: cfEdge;dur=18,cfOrigin;dur=0
Location: https://sumdu.edu.ua/
Speculation-Rules: "/cdn-cgi/speculation"
Report-To: {"group":"cf-nel","max_age":604800,"endpoints":[{"url":"https://a.nel.cloudflare.com/report/v4?s=TLg6QNh%2F2a0knmbYsyGaQkCbUflhQHi%2BJa4wtows%2ByCQbGZMqL6j3%2F3mnBbfunQxTtLqtGYO9RGYAiOCyp87VtzoUUYaBWsq3jXgOc2vhCceBoxCCz32ecSnzgBtqA%3D%3D"}]}
Nel: {"report_to":"cf-nel","success_fraction":0.0,"max_age":604800}
Server: cloudflare
CF-RAY: a5e0a615ff00029a-CDG
alt-svc: h3=":443"; ma=86400

338
<html>
<head><title>301 Moved Permanently</title></head>
<body><a href="https://sumdu.edu.ua/cdn-cgi/content?id=Ub3QQIKG6wBqMM1Q.M5rVZczPSNtg6.YUYnLh4bzQ6o-1795274115.516823-1.2.1.1-e2sv.3FLUzUj38UTXc7XinLw614x0lre44f5KyPv40eIvSBcUPUIas64gy5h92cS" aria-hidden="true" rel="nofollow noopener" style="display: none !important; visibility: hidden !important"></a>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
<script type="module" src="https://static.cloudflareinsights.com/beacon.min.js/vce701823e78c4301a35b7e07294d87301792088814092" integrity="sha512-UKNk8GbxkABxKcXYuLkO16EleLGUkcvJWtpSNmExE0Too3ZaFAxTscNWEc1OdAv5/BikHawapSCaiD2XCmp+Bw==" data-cf-beacon='{"version":"2024.11.0","token":"b27f9af4dd7144ee8b8ea9d9f77fcd5f","spa":2}' crossorigin="anonymous"></script>
</body>
</html>

0
```

**Кількість отриманих відповідей:** 2

**Коди стану отриманих відповідей:** 301 Moved Permanently; 301 Moved Permanently.

### Завдання A.5. Запит за допомогою клієнтської програми

**Команда (потребує підтвердження, що саме її було виконано):**

```bash
curl -v http://sumdu.edu.ua/
```

**Вивід:**

```text
[Фактичний вивід A.5 не зберігся в доступній історії. Вставити результат власного запуску.]
```

### Завдання A.6. Запит через захищене з'єднання

**Ресурс, на якому виконано завдання:** `sumdu.edu.ua`

**Підстава для використання резервного ресурсу:** Не використовувався; з'єднання з власним доменом встановлено.

**Команда:**

```bash
openssl s_client -connect sumdu.edu.ua:443 -servername sumdu.edu.ua -crlf -quiet
```

**Набраний запит:**

```http
GET / HTTP/1.1
Host: sumdu.edu.ua
Connection: close

```

**Вивід (збережено заголовки; довге HTML-тіло слід вставити з власного запису):**

```text
depth=2 C = US, O = Google Trust Services LLC, CN = GTS Root R4
verify return:1
depth=1 C = US, O = Google Trust Services, CN = WE1
verify return:1
depth=0 CN = sumdu.edu.ua
verify return:1
GET / HTTP/1.1
Host: sumdu.edu.ua
Connection: close

HTTP/1.1 403 Forbidden
Date: Sat, 21 Nov 2026 15:27:16 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 5285
Connection: close
Accept-Ch: Sec-CH-UA-Bitness, Sec-CH-UA-Arch, Sec-CH-UA-Full-Version, Sec-CH-UA-Mobile, Sec-CH-UA-Model, Sec-CH-UA-Platform-Version, Sec-CH-UA-Full-Version-List, Sec-CH-UA-Platform, Sec-CH-UA, UA-Bitness, UA-Arch, UA-Full-Version, UA-Mobile, UA-Model, UA-Platform-Version, UA
Cf-Mitigated: challenge
Content-Security-Policy: default-src 'none'; script-src 'nonce-Mtpq8UOq2YRVRRAdqVEY4Z' 'unsafe-eval' https://challenges.cloudflare.com; script-src-attr 'none'; style-src 'unsafe-inline'; img-src 'self' https://challenges.cloudflare.com; connect-src 'self' https://challenges.cloudflare.com; frame-src 'self' https://challenges.cloudflare.com blob:; child-src 'self' https://challenges.cloudflare.com blob:; worker-src blob:; form-action http: https:; base-uri 'self'
Server: cloudflare
Critical-Ch: Sec-CH-UA-Bitness, Sec-CH-UA-Arch, Sec-CH-UA-Full-Version, Sec-CH-UA-Mobile, Sec-CH-UA-Model, Sec-CH-UA-Platform-Version, Sec-CH-UA-Full-Version-List, Sec-CH-UA-Platform, Sec-CH-UA, UA-Bitness, UA-Arch, UA-Full-Version, UA-Mobile, UA-Model, UA-Platform-Version, UA
Cross-Origin-Embedder-Policy: require-corp
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Resource-Policy: same-origin
Origin-Agent-Cluster: ?1
Permissions-Policy: accelerometer=(),camera=(),clipboard-read=(),clipboard-write=(),geolocation=(),gyroscope=(),hid=(),magnetometer=(),microphone=(),payment=(),publickey-credentials-get=(),screen-wake-lock=(),serial=(),sync-xhr=(),usb=(),xr-spatial-tracking=*
Referrer-Policy: same-origin
Server-Timing: chlray;desc="a5e0b79cbb02f533"
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Speculation-Rules: "/cdn-cgi/speculation"
CF-RAY: a5e0b79cbb02f533-CDG
alt-svc: h3=":443"; ma=86400
```

*HTML-тіло відповіді A.6 у цьому відновленому файлі не дублюється; у фактичному виводі воно починалося з `<!DOCTYPE html><html lang="en-US"><head><title>Just a moment...</title>`.*

## Частина B. Розбір полів заголовка

Розбирається відповідь, отримана в завданні A.1.

**Загальна кількість полів заголовка у відповіді:** 12

| № | Поле заголовка | Значення | Призначення (власне формулювання) | Походження: сервер / проміжний вузол / не визначено | Обґрунтування |
|---|---|---|---|---|---|
| 1 | Date | Sat, 21 Nov 2026 14:49:53 GMT | Показує час формування відповіді | Не визначено | З відповіді не видно, чи поле створено основним сервером, чи змінено посередником |
| 2 | Content-Type | text/html; charset=UTF-8 | Вказує тип даних і кодування | Не визначено | Описує формат HTML, але не встановлює автора поля |
| 3 | Transfer-Encoding | chunked | Показує спосіб передавання тіла відповіді | Не визначено | Тіло передається частинами, спосіб міг установити проміжний вузол |
| 4 | Connection | close | Повідомляє про закриття з'єднання | Не визначено | Поле стосується поточного з'єднання |
| 5 | Server-Timing | cfEdge;dur=12,cfOrigin;dur=0 | Містить дані про час обробки запиту | Проміжний вузол | Назви cfEdge та cfOrigin пов'язані з Cloudflare |
| 6 | Location | https://sumdu.edu.ua/ | Вказує адресу перенаправлення | Не визначено | Код 301 означає перенаправлення, але походження поля точно невідоме |
| 7 | Speculation-Rules | "/cdn-cgi/speculation" | Вказує правила попереднього завантаження ресурсів | Проміжний вузол | Шлях /cdn-cgi/ пов'язаний із Cloudflare |
| 8 | Report-To | JSON із адресою звітів | Вказує адресу для надсилання звітів браузера | Проміжний вузол | Адреса звітування належить Cloudflare |
| 9 | Nel | JSON із параметрами звітування | Налаштовує звіти про мережеві помилки | Проміжний вузол | Містить параметри мережевого моніторингу |
| 10 | Server | cloudflare | Вказує програмне забезпечення, яке обробило запит | Проміжний вузол | У значенні прямо зазначено Cloudflare |
| 11 | CF-RAY | a5e080c62accacc1-CDG | Ідентифікує запит у мережі Cloudflare | Проміжний вузол | Службовий ідентифікатор Cloudflare |
| 12 | alt-svc | h3=":443"; ma=86400 | Повідомляє про альтернативний протокол | Не визначено | Повідомляє про HTTP/3; походження поля за одним виводом не доведено |

*Для полів Report-To та Nel повні значення містяться у виводі A.1. Визначення походження частини полів є припущенням.*

## Частина D. Висновки

### D.1. Що з поведінки сервера виявилося неочевидним або несподіваним

Під час виконання роботи мене здивувало, що сервер по-різному відповідав на схожі запити. У завданні A.1 я отримав `HTTP/1.1 301 Moved Permanently`, тобто перенаправлення на HTTPS. У завданні A.2 після видалення поля Host сервер повернув `400 Bad Request`. Коли я вказав чужий або неіснуючий домен, отримав `409 Conflict`. Також у завданні A.6 через захищене з'єднання сервер повернув `403 Forbidden`, оскільки Cloudflare вимагав додаткову перевірку. Я не очікував, що навіть правильно встановлене HTTPS-з'єднання не гарантує доступу до сторінки.

### D.2. Яке з полів заголовка викликало найбільше утруднення при визначенні походження

Найбільше труднощів у мене виникло з визначенням походження поля `Server-Timing`. У відповіді A.1 воно мало значення `cfEdge;dur=12,cfOrigin;dur=0`. Я зрозумів, що це інформація про час обробки запиту, але було складно визначити, хто саме додав цей заголовок. Оскільки у відповіді також було поле `Server: cloudflare`, я припустив, що `Server-Timing` додав проміжний вузол Cloudflare. Проте лише за текстом відповіді неможливо повністю перевірити це припущення.

### D.3. Яке питання залишилося без відповіді після виконання роботи

Після виконання практичної роботи мені залишилося незрозумілим, чому сервер іноді повертає різні коди стану для схожих запитів. Наприклад, у завданні A.3.2 я отримав `409 Conflict`, а в A.3.3 — `403 Forbidden`. Також мені цікаво, за якими правилами Cloudflare вирішує, коли потрібно показувати захисну перевірку замість звичайної сторінки. Для цього потрібно детальніше вивчити роботу серверів і систем захисту вебсайтів.

## Контрольні питання

### 1. У завданні A.1 сервер не надсилав відповіді, доки не було введено порожній рядок. Чим це зумовлено?

Порожній рядок означає завершення заголовків HTTP-запиту. Поки його немає, сервер може вважати, що клієнт ще не закінчив надсилати запит. Після порожнього рядка сервер починає обробку запиту та надсилає відповідь.

### 2. Порівняйте результати завдань A.1, A.2 та A.3.1–A.3.3. За яких значень поля Host і за якої версії протоколу сервер обслуговує запит, а за яких — ні? Яку задачу розв'язує поле Host?

У завданні A.1 із правильним полем `Host: sumdu.edu.ua` та HTTP/1.1 сервер повернув код `301 Moved Permanently`. Без поля Host у HTTP/1.1 (A.2) я отримав `400 Bad Request`. З чужим доменом `torproject.org` (A.3.1) та неіснуючим `opism-pr02.invalid` (A.3.2) сервер повернув `409 Conflict`. У HTTP/1.0 без Host (A.3.3) я отримав `403 Forbidden`. Поле Host потрібне, щоб сервер розумів, до якого саме вебсайту звертається клієнт, особливо якщо на одному сервері розміщено кілька сайтів.

### 3. Скільки відповідей надійшло у завданні A.4 і з якими кодами стану? Чи залежить відповідь сервера на порту 80 від запитаного шляху?

У завданні A.4 я отримав дві відповіді, обидві з кодом `301 Moved Permanently`. Перший запит був до `/opism-pr02-12345`, а другий — до `/`. В обох випадках сервер перенаправив запит на HTTPS, але зберіг відповідний шлях у заголовку Location. Отже, у моєму випадку код стану не залежав від запитаного шляху. Дві відповіді в одному з'єднанні означають, що клієнт може повторно використовувати його для кількох ресурсів і не відкривати нове з'єднання щоразу.

### 4. Які поля заголовка програма curl додала самостійно (завдання A.5)? З якою метою їх додано?

У типовому запиті `curl` додає `Host`, `User-Agent` та `Accept`. Поле `Host` вказує домен, `User-Agent` повідомляє назву клієнтської програми, а `Accept` визначає допустимі типи відповіді. **Точний перелік потребує перевірки за фактичним виводом A.5.**

### 5. За якими ознаками у вашому виводі виявляється присутність проміжного вузла?

У моїх результатах є поле `Server: cloudflare`, яке показує, що запити оброблялися через Cloudflare. Також присутні поля `CF-RAY` та `Server-Timing`, пов'язані з його роботою. Це свідчить про наявність проміжного вузла між клієнтом і основним вебсервером.

### 6. Три рядки, про які не йшлося на лекції, наведено в Додатку В.

Три рядки з виводу A.1 наведено нижче. Потрібно перевірити за лекцією, що саме ці поля не розглядалися.

## Додаток В. Відповіді на питання 6

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 | `Server-Timing: cfEdge;dur=12,cfOrigin;dur=0` | A.1 |
| 2 | `CF-RAY: a5e080c62accacc1-CDG` | A.1 |
| 3 | `alt-svc: h3=":443"; ma=86400` | A.1 |

## Додаток Д. Зведення результатів завдання A.3

**Вузол, з яким установлювалося з'єднання (у всіх пробах однаковий):** `sumdu.edu.ua:80`

| Проба | Значення поля `Host` | Версія | Код стану | Обсяг тіла відповіді | Збігається з A.1 (так / ні) |
|---|---|---|---|---|---|
| A.1 (вихідна) | `sumdu.edu.ua` | 1.1 | 301 | 825 байтів (розмір чанка `0x339`) | — |
| A.2 | поле відсутнє | 1.1 | 400 | 155 байтів | Ні |
| A.3.1 | `torproject.org` | 1.1 | 409 | 17 байтів | Ні |
| A.3.2 | `opism-pr02.invalid` | 1.1 | 409 | 17 байтів | Ні |
| A.3.3 | поле відсутнє | 1.0 | 403 | 57 байтів | Ні |

**Висновок за таблицею:**

Під час виконання завдань я змінював значення поля Host та версію HTTP. З правильним доменом сервер повернув код 301, без поля Host у HTTP/1.1 — 400, а з чужим і неіснуючим доменами — 409. У запиті HTTP/1.0 без поля Host сервер повернув 403. Отже, зміни в запиті впливали на відповідь сервера.

## Декларування використання технологій штучного інтелекту

**Рівень, передбачений методичкою:** Р3 — ШІ як співвиконавець.

**Чи використовувалися технології ШІ під час виконання роботи:** так.

| № | Інструмент (назва, версія) | Етап роботи | Дослівний текст запиту (промпту) | Як використано результат |
|---|---|---|---|---|
| 1 | ChatGPT (GPT-6) | Частина Б | `Допоможи заповнити таблицю Б` | Допомога із заповненням таблиці полів заголовка |

*Це формулювання запиту, яке ти попросив указати. Якщо в методичці вимагають дослівно всі фактично використані промпти, перевір його відповідність історії розмови та доповни таблицю іншими запитами.*

**Підтвердження:** наведені в частині A фактичні виводи команд отримано під час виконання практичної роботи; відсутні фрагменти не відтворювалися штучно.

---

*ОПІСМ (ОК-13) · Практична робота № 2 · відновлений README.md*
