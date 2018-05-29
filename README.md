# Python3 adaptation for scripts from lasttolibre project
<!-- vim-markdown-toc GFM -->
* [Instructions](#instructions)
  * [Script Overview](#script-overview)
  * [Using lastexport](#using-lastexport)
    * [Additional options](#additional-options)
  * [Using libreimport (scrobble import)](#using-libreimport-scrobble-import)
  * [Using libreimport2 (loved/banned import)](#using-libreimport2-lovedbanned-import)

<!-- vim-markdown-toc -->
Cloned from here: https://gitlab.com/aucampia/fmthings-lasttolibre/

Original location: https://gitorious.org/fmthings/lasttolibre/blobs/raw/master/lastexport.py (unavailable on the day of writing)

**I've actually fully updated and tested only `lastexport.py`. For other's I've just run 2to3 and haven't actually tested them.** So be carefull with them. Reports on their status or pull requests are welcome.

# Instructions

How to use the lastexport and libreimport scripts to create a dump of your last.fm tracks and importing them to libre.fm

## Script Overview

[lastexport.py](./lastexport.py) - Used for exporting your listening history (also loved/banned tracks) from last.fm or libre.fm to a text file.

[libreimport.py](./libreimport.py) and [scrobble.py](./scrobble.py) - Used for importing your tracks from a text file (created with lastexport.py) to libre.fm or any other service using GNU FM software.

[libreimport2.py](./libreimport2.py) - Used for importing loved/banned tracks to libre.fm or any other service using GNU FM software

If the scripts are not already executable, you might want to run:

```sh
cd /path/to/lasttolibre/
chmod +x *.py
```

## Using lastexport

You will need to verify that your track history and real-time listening data are both publicly available in your [Last.fm Privacy Settings](https://www.last.fm/settings/privacy)

To export all your tracks from last.fm, run:

```sh
./lastexport.py --user your_lastfm_username
```

The tracks will be exported to `exported_tracks.txt` by default, where each line represents a track with the following entries (if they exist), separated by a tab:

```
date    trackname    artistname    albumname    trackmbid    artistmbid    albummbid
```

That is usually all you need to know.

### Additional options

`lastexport.py` also recognizes the options `--page`, `--outfile` and `--server`.

`--page` lets you choose which page to start on.

Let's say you have a total of 300 pages of tracks but `lastexport.py` fails to download page 123, `lastexport.py` will then save the 122 pages you already have and quit.

You can then run `lastexport.py` again with:

```sh
./lastexport.py -u your_lastfm_username --page 123
```

And it will continue downloading tracks starting at page 123 and save them in the same file as the other 122 pages of tracks.

`--outfile` lets you specify a file name where the exported tracks will be saved,
instead of the default `exported_tracks.txt`:

```sh
./lastexport.py -u your_lastfm_username --outfile mytracks.txt
```

`--server` lets you specify which server to export track info from, last.fm is the default but you can also export from [libre.fm](http://libre.fm) or any other gnu.fm server.

```sh
./lastexport -u your_librefm_username -s libre.fm
```

or

```sh
./lastexport -u your_gnufm_username -s myowngnufmserver.net
```

`--type` lets you specify which type of data you want to export, scrobbles is the default but you can also export loved or banned tracks.

```sh
./lastexport.py -u your_librefm_username -s librefm -t loved -o mylovedtracks.txt
```

## Using libreimport (scrobble import)

To import all your tracks to libre.fm, run:

```sh
./libreimport.py your_librefm_username exported_tracks.txt
```

You can also specify which server to upload to with -s.

```sh
./libreimport.py -s http://mygnufmserver.com/ your_username
```

## Using libreimport2 (loved/banned import)

To import your loved tracks to libre.fm, run:

```sh
./libreimport2.py -u your_librefm_username -t loved -f mylovedtracks.txt
```

or for banned tracks:

```sh
./libreimport2.py -u your_librefm_username -t banned -f mybannedtracks.txt
```

You will be prompted for your libre.fm password and the tracks will then get uploaded.

By default it will import your tracks to libre.fm but you can specify another server to upload to with `-s mygnufmserver` or `--server=mygnufmserver`.
