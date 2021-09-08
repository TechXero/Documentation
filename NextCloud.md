Packages to install

`sudo snap install nextcloud --edge && sudo snap start nextcloud`

Maintenance mode

`sudo nextcloud.occ maintenance:mode --on`

Moving Data Dir 

`sudo mv /NextCloud/data/ /run/media/techxero/NextCloud/`

`sudo nano /var/snap/nextcloud/28354/nextcloud/config/config.php`

`https://www.youtube.com/watch?v=VHF89bZR0bg`

Enable/Disbale Encryption

```
sudo nextcloud.occ maintenance:mode --on/off
sudo nextcloud.occ encryption:disable/enable
```

Enable HTTPS

`sudo nextcloud.enable-https self-signed`

Allow ports through firewall

`sudo ufw allow 80,443/tcp`

Add the following in config.php to add trusted domain

`1 => 'domain.com',`

Install dnsmaq and edit config @ /etc/dnsmasq.conf

`Use the one on this repo`

Add selected domain to hosts file on every PC

`serverIP  domain.com`

Generate Preview to all

`sudo nextcloud.occ preview:generate-all -vvv`

Have the following in config.php

```
'enable_previews' => true,
'enabledPreviewProviders' =>
 array (
    0 => 'OC\\Preview\\TXT',
    1 => 'OC\\Preview\\MarkDown',
    2 => 'OC\\Preview\\OpenDocument',
    3 => 'OC\\Preview\\PDF',
    4 => 'OC\\Preview\\MSOffice2003',
    5 => 'OC\\Preview\\MSOfficeDoc',
    6 => 'OC\\Preview\\Image',
    7 => 'OC\\Preview\\Photoshop',
    8 => 'OC\\Preview\\TIFF',
    9 => 'OC\\Preview\\SVG',
   10 => 'OC\\Preview\\Font',
   11 => 'OC\\Preview\\MP3',
   12 => 'OC\\Preview\\Movie',
   13 => 'OC\\Preview\\MKV',
   14 => 'OC\\Preview\\MP4',
   15 => 'OC\\Preview\\AVI',
 ),
  ```
