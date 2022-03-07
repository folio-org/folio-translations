# folio-translations

To backport latest FOLIO translations into old installations
run the backport-translations script.

To remove new translation keys replace

`.[0] * .[1]`

by

`.[0] as $old | .[0] * .[1] | with_entries( select( .key as $key | $old | has( $key ) ) )`

in backport-translations but this may remove a key that the old code uses
and was forgotten to get added to the old language file.

Daily the platform-complete directory of this repository is automatically updated to the latest
translation files of https://github.com/folio-org/platform-complete .

