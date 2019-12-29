Bash Mandarin/English dictionary based on cedict.
It is volontary extremely basic, read the source and you will now what I mean.

```
Public domain, 2016 GrepSuzette, database is using another license though
Syntax: dictzh [OPTIONS] "[terms]"

OPTIONS:
 --help -h        Show help
 --version -v     Show version and exit
 --no-sep         Omit the ------ separator

Examples
    dictzh "ni3 hao3"        # pinyin are separated by a space
    dictzh "lao. shi."       # grep regex are used, so the dots will match any char
    dictzh "^你好 "           # search for a chinese def and only that
    dictzh hello             # loosely search for a single word
    dictzh "town center"     # multiple words will require quotes
```

# Installation

Before using, don't forget to edit the `dbPath` variable in file `dictzh`,
then just copy or make a symlink to `dictzh` so it's somewhere in your `$PATH`.

# Bonus

## Bash hook, so when you type chinese directly in CLI, instead of "No such command" you get its definition instead

This makes usage of `contrib/count-chinese-characters.py` and the
`command_not_found_handle()` function in bash. The later is a bash hook
allowing a function to run instead of showing an error message.

We intercept the command that was run, and if it's make of chinese character,
will find it in `dictzh` instead!

**Instructions**

1. run `pip install regex`,
2. have some function as follows in your `~/.bashrc` file:

```bash
# ... your instructions before, then at some point:

command_not_found_handle() {
    if [[ -x "~/path/to/count-chinese-characters.py && $( ~/path/to/count-chinese-characters.py "$1" ) > 10 ]]; then
        dictzh "$1"
    else 
        echo "bash: command not found: $1"
    fi
}
```

