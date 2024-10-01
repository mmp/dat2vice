`dat2vice` is a utility for people doing facility engineering for
[vice](https://pharr.org/vice). It extracts video maps from the
FAA DAT video map file format and converts them to _vice_'s internal format.

`dat2vice` takes a _manifest_ file that specifies the filenames of the DAT
files to convert as well as additional information about each one:

- "filename" is a string giving the path to the DAT file
- "brightness" (_optional_, 0 by default): should be either 0 or 1, with 0 signifying map group A, and 1 signifying map brightness group B.
- "label" gives the label that should be used for the map on the STARS DCB
- "title" gives the full name of the map, as is used in _vice_ scenario definition files.
- "number" gives the integer map number associated with the map.
- "color" (_optional_): gives which map color to use (1-8).
- "category" (_optional_): number from -1 to 9 giving the map category. 0 is the default if not specified.
  - -1: no category
  - 0: Geographic maps
  - 1: Controlled airspace
  - 2: Runway extensions
  - 3: Danger areas
  - 4: Aerodromes
  - 5: General aviation
  - 6: SIDs/STARs
  - 7: Military
  - 8: Geographic points
  - 9: Processing areas
- "radius" (_optional_) if specified, gives a radius in nautical miles beyond which extra data in the map is culled.

For brightness group 0, here are the map colors:
<table style="margin: auto;" border="1">
  <tr>
    <td style="width: 30px; height: 30px; background-color: rgb(140,140,140);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(0, 255, 255);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(255, 0, 255);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(238, 201, 0);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(238, 106, 80);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(162, 205, 90);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(218, 165, 32);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(72, 118, 255);"></td>
  </tr>
  <tr>
    <td style="text-align:center;">1</td>
    <td style="text-align:center;">2</td>
    <td style="text-align:center;">3</td>
    <td style="text-align:center;">4</td>
    <td style="text-align:center;">5</td>
    <td style="text-align:center;">6</td>
    <td style="text-align:center;">7</td>
    <td style="text-align:center;">8</td>
  </tr>
</table><br>
Here are the map colors for brightness group 1:
<table style="margin: auto;" border="1">
  <tr>
    <td style="width: 30px; height: 30px; background-color: rgb(140,140,140);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(132,112,255);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(118,238,198);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(237,145,33);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(218,112,214);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(238,180,180);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(50,205,50);"></td>
    <td style="width: 30px; height: 30px; background-color: rgb(255,106,106);"></td>
  </tr>
  <tr>
    <td style="text-align:center;">1</td>
    <td style="text-align:center;">2</td>
    <td style="text-align:center;">3</td>
    <td style="text-align:center;">4</td>
    <td style="text-align:center;">5</td>
    <td style="text-align:center;">6</td>
    <td style="text-align:center;">7</td>
    <td style="text-align:center;">8</td>
  </tr>
</table><br>


Here is an example:

```json
[
        {
            "filename": "xyz001sgp.dat",
            "brightness": 0,
            "label": "XYZ RNAV",
            "title": "XYZ RNAV APPROACHES",
            "number": 1
        },
        {
            "filename": "xyz012smvagp.dat",
            "brightness": 1,
            "label": "XYZ MVA",
            "title": "XYZ AREA MVAS",
            "color": 3,
            "number": 12,
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
