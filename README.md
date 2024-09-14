`dat2vice` is a utility for people doing facility engineering for
[vice](https://pharr.org/vice). It extracts video maps from the
FAA DAT video map file format and converts them to _vice_'s internal format.

`dat2vice` takes a _manifest_ file that specifies the filenames of the DAT
files to convert as well as additional information about each one:

- "filename" is a string giving the path to the DAT file
- "group" should be either 0 or 1, with 0 signifying map group A, and 1 signifying map group B.
- "label" gives the label that should be used for the map on the STARS DCB
- "name" gives the full name of the map, as is used in _vice_ scenario definition files.
- "id" gives the integer map number associated with the map.
- "radius" (_optional_) if specified, gives a radius in nautical miles beyond which extra data in the map is culled.

Here is an example:

```json
[
        {
            "filename": "xyz001sgp.dat",
            "group": 0,
            "label": "XYZ RNAV",
            "name": "XYZ RNAV APPROACHES",
            "id": 1
        },
        {
            "filename": "xyz012smvagp.dat",
            "group": 0,
            "label": "XYZ MVA",
            "name": "XYZ AREA MVAS",
            "id": 12,
            "radius": 60

        },
]
```

Given a manifest file, run `dat2vice` like this, giving it the filename for
the manifest file and the facility identifier for the maps:
```
> dat2vice manifest.json ZXX
```

You can also specify a culling radius via the `-radius` command-line option:
```
> dat2vice -radius 40 manifest.json ZXX
```
If `-radius` is given and per-map "radius" values are present, the per-map
value takes precedence.

If successful, `dat2vice` will generate two files in the current directory,
`ZXX-manifest.gob` and `ZXX-videomaps.gob.zst`. These can be then be used
in _vice_ scenarios.
