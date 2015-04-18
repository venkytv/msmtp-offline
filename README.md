# msmtp-offline
msmtp wrapper which queues email sent when offline

Simple script to manage emails sent using msmtp while offline.  It is modeled after the `msmtpq` set of scripts shipped with msmtp and add a few enhancements:
* It parses the msmtprc file and attempts to figure out the host and port to use for the network check.  This allows mails to be sent out, for instance, via the work SMTP server when connected to the network over VPN but will not attempt to the flush mail sent through the Gmail account.
* When a mail gets queued, a watcher process keeps checking periodically to see if the network is accessible again and attempts to flush all queued mails.

#### Dependencies

Requires ruby.  (This was developed on version 2.2.0, but the script *should* run on anything greater than 1.9.3.  Has not been tested, though.)

#### Installation

(This script is primarily designed for use with `mutt`, but does not have any hard dependencies on it.  Usage instuctions below apply only to `mutt`, though.)

* Copy the `msmtp-offline` and `msmtp-queue` scripts to any directory in your PATH
* Set the following lines in your `.muttrc` file:
```
  set sendmail = /path/to/msmtp-offline
  set sendmail_wait = -1
```
