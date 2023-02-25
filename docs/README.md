mtxbatch - Move (or swap) many tapes in a single command
=============================
Generates (and optionally runs) `mtx` commands to move sets of tapes from one location to another, or to swap tapes in one location with another set of tapes.  

Features
--------------------------
- Preview the list `mtx` commands that will be run
- Preserves ordering of tapes in set (by default, can optionally reverse order)
- Specify tapes as list, a range, or a combination of lists and ranges
- Specify which slot to use as temporary storage when swapping tapes
- Handles situations where slots from source location overlap with target location slots

Examples
--------------------------
Print the list of commands to move a batch of tapes from one slot range (1-20) to another (51-60):  
```
mtxbatch -f /dev/changer move 1-20 51-70
```

Print the list of commands to move a batch of tapes, AND run the commands as you print them:  
```
mtxbatch -f /dev/changer move 1-20 51-70 --run
```

Print a list of commands to move tapes from a list of ranges to another list of ranges:  
```
mtxbatch -f /dev/changer move 1-5,8,10-14 51-61
```

Print a list of command to swap tapes in one range with tapes in another range, also specifying the empty slot to use while swapping:  
```
mtxbatch -f /dev/changer swap 6-10 21-25 --use-empty 40
```

Print commands to swap a list of tapes in one range with another range, and reverse the order of tapes when swapping them:  
```
mtxbatch -f /dev/changer swap 6-10 21-25 --use-empty 40 --reverse
```

Run command to move a range of tapes to another range, but suppress printing out the commands being run:  
```
mtxbatch -f /dev/changer move 1-20 51-70 --run --quiet
```

Command Use
--------------------------
```
mtxbatch -f <changer-device> command SOURCE TARGET [flags]
```

`-f <changer-device>` This specifies the tape changer which will be passed on to the `mtx` command's `-f` flag.  

`command` This can be either `move` or `swap`. The `move` commmand will transfer all tapes from the SOURCE slots to the TARGET slots. The `swap` command will transfer all the tapes from SOURCE slots over to TARGET slots, and will also transfer all tapes from the TARGET slots back into the SOURCE slots.  

_Flags_:  

 - `--use-empty` Only used when performing the `swap` command. This flags takes a slot number argument where it will temporarily place tapes while swapping. The slot number specified must be empty.
 - `--allow-empty` When moving or swapping tapes, preserve any empty slots included in the command. i.e. The empty slot is "moved" as well.
 - `--reverse` Reverses tape order (first becomes last, last becomes first) while moving tapes; for the `swap` command, it will reverse both SOURCE and TARGET tape sets. This also works when specifying a list of tape slots/ranges for either SOURCE or TARGET.
 - `--reverse-source` Similar to `--reverse`, but only the SOURCE tapes are reversed when using the `swap` command.
 - `--reverse-target` Similar to `--reverse`, but only the TARGET tapes are reversed when using the `swap` command.
 - `--run` This will have `mtxbatch` attempt to run each command rather than just print them to stdout.
 - `--quiet` This will suppress the printing of commands to stdout.
 - `--skip-run-checks` Do not perform validity checks before attempting to run mtx commands.


Author and Copyright
--------------------------
Written by Nathan Collins (npcollins/gmail/com)  

Copyright © Michigan State University Board of Trustees  

Released under the Apache License 2.0  
