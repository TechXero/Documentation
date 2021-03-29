# -/ Fix Telegram MimeType :

1- Open the folder ~/.local/share/applications<br />
2- Find some files about telegram desktop, like example, my is userapp-Telegram Desktop-xxx.desktop<br />
3- Add the line at the end of file MimeType=application/x-xdg-protocol-tg;x-scheme-handler/tg;<br />
4- Reload your browser (maybe do not need…)<br />
5- Enjoy!<br />

# -/ Clear Pamac Cache

sudo mkdir /etc/pacman.d/hooks<br />
sudo nano /etc/pacman.d/hooks/clrcache.hook<br />

# -/ Links :

Panon -> https://store.kde.org/p/1326546<br />
Customization Saver -> https://store.kde.org/p/1298955/<br />

# ----> E.O.F
