# 🖥️ shred

## shred history file

```
shred ~/.bash_history
```

This CMD is useful in cases where an investigator locates the file; because of this CMD, they would be unable to read any content in the history file.

## view shredded history content

```
more ~./bash_history
```

## Use all above cmd in a single command

```
shred ~./bash_history && cat /dev/null > .bash_history && history -c && exit
```

This CMD first shreds the history file, then delete it, and finally clears the evidence of using this command.
