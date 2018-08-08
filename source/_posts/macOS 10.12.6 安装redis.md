---
date: 2017-08-08 15:52:29
---

#### macOS 10.12.6 安装redis

brew install redis

```
 jinlong  ~  brew install redis
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 3 taps (caskroom/cask, caskroom/versions, homebrew/core).
==> New Formulae
netcdf                     payara                     snapcraft
==> Updated Formulae
angular-cli                heroku                     pdf2htmlex
ansifilter                 htmldoc                    percona-server
apache-flink               hugo                       percona-server-mongodb
argyll-cms                 idris                      percona-server@5.5
autopano-sift-c            imagemagick                percona-server@5.6
bash-snippets              imagemagick@6              percona-toolkit
bazel                      imageworsener              percona-xtrabackup
bitrise                    imlib2                     pike
btfs                       influxdb                   plplot
buku                       io                         podofo
clojurescript              isync                      ponscripter-sekai
cocoapods                  jasper                     ponyc
dateutils                  jbig2enc                   poppler
dcmtk                      jp2a                       povray
dcraw                      jpeg                       pre-commit
dep                        jpeginfo                   presto
depqbf                     jpegoptim                  protobuf-c
devil                      jsdoc3                     pyinvoke
djvulibre                  knot-resolver              qemu
double-conversion          leptonica                  qrupdate
efl                        libagar                    re2c
eg                         libav                      residualvm
epeg                       libbpg                     rlvm
etcd                       libgaiagraphics            rtv
exploitdb                  libgeotiff                 sane-backends
fbida                      libgphoto2                 scalapack
fdroidserver               libgxps                    scour
ffmpegthumbnailer          libmwaw                    scummvm
firebase-cli               libpano                    sdl2_image
fizmo                      librasterlite              sdl_image
flactag                    libraw                     sfml
fltk                       libsvg-cairo               sjk
folly                      libtiff                    slackcat
fontforge                  libwmf                     sonarqube
fox                        libwps                     spandsp
freeswitch                 libxkbcommon               sqlmap
freetds                    little-cms                 svg2pdf
gd                         little-cms2                svg2png
gdal                       mame                       syntaxerl
gdk-pixbuf                 mandoc                     tiff2png
geeqie                     mapnik                     todolist
gegl                       mapnik@2                   twoping
ghostscript                mapserver                  ufoai
git ✔                      minetest                   ufraw
gitbucket                  minidlna                   veclibfort
gmic                       minio                      vice
gnuplot                    mjpegtools                 vips
gnuplot@4                  mldonkey                   vncsnapshot
gofabric8                  mongo-orchestration        volatility
gphoto2                    mono-libgdiplus            weboob
grace                      mpv                        webp
grafana                    mscgen                     wxmac
graphicsmagick             msgpack                    x11vnc
grib-api                   nagios                     xmake
gsmartcontrol              netpbm                     xmoto
gst-plugins-bad            node@6                     xplanet
gst-plugins-good           octave                     xsane
gst-plugins-ugly           onscripter                 yaz
hashcat                    open-scene-graph           yle-dl
haskell-stack              openclonk                  youtube-dl
hdf5                       openslide                  zbar
==> Deleted Formulae
jpeg@9

==> Downloading https://homebrew.bintray.com/bottles/redis-4.0.1.sierra.bottle.t
######################################################################## 100.0%
==> Pouring redis-4.0.1.sierra.bottle.tar.gz
==> Caveats
To have launchd start redis now and restart at login:
  brew services start redis
Or, if you don't want/need a background service you can just run:
  redis-server /usr/local/etc/redis.conf
==> Summary
🍺  /usr/local/Cellar/redis/4.0.1: 13 files, 2.8MB
```

#### 启动redis
brew services start redis

#### 测试
ping
