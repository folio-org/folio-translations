# Latest FOLIO translations

This repository maintains the latest
[FOLIO](https://folio.org/) [platform-complete](https://github.com/folio-org/platform-complete)
translations. Once a day it recreates the language files and updates the platform-complete directory
in this repository.

To backport latest FOLIO translations into an old installation
run the [backport-translations](backport-translations) script.

If you don't want new tanslation keys you may replace

`.[0] * .[1]`

by

`.[0] as $old | .[0] * .[1] | with_entries( select( .key as $key | $old | has( $key ) ) )`

in the backport-translations script but this may remove a key that the old code uses
and was forgotten to get added to the old language file.

To update only a single translation you can add 4 lines to
https://github.com/folio-org/platform-complete/blob/master/docker/Dockerfile :

Add `jq` to `apk add` resulting in

```
RUN apk upgrade \
 && apk add \
      alpine-sdk \
      jq \
      python3 \
 && rm -rf /var/cache/apk/*
```

Insert these 3 lines after `yarn build output` and before the nginx stage:

```
RUN wget https://github.com/gbv/folio-translations/raw/main/platform-complete/de.json
RUN jq -c -s '.[0] * .[1]' output/translations/de-*.json de.json > new.json
RUN mv new.json output/translations/de-*.json
```
