# Translate Shell

[![Build Status](https://travis-ci.org/soimort/translate-shell.png)](https://travis-ci.org/soimort/translate-shell)
[![Version](https://raw.githubusercontent.com/soimort/translate-shell/gh-pages/images/badge-release.png)](https://github.com/soimort/translate-shell/releases)
[![Download](https://raw.githubusercontent.com/soimort/translate-shell/gh-pages/images/badge-download.png)](http://www.soimort.org/translate-shell/trans)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/soimort/translate-shell?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[Translate Shell](http://www.soimort.org/translate-shell) (previously _Google Translate CLI_) is a command-line interface and interactive shell for [Google Translate](https://translate.google.com/). It works just the way you want it to be.

```
$ trans 'Saluton, Mondo!'
Saluton, Mondo!

Hello, World!

Translations of Saluton, Mondo!
[ Esperanto -> English ]
Saluton ,
    Hello,
Mondo !
    World!
```

Translations with detailed explanations are shown by default. You can also translate the text briefly, i.e., only the most relevant translation is shown: (this will give you the same result as in [Google Translate CLI Legacy](https://github.com/soimort/translate-shell/tree/legacy))

```
$ trans -brief 'Saluton, Mondo!'
Hello, World!
```

Translations can be done interactively; input your text line by line:

```
$ trans -shell -brief
> Was mich nicht umbringt, macht mich stärker.
What does not kill me makes me stronger.
> Юмор есть остроумие глубокого чувства.
Humor is a deep sense of wit.
> 學而不思則罔，思而不學則殆。
Learning without thought is labor lost, thought without learning is perilous.
> 幸福になるためには、人から愛されるのが一番の近道。
In order to be happy, the shortest way is from being loved by the people.
```

**Translate Shell** is a complete rewrite of [Google Translate CLI Legacy](https://github.com/soimort/translate-shell/tree/legacy), which is a tiny script written in 100 lines of AWK code. Translate Shell is backward compatible with Google Translate CLI Legacy in its command-line usage; furthermore, it provides more functionality including verbose translation, Text-to-Speech and interactive mode, etc.

## Prerequisites

### System Requirements

Any POSIX-compliant system should work, including but not limited to:

* GNU/Linux
* BSD family, including OS X
* illumos family, including SmartOS
* Cygwin on Windows

### Dependencies

* [GNU Awk](https://www.gnu.org/software/gawk/) (gawk) `>=4.0`
    * This program relies heavily on GNU-only extensions of the AWK language, which are not possible with POSIX AWK or other AWK implementations.
    * gawk comes with (almost) all GNU/Linux distributions. On BSD systems, it is available in ports. On OS X, it is available in MacPorts and Homebrew.
    * If you have an older version of gawk (`>=3.1`), you may still want to use [Google Translate CLI Legacy](https://github.com/soimort/translate-shell/tree/legacy).
    * Be aware that msys-gawk shipped with MinGW is outdated; however, you may still use Google Translate CLI Legacy as well.

### Optional Dependencies

* [GNU Bash](http://www.gnu.org/software/bash/) or [Zsh](http://www.zsh.org/)
    * You can use Translate Shell from any modern Unix shell of your choice (bash, zsh, ksh, tcsh, fish, etc.); however, it requires either bash or zsh installed for interpreting the wrapper script.
* [GNU FriBidi](http://fribidi.org/): an implementation of the Unicode Bidirectional Algorithm (bidi)
    * needed for displaying right-to-left (RTL) languages
* [MPlayer](http://www.mplayerhq.hu/), [mplayer2](http://www.mplayer2.org/), [mpv](http://mpv.io/), [mpg123](http://mpg123.org/), or [eSpeak](http://espeak.sourceforge.net/)
    * needed for the Text-to-Speech functionality
* [rlwrap](http://utopia.knoware.nl/~hlub/uck/rlwrap/#rlwrap): a GNU readline wrapper
    * needed for readline-style editing and history in the interactive mode
* [groff](http://www.gnu.org/software/groff/): GNU troff (pre-installed on most systems)
    * needed for formatting man pages
* [GNU Emacs](http://www.gnu.org/software/emacs/)
    * for using the Emacs interface

### Environment and Encoding

It is strongly recommended that you use UTF-8 codeset for your default locale, as it potentially supports all languages. You can check whether your codeset is UTF-8 using:

    $ locale

And you need to have necessary Unicode fonts installed for the languages you want to display.

## Try It Out

As long as you have `gawk` already, you're good to go!

    $ gawk "$(curl -Ls git.io/translate)" -I

## Installation

### Direct Download (Latest Release)

Download [this executable](http://git.io/trans) and place it into your path.

    $ wget git.io/trans
    $ chmod +x ./trans

Additionally, you may verify [this signature](http://www.soimort.org/translate-shell/trans.sig) if you want:

    $ gpg --verify trans.sig trans

### From Git

    $ git clone https://github.com/soimort/translate-shell
    $ cd translate-shell/
    $ sudo make install

The default `PREFIX` of installation is `/usr/local`. If you wish to install it to another path (e.g. `/usr`, `~/.local`), use:

    $ sudo make PREFIX=/usr install

If you prefer zsh, specify zsh as the build target: (normally you don't need to worry about that if both bash and zsh are installed on your system)

    $ sudo make TARGET=zsh install

### From Your Package Manager

#### OS X (via Homebrew) / Linux (via Linuxbrew)

Available as a self-hosted Homebrew formula:

    $ brew install http://www.soimort.org/translate-shell/translate-shell.rb

On Linux, you may ignore its dependencies (e.g. gawk) if you already have them in your system:

    $ brew install --ignore-dependencies http://www.soimort.org/translate-shell/translate-shell.rb

#### FreeBSD

Available in FreeBSD Ports collection:

    $ cd /usr/ports/textproc/google-translate-cli
    $ make install

#### Debian

Available in Debian Unstable:

    $ apt-get install translate-shell

#### Arch Linux

Available in the [Arch User Repository](https://aur.archlinux.org/packages/translate-shell/):

    $ cower -d translate-shell
    $ cd translate-shell/
    $ makepkg -si

## Examples

### Translate a Word

#### From any language to your language

Google Translate will detect the language of the source text automatically, and Translate Shell will translate it into the language of your locale.

    $ trans vorto

#### From any language to one or more specific languages

Translate a word into French:

    $ trans :fr word

Translate a word into Chinese and Japanese: (use a plus sign "`+`" as the delimiter)

    $ trans :zh+ja word

You can use an equals sign ("`=`") in place of "`:`". The traditional way of using a pair of curly brackets in Google Translate CLI Legacy is still supported: (depending on your shell)

    $ trans {=zh+ja} word

You can also use the `-target` option to specify this:

    $ trans -t zh+ja word

#### From a specific language

Google Translate could identify the language of the source text incorrectly; in that case, you will need to specify the language yourself:

    $ trans :en 手紙
    $ trans ja:en 手紙
    $ trans zh:en 手紙

You can also use the `-source` option to specify this:

    $ trans -s ja -t en 手紙

### Translate Multiple Words or a Phrase

Translate each word alone:

    $ trans en:zh freedom of speech

Put words into one argument and translate them as a whole:

    $ trans en:zh "freedom of speech"

### Translate a Sentence

Translating a sentence is much the same like translating a phrase; you can quote words into one argument:

    $ trans :zh "Words will always retain their power."
    $ trans :zh 'Words will always retain their power.'

To avoid punctuation marks and other special characters (e.g. "`!`") being interpreted by the shell, use *single quotes*:

    $ trans :zh 'Yes we can!'

There are some cases though, you may still want to use *double quotes*: (i.e. the sentence contains a single quotation mark "`'`")

    $ trans :zh "I'm lovin' it! McDonald's"

### Brief Mode

By default, Translate Shell displays translations in a verbose manner. If you prefer to see only the most relevant translation, you can fallback to brief mode using the `-brief` option:

    $ trans :fr -b "Saluton, Mondo"

In brief mode, phonetic notation (for some languages) is not shown by default. To enable this, put an at sign "`@`" in front of the language code:

    $ trans :@ja -b "Saluton, Mondo"

### Text-to-Speech

Use the `-play` option to listen to the translation:

    $ trans :ja -b -p "Saluton, Mondo"

### Right-to-Left (RTL) Languages

You can use the `-width` option to specify the screen width for padding when displaying right-to-left languages, if you want:

    $ trans :he -b -w 40 "Saluton, Mondo"

### Input and Output

When no non-option argument is given, Translate Shell will read from standard input, or from the file specified by the `-input` option:

    $ echo "Saluton, Mondo" | trans :fr -b
    $ trans :fr -b -i input.txt

Translations will be written to standard output, or to the file specified by the `-output` option:

    $ echo "Saluton, Mondo" | trans :fr -b -o output.txt

### Translate a File

Instead of using the `-input` option, a URI scheme of file can be provided as a parameter (`file://` followed by the file name):

    $ trans :fr file://input.txt

Brief mode is used when translating from URI schemes.

### Translate a Web Page

For the translation of a web page, a URI scheme of HTTP(S) can be provided as a parameter:

    $ trans :fr http://www.w3.org/

A browser session will be started for viewing the translation (in Google Translate). To specify the web browser, use the `-browser` option:

    $ trans :fr -browser firefox http://www.w3.org/

### Interactive Mode

Start an interactive shell, using the `-interactive` option:

    $ trans -I

## Text Editors

`trans` is a command-line program which can be easily integrated with your favorite text editor. Below are some useful tips.

### Emacs

#### Interactive shell

You can, of course, use Emacs as a front-end of Translate Shell, in the same way you emulate your favorite shell with <kbd>M-x shell</kbd>. There is a shortcut for starting Emacs with Translate Shell, using the `-emacs` option:

    $ trans -E

#### Text translation

When editing a text file, viewing the translation of a region is just one single command: (translating any language to Japanese, for example)

<kbd>M-| trans :ja</kbd>

#### Emacs mode

There is a simple minor mode for Emacs: [google-translate-mode.el](https://github.com/soimort/translate-shell/raw/develop/google-translate-mode.el)

* <kbd>C-c -</kbd> Show translation of the current word in message buffer.
* <kbd>C-c =</kbd> View verbose translation of the current word in popup dialog.
* <kbd>C-c +</kbd> Insert translation of the current word right after.

### Vim

#### Text translation

Add one line to your `~/.vimrc`:

    set keywordprg=trans\ :ja

Use <kbd>Shift-K</kbd> to view the translation of the word under the cursor.

## Usage

Use `$ trans -H` to view the [detailed man page](http://www.soimort.org/translate-shell/trans.1.html).

```
Usage:  trans [options] [source]:[target] [text] ...
        trans [options] [source]:[target1]+[target2]+... [text] ...

Options:
-V, -version
    Print version and exit.
-H, -h, -help
    Print this help message and exit.
-M, -m, -manual
    Show the manual.
-r, -reference
    Print a list of languages (displayed in endonyms) and their ISO 639 codes for reference, and exit.
-R, -reference-english
    Print a list of languages (displayed in English names) and their ISO 639 codes for reference, and exit.
-verbose
    Verbose mode. (default)
-d, -dictionary
    Dictionary mode.
-b, -brief
    Brief mode.
-show-original [yes|no]
    Show original text or not. (default: yes)
-show-original-phonetics [yes|no]
    Show phonetic notation of original text or not. (default: yes)
-show-translation [yes|no]
    Show translation or not. (default: yes)
-show-translation-phonetics [yes|no]
    Show phonetic notation of translation or not. (default: yes)
-show-prompt-message [yes|no]
    Show prompt message or not. (default: yes)
-show-languages [yes|no]
    Show source and target languages or not. (default: yes)
-show-original-dictionary [yes|no]
    Show dictionary entry of original text or not. (default: no)
-show-dictionary [yes|no]
    Show dictionary entry of translation or not. (default: yes)
-show-alternatives [yes|no]
    Show alternative translations or not. (default: yes)
-no-ansi
    Don't use ANSI escape codes in the translation.
-w [num], -width [num]
    Specify the screen width for padding when displaying right-to-left languages.
-indent [num]
    Specify the size of indent (in terms of spaces). (default: 4)
-v, -view
    View the translation in a terminal pager.
-pager [program]
    Specify the terminal pager to use, and view the translation.
-browser [program]
    Specify the web browser to use.
-p, -play
    Listen to the translation.
-player [program]
    Specify the command-line audio player to use, and listen to the translation.
-x [proxy], -proxy [proxy]
    Use proxy on given port.
-I, -interactive, -shell
    Start an interactive shell, invoking `rlwrap` whenever possible (unless `-no-rlwrap` is specified).
-no-rlwrap
    Don't invoke `rlwrap` when starting an interactive shell with `-I`.
-E, -emacs
    Start an interactive shell within GNU Emacs, invoking `emacs`.
-prompt [prompt_string]
    Customize your prompt string in the interactive shell.
-prompt-color [color_code]
    Customize your prompt color in the interactive shell.
-i [file], -input [file]
    Specify the input file name.
-o [file], -output [file]
    Specify the output file name.
-l [code], -lang [code]
    Specify your own, native language ("home/host language").
-s [code], -source [code]
    Specify the source language (language of the original text).
-t [codes], -target [codes]
    Specify the target language(s) (language(s) of the translated text).

See the man page trans(1) for more information.
```

## Environment Variables

You can export some environment variables as your default configuration. This will save you from typing the same command-line options each time.

* `PAGER`: for option `-pager`
* `BROWSER`: for option `-browser`
* `PLAYER`: for option `-player`
* `HTTP_PROXY` and `http_proxy`: for option `-proxy`
* `TRANS_PS`: for option `-prompt`
* `TRANS_PS_COLOR`: for option `-prompt-color`
* `HOME_LANG`: for option `-l`
* `SOURCE_LANG`: for option `-s`
* `TARGET_LANG`: for option `-t`

Example:

    $ export TARGET_LANG=zh
    $ trans text
    $ trans word

## Language Codes

Use `$ trans -R` to view the list of language codes.

```
┌─────────────────────────────┬──────────────────────┬─────────────────┐
│ Afrikaans           - af    │ Hausa          - ha  │ Persian    - fa │
│ Albanian            - sq    │ Hebrew         - he  │ Polish     - pl │
│ Arabic              - ar    │ Hindi          - hi  │ Portuguese - pt │
│ Armenian            - hy    │ Hmong          - hmn │ Punjabi    - pa │
│ Azerbaijani         - az    │ Hungarian      - hu  │ Romanian   - ro │
│ Basque              - eu    │ Icelandic      - is  │ Russian    - ru │
│ Belarusian          - be    │ Igbo           - ig  │ Serbian    - sr │
│ Bengali             - bn    │ Indonesian     - id  │ Sesotho    - st │
│ Bosnian             - bs    │ Irish          - ga  │ Sinhala    - si │
│ Bulgarian           - bg    │ Italian        - it  │ Slovak     - sk │
│ Catalan             - ca    │ Japanese       - ja  │ Slovenian  - sl │
│ Cebuano             - ceb   │ Javanese       - jv  │ Somali     - so │
│ Chichewa            - ny    │ Kannada        - kn  │ Spanish    - es │
│ Chinese Simplified  - zh-CN │ Kazakh         - kk  │ Sundanese  - su │
│ Chinese Traditional - zh-TW │ Khmer          - km  │ Swahili    - sw │
│ Croatian            - hr    │ Korean         - ko  │ Swedish    - sv │
│ Czech               - cs    │ Lao            - lo  │ Tajik      - tg │
│ Danish              - da    │ Latin          - la  │ Tamil      - ta │
│ Dutch               - nl    │ Latvian        - lv  │ Telugu     - te │
│ English             - en    │ Lithuanian     - lt  │ Thai       - th │
│ Esperanto           - eo    │ Macedonian     - mk  │ Turkish    - tr │
│ Estonian            - et    │ Malagasy       - mg  │ Ukrainian  - uk │
│ Filipino            - tl    │ Malay          - ms  │ Urdu       - ur │
│ Finnish             - fi    │ Malayalam      - ml  │ Uzbek      - uz │
│ French              - fr    │ Maltese        - mt  │ Vietnamese - vi │
│ Galician            - gl    │ Maori          - mi  │ Welsh      - cy │
│ Georgian            - ka    │ Marathi        - mr  │ Yiddish    - yi │
│ German              - de    │ Mongolian      - mn  │ Yoruba     - yo │
│ Greek               - el    │ Myanmar        - my  │ Zulu       - zu │
│ Gujarati            - gu    │ Nepali         - ne  │                 │
│ Haitian Creole      - ht    │ Norwegian      - no  │                 │
└─────────────────────────────┴──────────────────────┴─────────────────┘
```

## FAQ

* **Q**: *How many languages does Google Translate support?*

* **A**: 90 languages, as far as we know. There are 91 distinct language codes in total (including two codes for the Chinese language). A few aliases of these codes exist.

* **Q**: *What are these language codes?*

* **A**: A language code is used to identify a language. Most of these codes are ISO 639-1 codes (alpha-2), consisted of two alphabet letters; very few languages supported by Google Translate use the ISO 639-2 code (alpha-3), e.g., Hmong (`hmn`); the Chinese language has two language codes, distinguished by the uppercase region code after its ISO 639-1 code.

* **Q**: *Why are there two codes for Chinese? When to use them?*

* **A**: Two writing systems exist for the Chinese language: Simplified Chinese (`zh-CN`), used in China and Singapore; and Traditional Chinese (`zh-TW`), used in Taiwan and Hong Kong. Language code "`zh`" is an alias of "`zh-CN`".

* **Q**: *What about writing systems? What is the writing system of my language?*

* **A**: Some languages other than Chinese, also have multiple writing systems. Unfortunately, Google Translate seems to support only one of them:
    * Belarusian (`be`): Cyrillic alphabet
    * Bosnian (`bs`): Latin alphabet
    * Hausa (`ha`): Latin / Boko alphabet
    * Javanese (`jv` or `jw`): Latin alphabet
    * Kazakh (`kk`): Cyrillic alphabet
    * Mongolian (`mn`): Cyrillic alphabet
    * Punjabi (`pa`): Brahmic / Gurmukhī alphabet
    * Serbian (`sr`): Cyrillic alphabet
    * Sundanese (`su`): Latin alphabet
    * Tajik (`tg`): Cyrillic alphabet
    * Uzbek (`uz`): Latin alphabet

* **Q**: *What are right-to-left (RTL) languages, and why do I need GNU FriBidi for them?*

* **A**: 5 languages supported by Google Translate are written from right to left. In order to display bi-directional text correctly for these languages, GNU FriBidi, which implements Unicode bidirectional algorithm, will be used:
    * Arabic (`ar`)
    * Persian (`fa`)
    * Hebrew (`he` or `iw`)
    * Urdu (`ur`)
    * Yiddish (`yi` or `ji`)

* **Q**: *What is my "home" language?*

* **A**: By default, home language is set to the language of your current locale (i.e. environment variable `LC_CTYPE` and `LANG`). It will affect the display in verbose mode. If you are comfortable with your current locale (e.g. `en_US.UTF-8`), just leave it alone.

* **Q**: *What is the target language, if I don't specify one?*

* **A**: If no target language is specified, text will be translated into the language of your locale, i.e., your most preferable language.

* **Q**: *I tried to translate some very long text but failed to get the complete translation.*

* **A**: There is a limit of length for each translation. As a general suggestion, don't try to do everything in one hit. Split your text into sentences or lines and translate one at a time.

* **Q**: *My terminal does not support ANSI escape sequences and the display looks like a mess. How do I disable them?*

* **A**: Translate Shell uses ANSI escape sequences to display colors and other effects. You can disable them by either using the option `-no-ansi`, or telling Translate Shell that your terminal type is dumb via the environment variable `TERM`: `$ TERM=dumb trans ...`

## Reporting Bugs

**Please review the [guidelines for contributing](https://github.com/soimort/translate-shell/blob/stable/CONTRIBUTING.md) before reporting an issue.**

## Contributing

**Please review the [guidelines for contributing](https://github.com/soimort/translate-shell/blob/stable/CONTRIBUTING.md) before sending a pull request.**

## Licensing

This is free and unencumbered software released into the public domain.
