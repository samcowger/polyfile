# PolyFile
<p align="center">
  <img src="logo/polyfile_name.png?raw=true" width="256" title="PolyFile">
</p>
<br />

[![Build Status](https://travis-ci.com/trailofbits/polyfile.svg?branch=master)](https://travis-ci.com/trailofbits/polyfile)
[![PyPI version](https://badge.fury.io/py/polyfile.svg)](https://badge.fury.io/py/polyfile)
[![Slack Status](https://empireslacking.herokuapp.com/badge.svg)](https://empireslacking.herokuapp.com)

A utility to identify and map the semantic structure of files,
including polyglots, chimeras, and schizophrenic files. It can be used
in conjunction with its sister tool
[PolyTracker](https://github.com/trailofbits/polytracker) for
_Automated Lexical Annotation and Navigation of Parsers_, a backronym
devised solely for the purpose of collectively referring to the tools
as _The ALAN Parsers Project_.

## Quickstart

In the same directory as this README, run:
```
pip3 install -e .
```

This will automatically install the `polyfile` executable in your path.

## Usage

```
usage: polyfile [-h] [--filetype FILETYPE] [--list] [--html HTML]
                [--try-all-offsets] [--debug] [--quiet] [--version]
                [-dumpversion]
                [FILE]

A utility to recursively map the structure of a file.

positional arguments:
  FILE                  The file to analyze; pass '-' or omit to read from
                        STDIN

optional arguments:
  -h, --help            show this help message and exit
  --filetype FILETYPE, -f FILETYPE
                        Explicitly match against the given filetype (default
                        is to match against all filetypes)
  --list, -l            list the supported filetypes (for the `--filetype`
                        argument) and exit
  --html HTML, -t HTML  Path to write an interactive HTML file for exploring
                        the PDF
  --try-all-offsets, -a
                        Search for a file match at every possible offset; this
                        can be very slow for larger files
  --debug, -d           Print debug information
  --quiet, -q           Suppress all log output (overrides --debug)
  --version, -v         Print PolyFile's version information to STDERR
  -dumpversion          Print PolyFile's raw version information to STDOUT and
                        exit
```

To generate a JSON mapping of a file, run:

```
polyfile INPUT_FILE > output.json
```

You can optionally have PolyFile output an interactive HTML page containing a labeled, interactive hexdump of the file:
```
polyfile INPUT_FILE --html output.html > output.json
```

## File Support

PolyFile can identify all 10,000+ file formats in the [TrID database](http://mark0.net/soft-trid-deflist.html).
It currently has support for parsing and semantically mapping the following formats:
* PDF, using an instrumented version of [Didier Stevens' public domain, permissive, forensic parser](https://blog.didierstevens.com/programs/pdf-tools/)
* ZIP, including reursive identification of all ZIP contents
* JPEG/JFIF, using its [Kaitai Struct grammar](https://formats.kaitai.io/jpeg/index.html)
* [iNES](https://wiki.nesdev.com/w/index.php/INES)
* [Any other format](https://formats.kaitai.io/index.html) specified in a [KSY grammar](https://doc.kaitai.io/user_guide.html)

For an example that exercises all of these file formats, run:
```bash
curl -v --silent https://www.sultanik.com/files/ESultanikResume.pdf | polyfile --html ESultanikResume.html - > ESultanikResume.json
```

## Output Format

PolyFile outputs its mapping in an extension of the [SBuD](https://github.com/corkami/sbud) JSON format described [in the documentation](docs/json_format.md).

## Current Status and Known Deficiencies
* The instrumented Kaitai Struct parser generator implementation has only been tested on the JPEG/JFIF grammar;
  other KSY definitions may exercise portions of the KSY specification that have not yet been implemented

## License and Acknowledgements

This research was developed by [Trail of
Bits](https://www.trailofbits.com/) with funding from the Defense
Advanced Research Projects Agency (DARPA) under the SafeDocs program
as a subcontractor to [Galois](https://galois.com). It is licensed under the [Apache 2.0 lisense](LICENSE).
The [PDF parser](polyfile/pdfparser.py) is modified from the parser developed by Didier Stevens and released into the
 public domain. © 2019, Trail of Bits.
