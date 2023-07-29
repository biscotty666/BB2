up:: [[Fedora]]
tags:: #note/reference #on/Linux #on/SystemD #on/kinoite #on/Lappy #note/documentation
X:: [[SystemD]]  [[Laptop]]  [[Systemd Timers for Scheduling Tasks - Fedora Magazine]]
%%
#log/study
Topic:: [[SystemD]]
Note:: Learned how to implement timed jobs (new cron)
Date:: 2023-07-28
Related::  [[System Backups]] [[Infrastructure]]
%%
## System Backups

### Schema

Directories to backup:
- Obsidian
- Pictures
- Videos
- Learning
- Projects
- Documents
- R

Daily backup at midnight

Retention:
- 4 dailys
- 4 weeklies
- 12 monthlies
- 2 years

### Script

`~/.bin/restic-backup.sh`

```
#!/bin/sh
restic backup /home/biscotty/Obsidian
restic backup /home/biscotty/Pictures
restic backup /home/biscotty/Videos
restic backup /home/biscotty/Learning
restic backup /home/biscotty/Projects
restic backup /home/biscotty/Documents
restic backup /home/biscotty/R
restic forget --keep-daily 4 --keep-weekly 4 --keep-monthly 12 --keep-yearly 2 --prune
```

### Service

`~/.config/systemd/user/restic-backup.service`

```
[Unit]
Description=Daily backups with restic

[Service]
Type=simple
ExecStart=/home/biscotty/.bin/restic-backup.sh

[Install]
WantedBy=default.target
```

### Timer

`~/.config/systemd/user/restic-backup.timer`

```
[Unit]
Description=Daily Backup
RefuseManualStart=no
RefuseManualStop=no

[Timer]
#Execute job if it missed a run due to machine being off
Persistent=true
#Run 120 seconds after boot for the first time
OnBootSec=120
#Run every 1 minute thereafter
OnCalendar=*-*-* 00:00:00
#File describing job to execute
Unit=restic-backup.service

[Install]
WantedBy=timers.target
```


### `systemctl` commands for activation of timer

```
systemctl --user enable restic-backup.service
systemctl --user start restic-backup.service
systemctl --user enable restic-backup.timer
systemctl --user start restic-backup.timer
```

### Other useful `systemctl` commands

```
systemctl --user status restic-backup
systemctl --user list-unit-files # very long, pipe to grep
systemctl --user daemon-reload
systemctl --user reset-failed
```

---

### References