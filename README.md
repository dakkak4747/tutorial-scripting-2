
#!/bin/sh -u   

PATH=/bin:/usr/bin ; export PATH

umask 022

#First Line:"The shebang",line tells the kernel which program to run(bash).
#Second Line: Set the shell search PATH.
#Third Line: Set the shell umask (permision).

if [ $# -lt 1 ] # creating IF to determin if the arguments meet the requirment
then
        echo "Enter at least one Arguments to the script" >&2  # switch between first channel and the second channel.

        exit 1  # output standard error message 
fi

if [ -d "output" ]; then # check if "output' is a directory 

       if !  rm -r "output" 2>/dev/null; then # redirect the output message to the null file to not diplay it to the user
        echo "Error failed to remove existing dir 'output'"  >&2    # switch between first channel and the second channel for the output.   
        exit 2 # output Error message of creating and dealting a dir.

       fi
fi


if ! mkdir "output" 2>/dev/null; then  #output the error message to the empty file 'null' if the creation of the dir did not work
        echo "Error ; failed to create the dir 'output'" >&2 # switch between first channel and the second channel for the output
       exit 2  # output Error message of creating and dealting a dir.
fi


X=$1  # crearting a variable to be equals the first argument of the scribt.
if [ $X -le 0 ]    # creating an if statement to determine of 'X' is a positive or negative intger.
then
        echo "argument should be positive only" >&2
        exit 3     # Error output realted of  assigning the varaible to an value
fi


shift  # Remove the first argument 

# Process each of the remaining arguments
for num in "$@"; do # iltrates in the scribt Range 
    # Categorize the number
    if [ $num -lt $((X / 2)) ]; then
        dest="sect1"
    elif [ $num -ge $((X / 2)) ] && [ $num -lt $X ]; then
        dest="sect2"
    elif [ $num -ge $((2 * X)) ] && [ $num -lt $((3 * X)) ]; then
        dest="sect3"
    elif [ $num -ge $((3 * X)) ]; then
        dest="sect4"
    else
        continue  # Skip numbers that don't fit in any category
    fi

    # Determine number is odd or even 
    if [ $((num % 2)) -eq 0 ]; then
        file="${dest}_even.txt"
    else
        file="${dest}_odd.txt"
    fi

    # Append the number to the appropriate file (add the output to the end of the file)
    echo $num >> "output/$file"done  # End of the For Loop
