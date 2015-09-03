# Brazilian Portuguese Wordlist

## Objectives

The idea is to build a selection of Brazilian Portuguese words that are
**good enough** to be used on computer-generated mnemonic pasphrases, such as 
the ones described in the [BIP-0039 draft][bip39] and adopted by some
cryptocurrencies clients and wallets.

### Attributes of a "good enough" wordlist

Following the footsteps of other languages wordlists currently listed
in [this BIP39 document][wordlists], specially the French and Spanish ones, 
the selection of words for this list must consider the following 
features/attributes:

- High priority on simple and common portuguese words.
- Only words with 5-8 letters.
- Words can be uniquely determined typing the first 4 characters (sometimes less).
- No identical words with the [Spanish, English or French wordlists][wordlists]

In addition to that, some other attributes like:

- No accented words (we avoid words containing á,é,ê,ç… to keep it simple) 
- High priority to words that could be suggested by a mobile OS (words with a high frequency score on the [Android dictionary][androidwords], for example)

This list of attributes is still a work in progress, feedback is welcome (see
the Discussion section below). 

## Discussion

There is an [ongoing discussion about it at the bitcointalk forum][forumthread],
please drop by and say hello if you want to contribute :)


## Build

To build all lists use:

    $ npm test

## Development Scripts

The package.json file have some npm-scripts that are shortcuts for shell 
command lines, if you have node.js installed you can run then with:

    $ npm run <script_name>

The script names are listed bellow with their equivalent command lines:

### getdict:bip39

Download the bip39 recommended wordlists for Spanish, French and English

**Equivalent shell:**

    $ cd ./downloads
    $ curl -O https://raw.githubusercontent.com/fczuardi/bips/master/bip-0039/english.txt
    $ curl -O https://raw.githubusercontent.com/fczuardi/bips/master/bip-0039/spanish.txt 
    $ curl -O https://raw.githubusercontent.com/fczuardi/bips/master/bip-0039/french.txt


### getdict:android

Download the pt_BR dictionary csv from Google's Android repository

**Equivalent shell:**

    $ mkdir -p ./downloads
    $ cd ./downloads
    $ curl -O https://android.googlesource.com/platform/packages/inputmethods/LatinIME/+archive/master/dictionaries.tar.gz
    $ tar -zxvf dictionaries.tar.gz pt_BR* && gunzip pt_BR*.gz

### csv

Write a file named ```lists/all.csv``` that is the downloaded Android dictionary
filtered to display only:

- words without any of the following letters: áéíóúâêôãõç 
- that has 5-8 letters
- without any flags (flags=,), so, no adult or offensive words
- sorted by the 4th column (originalFreq)

**Equivalent shell:**

    $ cat downloads/pt_BR_wordlist.combined | \
    grep -E 'word=[^áéíóúâêôãõç,]{5,8},' | \
    sed -e 's:originalFreq=::' -e 's: word=::' -e 's:f=.*,::'| \
    sort -t, -k2 -rn >lists/all.csv

### filter:en

Write a file named ```lists/all-no_en.csv``` that is ```lists/all.csv``` without
the lines containing words of ```downloads/english.txt```

**Equivalent shell:**

    $ cat downloads/english.txt|grep '^a'| \
    sed -e 's:^\(.*\):\^\1,:' > /tmp/all_en_pattern
    $ grep -vf /tmp/all_en_pattern lists/all.csv > lists/all-no_en.csv


### filter:sp

Write a file named ```lists/all-no_en-no_sp.csv``` that is 
```lists/a-no_en.csv``` without the lines containing words 
of ```downloads/spanish.txt```

**Equivalent shell:**

    $ cat downloads/spanish.txt|grep '^a'| \
    sed -e 's:^\\(.*\\):\\^\\1,:' > /tmp/all_sp_pattern 
    $ grep -vf /tmp/all_sp_pattern lists/all-no_en.csv > lists/all-no_en-no_sp.csv


### filter:fr

Write a file named ```lists/all-no_en-no_sp-no_fr.csv``` that is 
```lists/all-no_en-no_sp.csv``` without the lines containing words 
of ```downloads/french.txt```

**Equivalent shell:**

    $ cat downloads/french.txt|grep '^a'| \
    sed -e 's:^\\(.*\\):\\^\\1,:' > /tmp/all_fr_pattern 
    $ grep -vf /tmp/all_fr_pattern lists/all-no_en-no_sp.csv > lists/all-no_en-no_sp-no_fr.csv

### crop10k

Write a file named ```lists/filtered-top-10k.csv``` that is the top 10,000 
words in frequency from ```lists/all-no_en-no_sp-no_fr.csv```

**Equivalent shell:**

    $ cat lists/all-no_en-no_sp-no_fr.csv| \
    head -n 10000 > lists/filtered-top-10k.csv
    
### a-z

Writes two separated csv files for each letter from a to z, 
each file contains the words from that letter one sorted alphabetically (alpha-a.csv, alpha-b.csv…) and the other sorted by frequency (score-a.csv,
score-b.csv…).

**Equivalent shell:**

    $ for x in {a..z}; \
    do grep "^$x" lists/filtered-top-10k.csv > lists/score-$x.csv &&\
    cat lists/score-$x.csv | sort > lists/alpha-$x.csv; 
    done


Separates

[androidwords]: https://android.googlesource.com/platform/packages/inputmethods/LatinIME/+/master/dictionaries/
[bip39]: https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki
[forumthread]: https://bitcointalk.org/index.php?topic=1167861.0
[wordlists]: https://github.com/fczuardi/bips/blob/master/bip-0039/bip-0039-wordlists.md