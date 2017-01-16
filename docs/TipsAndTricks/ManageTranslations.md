<!-- Name: TipsAndTricks/ManageTranslations -->
<!-- Version: 1 -->
<!-- Last-Modified: 2006/03/13 20:33:42 -->
<!-- Author: rm -->
# Hints for a hassle-free translation

The following tricks require grep, sed, sort and diff tools. 

## Duplicated Entries

You can find duplicated lines in your translation file by running this pipeline:


    grep -v "/\*\|\*/\|//" italian-iso-8859-1.php | sort | uniq -d 

The grep call removes the noise of duplicated comment delimiters.


    #!html
    <h2 style="color: #0c0">
    I'm pretty confident that these scripts are reliable ;)
    </h2>

## Check if your language file is updated

Get a sorted list of the translatable strings from the base file:


    grep -o "[^*]*=>\|\['[^']*']" english-iso-8859-15.php \
            | sed -e "s/[]'>=[]//g;s/^[[:space:]]*//;s/*[[:space:]]$//" | sort > base-strings.txt

We consider that duplicated strings have been already removed.

Then get strings translated in your language file:


    grep -o "[^*]*=>\|\['[^']*']" italian-iso-8859-1.php \
            | sed -e "s/[]'>=[]//g;s/^[[:space:]]*//;s/*[[:space:]]$//" | sort > my-lang-strings.txt

Find which string are not translated yet, they are the one starting with _-_ char. Entries starting with _+_ char are strings that are no more available so you can remove them.


    diff -ub base-strings.txt my-lang-strings.txt | grep "^+[^+]\|^-[^-]"

## There should be an automated way...

If you are lazy like wmk you can try these.


    #!/bin/sh
    
    # USAGE:
    # You should run it inside your seagull root dir (ie. trunk/)
    # $ /path/to/seauniqtrans.sh italian-iso-8859-1.php
    
    LANG=$1
    MODULES=$(ls modules)
    DIR=../seatrans/$(basename $LANG .php)
    
    mkdir -p $DIR
    
    for i in $MODULES
    do
            grep -v "/\*\|\*/\|//" modules/$i/lang/$LANG | sort | uniq -d \
                    > $DIR/$i.dup
    done

    #!/bin/sh
    
    # USAGE:
    # You should run it inside your seagull root dir (ie. trunk/)          
    # $ /path/to/seatrans.sh italian-iso-8859-1.php
     
    BASE=english-iso-8859-15.php
    LANG=$1
    MODULES=$(ls modules)
    DIR=../seatrans/$(basename $LANG .php)
    
    get_strings ()
    {
            grep -o "[^*]*=>\|\['[^']*']" modules/$1/lang/$2 \
                    | sed -e "s/[]'>=[]//g;s/^[[:space:]]*//;s/*[[:space:]]$//" \
                            | sort
    }
    
    mkdir -p $DIR
    
    for i in $MODULES
    do
            SBASE=$i-base-strings.txt
            SLANG=$i-$LANG-strings.txt
    
            get_strings $i $BASE > $SBASE
            get_strings $i $LANG > $SLANG
    
            diff -ub $SBASE $SLANG | grep "^+[^+]\|^-[^-]" > $DIR/$i.diff
    
            rm $SBASE $SLANG
    done