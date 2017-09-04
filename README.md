[![Analytics](https://ga-beacon.appspot.com/UA-317478-20/main-page)](https://github.com/cannin/voice_ebook_reader)

# Voice Activated EPUB E-Book Reader with Ability to Read Books Aloud

An e-reader for people with limited mobility and dexterity featuring voice controlled commands (e.g. for page turning) as well as text-to-speech functionality to read out loud book pages. The features and interface are minimal to be straight forward and should work on tablets, phones, and desktop computers. This e-book reader can be used for 1000s of books including those from Project Gutenberg that offers over 50,000 free e-books.

# Demo

```
http://reader.lunean.com
```

# Voice Commands

* next: Next page
* back: Previous page
* page N: Go to page N
* last page: Go to the last page read
* current page: Reader says the current page number
* read page: Reads current page aloud in English
* bookmark page: Bookmark current page
* go to bookmark: Access current page
* stop voice: Stop voice commands (page must refreshed to access voice commands again)

# Other Features

* Site can be be made fullscreen
* Voice commands paused when text is being read aloud; a visual cue (red dot) and chime is heard when voice commands are re-enabled
* Above the e-book content is the title and author, as well as the fullscreen button, current page/last page read/total pages/voice commands accessible indicator
* Can access any .epub e-book
* Swipe left/right to change pages
* Buttons to larger screens to turn pages

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

Different books can be access by using a query string in the URL with the book filename.

```
http://127.0.0.1:8080/?book=pg1.epub
```
