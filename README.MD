


# Качаем звук и видео с Youtube (android)

## Установка и настройка `termux`
Ставим `termux` https://play.google.com/store/apps/details?id=com.termux
В `termux` ставим всё необходимое и даём доступ к хранилищу:
```
pkg install ffmpeg
pkg install python
pip install youtube-dl
# UPD ставь лучше pip install yt-dlp он сейчас качает шустрее
termux-setup-storage
```

Ставим `X-plore` https://play.google.com/store/apps/details?id=com.lonelycatgames.Xplore он пригодится.

## Создание скрипта `utdl_01.sh`
Создаём где-нибудь текстовый файлик `utdl_01.sh`, в него помещаем код:
```sh
dpath="/storage/emulated/0/Download/youtube-dl"
mkdir $dpath -p
cd $dpath
read -p 'a--audio, v--video, u--update ' choose
case $choose in
  "u")
    echo 'updating..'
    pip install youtube-dl -U
    ;;
  "v")
    read -p 'paste video/playlist url ' vurl
    echo 'downloading video..'
    # youtube-dl $vurl
    youtube-dl $vurl -o '%(playlist_index)s %(title)s by %(uploader)s on %(upload_date)s in %(playlist)s.%(ext)s'
    ;;
  "a")
    read -p 'paste video/playlist url ' vurl
    echo 'downloading audio..'
    youtube-dl $vurl -f worstaudio -o '%(playlist_index)s %(title)s by %(uploader)s on %(upload_date)s in %(playlist)s.%(ext)s'
    ;;
  *)
    echo попробуй по новой и нажми a v или u
    ;;
esac
```
 В скрипте правим путь загрузки `dpath`

## Создание `alias`
В `termux` создаём `alias`, не забывая правильный путь к скрипту:
```sh
echo alias ytdl='"sh /storage/emulated/0/Download/youtube-dl/ytdl_01.sh"' >> .bashrc
```

Выключаем `termux` командой `exit` и снова запускаем `termux`.

В `termux` запускаем `alias`, набрав `ytdl`

Видим что-то вроде этого:
```
$ ytdl
a--audio, v--video, u--update v
paste video/playlist url
```
выбираем действие, вставляем ссылку на плейлист или видео, ищем файлы по пути `dpath`, указанному ранее.
### Profit :)
Звук скачивается в формате `.m4a`, его нормально воспроизводит плеер для аудиокниг `Simple Audiobook Player Free` 
https://play.google.com/store/apps/details?id=mdmt.sabp.free

----

Код распространяется под лицензией ` BEERWARE`, по всем вопросам обращайтесь к stackoverflow.
