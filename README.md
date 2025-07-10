
# Linux - Auto Mute Service

This is a self note that created by me, this service purpose is prevent the play a video in the office as max volume, I need to mute my pc at morning before came to office so I created it for do it automatically by pc because I can forget so I am human 👱🏻.

## Create Auto Mute Service

**Step 1: Create your mute script**

```bash
    mkdir -p $HOME/.local/bin
    nano $HOME/.local/bin/mute-audio.sh
```

Check your display with 'echo $DISPLAY' before and update the display var below, then paste this

```bash
    export DISPLAY=:1
    export XDG_RUNTIME_DIR=/run/user/$(id -u)
    /usr/bin/amixer set Master mute
```

Then close it and make it executable:

```bash
    chmod +x $HOME/.local/bin/mute-audio.sh
```

**Step 2: Create the systemd service file**

```bash
    mkdir -p $HOME/.config/systemd/user
    nano $HOME/.config/systemd/user/mute-audio.service
```

**Step 3: Create the timer unit**

```bash
    nano $HOME/.config/systemd/user/mute-audio.timer
```

Paste:

```bash
    [Unit]
    Description=Run mute-audio.service daily at 8:00 AM

    [Timer]
    OnCalendar=*-*-* 08:00
    Persistent=true

    [Install]
    WantedBy=timers.target
```

**Step 4: Enable and start the timer**

```bash
    systemctl --user daemon-reexec
    systemctl --user daemon-reload
    systemctl --user enable --now mute-audio.timer
```

**Step 5: Check timer status**

```bash
    systemctl --user list-timers
```

***That’s it! Your script will run at 8:00 AM using DISPLAY=:1.***

***Step 6: Start the service***

```bash
    systemctl --user start mute-audio.service
```

## Steps to update the timer time:

**Step 1: Open the timer file for editing**

```bash
    nano ~/.config/systemd/user/mute-audio.timer
```

**Step 2: Find the line that says**

```bash
    OnCalendar=*-*-* 08:00
```

**Step 3: Change the time to your desired schedule**

**Step 4: Save and exit the editor (Ctrl+O, Enter, Ctrl+X in nano).**

**Step 5: Reload systemd and restart the timer**

```bash
    systemctl --user daemon-reload
    systemctl --user restart mute-audio.timer
```

***Step 6: Check your timers***

```bash
    systemctl --user list-timers
```



