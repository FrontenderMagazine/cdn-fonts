# Хранение веб-шрифтов в CDN

Как известно, для улучшения производительности статику лучше хранить в CDN.
В частности веб-шрифты. К сожалению веб-шрифты хранящиеся в CDN по умолчанию 
не будут работать в Firefox и Internet Explorer — для корректного отображения 
потребуются настройки <abbr title="Cross-origin resource sharing">CORS</abbr>. 
Ниже вы сможете найти необходимый код.

## Код для `.htaccess` или `httpd.conf`

Код должен быть в `.htaccess` или `httpd.conf`:

    # конфигурация Apache 
    <FilesMatch ".(eot|ttf|otf|woff)">
        Header set Access-Control-Allow-Origin "*"
    </FilesMatch>

    # конфигурация nginx
    if ($filename ~* ^.*?\.(eot)|(ttf)|(woff)$){
        add_header Access-Control-Allow-Origin *;
    }

Access-Control-Allow-Origin конфигурирует <abbr title="Cross-origin resource sharing">CORS</abbr> 
так, что бы было возможно получать файлы шрифтов с любого домена. Или вы можете 
перечислить домены через запятую, если хотите разрешить их получение с конкретных 
доменов. И стоит проследить, что шрифт доступен во всех необходимых форматах, 
на случай, если браузер требует какой то конкретный.

Что бы проверить правильную настройку заголовков, можно использовать curl:

    $ curl -I https://some.cdn.otherdomain.net/media/fonts/somefont.ttf

    # Result
    HTTP/1.1 200 OK
    Server: Apache
    X-Backend-Server: developer1.webapp.scl3.mozilla.com
    Content-Type: text/plain; charset=UTF-8
    Access-Control-Allow-Origin: *
    ETag: "4dece1737ba40"
    Last-Modified: Mon, 10 Jun 2013 15:04:01 GMT
    X-Cache-Info: caching
    Cache-Control: max-age=604795
    Expires: Wed, 19 Jun 2013 16:22:58 GMT
    Date: Wed, 12 Jun 2013 16:23:03 GMT
    Connection: keep-alive

Если увидите в ответе `Access-Control-Allow-Origin: *` — все отлично.

Эта же стратегия используется в [Bootstrap CDN][1], так что можно быть уверенным,
что она хороша.

[1]: https://github.com/netdna/bootstrap-cdn/blob/master/.htaccess#L41
[2]: http://davidwalsh.name/css-attr-content-tooltips