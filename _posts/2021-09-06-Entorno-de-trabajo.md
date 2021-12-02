---
layout: single
title: Entorno de Trabajo
excerpt: "En este post veras los pasos y la configuración para poder crear tu entorno personalizado usando bspwn, sxhdk, picom, polybar powerlevel10k."
date: 2021-09-07
classes: wide
header:
  teaser: /assets/images/Entorno/Enkali.png
  teaser_home_page: true
#  icon: /assets/images/hackthebox.webp
categories:
  - Entono
  - Kali linux
tags:  
  - Kali
  - Programacion
  - Clases
  - #hashcat
  - #rules
---

![](/assets/images/Entorno/Enkali.png)

En este post te enseñare como configurar un buen entorno totalmente personalizado paso a paso. 

# ***Kali linux***

Lo primero que tenemos que hacer es actualizar nuestro sistema operativo con los siguientes comandos:

```ruby
sudo apt-get update
sudo apt-get upgrade -y && apt-get full-upgrade -y

```
Instalamos los siguientes paquetes:

```ruby
sudo apt install build-essential git vim xcb libxcb-util0-dev libxcb-ewmh-dev libxcb-randr0-dev libxcb-icccm4-dev libxcb-keysyms1-dev libxcb-xinerama0-dev libasound2-dev libxcb-xtest0-dev libxcb-shape0-dev  

```

# ***Instalar bspwm y sxhkd***

Bspwm nos ayudara a configurar el entorno de ventanas y sxhkd nos ayuda a gestionar mediante el teclado los shortcuts o atajos de teclado. Instalamos bspwm y sxhkd en modo normal:

```ruby
cd /home/f3n1x/Descargas/
git clone https://github.com/baskerville/bspwm.git
git clone https://github.com/baskerville/sxhkd.git
cd bspwm/
make
sudo make install
cd ../sxhkd/
make
sudo make install

sudo apt install bspwm
```
Ya tendriamos instalados el bspwm y el sxhkd

Cargamos en bspwm y sxhkd ficheros de ejemplo:

bspwm y sxhkd necesitan de un archivo para poder operar, ahora lo copiamos a la ruta indicada que seria ```~/.config```

```ruby
mkdir ~/.config/bspwm
mkdir ~/.config/sxhkd
cd /home/f3n1x/Descargas/bspwm/

NOS COPIAREMOS UN ARCHIVO DE CONFIGURACIONES QUE ESTA EN EN EXAMPLES A LA RUTA ~/.config/bspwm
cp examples/bspwmrc ~/.config/bspwm/
cp examples/sxhkdrc ~/.config/sxhkd/

DAMOS PERMISOS A LOS ARCHIVOS
chmod +x ~/.config/bspwm/bspwmrc

```

Abrimos el sxhkdrc y configuramos el tipo de terminal:


Abrimos el sxhkdrc y configuramos el tipo de terminal y distintos atajos de teclado, cualquier atajo de declado o shortcut que queramos agregar.


La tecla super es la tecla Windows, la tecla Return es la tecla Enter.

```ruby

#
# wm independent hotkeys
#

# terminal emulator
super + Return
	gnome-terminal

# program launcher
super + d
	rofi -show run

# make sxhkd reload its configuration files:
super + Escape
	pkill -USR1 -x sxhkd

#
# bspwm hotkeys
#

# quit/restart bspwm
super + alt + {q,r}
	bspc {quit,wm -r}

# close and kill
super + {_,shift + }w
	bspc node -{c,k}

# alternate between the tiled and monocle layout
super + m
	bspc desktop -l next

# send the newest marked node to the newest preselected node
super + y
	bspc node newest.marked.local -n newest.!automatic.local

# swap the current node and the biggest node
super + g
	bspc node -s biggest

#
# state/flags
#

# set the window state
super + {t,shift + t,s,f}
	bspc node -t {tiled,pseudo_tiled,floating,fullscreen}

# set the node flags
super + ctrl + {m,x,y,z}
	bspc node -g {marked,locked,sticky,private}

#
# focus/swap
#

# focus the node in the given direction
super + {_,shift + }{Left,Down,Up,Right}
	bspc node -{f,s} {west,south,north,east}

# focus the node for the given path jump
super + {p,b,comma,period}
	bspc node -f @{parent,brother,first,second}

# focus the next/previous node in the current desktop
super + {_,shift + }c
	bspc node -f {next,prev}.local

# focus the next/previous desktop in the current monitor
super + bracket{left,right}
	bspc desktop -f {prev,next}.local

# focus the last node/desktop
super + {grave,Tab}
	bspc {node,desktop} -f last

# focus the older or newer node in the focus history
super + {o,i}
	bspc wm -h off; \
	bspc node {older,newer} -f; \
	bspc wm -h on

# focus or send to the given desktop
super + {_,shift + }{1-9,0}
	bspc {desktop -f,node -d} '^{1-9,10}'

#
# preselect
#

# preselect the direction
super + ctrl + alt + {Left,Down,Up,Right}
	bspc node -p {west,south,north,east}

# preselect the ratio
super + ctrl + {1-9}
	bspc node -o 0.{1-9}

# cancel the preselection for the focused node
super + ctrl + space
	bspc node -p cancel

# cancel the preselection for the focused desktop
super + ctrl + shift + space
	bspc query -N -d | xargs -I id -n 1 bspc node id -p cancel

#
# move/resize
#

# expand a window by moving one of its side outward
#super + alt + {h,j,k,l}
#	bspc node -z {left -20 0,bottom 0 20,top 0 -20,right 20 0}

# contract a window by moving one of its side inward
#super + alt + shift + {h,j,k,l}
#	bspc node -z {right -20 0,top 0 20,bottom 0 -20,left 20 0}

# move a floating window
super + ctrl + {Left,Down,Up,Right}
	bspc node -v {-20 0,0 20,0 -20,20 0}

#---------------------------------------------------------------------------------------
# CUSTOM
#-------------------------------------------------------------------------------

# Custom move/resize
alt + ctrl+ {Left,Down,Up,Right}
	/home/f3n1x/.config/bspwm/scripts/bspwm_resize {west,south,north,east}

# Google-firefox
ctrl + alt + a
	firejail /opt/firefox/firefox

# Block screen
ctrl + alt + z
	i3lock-fancy

# Spotify
ctrl + alt + e
	spotify

# Burpsuite
	gksudo burpsuite

#subir brillo
ctrl + F4
	/home/f3n1x/.config/bspwm/scripts/subir

#bajar brillo
ctrl + F3
	/home/f3n1x/.config/bspwm/scripts/bajar

```

Creamos el archivo bspwm_resize:

```ruby

mkdir ~/.config/bspwm/scripts/
touch ~/.config/bspwm/scripts/bspwm_resize; chmod +x ~/.config/bspwm/scripts/bspwm_resize

```

- Mediante la siguiente configuración podremos en el futuro controlar las dimensiones de las vetanas, así como
modificarlas con atajos de teclado:

```ruby

#!/usr/bin/env dash

if bspc query -N -n focused.floating > /dev/null; then
	step=20
else
	step=100
fi

case "$1" in
	west) dir=right; falldir=left; x="-$step"; y=0;;
	east) dir=right; falldir=left; x="$step"; y=0;;
	north) dir=top; falldir=bottom; x=0; y="-$step";;
	south) dir=top; falldir=bottom; x=0; y="$step";;
esac

bspc node -z "$dir" "$x" "$y" || bspc node -z "$falldir" "$x" "$y"

```

# ***Procedemos a instalar la polybar***

La polybar es la barra que nos muestra las configuraciones y el espacio de trabajo en el que estamos, para ello instalamos primero los siguientes paquetes:

```ruby

sudo apt install cmake cmake-data pkg-config python3-sphinx libcairo2-dev libxcb1-dev libxcb-util0-dev libxcb-randr0-dev libxcb-composite0-dev python3-xcbgen xcb-proto libxcb-image0-dev libxcb-ewmh-dev libxcb-icccm4-dev libxcb-xkb-dev libxcb-xrm-dev libxcb-cursor-dev libasound2-dev libpulse-dev libjsoncpp-dev libmpdclient-dev libcurl4-openssl-dev libnl-genl-3-dev

```

- Si algo llegara a pasar algun problema lo primero que hacemos es un ```apt update ``` y luego volvemos a usar el ``` sudo apt install ``` para instalar los paquetes.

- Para instalar la polybar hacemos lo siguiente en modo usuario f3n1x:

```ruby

cd /home/f3n1x/Descargas/
git clone --recursive https://github.com/polybar/polybar
cd polybar/
mkdir build
cd build/
cmake ..
make -j$(nproc)
sudo make install

```
# ***Instalamos Picom***
Procedemos con la instalación de Picom para ajustar las transparencias (Compton ya está desactualizado).

Primeramente, instalamos los siguientes paquetes, no sin antes actualizar el sistema:

```ruby

sudo apt update
sudo apt install meson libxext-dev libxcb1-dev libxcb-damage0-dev libxcb-xfixes0-dev libxcb-shape0-dev libxcb-render-util0-dev libxcb-render0-dev libxcb-randr0-dev libxcb-composite0-dev libxcb-image0-dev libxcb-present-dev libxcb-xinerama0-dev libpixman-1-dev libdbus-1-dev libconfig-dev libgl1-mesa-dev libpcre2-dev libevdev-dev uthash-dev libev-dev libx11-xcb-dev libxcb-glx0-dev

```

En los paquetes que instalamos tambien se encuentran los drivers de audio y video, esto nos ayudara a que el bspwm se adapte a las proporciones si es que nos encontramos en una maquinva virtual. 


Posteriormente, ejecutamos los siguientes comandos bajo el usuario fen1x, bajo el directorio ```~/Descargas```:

```ruby

git clone https://github.com/ibhagwan/picom.git
cd picom/
git submodule update --init --recursive
meson --buildtype=release . build
ninja -C build
sudo ninja -C build install

```

# ***Instalamos rofi*** 

Rofi es un menu flotante y se instala con el siguiente comando:

```ruby

sudo apt install rofi

```
Posteriormente instalaremos el tema Nord para Rofi

```ruby

mkdir -p ~/.config/rofi/themes
cp ~/Descargas/blue-sky/nord.rasi ~/.config/rofi/themes

Posteriormente con 'rofi-theme-selector' seleccionamos el tema Nord.

```


- En este punto, reiniciamos el equipo y seleccionamos bspwm. Probamos que los shortcuts estén funcionando correctamente.


Configuramos un poco la terminal e instalamos las Hack Nerd Fonts, además del Firefox hay que descargarse la última versión, también instalaremos ```firejail``` con ```apt install firejail``` con el objetivo de lanzar firefox bajo este contexto enjaulado con sxhkd. Las fuentes de Hack Nerd Fonts deben ir descomprimidas en el directorio ```sudo /usr/local/share/fonts/``` una vez hecho hay que ejecutar el comando ```fc-cache -v```


- Descargamos el navegador firefox en su ultima version.


Vamos a la raiz como super usuario, con el comando: ``` cd /```


Asignamos como propietario y grupo al usuario que tengamos en mi caso f3n1x en le directorio ```opt/``` ```chown f3n1x:f3n1x opt/```


Movemos el firefox al directorio ```opt/```

```ruby
su f3n1x

mv /home/f3n1x/Descargas/firefox.tar.bz2 .

LO DESCOMPRIMIMOS CON:

tar -xf firefox.tar.bz2

```

# ***Instalamos firejail***

Firejail es como una caja donde enjaulamos aplicaciones, es como una medida de seguridad.

La instalaciones es con el siguiente comando:

```ruby
sudo apt install firejail
```

# ***Configuramos Firefox***
Vamos a la opcion de ```ajustes```, ```elegimos DUCKDUCKGO``` como buscador, a nivel de Privacidad y Seguridad desctivamos el ```Historial```


# ***Configuramos la terminal***

Desactivamos la ```barra de desplazamiento```, desactivamos la ```barra del menu```, ponemos el ```cursor en T```, desactivamos la ```campana de la terminal```.

# ***Instalamos las fuentes de Hack Nerd Font***

Vamos al navegador y buscamos Nerd fonts, vamos a la opcion download y descargamos las Hack Nerd Fonts y usamos los siguientes comandos para instalar.

```ruby
root# cd /usr/local/share/fonts/
root# mv /home/f3n1x/Descargas/Hack.zip .
root# unzip Hack.zip
root# rm -r Hack.zip
```


# ***Instalamos FoxyProxy para Firefox.***

Instalamos la extencion foxyproxy, eso nos ayuda a poder interactuar con Bursuite y nos intercepta las comunicaciones de red.

```ruby

NOS VAMOS A FOXYPROXY Y NOS VAMOS A OPCIONES
VAMOS A ADD Y CONFIGURAMOS UN NUEVO PROXY QUE SE LLAMARA BURPSUITE
COLOR
PROXY IP ADDRESS OR DNS NAME 127.0.0.1
PORT 8080 ESTE ES EL PUERTO POR DONDE BURPSUITE ESTA CORRIENDO O SE ESTA EJECUTANDO
LE DAMOS SAVE

TURN OFF PARA NAVEGAR NORMAL

```

# ***Instalamos feh***

Instalamos ```feh``` en usuario normal con ```sudo apt install feh``` para poder cargar fondos de pantalla.

Feh nos ayuda a cargar o definir un fondo de pantalla

```ruby

sudo apt install feh

```

- Cargamos en el archivo bspwmrc la siguiente línea:

```ruby

feh --bg-fill /home/f3n1x/Images/fondo.jpg

```

- Esto nos indica la ruta donde cargamos la imagen.

Para configurar nuestra ```Polybar,``` clonaremos primeramente en ```Descargas``` el siguiente repositorio:

```ruby

git clone https://github.com/VaughnValle/blue-sky.git

```

Posteriormente, ejecutaremos los siguientes comandos somo usuario normal:

```ruby
mkdir ~/.config/polybar
cd ~/Descargas/blue-sky/polybar/
cp * -r ~/.config/polybar
echo '~/.config/polybar/./launch.sh' >> ~/.config/bspwm/bspwmrc
cd fonts
sudo cp * /usr/share/fonts/truetype/
fc-cache -v

```

- Hacemos Windows + Shift + r para cargar la configuración y deberíamos ver la Polybar por arriba.

# ***Configurmos Picom***

Para configurar Picom y ajustar las transparencias además de los bordes de las ventana, ejecutamos los siguientes pasos:

```ruby

mkdir ~/.config/picom
cd ~/.config/picom
cp ~/Descargas/blue-sky/picom.conf .

```

- Editamos el archivo ```picom.conf``` y cambiamos ```backend = "glx"``` por ```'backend = "xrender"```, comentamos ```blur-streagth = 5,``` ```blur-method = "dual_kawase"``` todo lo que este en ```###background-blurring#``` Posteriormente comentamos todas las líneas referentes a ```glx``` (En algunos ordenadores al dejar el glx puesto se puede llegar a experimentar una lentitud muy molesta).
- Agregamos un ```opacity-rule``` donde podemos agregar programos para definir la opacidad.

Antes de recargar la configuración, hacemos un seguimiento del ratón para saber en qué ventana estamos con la siguiente instrucción en el ```bspwm:```

```ruby

bspc config focus_follows_pointer true

```

- Posteriormente, ejecutamos los siguientes comandos para aplicar los bordeados:

```ruby

echo 'picom --experimental-backends &' >> ~/.config/bspwm/bspwmrc 
echo 'bspc config border_width 0' >> ~/.config/bspwm/bspwmrc

```

Configuramos los colores de la polybar (paleta central), además de las paletas deseadas.

Si deseamos una nueva paleta lo hacemos con la palabra ```terciary``` que agregamos en el ```lauch.sh``` y en ```current.ini```

```ruby

#!/usr/bin/env sh

## Add this to your wm startup file.

# Terminate already running bar instances
killall -q polybar

## Wait until the processes have been shut down
while pgrep -u $UID -x polybar >/dev/null; do sleep 1; done

## Launch

## Left bar
polybar log -c ~/.config/polybar/current.ini &
polybar secondary -c ~/.config/polybar/current.ini &
polybar tertiary -c ~/.config/polybar/current.ini &

## Right bar
polybar top -c ~/.config/polybar/current.ini &
polybar primary -c ~/.config/polybar/current.ini &

## Center bar
polybar primary -c ~/.config/polybar/workspace.ini &


```

# ***Modulos***

```ruby

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
;;
;;	    ____        __      __              
;;	   / __ \____  / /_  __/ /_  ____ ______
;;	  / /_/ / __ \/ / / / / __ \/ __ / ____/
;;	 / ____/ /_/ / / /_/ / /_/ / /_/ / /    
;;	/_/    \____/_/\__, /_.___/\__,_/_/     
;;	              /____/                    
;;
;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

;; Global WM Settings

[global/wm]
; Adjust the _NET_WM_STRUT_PARTIAL top value
; Used for top aligned bars
margin-bottom = 0

; Adjust the _NET_WM_STRUT_PARTIAL bottom value
; Used for bottom aligned bars
margin-top = 0

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

;; File Inclusion
; include an external file, like module file, etc.

include-file = ~/.config/polybar/colors.ini

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

;; Bar Settings

[bar/main]
; Use either of the following command to list available outputs:
; If unspecified, the application will pick the first one it finds.
; $ polybar -m | cut -d ':' -f 1
; $ xrandr -q | grep " connected" | cut -d ' ' -f1
monitor =

; Use the specified monitor as a fallback if the main one is not found.
monitor-fallback =

; Require the monitor to be in connected state
; XRandR sometimes reports my monitor as being disconnected (when in use)
monitor-strict = false

; Tell the Window Manager not to configure the window.
; Use this to detach the bar if your WM is locking its size/position.
;override-redirect = true

; Put the bar at the bottom of the screen
bottom = true

; Prefer fixed center position for the `modules-center` block
; When false, the center position will be based on the size of the other blocks.
fixed-center = true

; Dimension defined as pixel value (e.g. 35) or percentage (e.g. 50%),
; the percentage can optionally be extended with a pixel offset like so:
; 50%:-10, this will result in a width or height of 50% minus 10 pixels
width = 17%
height = 60

; Offset defined as pixel value (e.g. 35) or percentage (e.g. 50%)
; the percentage can optionally be extended with a pixel offset like so:
; 50%:-10, this will result in an offset in the x or y direction 
; of 50% minus 10 pixels
offset-x = 20
offset-y = 20

; Background ARGB color (e.g. #f00, #ff992a, #ddff1023)
background = ${color.bg}

; Foreground ARGB color (e.g. #f00, #ff992a, #ddff1023)
foreground = ${color.fg}

; Background gradient (vertical steps)
;   background-[0-9]+ = #aarrggbb
;;background-0 = 

; Value used for drawing rounded corners
; Note: This shouldn't be used together with border-size because the border 
; doesn't get rounded
; Individual top/bottom values can be defined using:
;   radius-{top,bottom}
radius-top = 10.0
radius-bottom = 10.0

; Under-/overline pixel size and argb color
; Individual values can be defined using:
;   {overline,underline}-size
;   {overline,underline}-color
line-size = 2
line-color = ${color.ac}

; Values applied to all borders
; Individual side values can be defined using:
;   border-{left,top,right,bottom}-size
;   border-{left,top,right,bottom}-color
; The top and bottom borders are added to the bar height, so the effective
; window height is:
;   height + border-top-size + border-bottom-size
; Meanwhile the effective window width is defined entirely by the width key and
; the border is placed withing this area. So you effectively only have the
; following horizontal space on the bar:
;   width - border-right-size - border-left-size
border-bottom-size = 0
border-color = ${color.ac}

; Number of spaces to add at the beginning/end of the bar
; Individual side values can be defined using:
;   padding-{left,right}
padding = 2

; Number of spaces to add before/after each module
; Individual side values can be defined using:
;   module-margin-{left,right}
module-margin-left = 1
module-margin-right = 1

; Fonts are defined using <font-name>;<vertical-offset>
; Font names are specified using a fontconfig pattern.
;   font-0 = NotoSans-Regular:size=8;2
;   font-1 = MaterialIcons:size=10
;   font-2 = Termsynu:size=8;-1
;   font-3 = FontAwesome:size=10
; See the Fonts wiki page for more details

font-0 = "Iosevka Nerd Font:size=13;3"
font-1 = "Iosevka Nerd Font:bold:size=24;2"
font-2 = "Iosevka Nerd Font:size=22;6"
font-3 = "Source Code Pro:size=10;3"
font-5 = "Helvetica Rounded:style=Bold:size=10.5;3"
font-4 = "Montserrat Medium:style=Medium:size=12;3"
font-6 = "Hurmit Nerd Font Mono:style=medium:pixelsize=17;3"
; Modules are added to one of the available blocks
;   modules-left = cpu ram
;   modules-center = xwindow xbacklight
;   modules-right = ipc clock

;; Available modules
;;
;alsa backlight battery
;bspwm cpu date
;filesystem github i3
;subscriber demo memory
;menu-apps mpd wired-network
;wireless-network network pulseaudio
;name_you_want temperature my-text-label
;backlight keyboard title workspaces 
;;
;; User modules
;checknetwork updates window_switch launcher powermenu sysmenu menu
;;
;; Bars
;cpu_bar memory_bar filesystem_bar mpd_bar 
;volume brightness battery_bar 

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
[bar/primary]
inherit = bar/main
width = 2.5%
height = 17
offset-x = 97.3%
offset-y = 2
bottom = false
padding = 0
module-margin-left = 0
module-margin-right = 0
background = ${color.trans}
;foreground = ${color.blue}
foreground = #44abff
;modules-center = web sep2 files sep2 edit sep2 apps
modules-center = sysmenu
wm-restack = bspwm


[bar/secondary]
inherit = bar/main
width = 11%
height = 17
offset-x = 40%
offset-y = 2
background = ${color.trans}
#foreground = ${color.red}
bottom = false
padding = 1
;padding-top = 2
module-margin-left = 0
module-margin-right = 0
;modules-left = date sep mpd
modules-center = date
wm-restack = bspwm

[bar/tertiary]
inherit = bar/main
width = 11%
height = 17
offset-x = 20.7%
offset-y = 2
background = ${color.trans}
foreground = ${color.white}
bottom = false
padding = 1
;padding-top = 2
module-margin-left = 0
module-margin-right = 0
;modules-left = date sep mpd
modules-center = vpn
wm-restack = bspwm

[bar/log]
inherit = bar/main
width = 2%
height = 18
offset-x = 0.7%
offset-y = 2
bottom = false
foreground = ${color.red}
background = ${color.trans}
padding = 0
module-margin-left = 0
module-margin-right = 0
modules-center = my-text-label
wm-restack = bspwm
;override-redirect = true

[bar/top]
inherit = bar/main
width = 47%
height = 17
offset-x = 54.5%
offset-y = 2
bottom = false
font-0 = "Iosevka Nerd Font:size=12;3"
background = ${color.trans}
padding = 0
module-margin-left = 0
module-margin-right = 1
;wm-restack = bspwm
modules-center = wifi sep3 network2 sep3 memory sep3 cpu sep3 temperature sep3 alsa sep3 battery

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

; The separator will be inserted between the output of each module
separator =

; This value is used to add extra spacing between elements
; @deprecated: This parameter will be removed in an upcoming version
spacing = 0

; Opacity value between 0.0 and 1.0 used on fade in/out
dim-value = 1.0

; Value to be used to set the WM_NAME atom
; If the value is empty or undefined, the atom value
; will be created from the following template: polybar-[BAR]_[MONITOR]
; NOTE: The placeholders are not available for custom values
wm-name = openbox

; Locale used to localize various module data (e.g. date)
; Expects a valid libc locale, for example: sv_SE.UTF-8
locale = 

; Position of the system tray window
; If empty or undefined, tray support will be disabled
; NOTE: A center aligned tray will cover center aligned modules
;
; Available positions:
;   left
;   center
;   right
;   none
tray-position = none

; If true, the bar will not shift its
; contents when the tray changes
tray-detached = false

; Tray icon max size
tray-maxsize = 16

; DEPRECATED! Since 3.3.0 the tray always uses pseudo-transparency
; Enable pseudo transparency
; Will automatically be enabled if a fully transparent
; background color is defined using `tray-background`
tray-transparent = false

; Background color for the tray container 
; ARGB color (e.g. #f00, #ff992a, #ddff1023)
; By default the tray container will use the bar
; background color.
tray-background = ${color.bg}

; Tray offset defined as pixel value (e.g. 35) or percentage (e.g. 50%)
tray-offset-x = 0
tray-offset-y = 0

; Pad the sides of each tray icon
tray-padding = 0

; Scale factor for tray clients
tray-scale = 1.0

; Restack the bar window and put it above the
; selected window manager's root
;
; Fixes the issue where the bar is being drawn
; on top of fullscreen window's
;
; Currently supported WM's:
;   bspwm
;   i3 (requires: `override-redirect = true`)
wm-restack = bspwm

; Set a DPI values used when rendering text
; This only affects scalable fonts
; dpi = 

; Enable support for inter-process messaging
; See the Messaging wiki page for more details.
enable-ipc = true

; Fallback click handlers that will be called if
; there's no matching module handler found.
click-left = 
click-middle = 
click-right =
scroll-up =
scroll-down =
double-click-left =
double-click-middle =
double-click-right =

; Requires polybar to be built with xcursor support (xcb-util-cursor)
; Possible values are:
; - default   : The default pointer as before, can also be an empty string (default)
; - pointer   : Typically in the form of a hand
; - ns-resize : Up and down arrows, can be used to indicate scrolling
cursor-click = 
cursor-scroll = 

;; WM Workspace Specific

; bspwm
;;scroll-up = bspwm-desknext
;;scroll-down = bspwm-deskprev
;;scroll-up = bspc desktop -f prev.local
;;scroll-down = bspc desktop -f next.local

;i3
;;scroll-up = i3wm-wsnext
;;scroll-down = i3wm-wsprev
;;scroll-up = i3-msg workspace next_on_output
;;scroll-down = i3-msg workspace prev_on_output

;openbox
;awesome
;etc

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

;; Application Settings

[settings]
; The throttle settings lets the eventloop swallow up til X events
; if they happen within Y millisecond after first event was received.
; This is done to prevent flood of update event.
;
; For example if 5 modules emit an update event at the same time, we really
; just care about the last one. But if we wait too long for events to swallow
; the bar would appear sluggish so we continue if timeout
; expires or limit is reached.
throttle-output = 5
throttle-output-for = 10

; Time in milliseconds that the input handler will wait between processing events
throttle-input-for = 30

; Reload upon receiving XCB_RANDR_SCREEN_CHANGE_NOTIFY events
screenchange-reload = false

; Compositing operators
; @see: https://www.cairographics.org/manual/cairo-cairo-t.html#cairo-operator-t
compositing-background = source
compositing-foreground = over
compositing-overline = over
compositing-underline = over
compositing-border = over

; Define fallback values used by all module formats
format-foreground = 
format-background =
format-underline =
format-overline =
format-spacing =
format-padding =
format-margin =
format-offset =

; Enables pseudo-transparency for the bar
; If set to true the bar can be transparent without a compositor.
pseudo-transparency = false

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
;;
;;	    __  ___          __      __         
;;	   /  |/  /___  ____/ /_  __/ /__  _____
;;	  / /|_/ / __ \/ __  / / / / / _ \/ ___/
;;	 / /  / / /_/ / /_/ / /_/ / /  __(__  ) 
;;	/_/  /_/\____/\__,_/\__,_/_/\___/____/  
;;
;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
[module/my-text-label]
type = custom/text
content = 
;interval = 1.0
;time = %k : %M
;date = %b %e
;format = <label1>
;format-foreground = ${color.white}
;label1 = aadasd
;label1-font = 7

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

[module/date]
type = internal/date

interval = 1.0
time = %k : %M : %S
date = %b %e
;padding-top = 2
format = <label>
format-foreground = ${color.white}
;format-background = $(color.blue}
;label = %{T7} %{T6}%date%|%time%
;label = %{T7} %{T6}Pop! OS  |   %time%
label = %time% | %date% 
label-font = 6

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

[module/mpd]
type = internal/mpd

interval = 2

format-online = <label-song>
format-online-foreground = ${color.fg}

label-song = "%title%"
label-song-maxlen = 12
label-song-ellipsis = true
label-offline = "Offline"

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

;; Apps
 
[module/term]
type = custom/text

content = "%{T3}%{T-}"
content-foreground = ${color.black}
content-background = ${color.bg}
content-padding = 0

click-left  = termite &

[module/web]
type = custom/text

content = "%{T3}%{T-}"
content-foreground = ${color.white}
content-background = ${color.bg}
content-padding = 0

click-left  = firefox &

[module/files]
type = custom/text

content = "%{T3}%{T-}"
content-foreground = ${color.blue}
content-background = ${color.bg}
content-padding = 0

click-left  = thunar &

[module/edit]
type = custom/text

content = "%{T3}%{T-}"
content-foreground = ${color.blue-gray}
content-background = ${color.bg}
content-padding = 0

click-left  = geany &

[module/apps]
type = custom/text

content = "%{T3}%{T-}"
content-foreground = ${color.fg}
content-background = ${color.bg}
content-padding = 0

click-left  = ~/.config/polybar/scripts/launcher &

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

[module/vpn]
type = custom/script
interval = 2
exec = ~/.config/bin/vpnstatus.sh

[module/sep3]
type = custom/text
content = "\"
content-foreground = ${color.mor}
#content-padding = 1
;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

[module/alsa]
type = internal/alsa
;format-background = ${color.blue}
format-volume = <ramp-volume>
format-muted = <label-muted>
label-muted = %{A3:pavucontrol &:}婢%{A}
label-muted-foreground = ${color.gray}
;click-right = pavucontrol
ramp-volume-0 = %{A3:pavucontrol &:} 奄%{A}
ramp-volume-1 = %{A3:pavucontrol &:}奔%{A}
ramp-volume-2 = %{A3:pavucontrol &:}奔%{A}
ramp-volume-3 = %{A3:pavucontrol &:}墳%{A}
ramp-volume-4 = %{A3:pavucontrol &:}墳%{A}
ramp-volume-foreground = ${color.white}

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

[module/battery]
type = internal/battery

full-at = 100
battery = BAT0
adapter = ACAD

poll-interval = 2
time-format = %H:%M

format-charging = <animation-charging><label-charging>
label-charging = %{T6} %percentage%%
label-charging-background = ${color.alt-bg}
label-charging-foreground = ${color.red}
format-charging-background = ${color.alt-bg}
format-charging-foreground = ${color.red}

format-discharging = <ramp-capacity><label-discharging> 
label-discharging = %{T6} %percentage%%  
label-discharging-background = ${color.alt-bg}
label-discharging-foreground = ${color.cyan}
format-discharging-background = ${color.alt-bg}
format-discharging-foreground = ${color.cyan}

format-full = <label-full>
format-full-foreground = ${color.white}
;format-full-background = $(color.white}
label-full = 

ramp-capacity-0 = 
ramp-capacity-1 = 
ramp-capacity-2 = 
ramp-capacity-3 = 
ramp-capacity-4 = 
#ramp-capacity-5 = 
#ramp-capacity-6 = 
#ramp-capacity-7 = 
#ramp-capacity-8 = 
#ramp-capacity-9 = 
ramp-capacity-foreground = ${color.white}

animation-charging-0 = 
animation-charging-1 = 
animation-charging-2 = 
animation-charging-3 = 
animation-charging-4 = 
#animation-charging-5 = 
#animation-charging-6 = 
#animation-charging-7 = 
#animation-charging-8 = 
#animation-charging-9 = 
animation-charging-foreground = ${color.white}
animation-charging-framerate = 750

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

[module/wifi]
type = internal/network
interface = wlan0
click-left = kcmshell5 kcm_networkmanagement
interval = 1.0
accumulate-stats = true
unknown-as-up = true

format-connected = <label-connected>
format-connected-foreground = ${color.vh}
;content-background = $(color.blue}
format-disconnected = <label-disconnected>
format-disconnected-foreground = ${color.red}

#label-connected = 直
#label-disconnected = 睊
label-connected = 直%{T}  %{T6}%downspeed%
label-disconnected = " 睊 "
;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

[module/network2]
type = internal/network

interface = eth0
interval = 1.0

accumulate-stats = true
unknown-as-up = true

format-connected =<label-ip><label-connected>
format-connected-background = ${color.alt-bg}
format-connected-foreground = ${color.vh}
format-connected-padding = 0
 
format-disconnected = <label-disconnected>
format-disconnected-background = ${color.alt-bg}
format-disconnected-foreground = ${color.red}
format-disconnected-padding = 0


label-connected = %{T}  %{T6}%downspeed% 
#%{T6}%local_ip% 
label-disconnected = "Disconnected"
;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_

[module/sysmenu]
type = custom/text
content = 襤

;content-foreground = ${color.white}
click-left = ~/.config/polybar/scripts/powermenu_alt

;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_
;;	    __________  ______
;;	   / ____/ __ \/ ____/
;;	  / __/ / / / / /_    
;;	 / /___/ /_/ / __/    
;;	/_____/\____/_/       
;;
;; _-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_-_


[module/network1]
type = internal/network

interface = wlan0
interval = 1.0

accumulate-stats = true
unknown-as-up = true

format-connected = <label-connected>
format-connected-background = ${color.alt-bg}
format-connected-foreground = ${color.green}
format-connected-padding = 0
 
format-disconnected = <label-disconnected>
format-disconnected-background = ${color.alt-bg}
format-disconnected-foreground = ${color.red}
format-disconnected-padding = 0

# label-connected = %essid%
label-connected =  %downspeed% 
label-disconnected = "Disconnected"


[module/cpu1]
type = internal/cpu

; Seconds to sleep between updates
; Default: 1
interval = 0.5

format =  <label> 
format-background = ${color.alt-bg}
format-foreground = ${color.red}
format-padding = 0

label = %percentage:2%%%{T-}

; label = %output%


[module/cpu]
type = internal/cpu
interval = 0.5
format-prefix = 
#format-prefix-foreground = ${colors.foreground-alt}
format-prefix-foreground = #00ffd5
#1format-prefix-foreground = #3cff00
#format-underline = #f90000
label = %{T6} %percentage:2%%
#label-foreground = #00ffd5
label-foreground = ${color.blshade7}

[module/memory]
type = internal/memory
interval = 0.5
format-prefix = 
format-prefix-foreground = ${color.yellow}
#format-prefix-foreground = #3cff00
#format-underline = #4bffdc
label = %{T6} %percentage_used%%
#label-foreground = #00ffd5
label-foreground = #ffffff

[module/temperature]
type = internal/temperature
interval = 0.5
thermal-zone = 0
warn-temperature = 60

format-prefix = 
format-prefix-foreground = ${color.red}
format = <ramp> <label>
#format-underline = #f50a4d
#format-underline = #0
#format-warn = <ramp> <label-warn>
#format-warn-underline = ${self.format-underline}

label = %{T6}%temperature-c%
label-warn = %{T6}%temperature-c%
label-warn-foreground = ${color.red}
#label-warn-foreground = ${colors.secondary}
label-foreground = ${color.yellow}

ramp-0 = 
ramp-1 = 
ramp-2 = 
#ramp-foreground = ${colors.foreground-alt}
ramp-foreground = ${color.red}

```
# ***Nuevos Modulos***

```ruby

Estructura de un nuevo modulo:

Previamente tenemos que crear una carpeta en "~/.config/bin"

[module/tumodulo]
type = custom/script
interval = 2
exec = ~/.config/bin/binario.sh

Por ejemplo, para el módulo del ethernet_status:

#!/bin/sh

echo "%{F#2495e7} %{F#ffffff}$(/usr/sbin/ifconfig eth0 | grep "inet " | awk '{print $2}')%{u-}"

Para el módulo de hackthebox_status.sh:

#!/bin/sh

IFACE=$(/usr/sbin/ifconfig | grep tun0 | awk '{print $1}' | tr -d ':')

if [ "$IFACE" = "tun0" ]; then
	echo "%{F#1bbf3e} %{F#ffffff}$(/usr/sbin/ifconfig tun0 | grep "inet " | awk '{print $2}')%{u-}"
else
	echo "%{F#1bbf3e}%{u-} Disconnected"
fi

```



# ***Instalamos i3lock-fancy y el imagemagick***

```

apt-get install i3lock-fancy imagemagick -y

```



- Reiniciamos el sistema y una vez arrancado, incorporamos en el archivo ```bspwmrc``` la siguiente línea para arreglar el cursor:

```ruby

xsetroot -cursor_name left_ptr &

```

# ***Instalamos la powerlevel10k en zsh***



Verificamos que tipo de terminal tenemos con: ```echo $SHELL``` vamos a descargar la powerlevel10k desde github, ponemos en el buscador```powerlevel10k github```

- Usamos el siguiente comando primero como usuario normal para instalar la powerlevel10k:

```ruby

cd ~ 
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
zsh

```
- Seguimos las configuraciones y listo.


Posteriormente retocamos el archivo p10k con ```nano .p10k.zsh```, comentamos el ```vcs```, agregamos ```status```, agregamos ```command_execution_time```, agregamos ```contexts``` y comentamos todo lo que este abajo de ```typeset -g POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS```



- Ahora hacemos lo mismo para ```root```

```ruby

root# cd ~
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
zsh
SIGUIMOS LAS CONFIGURANCIONES Y LISTO
PORTERIORMENTE RETOCAMOS EL ARCHIVO P10K DEL USUARIO ROOT
root# nano .p10k.zsh
comentamos el #vcs
agragamos el status
agregamos el command_execution_time
agregamos el contexts
comentamos todo lo que este abajo de typeset -g POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS
Para el de root, podemos ir a 'POWERLEVEL9K_CONTEXT_ROOT_TEMPLATE' para asignar el Hashtag.
Comentamos la siguiente línea:
# POWERLEVEL9K_CONTEXT_PREFIX='%246Fwith '

```

- Creamos enlace simbólico de la ```zshrc``` para ```root```

```ruby

root# cd ~
root# ln -s -f /home/f3n1x/.zshrc .zshrc
root# ls -la

```

Cambiamos el tipo de ```shell``` por defecto tanto para ```root``` como para el usuario con bajos privilegios con los siguientes comandos.

```ruby

usermod --shell /usr/bin/zsh tuUsuario
usermod --shell /usr/bin/zsh root

```


Para evitar un pequeño problema de permisos a la hora de desde el usuario ```root``` migrar con ```su``` al usuario con bajos privilegios, ejecutamos los siguientes comandos:

```

chown s4vitar:s4vitar /root
chown s4vitar:s4vitar /root/.cache -R
chown s4vitar:s4vitar /root/.local -R

```

# ***Instalamos bat, lsd, fzf y ranger***
Agregamos el # Fix the Java Problem a la ```zshrc```

```ruby

export _JAVA_AWT_WM_NONREPARENTING=1

```

# ***Instalamos el bat***

Vamos a esta url [https://github.com/sharkdp/bat](https://github.com/sharkdp/bat) vamos a ```releases``` y descargamos la ultima vercion ```amd64.deb```, lo instalamos con ```dpkg -i nombre.amd64.deb```



En la ```zshrc``` nos creamos un alias para que el ```cat``` sea un ```bat```.

```ruby

alias cat='/usr/bin/bat'
alias catn='/usr/bin/cat'
alias catnl='/usr/bin/bat --paging=never'

```


# ***Instalamos el lsd***


Vamos a esta url [https://github.com/Peltoche/lsd](https://github.com/Peltoche/lsd) vamos a ```releases``` y descargamos la ultima vercion ```amd64.deb```, lo instalamos con ```dpkg -i nombre.amd64.deb```

En la ```zshrc``` nos creamos un alias para que el ```ls``` sea un ```lsd```.


```ruby

# Manual aliases
alias ll='lsd -lh --group-dirs=first'
alias la='lsd -a --group-dirs=first'
alias l='lsd --group-dirs=first'
alias lla='lsd -lha --group-dirs=first'
alias ls='lsd --group-dirs=first'

```

# ***Instalamos el fzf***

Es un auto completado inteligente de rutas, lo tenemos que instalar tanto para root como para el usuario normal.

- USURARIO NORMAL
Nos vamos a la siguiente url [https://github.com/junegunn/fzf](https://github.com/junegunn/fzf)

Para instalar vamos a la opcion ```usign git``` y copiamos este comando:


```ruby

git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install

LE DAMOS Y
LE DAMOS Y
LE DAMOS Y

```

- USUARION ROOT

Nos colocamos como ```super usuarios``` y copiamos el siguiente comando:

```ruby

git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install

LE DAMOS Y
LE DAMOS Y
LE DAMOS N

```
- Para ponerlo en marcha usamos 
Las teclas ctrl + r nos busca el historial.
Las teclas ctrl + t nos busca los directorios. 

# ***Instalar el plugin sudo***

Podemos instalar plugin en zsh para que nos automatize tarea.

Para instalar este ```plugin sudo``` vamos a este url: [https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/sudo/sudo.plugin.zsh](https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/sudo/sudo.plugin.zsh)



```ruby

ENTRAMOS EN RAW


Y NOS COPIAMOS EL LINK PARA DESCARGAR EL PLUGIN


https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/sudo/sudo.plugin.zsh


NOS PONEMOS COMO USUARIO ROOT

VAMOS AL DIRECCTORIO usr/share

Y NOS CREAMOS UNA CARPETA LLAMADA ZSH-PLUGINS
mkdir zsh-plugins

HACEMOS COMO PROPIETARIO DE ZSH-PLUGINS AL NUESTRO USUARIO

chown f3n1x:f3n1x zsh-plugins

NOS VOLVEMOS USUARIO NORMAL 

su f3n1x

DESCARGAMOS EL PLUGIN
cd zsh-plugins
wget https://github.com/ohmyzsh/ohmyzsh/blob/master/plugins/sudo/sudo.plugin.zsh

Ya tememos copiado el plugin

AHORA EN LA ZSHRC AL FINAL COPIAMOS LA SIGUIENTE LINEA
source /usr/share/zsh-plugins/sudo.plugin.zsh

```


# ***Instalamos ranger***

```ruby

sudo apt install ranger
ranger 

```


# ***Instalamos Oh My Tmux***

En el navegador escribimos ```oh my tmux github``` o vamos a esta url [https://github.com/gpakosz/.tmux](https://github.com/gpakosz/.tmux)

Tenemos que usar estos comandos como ```usuario normal y como root```.

```ruby

usuario normal

$ cd
$ git clone https://github.com/gpakosz/.tmux.git
$ ln -s -f .tmux/.tmux.conf
$ cp .tmux/.tmux.conf.local .

usuario root

$ cd
$ git clone https://github.com/gpakosz/.tmux.git
$ ln -s -f .tmux/.tmux.conf
$ cp .tmux/.tmux.conf.local .

```



