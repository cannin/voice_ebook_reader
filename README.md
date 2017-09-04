# Voice Activated EPUB E-Book Reader with Ability to Read Books Aloud

# Voice Commands

* next: Next page
* back: Previous page
* page X: Go to page X
* last page: Go to the last page read
* current page: Reader says the current page number
* read page: Read page aloud
* bookmark page: Bookmark current page
* go to bookmark: Access current page
* stop voice: Stop voice commands (page must refreshed to access voice commands again)

# Other Features

* Site can be be made fullscreen
* Voice commands paused when text is being read aloud; a visual cue (red dot) and chime is heard when voice commands are re-enabled
* Above the e-book content is the title and author, as well as the fullscreen button, current page/last page read/total pages/voice commands accessible indicator
* Can access any .epub e-book
* Swipe left/right to change pages

# Setup
## Run Locally with Docker
```
docker run --rm --name reader -p 8080:8080 -v=$(pwd):/src -w /src -t cannin/nodejs-http-server:0.10.25
```

Site then available at http://127.0.0.1:8080


## Run Remotely with Docker

NOTE: Voice commands require hosting on an HTTPS-enabled server

```
docker rm -f nginx-proxy; docker run -d -p 80:80 -p 443:443 \
    --name nginx-proxy \
    -v /home/user/certs:/etc/nginx/certs:ro \
    -v /etc/nginx/vhost.d \
    -v /usr/share/nginx/html \
    -v /var/run/docker.sock:/tmp/docker.sock:ro \
    --label com.github.jrcs.letsencrypt_nginx_proxy_companion.nginx_proxy \
    jwilder/nginx-proxy

docker rm -f lets-encrypt; docker run -d \
    --name lets-encrypt \
    -v /home/user/certs:/etc/nginx/certs:rw \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    --volumes-from nginx-proxy \
    jrcs/letsencrypt-nginx-proxy-companion

docker rm -f reader; docker run --name reader --expose 8080 --restart=always -e "VIRTUAL_HOST=reader.example.com" -e "LETSENCRYPT_HOST=reader.example.com" -e "LETSENCRYPT_EMAIL=user@mail.com" -v /home/user/reader:/src -w /src -t cannin/nodejs-http-server:0.10.25
```

# Books

Books are stored in the books/ folder.

## Accessing Other Books

Different books can be access by using a query string

```
http://127.0.0.1:8080/?book=pg1.epub
```
