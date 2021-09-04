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
'enable_previews' => true,
  'enabledPreviewProviders' =>
  array (
    'OC\Preview\Movie',
    'OC\Preview\PNG',
    'OC\Preview\JPEG',
    'OC\Preview\GIF',
    'OC\Preview\BMP',
    'OC\Preview\XBitmap',
    'OC\Preview\MP3',
    'OC\Preview\MP4',
    'OC\Preview\TXT',
    'OC\Preview\MarkDown',
    'OC\Preview\PDF'
  ),
  ```
