# vaxAX

A linux-centric partition backup script.

Inspired by [What's your backup strategy?](http://www.codinghorror.com/blog/2008/01/whats-your-backup-strategy.html)

## Setup

Copy `backup_external` to wherever in your system you keep custom scripts etc.  Make sure it's executable.

Add source and target partitions to the `backups` (python) dict in the format indicated by the comment.  You should use the partitions' UUIDs, which you can get with linux's `blkid` command:

    $ blkid /dev/sda2
    /dev/sda2: LABEL="root" UUID="f1e486e7-8dbf-484a-a053-e2d47a94b520" TYPE="ext4"

Add an entry to your crontab (root's crontab; may not work under normal users' crontabs) to run as often as you want to backup:

    # Backup root and home partitions to external drive
    PATH=/usr/bin:/bin:/sbin
      0 5  *   *   *     /path/to/backup_external

The above will run the backups at 5am every day, assuming all described drives are attached to the computer, and the computer is running.  The `PATH=` line is necessary for the script to be able to access various system utilities needed to do the job.

## Hardware

I have a 120GB SSD.  I backup my home and root partitions (the only two partitions, excluding swap) to identically-sized partitions on a 160GB HDD, attached by external usb enclosure.

Generally, this is the approach you want to take: backing up your main drive, in full, whatever its type, to a relatively cheap but reliable drive of the same size.  It can get pricey, yes, but what is your data worth?  And, if it's your main machine, how much time have you spent getting your environment *just right*?  Mmhmm.

In addition to having a full backup of *all* your data, you can install grub on the backup drive and, with a few tweaks to the backup files (see below), have a fully-bootable backup.

Also, if you make the backup bootable -- be sure to test that it is, indeed, bootable.  It's fairly stressful to find out you have more work to do when your drive crashes mid-workweek and you need an environment *now*.

## Upon catastrophe

So your main drive has catastrophically died in a your-data-is-screwed kinda way.

But hey, you've got *backups*!

If you've been mirroring your partitions to a same-sized drive, you can swap the dead drive out for the backup, change your grub config and fstab on the backup drive to point to the new UUIDs (if that's how they were setup before; if not, the same "redirections" apply with whatever reference method you're using), boot up (if grub was installed to the backup drive), and you should be back in business.

And your first order of business?

**Buy a new backup drive**.

If you haven't been mirroring your root/boot/whatever partition, have fun reinstalling the latest version of your favorite distro!  Hey, at least your data is safe!
