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

    
[androidwords]: https://android.googlesource.com/platform/packages/inputmethods/LatinIME/+/master/dictionaries/
[bip39]: https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki
[forumthread]: https://bitcointalk.org/index.php?topic=1167861.0
[wordlists]: https://github.com/fczuardi/bips/blob/master/bip-0039/bip-0039-wordlists.md