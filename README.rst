OFX Statement
-------------

.. image:: https://github.com/kedder/ofxstatement/actions/workflows/test.yml/badge.svg?branch=master
    :target: https://github.com/kedder/ofxstatement/actions/workflows/test.yml
.. image:: https://coveralls.io/repos/kedder/ofxstatement/badge.png?branch=master
    :target: https://coveralls.io/r/kedder/ofxstatement?branch=master
.. image:: http://www.mypy-lang.org/static/mypy_badge.svg
    :target: http://mypy-lang.org/
.. image:: https://img.shields.io/badge/code%20style-black-000000.svg
    :target: https://github.com/psf/black

Ofxstatement is a tool to convert proprietary bank statements to OFX format, suitable
for importing into personal accounting systems like `GnuCash`_ or `homebank`_. This
package provides a command line tool to run: ``ofxstatement``. Run ``ofxstatement -h``
for more help.  ``ofxstatement`` works under Python 3 and is not compatible with Python
2.

.. _GnuCash: https://gnucash.org/
.. _homebank: https://www.gethomebank.org/



Rationale
=========

Most internet banking systems are capable of exporting account transactions to
some sort of computer readable format, but few support standard data formats,
like `OFX`_.  On the other hand, personal accounting systems such as `GnuCash`_
support standard formats only, and will probably never support proprietary
statement formats of online banking systems.

To bridge the gap between them, this ofxstatement tool was created.

.. _GnuCash: https://gnucash.org/
.. _OFX: https://en.wikipedia.org/wiki/Open_Financial_Exchange

Mode of Operation
=================

The ``ofxstatement`` tool is intended to be used in the following workflow:

1. At the end of each month, use your online banking service to export
   statements from all of your bank accounts in a format known to
   ofxstatement.

2. Run ``ofxstatement`` on each exported file to convert it to OFX.
   Shell scripts or a Makefile may help to automate this routine.

3. Import the generated OFX file into your personal accounting system.

Installation and Usage
======================

Before using ``ofxstatement``, you have to install a plugin for your bank (or
write your own!). Plugins are installed as regular python packages, with
easy_install or pip, for example::

  $ pip3 install ofxstatement-lithuanian

Note that ofxstatement itself will be installed automatically this way. After
the installation, the ``ofxstatement`` utility will be available.

Users of *Ubuntu* and *Debian* operating systems can install ofxstatement from 
official package repositories::

  $ apt install ofxstatement ofxstatement-plugins 

You can check that ofxstatement is working by running::

  $ ofxstatement list-plugins

You should get a list of your installed plugins.

After installation, the usage is simple::

  $ ofxstatement convert -t <plugin> bank_statement.csv statement.ofx

The resulting ``statement.ofx`` is then ready to be imported into a personal
accounting system.

Known Plugins
=============

There are several user-developed plugins available:

================================= ============================================
Plugin                            Description
================================= ============================================
`ofxstatement-mt940`_             Swift MT940 statements
`ofxstatement-iso20022`_          Generic ISO-20022 format
`ofxstatement-paypal-2`_          PayPal, ``*.csv`` for private accounts
`ofxstatement-transferwise`_      Transferwise CSV (international)
`ofxstatement-nordigen`_          Plugin for GoCardless and Nordigen data parsing

`ofxstatement-lithuanian`_        Plugins for several banks, operating in
                                  Lithuania: Swedbank, Danske and common Lithuanian exchange format - LITAS-ESIS.

`ofxstatement-czech`_             Plugin for Poštovní spořitelna
                                  (``maxibps``) and banks using GPC
                                  format (e.g., FIO banka, module
                                  ``gpc``).

`ofxstatement-airbankcz`_         Plugin for Air Bank a.s. (Czech Republic)
`ofxstatement-raiffeisencz`_      Plugin for Raiffeisenbank a.s. (Czech Republic)
`ofxstatement-skippaycz`_         Plugin for Skip Pay s.r.o. (Czech Republic)
`ofxstatement-unicreditcz`_       Plugin for UniCredit Bank Czech Republic and Slovakia
`ofxstatement-equabankcz`_        Plugin for Equa Bank a.s. (Czech Republic)
`ofxstatement-cz-komercni`_       Komerční banka (Czech Republic)
`ofxstatement-mbankcz`_           mBank S.A. (Czech Republic)
`ofxstatement-partnersbankacz`_   Partners Banka, a.s. (Czech Republic)
`ofxstatement-otp`_               Plugin for OTP Bank, operating in Hungary
`ofxstatement-bubbas`_            Set of plugins, developed by @bubbas:
                                  ``dkb_cc`` and ``lbbamazon``.

`banking.statements.osuuspankki`_ Finnish Osuuspankki bank
`banking.statements.nordea`_      Nordea bank (at least Finnish branch of it)
`ofxstatement-seb`_               SEB (Sweden), it parses Export.xlsx for private accounts
`ofxstatement-lansforsakringar`_  Länsförsäkringar (Sweden), it parses Kontoutdrag.xls for private accounts

`ofxstatement-be-belfius`_        Belfius (Belgium)
`ofxstatement-be-keytrade`_       KeytradeBank (Belgium)
`ofxstatement-be-ing`_            ING (Belgium)
`ofxstatement-be-kbc`_            KBC (Belgium)
`ofxstatement-be-argenta`_        Argenta (Belgium) (by @woutbr)
`ofxstatement-be-argenta-nick`_   Argenta (Belgium, also supports french version) (fork by @Nick-DT)
`ofxstatement-be-crelan`_         Crelan (Belgium)
`ofxstatement-be-triodos`_        Belgian Triodos Bank CSV statements
`ofxstatement-be-newb`_           Belgian cooperative bank newB
`ofxstatement-be-vdk-fr`_         VDK (Belgium, French statements)
`ofxstatement-be-hellobank-fr`_   Hello Bank (Belgium, French statements)

`ofxstatement-germany`_           Plugin for several german banks (1822direkt and Postbank at the moment)
`ofxstatement-dab`_               DAB Bank (Germany)
`ofxstatement-consors`_           Consorsbank (Germany)
`ofxstatement-de-triodos`_        German Triodos Bank CSV statements (also works for GLS Bank)
`ofxstatement-sp-freiburg`_       Sparkasse Freiburg-Nördlicher Breisgau (Germany)
`ofxstatement-de-ing`_            Ing Diba Bank (Germany)
`ofxstatement-mastercard-de`_     Mastercard PDF statements (Germany)
`ofxstatement-sparkasse-de`_      Sparkasse PDF statements (Germany)
`ofxstatement-austrian`_          Plugins for several banks, operating in Austria:
                                  Easybank, ING-Diba, Livebank, Raiffeisenbank.
`ofxstatement-postfinance`_       Swiss PostFinance (E-Finance Java text based bank/credit statements).

`ofxstatement-fineco`_            FinecoBank (Italy)
`ofxstatement-intesasp`_          Intesa San Paolo xlsx balance file (Italy)
`ofxstatement-chebanca`_          CheBanca! xlsx format (Italy)
`ofxstatement-n26`_               N26 Bank (Italy)
`ofxstatement-it-banks`_          Widiba and Webank (Italy)
`ofxstatement-bancoposta`_        BancoPosta - Poste Italiane (Italy)
`ofxstatement-hype`_              Hype - Banca Sella (Italy)

`ofxstatement-betterment`_        Betterment (USA)
`ofxstatement-simple`_            Simple (USA, defunct) JSON financial statement format

`ofxstatement-mbank-sk`_          MBank.sk (Slovakia)
`ofxstatement-latvian`_           Latvian banks
`ofxstatement-ee-seb`_            SEB (Estonia), parses proprietary csv file
`ofxstatement-ee-swedbank`_       Swedbank (Estonia), parses proprietary csv file
`ofxstatement-polish`_            Support for some Polish banks and financial institutions
`ofxstatement-russian`_           Support for several Russian banks: Avangard, AlfaBank, Tinkoff, SberBank (both debit and csv), VTB.
`ofxstatement-is-arionbanki`_     Arion bank (Iceland)
`ofxstatement-revolut`_           Revolut Mastercard
`ofxstatement-al_bank`_           Arbejdernes Landsbank (Denmark)
`ofxstatement-cd-tmb`_            Trust Merchant Bank (DRC)
`ofxstatement-zm-stanbic`_        Stanbic Bank (Zambia)
`ofxstatement-dutch`_             Dutch financial institutes like ICSCards and ING
`ofxstatement-french`_            French financial institutes like BanquePopulaire
`ofxstatement-schwab-json`_       Charles Schwab investment history JSON export
`ofxstatement-bbva`_              BBVA (Spain)
`ofxstatement-qif`_               Converts Quicken Interchange Format (QIF) formatted bank transaction files
`ofxstatement-santander`_         Converts Santander transaction files
`ofxstatement-santander-es`_      Converts Santander ES transaction files
================================= ============================================


.. _ofxstatement-lithuanian: https://github.com/kedder/ofxstatement-lithuanian
.. _ofxstatement-czech: https://gitlab.com/mcepl/ofxstatement-czech
.. _ofxstatement-airbankcz: https://github.com/milankni/ofxstatement-airbankcz
.. _ofxstatement-raiffeisencz: https://github.com/milankni/ofxstatement-raiffeisencz
.. _ofxstatement-skippaycz: https://github.com/archont00/ofxstatement-skippaycz
.. _ofxstatement-unicreditcz: https://github.com/milankni/ofxstatement-unicreditcz
.. _ofxstatement-equabankcz: https://github.com/kosciCZ/ofxstatement-equabankcz
.. _ofxstatement-mbankcz: https://github.com/SinyaWeo/ofxstatement-mbankcz
.. _ofxstatement-partnersbankacz: https://github.com/archont00/ofxstatement-partnersbankacz
.. _ofxstatement-otp: https://github.com/abesto/ofxstatement-otp
.. _ofxstatement-bubbas: https://github.com/bubbas/ofxstatement-bubbas
.. _banking.statements.osuuspankki: https://github.com/koodaamo/banking.statements.osuuspankki
.. _banking.statements.nordea: https://github.com/koodaamo/banking.statements.nordea
.. _ofxstatement-germany: https://github.com/MirkoDziadzka/ofxstatement-germany
.. _ofxstatement-austrian: https://github.com/nblock/ofxstatement-austrian
.. _ofxstatement-postfinance: https://pypi.python.org/pypi/ofxstatement-postfinance
.. _ofxstatement-mbank-sk: https://github.com/epitheton/ofxstatement-mbank-sk
.. _ofxstatement-be-belfius: https://github.com/renardeau/ofxstatement-be-belfius
.. _ofxstatement-be-keytrade: https://github.com/Scotchy49/ofxstatement-be-keytrade
.. _ofxstatement-be-ing: https://github.com/jbbandos/ofxstatement-be-ing
.. _ofxstatement-be-kbc: https://github.com/plenaerts/ofxstatement-be-kbc
.. _ofxstatement-be-argenta: https://github.com/woutbr/ofxstatement-be-argenta
.. _ofxstatement-be-argenta-nick: https://github.com/Nick-DT/ofxstatement-be-argenta
.. _ofxstatement-be-crelan: https://gitlab.com/MagnificentMoustache/ofxstatement-be.crelan
.. _ofxstatement-be-newb: https://github.com/SDaron/ofxstatement-be-newb
.. _ofxstatement-betterment: https://github.com/cmayes/ofxstatement-betterment
.. _ofxstatement-simple: https://github.com/cmayes/ofxstatement-simple
.. _ofxstatement-latvian: https://github.com/gintsmurans/ofxstatement-latvian
.. _ofxstatement-iso20022: https://github.com/kedder/ofxstatement-iso20022
.. _ofxstatement-seb: https://github.com/gerasiov/ofxstatement-seb
.. _ofxstatement-paypal-2: https://github.com/Alfystar/ofxstatement-paypal-2
.. _ofxstatement-polish: https://github.com/yay6/ofxstatement-polish
.. _ofxstatement-russian: https://github.com/gerasiov/ofxstatement-russian
.. _ofxstatement-dab: https://github.com/JohannesKlug/ofxstatement-dab
.. _ofxstatement-consors: https://github.com/JohannesKlug/ofxstatement-consors
.. _ofxstatement-is-arionbanki: https://github.com/Dagur/ofxstatement-is-arionbanki
.. _ofxstatement-be-triodos: https://github.com/renardeau/ofxstatement-be-triodos
.. _ofxstatement-de-triodos: https://github.com/pianoslum/ofxstatement-de-triodos
.. _ofxstatement-lansforsakringar: https://github.com/lbschenkel/ofxstatement-lansforsakringar
.. _ofxstatement-revolut: https://github.com/mlaitinen/ofxstatement-revolut
.. _ofxstatement-transferwise: https://github.com/kedder/ofxstatement-transferwise
.. _ofxstatement-n26: https://github.com/3v1n0/ofxstatement-n26
.. _ofxstatement-sp-freiburg: https://github.com/omarkohl/ofxstatement-sparkasse-freiburg
.. _ofxstatement-al_bank: https://github.com/lbschenkel/ofxstatement-al_bank
.. _ofxstatement-fineco: https://github.com/frankIT/ofxstatement-fineco
.. _ofxstatement-intesasp: https://github.com/Jacotsu/ofxstatement-intesasp
.. _ofxstatement-de-ing: https://github.com/fabolhak/ofxstatement-de-ing
.. _ofxstatement-germany: https://github.com/MirkoDziadzka/ofxstatement-germany
.. _ofxstatement-cz-komercni: https://github.com/medovina/ofxstatement-cz-komercni
.. _ofxstatement-cd-tmb: https://github.com/BIZ4Africa/ofxstatement-cd-tmb
.. _ofxstatement-zm-stanbic: https://github.com/BIZ4Africa/ofxstatement-zm-stanbic
.. _ofxstatement-dutch: https://github.com/gpaulissen/ofxstatement-dutch
.. _ofxstatement-french: https://github.com/gpaulissen/ofxstatement-french
.. _ofxstatement-mt940: https://github.com/gpaulissen/ofxstatement-mt940
.. _ofxstatement-it-banks: https://github.com/ecorini/ofxstatement-it-banks
.. _ofxstatement-ee-seb: https://github.com/rsi2m/ofxstatement-ee-seb
.. _ofxstatement-ee-swedbank: https://github.com/rsi2m/ofxstatement-ee-swedbank
.. _ofxstatement-chebanca: https://github.com/3v1n0/ofxstatement-chebanca
.. _ofxstatement-mastercard-de: https://github.com/FliegendeWurst/ofxstatement-mastercard-de
.. _ofxstatement-sparkasse-de: https://github.com/FliegendeWurst/ofxstatement-sparkasse-de
.. _ofxstatement-bancoposta: https://github.com/lorenzogiudici5/ofxstatement-bancoposta
.. _ofxstatement-hype: https://github.com/lorenzogiudici5/ofxstatement-hype
.. _ofxstatement-schwab-json: https://github.com/edwagner/ofxstatement-schwab-json
.. _ofxstatement-bbva: https://github.com/3v1n0/ofxstatement-bbva
.. _ofxstatement-qif: https://github.com/robvadai/ofxstatement-qif
.. _ofxstatement-santander: https://github.com/robvadai/ofxstatement-santander
.. _ofxstatement-santander-es: https://github.com/M3lo00/ofxstatement-santander-es
.. _ofxstatement-be-vdk-fr: https://github.com/Jibuus/ofxstatement-be-vdk-fr
.. _ofxstatement-be-hellobank-fr: https://github.com/Jibuus/ofxstatement-be-hellobank-fr
.. _ofxstatement-nordigen: https://github.com/jstammers/ofxstatement-nordigen

Advanced Configuration
======================

While ofxstatement can be used without any configuration, some plugins may
accept additional configuration parameters. These parameters can be specified
in a configuration file. The configuration file can be edited using the ``edit-config``
command that opens your favorite editor (defined by environment variable
EDITOR or else the default for your platform) with the configuration file::

  $ ofxstatement edit-config

The configuration file format is in the standard .ini format. The
configuration is divided into sections that correspond to the ``--type``
command line parameter. Each section must provide a ``plugin`` option that
points to one of the registered conversion plugins. Other parameters are
plugin specific.

A sample configuration file::

    [swedbank]
    plugin = swedbank

    [danske:usd]
    plugin = litas-esis
    charset = cp1257
    currency = USD
    account = LT123456789012345678


Such a configuration will let ofxstatement know about two statement file
formats handled by the plugins ``swedbank`` and ``litas-esis``. The ``litas-esis``
plugin will load statements using the ``cp1257`` charset and set a custom currency
and account number. This way, GnuCash will automatically associate the
generated .ofx file with a particular GnuCash account.

To convert the proprietary CSV file ``danske.csv`` into the OFX file ``danske.ofx``, run::

    $ ofxstatement -t danske:usd danske.csv danske.ofx

Note that configuration parameters are plugin specific. See the plugin
documentation for more info.

To use a custom configuration file, pass the ``-c`` / ``--config`` option::

    $ ofxstatement convert -t pluginname -c /path/to/myconfig.ini input.csv output.ofx


Development / Testing
=====================

``ofxstatemnt`` uses `pipenv`_ to manage the development environment and
dependencies::

  $ pip install pipenv
  $ git clone https://github.com/<your_account>/ofxstatement.git
  $ cd ofxstatement
  $ pipenv sync --dev

.. _pipenv: https://github.com/pypa/pipenv

And finally run the test suite::

  $ pipenv shell
  $ pytest

When satisfied, you may create a pull request.

Writing your own Plugin
=======================

If the plugin for your bank has not been developed yet (see `Known plugins`_
section above) you can easily write your own, provided you have some knowledge
about the Python programming language. There is an `ofxstatement-sample`_
plugin project available that provides sample boilerplate and describes the
plugin development process in detail.

.. _ofxstatement-sample: https://github.com/kedder/ofxstatement-sample
