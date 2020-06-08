**This fork is based on stephanlensky's fork of redsudo's fork of mreweilk's fork of svbnet's Redsea where I've disabled a bunch of features to allow for Dolby Atmos downloading and downloading other song types is untested but will probably still work (not including MQA, that will not work). Thank you to 112madgamer for doing the [research](https://github.com/evan-goode/mania/issues/5#issuecomment-629750935)!**

To get this to work you MUST set your own auth header retrieved from MITM'ing a Tidal APK that has its target version changed, which is included. To do this you must install Fiddler-Everywhere on any computer and follow [this guide](https://www.telerik.com/blogs/how-to-capture-android-traffic-with-fiddler). Note that the guide is for the old version of Fiddler, so the placement of the options will be different, and so will the port. Once you have done that, play a song on tidal, select any api.tidal.com entry, and copy the text next to 'X-Tidal-SessionId:' and 'Authorization:' in the header tab on the right side, and copy it into the config file.

RedSea
======
Music downloader and tagger for Tidal. For educational use only, and may break in the future.

Current state
-------------
RedSea is currently being worked on by members of RED. Reach out to RedSudo for more info

Introduction
------------
RedSea is a music downloader and tagger for the Tidal music streaming service. It is designed partially as a Tidal API example.
API implementation - it is contained in `config/tidal_api.py` and only requires `requests` to be
installed. Note that you will you have to implement the Tidal lossless download hack yourself -- you can find this in `mediadownloader.py`.

Requirements
------------
* Python 3.5
* requests
* mutagen
* pycrypto

Setting up (with pip)
------------------------
1. Run `pip install -r requirements.txt` to install dependencies
2. Run `python redsea.py -h` to view the help file
3. Run `python redsea.py urls` to download lossless files from urls

Setting up (with Pipenv)
------------------------
1. Run `pipenv install --three` to install dependencies in a virtual env using Pipenv
2. Run `pipenv run python redsea.py -h` to view the help file
3. Run `pipenv run python redsea.py urls` to download lossless files from urls

How to use
----------
    usage: redsea.py [-h] [-p PRESET] [-a ACCOUNT] [-s] urls [urls ...]

    A music downloader for Tidal.

    positional arguments:
    urls                    The URLs to download. You may need to wrap the URLs in
                            double quotes if you have issues downloading.

    optional arguments:
    -h, --help              show this help message and exit
    -p PRESET, --preset PRESET
                            Select a download preset. Defaults to Lossless only.
                            See /config/settings.py for presets
    -a ACCOUNT, --account ACCOUNT
                            Select a session/account to use. Defaults to
                            the "default" session. If it does not exist, you
                            will be prompted to create one
    -s, --skip            Pass this flag to skip track and continue when a track
                            does not meet the requested quality

Tidal issues
------------
* Sometimes, tracks will be tagged with a useless version (for instance, "(album version)"), or have the same version twice "(album version)(album version)". This is because tracks in
    Tidal are not consistent in terms of metadata - sometimes a version may be included in the track title, included in the version field, or both.
    
* Tracks may be tagged with an inaccurate release year; this may be because of Tidal only having the "rerelease" or "remastered" version but showing it as the original.

TODO
----
* Filename sanitisation is overzealous
* Playlists are treated like albums

Config reference
----------------

`BRUTEFORCEREGION`: When True, redsea will iterate through every available account and attempt to download when the default or specified session fails to download the release

### `Stock Presets`

`default`: FLAC 44.1k / 16bit only

`best_available`: Download the highest available quality (MQA > FLAC > 320 > 96)

`mqa_flac`: Accept both MQA 24bit and FLAC 16bit

`MQA`: Only allow FLAC 44.1k / 24bit (includes 'folded' 96k content)

`FLAC`: FLAC 44.1k / 16bit only

`320`: AAC ~320 VBR only

`96`: AAC ~96 VBR only


### `Preset Configuration Variables`

`keep_cover_jpg`: Whether to keep the cover.jpg file in the album directory

`embed_album_art`: Whether to embed album art or not into the file.

`save_album_json`: save the album metadata as a json file

`tries`: How many times to attempt to get a valid stream URL.

`path`: Base download directory

Format variables are `{title}`, `{artist}`, `{album}`, `{tracknumber}`.

`track_format`: How tracks are formatted. The relevant extension is appended to the end.

`album_format`: Base album directory - tracks and cover art are stored here. May have slashes in it, for instance {artist}/{album}.
