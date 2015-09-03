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

### csv:a

Write a file named ```lists/a.csv``` that is the downloaded Android dictionary
filtered to display only:

- words starting with 'a'
- without any of the following letters: áéíóúâêôãõç 
- that has 5-8 letters
- without any flags (flags=,), so, no adult or offensive words
- sorted by the 4th column (originalFreq)

**Equivalent shell:**

    $ cat downloads/pt_BR_wordlist.combined | \
    grep "word=a[^áéíóúâêôãõç]\{9,12\},flags=," | \
    sed -e 's:originalFreq=::' -e 's: word=::' | \
    sort -t, -k4 -rn >lists/a.csv

### filter:en:a

Write a file named ```lists/a-no_en.csv``` that is ```lists/a.csv``` without
the lines containing words of ```downloads/english.txt```

**Equivalent shell:**

    $ cat downloads/english.txt|grep '^a'| \
    sed -e 's:^\(.*\):\^\1,:' > /tmp/a_en_pattern
    $ grep -vf /tmp/a_en_pattern lists/a.csv > lists/a-no_en.csv


### filter:sp:a

Write a file named ```lists/a-no_en-no_sp.csv``` that is 
```lists/a-no_en.csv``` without the lines containing words 
of ```downloads/spanish.txt```

**Equivalent shell:**

    $ cat downloads/spanish.txt|grep '^a'| \
    sed -e 's:^\\(.*\\):\\^\\1,:' > /tmp/a_sp_pattern 
    $ grep -vf /tmp/a_sp_pattern lists/a-no_en.csv > lists/a-no_en-no_sp.csv


### ```filter:fr:a```

Write a file named ```lists/a-no_en-no_sp-no_fr.csv``` that is 
```lists/a-no_en-no_sp.csv``` without the lines containing words 
of ```downloads/french.txt```

**Equivalent shell:**

    $ cat downloads/french.txt|grep '^a'| \
    sed -e 's:^\\(.*\\):\\^\\1,:' > /tmp/a_fr_pattern 
    $ grep -vf /tmp/a_fr_pattern lists/a-no_en-no_sp.csv > lists/a-no_en-no_sp-no_fr.csv


    
[androidwords]: https://android.googlesource.com/platform/packages/inputmethods/LatinIME/+/master/dictionaries/
[bip39]: https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki
[forumthread]: https://bitcointalk.org/index.php?topic=1167861.0
[wordlists]: https://github.com/fczuardi/bips/blob/master/bip-0039/bip-0039-wordlists.md
