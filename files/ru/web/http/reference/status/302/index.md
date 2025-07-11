---
title: 302 Found
slug: Web/HTTP/Reference/Status/302
---

HTTP код перенаправления **`302 Found`** означает, что запрошенный ресурс был временно перемещён по адресу, указанному в заголовке {{HTTPHeader("Location")}}. Получив такой ответ браузер перенаправляется на новую страницу, но поисковые системы не обновляют свои ссылки на ресурс (в жаргоне SEO говорят, что вес ссылки (link-juice) не меняется и не отправляется на новый URL-адрес).

Несмотря на требование спецификации не изменять при перенаправлении метод и тело запроса, не все программные клиенты выполняют его, и с некоторыми из них можно столкнуться до сих пор. Поэтому рекомендуется установить код `302` только как ответ для методов {{HTTPMethod("GET")}} или {{HTTPMethod("HEAD")}}. Для остальных случаев лучше использовать код {{HTTPStatus("307", "307 Temporary Redirect")}}, поскольку в этом случае изменение метода явно запрещено.

В тех случаях, когда вы хотите, чтобы используемый метод был изменён на {{HTTPMethod("GET")}}, используйте код {{HTTPStatus("303", "303 See Other")}}. Это полезно, если вы хотите дать ответ на метод {{HTTPMethod("PUT")}}, который не является загруженным ресурсом, но является подтверждающим сообщением (например, «Вы успешно загрузили XYZ»).

## Статус

```
302 Found
```

## Пример

### Запрос клиента

```
GET / HTTP/1.1
Host: www.example.org
```

### Ответ сервера

```
HTTP/1.1 302 Found
Location: http://www.example.org/index.asp
```

## Спецификации

| Спецификация                           | Название                                                      |
| -------------------------------------- | ------------------------------------------------------------- |
| {{RFC("7231", "302 Found" , "6.4.3")}} | Hypertext Transfer Protocol (HTTP/1.1): Semantics and Content |

## Совместимость с браузерами

{{Compat}}

## Смотрите также

- {{HTTPStatus("307", "307 Temporary Redirect")}}, эквивалент рассматриваемого кода ответа, но явно запрещающий изменение метода запроса.
- {{HTTPStatus("303", "303 See Other")}}, временное перенаправление, которое изменяет метод запроса на {{HTTPMethod("GET")}}.
- {{HTTPStatus("301", "301 Moved Permanently")}}, постоянное перенаправление.
