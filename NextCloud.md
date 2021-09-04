Add the following in config.php to add trusted domain

`1 => 'domain.com',`

Install dnsmaq and edit config @ /etc/dnsmasq.conf

```
afdasdasd
asdasdasd
asdasdasd
asdasdasd
```

Add selected domain to hosts file on every PC

`serverIP  domain.com`

Generate Preview to all

`sudo nextcloud.occ preview:generate-all -vvv`

Have the following in config.php

```
'preview_libreoffice_path' => '/usr/bin/libreoffice',
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