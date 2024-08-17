# get shit done: blocking websites and programs so you can, well, get shit done
**NOTE** This currently only works on my machine. Unless you use systemd and dhcpcd, you will run into errors here. This is because I did this in 48 hours and sometimes hard-coding something is a lot easier.

## What is this?

This is a project that I wrote for the 2024 UQ Computing Society hackathon. I wrote this because I couldn't get work done as I kept on getting distracted by websites such as Discord, Reddit, GBATemp, and other social media/forum/messaging sites.

This will automate changes to crontabs and /etc/hosts so that it will unlock after you get some work done on a coding file, or after a certain amount of time. To use it, you need Linux, and a working cron daemon.

## How do I use it?

To start a session, run `sudo ./gsd start` (or replace `./gsd` with whatever the name of the binary is), and then add flags for whether you want it to track time or a file. **This needs to be run under sudo** as it writes to /etc/hosts and restarts your network service! 

## How do I set it up?

Ideally, I want to make it so that you can set up entirely though commands: using `./gsd network` to specify a command to be run (under sudo) to restart the network and apply changes to /etc/hosts, and use `./gsd add -w` and `./gsd add -p` to block websites and programs, respectively. However, this relies on having a `config.toml` file which I haven't gotten around to making a generator for. For this, an example of what this file should look like is provided below:

```toml
[system]
# command to restart network service to refresh /etc/hosts. doesn't include sudo.
network_command = 'systemctl restart dhcpcd'
# path to where your binary is. this can also be changed with ./gsd directory [path].
program_dir = "/home/berri/Documents/coding/get-shit-done"

[blocklist]
# make sure to include www. if it is part of the URL. check on a per-website basis: comapre
# www.reddit.com and discord.com
websites = [
  'discord.com',
  'www.reddit.com',
  'gbatemp.net',
  'www.facebook.com',
  'www.tumblr.com',
  'tech.lgbt',
]
# programs should be what the process name is called. e.g. 'Discord' will kill discord
# proccesses but 'discord' won't
programs = [
  'Discord',
  'steam',
]
```

## What are the limitations of this?

- You need Linux. Sorry, fellow procrastinators who use Windows.
- You can only track plaintext files. If you want 100 words written on some essay, then the track may not work for you.
- On a similar note, this only works for lines, not words at the current moment. This may be changed as I have to write LaTeX files quite often...
- It's easy to fudge. You can just write meaningless comments to increase line count to unlock websites, and there's nothing stopping you from reverting changes to crontabs and /etc/hosts. If you want this, there's a Perl program out there for you called Lockout: <https://thomer.com/lockout/> which will lock you out of root; this is however a big risk to take. get shit done is safer!

## What are some improvements that could be made here?
- It's way too easy to break out of. There is nothing stopping you from going and editing crontabs and /etc/hosts yourself - I just hope that it's too much effort for you absentmindedly browsing the web. That being said, I can add anti-editing measures for particularly determined procrastinators.
- It, well, only works on my machine right now (see top of readme). This is because it's hardcoded to send the command "systemctl restart dhcpcd" at two places, and I didn't have time to fix this up.
- I could make it possible to track more than just lines in a plaintext file - reading words from a WYSISYG document format and reading words from a plaintext file are both things that would be useful if implemented.
- This doesn't have a GUI. It's not neccesary, but it'd be cool.
- I could add a log of when it's used, so you can track how much time you've spent working on a project.
- I could make it run on Windows.
That being said, all of these are only going to get implemented if this gets popular or I get the whim to :)

## Why did you decide to write this in Rust?

"hey Avery you should try writing something in Rust sometime it's such a cool language"
