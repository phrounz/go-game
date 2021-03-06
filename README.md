
# go-game

Now working with ebiten v2!

See [go-game-old](https://github.com/phrounz/go-game-old) for the previous version.

## Install dependancies

Install those:
 * Golang
 * Git
 * GitHub Desktop

Run in a terminal:
```
go get github.com/hajimehoshi/ebiten
go get -u github.com/dave/wasmgo
##(not useful/working?) go get -u github.com/gopherjs/gopherwasm
```

You may also need those dependancies on Linux:
```
# on redhat-like distrib (e.g. Fedora)
sudo dnf install libXcursor-devel libXinerama-devel libXi-devel
# on debian-like (NOT TESTED)
sudo apt-get install libXcursor-dev libXinerama-dev libXi-dev

go get github.com/golang/freetype/truetype
go get golang.org/x/image/font
```

More info:
 * https://rominirani.com/setup-go-development-environment-with-atom-editor-a87a12366fcf
 * https://ebiten.org/helloworld.html

## Run as an application

### Debug

```
# install the debugger
go get -u github.com/go-delve/delve/cmd/dlv # if you want the dlv debugger
go build github.com/go-delve/delve/cmd/dlv # and put ./dlv into your $PATH

cd ./src/ludumdare44
dlv debug

(dlv) continue
(dlv) quit
```

Or press F5 while in Atom on the main.go file tab, and select config "Debug"
(AFAIK, must have been run in the console once before that.)

### Build on Windows

```
# version depending of the data/ directory
cd src\ludumdare44 && go build -tags USE_WINSYSTEMMETRICS && ludumdare44.exe

# self-contained version
go build -o ./release/ludumdare44.exe -tags "USE_SELFCONTAINED_MODE USE_WINSYSTEMMETRICS" ./src/ludumdare44 && .\release\ludumdare44.exe
```

### Build on Linux

```
# version depending of the data/ directory
cd ./src/ludumdare44 && go build && ./ludumdare44

# self-contained version
go build -o ./release/ludumdare44 -tags USE_SELFCONTAINED_MODE ./src/ludumdare44 && ./release/ludumdare44
```

## Build as a web page

[More info](https://ebiten.org/documents/webassembly.html)

### Build as a web page (new)
```
# on linux:
GOOS=js GOARCH=wasm go build -tags USE_SELFCONTAINED_MODE -o ./web_release/test.wasm ./src/ludumdare44
firefox web_release

# on windows:
set GOOS=js
set GOARCH=wasm
go build -tags USE_SELFCONTAINED_MODE -o ./web_release/test.wasm ./src/ludumdare44
```
 * To make it work on recent versions of Firefox (at least on local tests), 
    you need to access `about:config` in the URL 
    and set `privacy.file_unique_origin` to false.
    (Reset it to true when you have finished testing.)
 * Click on wasm_exec.html
 * Click on Play!
 * Wait a minute

### Build as a web page (old - not working)

```
cd ./src/ludumdare44
go build github.com/dave/wasmgo
wasmgo serve -f "-tags USE_SELFCONTAINED_MODE"
# While it is running, open Firefox to the url: http://localhost:8080/
```

Or Build as a web page, directly with editable code: (**Warning: Not working, play.jsgo.io services have been shut down, so it does not work any more.**) First commit on a public repository (e.g. https://github.com/phrounz/go-game )
and then: [Play](https://play.jsgo.io/github.com/phrounz/go-game/src/ludumdare44) - [Compile](https://compile.jsgo.io/github.com/phrounz/go-game/src/ludumdare44) - [Run](https://jsgo.io/github.com/phrounz/go-game/src/ludumdare44)

## To make generate_data_go.pl work (to regenerate pictures for self-contained mode)

### On Windows

 * Install [Strawberry Perl](http://strawberryperl.com/)
 * Install [ImageMagick](https://imagemagick.org)
 * hack `$IMAGEMAGICK_CMD_LINE_UTILITY` in the source code
```
cd ./src/ludumdare44
go get https://github.com/hajimehoshi/file2byteslice
go build https://github.com/hajimehoshi/file2byteslice
perl generate_data_go.pl
```

### On Linux

 * Perl should already be installed
 * Install ImageMagick
```
# on redhat-like distrib (e.g. Fedora)
dnf install ImageMagick
```
 * hack `$IMAGEMAGICK_CMD_LINE_UTILITY` in the source code, replace by 'magick'
 * get file2byteslice
 ```
cd ./src/ludumdare44
go get https://github.com/hajimehoshi/file2byteslice
go build https://github.com/hajimehoshi/file2byteslice
perl generate_data_go.pl
```

## Install a full Atom environment to develop:

Install Atom.

Install Atom packages with Ctrl+, -> Install:

```
go-plus
minimap
```

Reminder: on Atom, Ctrl+Shift+M show preview for .md files like this one.
