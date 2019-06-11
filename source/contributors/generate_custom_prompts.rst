.. _generate_custom_prompts:

*************************
Generate your own prompts
*************************

If you want your Wazo to speak in your language that is not supported by Wazo, and you don't want to
record the whole package of sounds in a studio, you may generate them yourself with some
text-to-speech services.

The following procedure will generate prompts for ``pt_BR`` (portuguese from Brazil) based on the
Google TTS service.

.. note:: There are two sets of prompts: the `Asterisk prompts`_ and the Wazo prompts. This procedure
          only covers the Wazo prompts, but it may be adapted for Asterisk prompts.

.. _Asterisk prompts: http://www.asterisksounds.org/en

#. Create an account on Transifex and join the team of translation of Wazo.

#. Translate the prompts in the wazo-prompt resource.

#. Go to https://www.transifex.com/wazo/wazo/wazo-prompt/pt_BR/download/for_use/ and
   download the file on your Wazo. You should have a file named like
   ``for_use_wazo_wazo-prompt_pt_BR.ini``.

#. On your Wazo, download the tool to automate the use of Google TTS::

    wget https://github.com/zaf/asterisk-googletts/raw/master/cli/googletts-cli.pl
    chmod +x googletts-cli.pl

#. Then run the following script to generate the sound files (set ``LANGUAGE`` and ``COUNTRY`` to
   your own language)::

    LANGUAGE=pt
    COUNTRY=BR
    mkdir -p wav/{digits,letters}
    cat for_use_wazo_wazo-prompt_${LANGUAGE}_${COUNTRY}.ini | while IFS='=' read file text ; do
      echo $file
      ./googletts-cli.pl -t "$text" -l ${LANGUAGE}-${COUNTRY} -s 1.4 -r 8000 -o wav/$file.wav
    done

#. Install the prompts on your system::

    mv wav /usr/share/asterisk/sounds/${LANGUAGE}_${COUNTRY}

Note that this last modification may be erased after running ``wazo-upgrade``.

And that's it, you can configure a user to use your new language and he will hear the prompts in
your language. You may also want to use the :ref:`wazo-confd HTTP API <rest-api>` to mass-update
your users.
