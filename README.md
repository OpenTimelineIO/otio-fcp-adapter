# OpenTimelineIO CMX3600 EDL Adapter

The `cmx_3600` adapter is part of OpenTimelineIO's core adapter plugins.  
It provides reading and writing of CMX3600 formatted Edit Decision Lists (EDL). 
For more information on the CMX3600 format please check the links in the 
[reference](edl-references) section 

# Adapter Feature Matrix

The following features of OTIO are supported by the `cmx_3600` adapter:

|Feature                  | Support |
|-------------------------|:-------:|
|Single Track of Clips    | ✔       |
|Multiple Video Tracks    | ✖       |
|Audio Tracks & Clips     | ✔       |
|Gap/Filler               | ✔       |
|Markers                  | ✔       |
|Nesting                  | ✖       |
|Transitions              | ✔       |
|Audio/Video Effects      | ✖       |
|Linear Speed Effects     | ✔       |
|Fancy Speed Effects      | ✖       |
|Color Decision List      | ✔       |
|Image Sequence Reference | ✔       |


# Style Variations
The `cmx_3600` adapter supports writing EDL's with slight variations required by 
certain applications. At the moment the supported styles are:
* `avid` = [Avid Media Composer](https://www.avid.com/media-composer) (default)
* `nucoda` = [Nucoda](https://digitalvision.world/products/nucoda/)
* `premiere` = [Adobe Premiere Pro](https://www.adobe.com/products/premiere.html)


## Main Functions
The two main functions below are usually called indirectly through 
`otio.adapters.read_from_[file|string]` and `otio.adapters.write_to_[file|string]`.
However, since the `cmx_3600` adapter provides some additional arguments we 
should mention them here.

### read_from_string(input_str, rate=24, ignore_timecode_mismatch=False)

Reads a CMX Edit Decision List (EDL) from a string.  
Since EDLs don't contain metadata specifying the rate they are meant
for, you may need to specify the `rate` parameter (default is 24).  
By default, read_from_string will throw an exception if it discovers
invalid timecode in the EDL. For example, if a clip's record timecode
overlaps with the previous cut.  
Since this is a common mistake in many EDLs, you can specify 
`ignore_timecode_mismatch=True`, which will
supress these errors and attempt to guess at the correct record
timecode based on the source timecode and adjacent cuts.  
For best results, you may wish to do something like this:

``` python
try:
    timeline = otio.adapters.read_from_string("mymovie.edl", rate=30)
except EDLParseError:
   print('Log a warning here')
   try:
       timeline = otio.adapters.read_from_string(
           "mymovie.edl",
           rate=30,
           ignore_timecode_mismatch=True)
   except EDLParseError:
       print('Log an error here')
```

### write_to_string(input_otio, rate=None, style='avid', reelname_len=8)

Writes a CMX Edit Decision List (EDL) to a string.  
This function introduces `style` and `reelname_len` parameters.  
The `style` parameter let's you produce slight variations of EDL's 
(`avid` by default). Other supported styles are "nucoda" and "premiere".  
The `reelname_len` parameter lets you determine how many characters are in the 
reel name of the EDL (default is 8). Setting it to `None` will not set a limit 
of characters.


# EDL References

- Full specification: [SMPTE 258M-2004 "For Television −− Transfer of Edit Decision Lists"](https://ieeexplore.ieee.org/document/7291839) (See also [this older document](http://xmil.biz/EDL-X/CMX3600.pdf))
- [Reference](https://prohelp.apple.com/finalcutpro_help-r01/English/en/finalcutpro/usermanual/chapter_96_section_0.html)


# License
OpenTimelineIO and the "cmx_3600" adapter are open source software. Please see the [LICENSE](LICENSE) 
for details.

Nothing in the license file or this project grants any right to use Pixar or any other contributor’s trade names, trademarks, service marks, or product names.


## Contributions

If you want to contribute to the project, 
please see: https://opentimelineio.readthedocs.io/en/latest/tutorials/contributing.html


# Contact

For more information, please visit http://opentimeline.io/
or https://github.com/AcademySoftwareFoundation/OpenTimelineIO
or join our discussion forum: https://lists.aswf.io/g/otio-discussion
