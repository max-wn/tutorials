## useful vim commands

### basis
`:q!`          --> quit without saving
`:wq`          --> quit with saving the document
`:wq filename` --> quit and save with the folename

### tabs
`:tabnew`   --> creates a new tab
`:tabp`     --> goes to the previous tab
`:tabn`     --> goes to the next tab
`:tabfirst` --> goes to the first tab
`:tablast`  --> goes to the last tab

### good example of put
`:put=range(start,end)`       --> writes a list of numbers below the cursor
`:put=range(start,end,jump)`  --> writes a list of numbers below the cursor
`:for i in range(1,10) | put='192.168.0.'.i | endfor`

### edit
`:g/^/m0`      --> reverse all the lines in the document
`:s/old/new/g` --> change all old to new on cursor line
`:g&`          --> apply `:s/old/new/g` to all document

### registers
`:registers` --> shows us what is in all the  current registers (opens in place of the current document)

### files
`:Sexplore`        --> Opens a file explorer in a horizontal window
`:Vexplore`        --> Opens a file explorer in a Vertical Window
`:Texplore`        --> Opens a file explorer in a new tab
`:browse oldfiles` --> lets us browse the oldfiles we have previously edited
`:oldfiles`        --> shows us files that we have previously edited
`:e`               --> open a file
`:cd`              --> gives us the current working directory
`:read !date`      --> write in file teh result of command `date`
`:cat filename`    --> write in current file the result of `cat` command on filename file

### macro
Remember that you record macros with `q`, and you record it into one of 26 vims
registers (a to z) you stop recording with `q` again, and you play the macro
back with `@` (you can also play the macro back multiple times by putting a
number in front of the `@`)

---

THE END
