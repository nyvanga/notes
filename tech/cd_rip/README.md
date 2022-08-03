# Ripping CD's

First install `abcde`:
```
sudo apt-get install abcde
```

Then create `~/.abcde.conf` with this content (for more recent information see `/etc/abcde.conf`):
```
# Encode tracks immediately after reading. Saves disk space:
LOWDISK=y

# Specify the method to use to retrieve the track information.
# Alternative: 'musicbrainz':
CDDBMETHOD=cddb
CDDBURL="http://gnudb.gnudb.org/~cddb/cddb.cgi"

# Make a local cache of cddb entries and then volunteer to use 
# these entries when and if they match the cd:
CDDBCOPYLOCAL="y"
CDDBLOCALDIR="$HOME/.cddb"
CDDBLOCALRECURSIVE="y"
CDDBUSELOCAL="y"

# Setup flac encoding
FLACENCODERSYNTAX=flac
FLAC=flac
FLACOPTS='-s -e -V -8'
OUTPUTTYPE="flac"

# CD reader program to use - currently recognized options are 'cdparanoia',
# 'libcdio' (cd-paranoia),'icedax', 'cdda2wav', 'dagrab', 'pird' and 'flac'.
CDROMREADERSYNTAX=cdparanoia            
                                     
# Give the location of the ripping program and pass any extra options,
# if using libcdio set 'CD_PARANOIA=cd-paranoia'.
CDPARANOIA=cdparanoia  
CDPARANOIAOPTS="--never-skip=40"

# Give the location of the CD identification program:       
CDDISCID=cd-discid           
                               
# Give the base location here for the encoded music files.
OUTPUTDIR="$HOME/Music"
WAVOUTPUTDIR="$HOME/Downloads"

# Decide here how you want the tracks labelled for a standard 'single-artist',
# multi-track encode and also for a multi-track, 'various-artist' encode:
OUTPUTFORMAT='${ARTISTFILE}/${YEAR} ${ALBUMFILE}/${TRACKNUM} ${TRACKFILE}'
VAOUTPUTFORMAT='${ARTISTFILE}/${YEAR} ${ALBUMFILE}/${TRACKNUM} (${ARTISTFILE}) ${TRACKFILE}'

# Download album art
ALBUMARTDIR="$OUTPUTDIR/$ARTDIR"
ALBUMARTFILE="cover.jpg"
ALBUMARTTYPE="JPEG"

# This function takes out dots preceding the album name, and removes a grab
# bag of illegal characters.
mungefilename ()
{
  echo "$@" | sed -e 's/^\.*//' | tr -d ":><|*/\"'?[:cntrl:]"
}

MAXPROCS=2                                # Run a few encoders simultaneously
PADTRACKS=y                               # Makes tracks 01 02 not 1 2
EXTRAVERBOSE=2                            # Useful for debugging
EJECTCD=y                                 # Please eject cd when finished :-)
```
