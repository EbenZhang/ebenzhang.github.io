`git config alias.pullall '!git pull && git submodule update --init --recursive'`

If you want arguments to be passed to git pull, then use this instead:

    git config alias.pullall '!f(){ git pull "$@" && git submodule update --init --recursive; }; f'