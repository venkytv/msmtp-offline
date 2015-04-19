# msmtp-offline
msmtp wrapper which queues email sent when offline

Simple script to manage emails sent using msmtp while offline.  It is modeled after the `msmtpq` set of scripts shipped with msmtp and add a few enhancements:
* It parses the msmtprc file and attempts to figure out the host and port to use for the network check.  This allows mails to be sent out, for instance, via the work SMTP server when connected to the network over VPN but will not attempt to the flush mail sent through the Gmail account.
* When a mail gets queued, a watcher process keeps checking periodically to see if the network is accessible again and attempts to flush all queued mails.

#### Dependencies

Requires ruby.  (This was developed on version 2.2.0, but the script *should* run on anything greater than 1.9.3.  Has not been tested, though.)

#### Installation and Usage

(This script is primarily designed for use with `mutt`, but does not have any hard dependencies on it.  Usage instuctions below apply only to `mutt`, though.)

* Copy the `msmtp-offline` and `msmtp-queue` scripts to any directory in your PATH
* Create a `log` directory in your HOME, if you don't have one already.
* Set the following lines in your `.muttrc` file:
```
  set sendmail = /path/to/msmtp-offline
  set sendmail_wait = -1
```

If you need the script to do the sending in the background (necessary for use with Emacs, as I understand), pass the `+fork` option:
```
   /path/to/msmtp-offline +fork <any-msmtp-options>
```

Or if you prefer environment variables, set the `FORK_SEND` environment variable:
```
   FORK_SEND=1 /path/to/msmtp-offline <any-msmtp-options>
```

If you want to disable to watcher which periodically tests network accessibility, pass the `+nowatcher` option:
```
   /path/to/msmtp-offline +nowatcher <any-msmtp-options>
```

Or set the `NO_WATCHER` environment variable.

If the watcher is disabled, msmtp-offline will only attempt to flush any queued
messages the next time you send an email.  Of course, you can always use
`msmtp-queue` to flush queued mails manually.

If you want to disable connection tests entirely and hold all email until they
are flushed manually, use the `+forcequeue` option or the `FORCE_QUEUE`
environment variable.
```
   /path/to/msmtp-offline +forcequeue <any-msmtp-options>
   FORCE_QUEUE=1 /path/to/msmtp-offline <any-msmtp-options>
```

__IMPORTANT__: msmtp-offline options (starting with '+') need to be specified *before* any `msmtp` options.

#### Known Issues

* Uses `flock`, which has known issues with NFS-mounted directories.  If your home directory is NFS-mounted, your milage may vary.
