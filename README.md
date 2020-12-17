# npiperelay

This is a fork of https://github.com/jstarks/npiperelay which is not maintained anymore.
For my use case npiperelay has to handle blocked named pipes. The issue https://github.com/jstarks/npiperelay/issues/7 has no reaction neither has my pull request https://github.com/jstarks/npiperelay/pull/18

I'm using npiperelay to access the Windows ssh-agent named pipe from WSL. I have no idea if other use cases are or will be affected by changes to this fork.

# How it works
1. Download and unzip npiperelay from the release page. I saved it in `~/scripts/`
1. Create a script based on this template. I named it `~/scripts/start_ssh_agent.sh`. You have to replace `<your_home_dir>` and double check the file names and locations.

    export SSH_AUTH_SOCK=<your_home_dir>/.ssh_agent_npiperelay.sock
    ss -a | grep -q $SSH_AUTH_SOCK
    if [ $? -ne 0   ]; then
        rm -f $SSH_AUTH_SOCK
        ( setsid socat UNIX-LISTEN:$SSH_AUTH_SOCK,umask=077,fork EXEC:'<your_home_dir>/scripts/npiperelay.exe -ei -s //./pipe/ssh-pageant',nofork& ) >/dev/null 2>&1
    fi
1. Source this file for example in your .bashrc `. <your_home_dir>/scripts/start_ssh_agent.sh`. Mind the `. ` in the beginning!
