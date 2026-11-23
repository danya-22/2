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

```
nc -C sumdu.edu.ua 80
```

**Набраний запит:**

```
GET / HTTP/1.1
Host: sumdu.edu.ua
Connection: close

```

**Відповідь:**

```
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

```
printf 'GET / HTTP/1.1\r\nConnection: close\r\n\r\n' | nc sumdu.edu.ua 80
```

**Вивід:**

```
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

```
printf 'GET / HTTP/1.1\r\nHost: torproject.org\r\nConnection: close\r\n\r\n' | nc sumdu.edu.ua 80
```

**Вивід:**

```
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

```
printf 'GET / HTTP/1.1\r\nHost: opism-pr02.invalid\r\nConnection: close\r\n\r\n' | nc sumdu.edu.ua 80
```

**Вивід:**

```
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

```
printf 'GET / HTTP/1.0\r\n\r\n' | nc sumdu.edu.ua 80
```

**Вивід:**

```
HTTP/1.1 403 Forbidden
Date: Mon, 23 Nov 2026 12:37:06 GMT
Content-Length: 57
Connection: close
Cache-Control: private, max-age=0, no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Referrer-Policy: same-origin
Expires: Thu, 01 Jan 1970 00:00:01 GMT
CF-RAY: a5f0392e092bd8d6-FRA

Cloudflare encountered an error processing this request:
```

*Примітка: тіло відповіді A.3.3 у збереженому фрагменті відсутнє.*

### Завдання A.4. Два запити в одному з'єднанні

**Команда:**

```
printf 'GET /opism-pr02-12345 HTTP/1.1\r\nHost: sumdu.edu.ua\r\n\r\nGET / HTTP/1.1\r\nHost: sumdu.edu.ua\r\nConnection: close\r\n\r\n' | nc -C sumdu.edu.ua 80
```

**Вивід:**

```
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

**Команда:**

```
curl -v http://sumdu.edu.ua/
```

**Вивід:**

```
* Host sumdu.edu.ua:80 was resolved.
* IPv6: 2606:4700:20::681a:ce4, 2606:4700:20::681a:de4, 2606:4700:20::ac43:4b35
* IPv4: 104.26.12.228, 104.26.13.228, 172.67.75.53
*   Trying 104.26.12.228:80...
* Connected to sumdu.edu.ua (104.26.12.228) port 80
> GET / HTTP/1.1
> Host: sumdu.edu.ua
> User-Agent: curl/8.5.0
> Accept: */*
> 
< HTTP/1.1 301 Moved Permanently
< Date: Mon, 23 Nov 2026 12:31:07 GMT
< Content-Type: text/html; charset=UTF-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Location: https://sumdu.edu.ua/
< Speculation-Rules: "/cdn-cgi/speculation"
< Report-To: {"group":"cf-nel","max_age":604800,"endpoints":[{"url":"https://a.nel.cloudflare.com/report/v4?s=MrFeiyHc7QHs5GD%2B6HdAxjO%2B%2B%2B6fLP%2Fa4uXJopZhFCegU66M2rIg5Kz5OHESWsumM%2FZ%2BjRJza0jJgHZeD0b3lXomZO3i8WytXhS0I2EVJEXq9c7p%2Fmaq6mN2SSrEjQ%3D%3D"}]}
< Nel: {"report_to":"cf-nel","success_fraction":0.0,"max_age":604800}
< Server: cloudflare
< CF-RAY: a5f030652f75dc70-FRA
< alt-svc: h3=":443"; ma=86400
< 
<html>
<head><title>301 Moved Permanently</title></head>
<body><a href="https://sumdu.edu.ua/cdn-cgi/content?id=qWvRB3Osup9VifmwD_7ZJQ_oeg9TrpJxqHwjSZjni10-1795437067.068494-1.2.1.1-yug1QiCdVfQRy5r3UOcTNrJCK.69vfl.IM_ESQKIjxfEq.u_.Vo4ef6fXzJJCRh_" aria-hidden="true" rel="nofollow noopener" style="display: none !important; visibility: hidden !important"></a>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>cloudflare</center>
</body>
</html>
* Connection #0 to host sumdu.edu.ua left intact
```

### Завдання A.6. Запит через захищене з'єднання

**Команда:**

```

openssl s_client -connect sumdu.edu.ua:443 -servername sumdu.edu.ua -crlf -quiet

```

**Набраний запит:**

```
GET / HTTP/1.1
Host: sumdu.edu.ua
Connection: close

```

**Вивід:**

```
openssl s_client -connect sumdu.edu.ua:443 -servername sumdu.edu.ua -crlf -quiet
depth=2 C = US, O = Google Trust Services LLC, CN = GTS Root R4
verify return:1
depth=1 C = US, O = Google Trust Services, CN = WE1
verify return:1
depth=0 CN = sumdu.edu.ua
verify return:1

GET / HTTP/1.1
Host: sumdu.edu.ua

HTTP/1.1 403 Forbidden
Date: Mon, 23 Nov 2026 12:38:38 GMT
Content-Type: text/html; charset=UTF-8
Content-Length: 5264
Connection: close
Accept-Ch: Sec-CH-UA-Bitness, Sec-CH-UA-Arch, Sec-CH-UA-Full-Version, Sec-CH-UA-Mobile, Sec-CH-UA-Model, Sec-CH-UA-Platform-Version, Sec-CH-UA-Full-Version-List, Sec-CH-UA-Platform, Sec-CH-UA, UA-Bitness, UA-Arch, UA-Full-Version, UA-Mobile, UA-Model, UA-Platform-Version, UA-Platform, UA
Cf-Mitigated: challenge
Content-Security-Policy: default-src 'none'; script-src 'nonce-w6lZsGSCGDp5AHrAoVXBoj' 'unsafe-eval' https://challenges.cloudflare.com; script-src-attr 'none'; style-src 'unsafe-inline'; img-src 'self' https://challenges.cloudflare.com; connect-src 'self' https://challenges.cloudflare.com; frame-src 'self' https://challenges.cloudflare.com blob:; child-src 'self' https://challenges.cloudflare.com blob:; worker-src blob:; form-action http: https:; base-uri 'self'
Server: cloudflare
Critical-Ch: Sec-CH-UA-Bitness, Sec-CH-UA-Arch, Sec-CH-UA-Full-Version, Sec-CH-UA-Mobile, Sec-CH-UA-Model, Sec-CH-UA-Platform-Version, Sec-CH-UA-Full-Version-List, Sec-CH-UA-Platform, Sec-CH-UA, UA-Bitness, UA-Arch, UA-Full-Version, UA-Mobile, UA-Model, UA-Platform-Version, UA-Platform, UA
Cross-Origin-Embedder-Policy: require-corp
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Resource-Policy: same-origin
Origin-Agent-Cluster: ?1
Permissions-Policy: accelerometer=(),camera=(),clipboard-read=(),clipboard-write=(),geolocation=(),gyroscope=(),hid=(),magnetometer=(),microphone=(),payment=(),publickey-credentials-get=(),screen-wake-lock=(),serial=(),sync-xhr=(),usb=(),xr-spatial-tracking=*
Referrer-Policy: same-origin
Server-Timing: chlray;desc="a5f03b680e6edbfe"
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Speculation-Rules: "/cdn-cgi/speculation"
Report-To: {"group":"cf-nel","max_age":604800,"endpoints":[{"url":"https://a.nel.cloudflare.com/report/v4?s=gX0odNkS%2Bz40Q4fakQKQdbwsZ0aKDaQ%2FdGCAlZuQPy0Z8onXTl0pPwxjJFA6F%2Fu8RKXH%2FQhCeNXEfwyuhIZqvwjVopMOlRmkz5a4z9srGj%2BiD%2BVW7jRnd%2BmHTG8EIQ%3D%3D"}]}
Nel: {"report_to":"cf-nel","success_fraction":0.0,"max_age":604800}
CF-RAY: a5f03b680e6edbfe-FRA
alt-svc: h3=":443"; ma=86400

<!DOCTYPE html><html lang="en-US"><head><title>Just a moment...</title><meta http-equiv="Content-Type" content="text/html; charset=UTF-8"><meta http-equiv="X-UA-Compatible" content="IE=Edge"><meta name="robots" content="noindex,nofollow"><meta name="viewport" content="width=device-width,initial-scale=1"><meta http-equiv="content-security-policy" content="default-src &#39;none&#39;; script-src &#39;nonce-w6lZsGSCGDp5AHrAoVXBoj&#39; &#39;unsafe-eval&#39; https://challenges.cloudflare.com; script-src-attr &#39;none&#39;; style-src &#39;unsafe-inline&#39;; img-src &#39;self&#39; https://challenges.cloudflare.com; connect-src &#39;self&#39; https://challenges.cloudflare.com; frame-src &#39;self&#39; https://challenges.cloudflare.com blob:; child-src &#39;self&#39; https://challenges.cloudflare.com blob:; worker-src blob:; form-action http: https:; base-uri &#39;self&#39;"><style>*{box-sizing:border-box;margin:0;padding:0}html{line-height:1.15;-webkit-text-size-adjust:100%;color:#313131;font-family:system-ui,-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,"Helvetica Neue",Arial,"Noto Sans",sans-serif,"Apple Color Emoji","Segoe UI Emoji","Segoe UI Symbol","Noto Color Emoji"}body{display:flex;flex-direction:column;height:100vh;min-height:100vh}.main-content{margin:8rem auto;padding-left:1.5rem;max-width:60rem}@media (width <= 720px){.main-content{margin-top:4rem}}#challenge-error-text{background-image:url("data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIzMiIgaGVpZ2h0PSIzMiIgZmlsbD0ibm9uZSI+PHBhdGggZmlsbD0iI0IyMEYwMyIgZD0iTTE2IDNhMTMgMTMgMCAxIDAgMTMgMTNBMTMuMDE1IDEzLjAxNSAwIDAgMCAxNiAzbTAgMjRhMTEgMTEgMCAxIDEgMTEtMTEgMTEuMDEgMTEuMDEgMCAwIDEtMTEgMTEiLz48cGF0aCBmaWxsPSIjQjIwRjAzIiBkPSJNMTcuMDM4IDE4LjYxNUgxNC44N0wxNC41NjMgOS41aDIuNzgzem0tMS4wODQgMS40MjdxLjY2IDAgMS4wNTcuMzg4LjQwNy4zODkuNDA3Ljk5NCAwIC41OTYtLjQwNy45ODQtLjM5Ny4zOS0xLjA1Ny4zODktLjY1IDAtMS4wNTYtLjM4OS0uMzk4LS4zODktLjM5OC0uOTg0IDAtLjU5Ny4zOTgtLjk4NS40MDYtLjM5NyAxLjA1Ni0uMzk3Ii8+PC9zdmc+");background-repeat:no-repeat;background-size:contain;padding-left:34px}</style><meta http-equiv="refresh" content="360"></head><body><div class="main-wrapper" role="main"><div class="main-content"><noscript><div class="h2"><span id="challenge-error-text">Enable JavaScript and cookies to continue</span></div></noscript></div></div><script nonce="w6lZsGSCGDp5AHrAoVXBoj">(function(){window._cf_chl_opt = {cFPWv: 'b',cH: 'ZEON5YN03FXcmUg3JojU2vdSe9_1YjEq43ztkHLv_OE-1795437518-1.2.1.1-wNLjz5p4XXyLyy0Es7i5BW4onBkGqmzCEIs7xsHyzuwDN0UjYhcZrxTPflzcFivB',cITimeS: '1795437518',cN: 'w6lZsGSCGDp5AHrAoVXBoj',cRay: 'a5f03b680e6edbfe',cTplC:0,cTplO:0,cTplV:5,cType: 'managed',cUPMDTk:"/?__cf_chl_tk=OS_BXrdqfw0YpnJ3_nNgJsXcBKrdPxirkucnSOaFkVE-1795437518-1.0.1.1-OEpRLCZikRC2cmD58J3AY3weP_lSKO82qPFT9ler6d8",cvId: '3',cZone: 'sumdu.edu.ua',fa:"/?__cf_chl_f_tk=OS_BXrdqfw0YpnJ3_nNgJsXcBKrdPxirkucnSOaFkVE-1795437518-1.0.1.1-OEpRLCZikRC2cmD58J3AY3weP_lSKO82qPFT9ler6d8",md: 'WZamW99WGOl2PCYS25c9sQ8__HtXDGgN0Xrp2SH0fdo-1795437518-1.2.1.1-.OkD0g9vVMI4APdaTml_0XYdw9mfxKGa8ykviwgICbit.vpJdggywhHIFZ6BjDjXT1YzRQupjE9MtG_pDbaRGXmUsj6uD.PUOxOZAcw4fntVOfQWn4hjxFp_tSv_L9_OPc6coLTT0iP_YG4WVffnB2Oa6KuymcOKZIffZWk.RKXCL2dphj11Eb02BV1ZBB8TLw1_pJPu7_aZr2Yf6WB0Yc8Ilqwn.ATPzvFU.nPFCcSUJMDcKkaxuwOVxJ6Np970gfzR6KtkFgbo.pCFZ9_djt5NB6DRafzcI_Z0jl2fVo1dvqojC_FFtEOfz1nKVNQB0BsC6IUZSBvRMuWPFrPzhv35UzDcmS448fblLlgqb7qz.G_xMEQksPZGvqlrEQL2ab.n5.TFEDAkN0E3gljEMxS6T.ROFCvdfIbW7.DVLSqsOtA6NbupeM4VmuENRmQtKZ5sAimFx2qG1hlX_1KpkKBwqm6S.WwgCz0FFit1kcM9cBqXyveqag2V8UdX89nO_8JX0L.XMnfGDjV8sV0Jh1.kwTlvQhTnKeG8PQYyUc2o6xPDkiPXrQtMyojvRFrmGHQTPYR7IdRVcm6e4GcwxgeTfs8YkffGNdGlatoXqvKFuaP2xMTXB8KXirbgOtU_gjioKCUeSIgGfm7uV61UHz6I3NN89eEdYdxWflJBJoLSwdzOItry3UbqmypW9535MJCijE1Vi0EMNI1h9YLllnSyGTLEXp510svS.qVL9CJwx3HYHqUo9HT0wohIO0OO4TitQ4g4UeEZUGfRBiUM13mh0q2U0eYGXjyp9EjjlivTfsK_VauoOk5erRUNG0BOdgn1fsaGWdeCLIRNpz83zYdVnfUS83sjtA9z4K4Vc98ZBCUpz_D6XEAVxO6IK.eJhm0IJcooZ0EBdSxZxxJ1fNvwHzb3Fy3rdISNlf1QVM9ObAKY0OGQSiSWtaY01jedSMk93Vc84.wLwB.L0ydP9F6bFZca.VFr_LCuY.J4SkwmIOvlqKd6_4Bps91A4wconBf5z1zmKyYd7vt8Rk7mkw',mdrd: 'mextom_RVRPJ8EoRwmRJrEUB0Ms75bh8Gj_Hlrfx6Po-1795437518-1.2.1.1-x.NqxWc7LkcBH.TqIZxXK1OE24DuQgNT5TuXabor8pdCYFuA1KsWUau2Uw60VM550qVNtnCY_foYElh5YP.8kl9odqCxO_5bPewma1skzV3jwKNAJuMZ64Bh8rkvb6HC5zd6EQe0Cgv2mblD1oUro4SpBU8A0lc6jiulcJRkX2Q',};var a = document.createElement('script');a.nonce = 'w6lZsGSCGDp5AHrAoVXBoj';a.src = '/cdn-cgi/challenge-platform/h/b/orchestrate/chl_page/v1?ray=a5f03b680e6edbfe';window._cf_chl_opt.cOgUHash = location.hash === '' && location.href.indexOf('#') !== -1 ? '#' : location.hash;window._cf_chl_opt.cOgUQuery = location.search === '' && location.href.slice(0, location.href.length - window._cf_chl_opt.cOgUHash.length).indexOf('?') !== -1 ? '?' : location.search;if (window.history && window.history.replaceState) {var ogU = location.pathname + window._cf_chl_opt.cOgUQuery + window._cf_chl_opt.cOgUHash;history.replaceState(null, null,"/?__cf_chl_rt_tk=OS_BXrdqfw0YpnJ3_nNgJsXcBKrdPxirkucnSOaFkVE-1795437518-1.0.1.1-OEpRLCZikRC2cmD58J3AY3weP_lSKO82qPFT9ler6d8"+ window._cf_chl_opt.cOgUHash);a.onload = function() {history.replaceState(null, null, ogU);}}document.getElementsByTagName('head')[0].appendChild(a);}());</script></body></html>
```

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

**| Проба | Значення поля `Host` | Версія | Код стану | Обсяг тіла відповіді | Збігається з A.1 (так / ні) |
|---|---|---|---|---|---|
| A.1 (вихідна) | `sumdu.edu.ua` | 1.1 | 301 | 825 байтів (розмір чанка `0x339`) | — |
| A.2 | поле відсутнє | 1.1 | 400 | 155 байтів | Ні |
| A.3.1 | `torproject.org` | 1.1 | 409 | 17 байтів | Ні |
| A.3.2 | `opism-pr02.invalid` | 1.1 | 409 | 17 байтів | Ні |
| A.3.3 | поле відсутнє | 1.0 | 403 | 57 байтів | Ні |**

**Висновок за таблицею:**

Під час виконання завдань я змінював значення поля Host та версію HTTP. З правильним доменом сервер повернув код 301, без поля Host у HTTP/1.1 — 400, а з чужим і неіснуючим доменами — 409. У запиті HTTP/1.0 без поля Host сервер повернув 403. Отже, зміни в запиті впливали на відповідь сервера.

## Декларування використання технологій штучного інтелекту

**Рівень, передбачений методичкою:** Р3 — ШІ як співвиконавець.

**Чи використовувалися технології ШІ під час виконання роботи:** так.

| № | Інструмент (назва, версія) | Етап роботи | Дослівний текст запиту (промпту) | Як використано результат |
|---|---|---|---|---|
| 1 | ChatGPT (GPT-6) | Частина Б | `Допоможи заповнити таблицю Б` | Допомога із заповненням таблиці полів заголовка |

**Підтвердження:** наведені в частині A фактичні виводи команд отримано під час виконання практичної роботи; відсутні фрагменти не відтворювалися штучно.

