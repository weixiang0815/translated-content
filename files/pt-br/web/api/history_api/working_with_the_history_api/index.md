---
title: Exemplo de navegação Ajax
slug: Web/API/History_API/Working_with_the_History_API
---

Esse é um exemplo de um web site em AJAX web site composto por apenas três páginas (_first_page.php_, _second_page.php_ e _third_page.php_). Para ver como funciona, crie os arquivos a seguir (ou _git clone_ [https://github.com/giabao/mdn-ajax-nav-example.git](https://github.com/giabao/mdn-ajax-nav-example) ):

> [!NOTE]
> Para integrar completamente os elementos {{HTMLElement("form")}} com esse _mecanismo_, porfavor dê uma olhada no parágrafo [Enviando formulários e enviando arquivos](/pt-BR/docs/Web/API/XMLHttpRequest_API/Using_XMLHttpRequest#submitting_forms_and_uploading_files).

**first_page.php**:

```php
<?php
    $page_title = "Primeira página";

    $as_json = false;
    if (isset($_GET["view_as"]) && $_GET["view_as"] == "json") {
        $as_json = true;
        ob_start();
    } else {
?>
<!doctype html>
<html>
<head>
<?php
        include "include/header.php";
        echo "<title>" . $page_title . "</title>";
?>
</head>

<body>

<?php include "include/before_content.php"; ?>

<p>Esse parágrafo só é mostrado quando a navegação começa em <strong>first_page.php</strong>.</p>

<div id="ajax-content">
<?php } ?>

    <p>Esse é o conteúdo de <strong>first_page.php</strong>.</p>

<?php
    if ($as_json) {
        echo json_encode(array("page" => $page_title, "content" => ob_get_clean()));
    } else {
?>
</div>

<p>Esse parágrafo só é mostrado quando a navegação começa em <strong>first_page.php</strong>.</p>

<?php
        include "include/after_content.php";
        echo "</body>\n</html>";
    }
?>
```

**second_page.php**:

```php
<?php
    $page_title = "Segunda página";

    $as_json = false;
    if (isset($_GET["view_as"]) && $_GET["view_as"] == "json") {
        $as_json = true;
        ob_start();
    } else {
?>
<!doctype html>
<html>
<head>
<?php
        include "include/header.php";
        echo "<title>" . $page_title . "</title>";
?>
</head>

<body>

<?php include "include/before_content.php"; ?>

<p>Esse parágrafo só é mostrado quando a navegação começa em <strong>second_page.php</strong>.</p>

<div id="ajax-content">
<?php } ?>

    <p>Esse é o conteúdo de <strong>second_page.php</strong>.</p>

<?php
    if ($as_json) {
        echo json_encode(array("page" => $page_title, "content" => ob_get_clean()));
    } else {
?>
</div>

<p>Esse parágrafo só é mostrado quando a navegação começa em <strong>second_page.php</strong>.</p>

<?php
        include "include/after_content.php";
        echo "</body>\n</html>";
    }
?>
```

**third_page.php**:

```php
<?php
    $page_title = "Terceira página";
    $page_content = "<p>Esse é o conteúdo de <strong>third_page.php</strong>. This content is stored into a php variable.</p>";

    if (isset($_GET["view_as"]) && $_GET["view_as"] == "json") {
        echo json_encode(array("page" => $page_title, "content" => $page_content));
    } else {
?>
<!doctype html>
<html>
<head>
<?php
        include "include/header.php";
        echo "<title>" . $page_title . "</title>";
?>
</head>

<body>

<?php include "include/before_content.php"; ?>

<p>Esse parágrafo só é mostrado quando a navegação começa em <strong>third_page.php</strong>.</p>

<div id="ajax-content">
<?php echo $page_content; ?>
</div>

<p>Esse parágrafo só é mostrado quando a navegação começa em <strong>third_page.php</strong>.</p>

<?php
        include "include/after_content.php";
        echo "</body>\n</html>";
    }
?>
```

**css/style.css**:

```css
#ajax-loader {
  position: fixed;
  display: table;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

#ajax-loader > div {
  display: table-cell;
  width: 100%;
  height: 100%;
  vertical-align: middle;
  text-align: center;
  background-color: #000000;
  opacity: 0.65;
}
```

**include/after_content.php**:

```php
<p>Esse é o rodapé. Ele é compartilhado entre todas as páginas ajax.</p>
```

**include/before_content.php**:

```php
<p>
[ <a class="ajax-nav" href="first_page.php">Primeiro exemplo</a>
| <a class="ajax-nav" href="second_page.php">Segundo exemplo</a>
| <a class="ajax-nav" href="third_page.php">Terceiro exemplo</a>
| <a class="ajax-nav" href="unexisting.php">Página inexistente</a> ]
</p>
```

**include/header.php**:

```php
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
<script type="text/javascript" src="js/ajax_nav.js"></script>
<link rel="stylesheet" href="css/style.css" />
```

**js/ajax_nav.js**:

(antes de implementar em um ambiente de trabalho, **porfavor leia [a nota sobre a compatibilidade de declaração de const](#const_compatibility)**)

```js
"use strict";

const ajaxRequest = new (function () {
  function closeReq() {
    oLoadingBox.parentNode && document.body.removeChild(oLoadingBox);
    bIsLoading = false;
  }

  function abortReq() {
    if (!bIsLoading) {
      return;
    }
    oReq.abort();
    closeReq();
  }

  function ajaxError() {
    alert("Unknown error.");
  }

  function ajaxLoad() {
    var vMsg,
      nStatus = this.status;
    switch (nStatus) {
      case 200:
        vMsg = JSON.parse(this.responseText);
        document.title = oPageInfo.title = vMsg.page;
        document.getElementById(sTargetId).innerHTML = vMsg.content;
        if (bUpdateURL) {
          history.pushState(oPageInfo, oPageInfo.title, oPageInfo.url);
          bUpdateURL = false;
        }
        break;
      default:
        vMsg = nStatus + ": " + (oHTTPStatus[nStatus] || "Unknown");
        switch (Math.floor(nStatus / 100)) {
          /*
                    case 1:
                        // Informational 1xx
                        console.log("Information code " + vMsg);
                        break;
                    case 2:
                        // Successful 2xx
                        console.log("Successful code " + vMsg);
                        break;
                    case 3:
                        // Redirection 3xx
                        console.log("Redirection code " + vMsg);
                        break;
                    */
          case 4:
            /* Client Error 4xx */
            alert("Client Error #" + vMsg);
            break;
          case 5:
            /* Server Error 5xx */
            alert("Server Error #" + vMsg);
            break;
          default:
            /* Unknown status */
            ajaxError();
        }
    }
    closeReq();
  }

  function filterURL(sURL, sViewMode) {
    return (
      sURL.replace(rSearch, "") +
      (
        "?" +
        sURL
          .replace(rHost, "&")
          .replace(rView, sViewMode ? "&" + sViewKey + "=" + sViewMode : "")
          .slice(1)
      ).replace(rEndQstMark, "")
    );
  }

  function getPage(sPage) {
    if (bIsLoading) {
      return;
    }
    oReq = new XMLHttpRequest();
    bIsLoading = true;
    oReq.onload = ajaxLoad;
    oReq.onerror = ajaxError;
    if (sPage) {
      oPageInfo.url = filterURL(sPage, null);
    }
    oReq.open("get", filterURL(oPageInfo.url, "json"), true);
    oReq.send();
    oLoadingBox.parentNode || document.body.appendChild(oLoadingBox);
  }

  function requestPage(sURL) {
    if (history.pushState) {
      bUpdateURL = true;
      getPage(sURL);
    } else {
      /* Ajax navigation is not supported */
      location.assign(sURL);
    }
  }

  function processLink() {
    if (this.className === sAjaxClass) {
      requestPage(this.href);
      return false;
    }
    return true;
  }

  function init() {
    oPageInfo.title = document.title;
    history.replaceState(oPageInfo, oPageInfo.title, oPageInfo.url);
    for (
      var oLink, nIdx = 0, nLen = document.links.length;
      nIdx < nLen;
      document.links[nIdx++].onclick = processLink
    );
  }

  const /* customizable constants */
    sTargetId = "ajax-content",
    sViewKey = "view_as",
    sAjaxClass = "ajax-nav",
    /* not customizable constants */
    rSearch = /\?.*$/,
    rHost = /^[^\?]*\?*&*/,
    rView = new RegExp("&" + sViewKey + "\\=[^&]*|&*$", "i"),
    rEndQstMark = /\?$/,
    oLoadingBox = document.createElement("div"),
    oCover = document.createElement("div"),
    oLoadingImg = new Image(),
    oPageInfo = {
      title: null,
      url: location.href,
    },
    oHTTPStatus =
      /* http://www.iana.org/assignments/http-status-codes/http-status-codes.xml */ {
        100: "Continue",
        101: "Switching Protocols",
        102: "Processing",
        200: "OK",
        201: "Created",
        202: "Accepted",
        203: "Non-Authoritative Information",
        204: "No Content",
        205: "Reset Content",
        206: "Partial Content",
        207: "Multi-Status",
        208: "Already Reported",
        226: "IM Used",
        300: "Multiple Choices",
        301: "Moved Permanently",
        302: "Found",
        303: "See Other",
        304: "Not Modified",
        305: "Use Proxy",
        306: "Reserved",
        307: "Temporary Redirect",
        308: "Permanent Redirect",
        400: "Bad Request",
        401: "Unauthorized",
        402: "Payment Required",
        403: "Forbidden",
        404: "Not Found",
        405: "Method Not Allowed",
        406: "Not Acceptable",
        407: "Proxy Authentication Required",
        408: "Request Timeout",
        409: "Conflict",
        410: "Gone",
        411: "Length Required",
        412: "Precondition Failed",
        413: "Request Entity Too Large",
        414: "Request-URI Too Long",
        415: "Unsupported Media Type",
        416: "Requested Range Not Satisfiable",
        417: "Expectation Failed",
        422: "Unprocessable Entity",
        423: "Locked",
        424: "Failed Dependency",
        425: "Unassigned",
        426: "Upgrade Required",
        427: "Unassigned",
        428: "Precondition Required",
        429: "Too Many Requests",
        430: "Unassigned",
        431: "Request Header Fields Too Large",
        500: "Internal Server Error",
        501: "Not Implemented",
        502: "Bad Gateway",
        503: "Service Unavailable",
        504: "Gateway Timeout",
        505: "HTTP Version Not Supported",
        506: "Variant Also Negotiates (Experimental)",
        507: "Insufficient Storage",
        508: "Loop Detected",
        509: "Unassigned",
        510: "Not Extended",
        511: "Network Authentication Required",
      };

  var oReq,
    bIsLoading = false,
    bUpdateURL = false;

  oLoadingBox.id = "ajax-loader";
  oCover.onclick = abortReq;
  oLoadingImg.src =
    "data:image/gif;base64,R0lGODlhEAAQAPIAAP///wAAAMLCwkJCQgAAAGJiYoKCgpKSkiH/C05FVFNDQVBFMi4wAwEAAAAh/hpDcmVhdGVkIHdpdGggYWpheGxvYWQuaW5mbwAh+QQJCgAAACwAAAAAEAAQAAADMwi63P4wyklrE2MIOggZnAdOmGYJRbExwroUmcG2LmDEwnHQLVsYOd2mBzkYDAdKa+dIAAAh+QQJCgAAACwAAAAAEAAQAAADNAi63P5OjCEgG4QMu7DmikRxQlFUYDEZIGBMRVsaqHwctXXf7WEYB4Ag1xjihkMZsiUkKhIAIfkECQoAAAAsAAAAABAAEAAAAzYIujIjK8pByJDMlFYvBoVjHA70GU7xSUJhmKtwHPAKzLO9HMaoKwJZ7Rf8AYPDDzKpZBqfvwQAIfkECQoAAAAsAAAAABAAEAAAAzMIumIlK8oyhpHsnFZfhYumCYUhDAQxRIdhHBGqRoKw0R8DYlJd8z0fMDgsGo/IpHI5TAAAIfkECQoAAAAsAAAAABAAEAAAAzIIunInK0rnZBTwGPNMgQwmdsNgXGJUlIWEuR5oWUIpz8pAEAMe6TwfwyYsGo/IpFKSAAAh+QQJCgAAACwAAAAAEAAQAAADMwi6IMKQORfjdOe82p4wGccc4CEuQradylesojEMBgsUc2G7sDX3lQGBMLAJibufbSlKAAAh+QQJCgAAACwAAAAAEAAQAAADMgi63P7wCRHZnFVdmgHu2nFwlWCI3WGc3TSWhUFGxTAUkGCbtgENBMJAEJsxgMLWzpEAACH5BAkKAAAALAAAAAAQABAAAAMyCLrc/jDKSatlQtScKdceCAjDII7HcQ4EMTCpyrCuUBjCYRgHVtqlAiB1YhiCnlsRkAAAOwAAAAAAAAAAAA==";
  oCover.appendChild(oLoadingImg);
  oLoadingBox.appendChild(oCover);

  onpopstate = function (oEvent) {
    bUpdateURL = false;
    oPageInfo.title = oEvent.state.title;
    oPageInfo.url = oEvent.state.url;
    getPage();
  };

  window.addEventListener
    ? addEventListener("load", init, false)
    : window.attachEvent
      ? attachEvent("onload", init)
      : (onload = init);

  // Public methods

  this.open = requestPage;
  this.stop = abortReq;
  this.rebuildLinks = init;
})();
```

> [!NOTE]
> A atual implementação de [`const`](/pt-BR/docs/JavaScript/Reference/Statements/const) (declaração de constante) **não é parte do ECMAScript 5**. É suportada no Firefox e no Chrome (V8) e parcialmente suportada no Opera 9+ e no Safari. **Ela não é suportada nas versões do Internet Explorer 6 ao 9, ou na versão _preview_ do Internet Explorer 10**. [`const`](/pt-BR/docs/JavaScript/Reference/Statements/const) será definida no ECMAScript 6, mas com semânticas diferentes. Similarmente ao que acontece com variáveis definidas como [`let`](/pt-BR/docs/Web/JavaScript/Reference/Statements/let), constantes declaradas com [`const`](/pt-BR/docs/JavaScript/Reference/Statements/const) serão _block-scoped_, limitando seu escopo no bloco. **Nós só usamos isso com propósito didático. Se você quer total compatibilidade com os navegadores, substitua todas as declarações [`const`](/pt-BR/docs/JavaScript/Reference/Statements/const) por declarações [`var`](/pt-BR/docs/JavaScript/Reference/Statements/var).**

Para mais informações, veja: [Manipulando o histórico do navegador](/pt-BR/docs/Web/API/History_API).

## Veja também

- {{ domxref("window.history") }}
- {{ domxref("window.onpopstate") }}
