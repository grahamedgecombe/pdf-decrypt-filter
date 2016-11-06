# pdf-decrypt-filter

## Introduction

`pdf-decrypt-filter` automatically decrypts PDF attachments in an email. It's
designed for automated use with a Mail Delivery Agent such as `procmail` or
`maildrop`.

## Using with `procmail`

Use a recipe like this in your `.procmailrc` file:

    :0 fw
    * ^From:.*encrypted-pdf-sender@example\.com
    | /path/to/pdf-decrypt-filter --password=hunter2

As rewriting the emails can break things like DKIM signatures, you might want to
write the original emails to a file for backup purposes:

    :0 cw :
    * ^From:.*encrypted-pdf-sender@example\.com
    | gzip >> pdfs.gz

    :0 afw
    | /path/to/pdf-decrypt-filter --purge --password=hunter2

By default `pdf-decrypt-filter` will keep the encrypted PDF attachment in
addition to the new plaintext attachment. The `--purge` flag can be used to
override this behaviour and remove the encrypted PDF attachment.

## Limitations

* Only supports a single password.
* The entire script will fail if the password is incorrect.
* Assumes the mail is a single `multipart/mixed` part directly containing one or
  more `application/pdf` attachments.

## Dependencies

* [Perl 5][perl]
* [File::Slurp][slurp]
* [MIME::Tools][mimetools]
* [pdftk][pdftk]

## License

`pdf-decrypt-filter` is available under the terms of the [ISC license][isc],
which is similar to the 2-clause BSD license. See the `LICENSE` file for the
copyright information and licensing terms.

[perl]: https://www.perl.org/
[slurp]: https://metacpan.org/pod/File::Slurp
[mimetools]: https://metacpan.org/pod/MIME::Tools
[pdftk]: https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/
[isc]: https://www.isc.org/downloads/software-support-policy/isc-license/
