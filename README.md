# Helios Translation

This repository provides a translation template to ease the process of
translating _helios-server_ from Helios Voting (https://heliosvoting.org/)
to languages other than the default English. An example translation is also
provided (in Brazilian Portuguese).

## How do I use all this?
   You can download the _helios-server_ code found in here, install it
   according to original instructions and apply the translation patch
   found in here. It is not guaranteed this will work with newer versions
   cloned directly from the original upstream (actually, it probably won't;
   see further below why).

   The application of the translation patch already _tags_ (hopefully) all
   strings in the _helios-server_ code and provides an example Brazilian
   Portuguese translation under _helios-server/locale/pt_BR/_. Translations
   in other languages can be provided by creating a directory
   _<lang>/LC_MESSAGES/_ under _helios-server/locale/_, copying the _django.po_
   template from this repository in there and filling the translations inside
   the copied _django.po_.

   For example, to include an Helios translation to Dutch (nl), create
   directory _helios-server/locale/nl/LC_MESSAGES/_ and copy the _django.po_
   template in there. In code (via a console of your choice and from inside
   the _helios-server_ root):

   ```console
   # mkdir -p locale/nl/LC_MESSAGES/
   # cp /path/to/template/django.po locale/nl/LC_MESSAGES/django.po
   ```

   Now open the _django.po_ template using your favourite text editor and
   fill in all **msgstr** empty strings with text translations that
   correspond to the accompanying **msgid** English versions.

   For a reference on how the translations in the django.po file can be filled,
   the pt_BR/django.po file in this repository can be consulted.


## More Information

### The Good
   Helios is quite easy to translate! It uses the Django framework, which
   already provides facilities that ease the effort of translating Helios
   to pretty much any language. The strings in Helios can be easily tagged
   n code for translation. They are then united in a single file called
   _django.po_, inside which you can specify the individual string
   translations to the language of your preference.

### The Bad
   First of all, in order for any of this to work, every single string in
   Helios' code that is presented to the user needs to be _tagged_ in every
   single place it occurs. Unfortunately, the _helios-server_ raw code does
   not do that by default, meaning **you** will have to search the whole
   _helios-server_ code for every single occurrence of every single string
   that might be presented to the user and **tag** them in accordance to the
   Django translation specifications that can be found
   [here](https://docs.djangoproject.com/en/3.0/topics/i18n/translation/).
   Furthermore, after tagging all of those strings, you will still have to
   translate them, and there are many, many different strings (about 400!).

### The Ugly
   Helios' code also contains static files that are not touched by Django's
   translation machinery. This means those files cannot be translated via
   a django.po file; they need, instead, an individual and manual translation
   effort.

### Some good news
   In order to ease the translation effort, a translation patch is provided
   here. Ideally, it already tags automatically the majority of the strings
   of the code. Additionaly, the patch already creates an example Brazilian
   Portuguese translation inside the _helios_server/locale_ directory. This
   can be uses as a practical example of how to create one's own translation
   to one's language of preference.

### And some more bad news
   The main problem with this patching approach is: since, for some reason,
   the original English text strings are dispersed throughout everywhere in
   the code, every single time the original Helios code is modified upstream
   there is a major change that the translation patch will be broken. And a
   broken translation patch will fail to fully tag the text strings. That
   makes it somewhat difficult to keep this effort in sync with
   

## Notes on translation

### General translation
    The default translation process is rather simple: simply edit the _msgstr_
    line in the template with a translation string that corresponds to the
    accompanying _msgid_ string. Example:

    ```console
    msgid "Original string"
    msgstr "My translation"
    ```

### Translating long strings
    Upon inspecting the django.po file you will notice that some of the strings
    in there are quite long. If your translation string needs to span multiple
    lines then you will need to make a **multiline translation**. A multiline
    translation leaves the _msgstr_ line with an empty string and fills the
    translation in the following lines below until it is done, making sure
    every line is a single string. The Django translation mechanism then takes
    care of joining those lines in a single string.

    An example will make it clear:

    ```console
    msgid ""
    "This is a somewhat long string that spans multiple lines. I will most "
    "probably need to translate this into the language of my preference by "
    "using a multiline translation."
    msgstr ""
    "Here goes the translation of the above string into the language of my "
    "preference. Notice that the 'msgstr' line contains an empty string and "
    "also notice that every line is a separate string of its own."
   ```

   In principle there is abolutely no compatibility problem if the original
   string is short and your translation is long (do try to keep it somewhat
   compatible in size, though; this avoids problems with possibly poor GUI
   decisions or limitations):

   ```console
   msgid "A short string"
   msgstr ""
   "For some reason the short string above translates to something quite big "
   "in the language of my preference. That is not a problem: all I have to do "
   "is provide a multiline translation."
   ```

   Finally, do notice that each string in a multiline translation ends with a
   space character. Remember that Django will join all the single strings into
   one big string in the end: without the space, the final word in a line string
   will be concatenated to the word in the beginning of the following line. As
   an alternative, instead of ending each string with a space one may opt to
   **start** each string with a space: the final result will be the same. Just
   make sure words are not concatenated senselessly to each other.
   

### Strings with formatting
    Some strings inside the django.po file will contain formatting information,
    such as new lines or tags as <li>, <b>, <u> etc. For such strings, try to
    keep the same formatting (or as close as possible) in the translation you
    provide so as to keep the same visual as what is seen by the user in the
    default English language. Examples may be found in the Brazilian Portuguese
    version provided here.


### String contexts
    Some strings might be accompanied of an additional attribute by the name
    of _msgctxt_. This specifies the **context** in which the string is used
    in the code. It is an attempt to give the translator hints on what the
    string being translated is referring to. That being said, some of the
    contextual markers might not make sense in every language. As an example,
    some languages, like Brazilian Portuguese, need to flex adjectives
    according to gender, which is what the "masculine" and "feminine" contexts
    try to establish. In languages that do not flex according to gender, this
    contextual marker makes no sense.