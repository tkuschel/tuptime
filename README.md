tuptime
=======

Tuptime reports the historical and statistical real-time statistics about the system and preserves them between restarts.
It's like the uptime command, but with more interesting output.


### Sample output

Just after install:

	System startups:        1  06/21/2025 05:21:07 PM
	System shutdowns:       0 ok  +  0 bad
	System life:            21m 30s

	Longest uptime:         21m 30s  from  06/21/2025 05:21:07 PM
	Average uptime:         21m 30s
	System uptime:          100.0%  =  21m 30s

	Longest downtime:	0s
	Average downtime:       0s
	System downtime:        0.0%  =  0s

	Current uptime:         21m 30s  since  06/21/2025 05:21:07 PM

Several days later:

	System startups: 	4  since  06/21/2025 05:21:07 PM
	System shutdowns: 	3 ok  +  0 bad
	System life: 	        149d 5h 45m 32s

	Longest uptime: 	60d 22h 23m 6s  from  06/21/2025 05:21:07 PM
	Average uptime: 	37d 7h 26m 14s
	System uptime: 	        100.0%  =  149d 5h 44m 57s

	Longest downtime: 	14s  from  08/21/2025 03:44:13 PM
	Average downtime: 	12s
	System downtime: 	0.0%  =  35s

	Current uptime: 	54d 8h 55m 33s  since  09/24/2025 02:11:06 PM

Swich to -t | --table option:

    No.              Startup T.           Uptime             Shutdown T.  End  Downtime
      1  06/21/2025 05:21:07 PM  60d 22h 23m 06s  08/21/2025 03:44:13 PM  OK        14s
      2  08/21/2025 03:44:27 PM  17d 05h 14m 36s  09/07/2025 08:59:03 PM  OK        11s
      3  09/07/2025 08:59:14 PM  16d 17h 11m 42s  09/24/2025 02:10:56 PM  OK        10s
      4  09/24/2025 02:11:06 PM  54d 09h 07m 11s  11/17/2025 10:20:17 PM  BAD    3m 48s
      5  11/17/2025 10:24:05 PM       1h 03m 44s
        . . .

Or swich to -l | --list option:

    Startup:  1  at  06/21/2025 05:21:07 PM
    Uptime:   60d 22h 23h 06s
    Shutdown: OK  at  08/21/2025 03:44:13 PM
    Downtime: 14s
    
    Startup:  2  at  08/21/2025 03:44:27 PM
    Uptime:   17d 05h 14h 36s
    Shutdown: OK  at  09/07/2025 08:59:03 PM
    Downtime: 11s
   
    Startup:  3  at  09/07/2025 08:59:14 PM
    Uptime:   16d 17h 11h 42s
    Shutdown: OK  at  09/24/2025 02:10:56 PM
    Downtime: 10s
	. . .


### Basic Installation


#### By package manager

* Debian: https://packages.debian.org/tuptime
* Ubuntu: https://packages.ubuntu.com/tuptime
* Fedora, EPEL: https://src.fedoraproject.org/rpms/tuptime
* FreeBSD: https://www.freshports.org/sysutils/tuptime
* Archlinux: https://aur.archlinux.org/packages/tuptime
* OpenSUSE: https://software.opensuse.org/package/tuptime (Community Maintained / Unofficial)

#### By one-liner script

	bash < <(curl -Ls https://git.io/tuptime-install.sh)


#### By manual method

Briefly in a Linux or FreeBSD system...

Clone the repo:

	git clone --depth=1 https://github.com/rfmoz/tuptime.git

Copy the 'tuptime' file located under 'latest/' directory to '/usr/bin/' and make it executable:

	cp tuptime/src/tuptime /usr/bin/tuptime
	chmod ugo+x /usr/bin/tuptime

Assure that the system pass the prerequisites:

	python 3.X 

Run first with a privileged user:

	tuptime

Pick from 'src/' folder the right file for your cron and init manager, setup both
properly. See 'tuptime-manual.txt' for more information.


### Highlights about Tuptime internals

- It doesn't run as a daemon, at least, it only needs execution when the init manager startup and shutdown the system. To avoid issues with a switch off without a proper shutdown, like power failures, a cron job and a .timer unit are shipped with the project to update the registers each n minutes. As a system administrator, you can easily choose the best number for your particular system requirements.

- It is written in Python using common modules and as few as possible, quick execution, easy to see what is inside it, and modify it for fit for your particular use case.

- It registers the times in a sqlite database. Any other software can use it. The specs are in the tuptime-manual.txt. Also, it has the option to output the registers in seconds and epoch or/and in csv format, easy to pipe it to other commands.

- Its main purpose is tracking all the system startups/shutdowns and present that information to the user in a more understandable way. Don't have mail alerts when a milestones are reached or the limitation of keep the last n records.

- It's written to avoid false startups registers. This is an issue that sometimes happens when the NTP adjust the system clock, on virtualized environments, on servers with high load, when the system resynchronized with their RTC clock after a suspend and resume cycle...

- It can report:
  - Registers as a table or list ordering by any label.
  - The whole life of the system or only a part of it, closing the range between startups/shutdowns or timestamps.
  - Accumulated running and sleeping time over an uptime.
  - The kernel version used and boot idenfiers.
  - The system state at specific point in time.


### Alternatives

journalctl --list-boots - Show a tabular list of boot numbers (relative to the current boot), their IDs, and the timestamps of the first and last message pertaining to the boot. Close output than 'tuptime  -bit'.
https://github.com/systemd/systemd/

uptimed - Is an uptime record daemon keeping track of the highest uptimes a computer system ever had. It uses the system boot time to keep sessions apart from each other.
https://github.com/rpodgorny/uptimed

downtimed - Is a program for monitoring operating system downtime, uptime, shutdowns and crashes and for keeping record of such events.
https://dist.epipe.com/downtimed/

lastwake - Analyzes the system journal and prints out wake-up and sleep timestamps; for each cycle it tells whether the system was suspended to RAM or to disk (hibernated).
https://github.com/arigit/lastwake.py

(bonus) dateutils - Not an alternative, but it is a nifty collection of tools to work with dates.
https://github.com/hroptatyr/dateutils

ruptime - Is a modern rwhod replacement that is easy to customize, not limited to a network, and does not send clear text data over the network.
https://github.com/alexmyczko/ruptime


### More information

Please, read tuptime-manual.txt for a complete reference guide.
