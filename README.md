<img src="https://github.com/openfv/openfv/raw/master/img/openfv-header-crunch.png" alt="Open Font Version Specification">

# Open Font Version Specification

**Version 0.3.0**

**Status: Experimental, unstable draft**

## Table of Contents

- [Introduction and Rationale](#introduction-and-rationale)
- [Definitions](#definitions)
- [OpenFV Specification](#openfv-specification)
    - [name Table ID 5 Record Specification](#1-name-table-id-5-record-version-string)
    - [head Table fontRevision Record Specification](#2-head-table-fontrevision-record-version-string)
    - [Version number semantics](#3-version-number-semantics)
- [Examples](#examples)
    - [name Table ID 5 Record Examples](#name-table-id-5-record)
    - [head Table fontRevision Record Examples](#head-table-fontrevision-record)
- [License](#license)

## Introduction and Rationale

Software version strings commonly include the following semantics:

 - the magnitude and backward compatibility of source code changes relative to the last version
 - the readiness of the source code for production level release to end users
 - the source code state at build time

The established and widely used [SemVer](https://github.com/semver/semver) approach to semantic software versioning uses the version number format `MAJOR.MINOR.PATCH` with a software API-based standard for incremented values during the software development process.  Additional metadata are commonly appended to this SemVer syntax to indicate the readiness of the software for general use by the end user as work progresses towards a release milestone (e.g. `v1.2.0-alpha`, `v1.2.0-beta`,`v1.2.0-rc.1`, `v1.2.0-rc.2`, `v1.2.0`).  Projects that create compiled binary artifacts from source code frequently associate a label with the build artifact to establish a record of the state of the source code at build time (e.g., a git commit SHA1 hash string).

These versioning concepts apply to the development of typeface software and are desirable during the development process; however, they are not all defined under the OpenType font versioning specification. Typeface software version strings are compiled into the nameID 5 record of the OpenType name table and the fontRevision record of the OpenType head table. These records are defined in the OpenType format specification as below, and explained in the SIL [Font Development Best Practices documentation](https://silnrsi.github.io/FDBP/en-US/Versioning.html) ([source](https://github.com/silnrsi/FDBP).)

##### OpenType name table ID 5 record definition

>Version string. Should begin with the syntax 'Version <number>.<number>' (upper case, lower case, or mixed, with a space between “Version” and the number).
>
>The string must contain a version number of the following form: one or more digits (0-9) of value less than 65,535, followed by a period, followed by one or more digits of value less than 65,535. Any character other than a digit will terminate the minor number. A character such as “;” is helpful to separate different pieces of version information.
>
>The first such match in the string can be used by installation software to compare font versions. Note that some installers may require the string to start with “Version ”, followed by a version number as above.

([Source](https://www.microsoft.com/typography/otspec/name.htm#nameIDs))

##### OpenType head table fontRevision record definition

> Set by font manufacturer

([Source](https://www.microsoft.com/typography/otspec/head.htm))

The OpenType specification defines a version number as `MAJOR.MINOR`.  There is no specification for a `PATCH`/`BUILD` version number nor version number metadata strings.  Font versioning deviates further from the SemVer version number format as a result of the font compiler convention to include zero padding in `MINOR` version numbers.  While this is not always the case in nameID 5 records, this is a consistent format used in the head.fontRevision record.  The interpretation of version numbers with this approach is not intuitive.  The version number strings `Version 1.1`, `Version 1.01`, and `Version 1.001` are all defined as "different" though these numbers may all represent the same stage of development (i.e., one iteration beyond the first major release).  These differences in version numbers emerge due to conventions established by project author(s) and by the tools that they use to compile fonts from their source code.  The OpenType definitions do not specify a format to indicate the development status of a typeface project relative to its version milestone, nor do they define an approach to maintain information about the source code state at build time within the font build artifact.  The lack of a formal standard to address the above issues provided the impetus for this specification.

The Open Font Version (OpenFV) Specification represents a compliant extension of the OpenType name table nameID 5 record and OpenType head.fontRevision record specifications. OpenFV is intended to serve as a typeface software versioning standard for the development, testing, release, and use of typeface source code and the build artifacts (fonts) derived from the source.  This specification defines a version number string syntax with semantic underpinnings that maintains informative data for both developers and users.


## Definitions

- *build* - a compiled file of any typeface build artifact type that is defined by the state of the source code at build time (i.e., a font binary file)
- *development* - source code and compiled build artifact state that includes incomplete implementation of work to achieve a release milestone
- *metadata* - data that fall outside of the OpenFV definition of the version number string
- *POSITION ONE* - the first substring position of a version string
- *POSITION TWO* - the second substring position of a version string
- *release* - source code and compiled build artifact state milestone intended for production level use by the end user
- *status* - a binary indicator of the development/release status of the source code
- *state* - an indicator of source code state at build time

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).


## OpenFV Specification

### 1. name Table ID 5 Record Version String

#### a. Version String Syntax

Version strings in the OpenType name table ID 5 record MUST be defined as semicolon delimited substrings of mandatory and optional data elements.

The OpenFV Specification syntax for the substring elements of the full version string is:

```
[Font Version Number]; [Status/State Metadata]; [Other Metadata]
```

The font version string MUST include:

- The font version number substring

The font version string MAY include:

- Status/state metadata substring
- Other metadata substrings

#### b. Font Version Number Substring

The font version number substring:

- MUST be located at substring POSITION ONE.
- MUST be defined as the capitalized word "Version", a space glyph (U+0020), `MAJOR` version number digit(s), a period (U+002E), `MINOR` version number digits.
- MUST NOT include metadata.
- MAY include a semicolon at its final character position if metadata follow the font version number substring at substring POSITION TWO.  Whitespace characters SHOULD NOT be included between the `MINOR` version number and the semicolon.
- MUST NOT include a semicolon if there are no metadata following the version number substring.

The `MAJOR` version number:

- MUST have between one and three digits with a minimum value of 0 and a maximum value of 999.

The `MINOR` version number:

- MUST have exactly three digits.  For numbers less than 100, zero padding MUST be used in the `MINOR` version number.  The `MINOR` version number MUST have a minimum value of 000 and a maximum value of 999.


#### c. State Metadata Substring

The State metadata substring:

- SHALL define source code state at font build time.
- is OPTIONAL.
- MUST be located at substring POSITION TWO of the font version string.
- MUST NOT include characters outside of the set: `a-zA-Z0-9._-`
- MUST include the delimiter `[` as the initial character and the delimiter `]` as the final character of the substring.  The string contents inside these delimiters SHALL be defined as the "state label". The state label SHOULD be 50 characters or less.
- MAY include a status substring (see below).


#### d. Status Metadata Substring

The Status metadata substring:

- SHALL define the binary development/release status of source code and font builds as defined in the Definitions section.
- is OPTIONAL.
- MUST be located at substring POSITION TWO.
- MUST use one of the following case-sensitive formats with zero or one preceding space characters for version strings that DO NOT include a state substring:
    - `DEV`
    - `RELEASE`
- MUST use one of the following case-sensitive formats without preceding space characters for version strings that DO include a state substring:
    - `-dev`
    - `-release`

#### e. Other Metadata Substrings

Other metadata substrings:

- are OPTIONAL.
- MAY be included one or more times in the font version string
- MUST follow all version number substring and state/status substring data with a semicolon delimiter followed by zero or one space character preceding each Other metadata substring.
- SHOULD be limited to 50 characters or less.

### 2. head table fontRevision Record Version String

The font version number in the fontRevision record of the OpenType head table:

- MUST be defined as `MAJOR` version number digit(s), a period, `MINOR` version number digits
- MUST have a MAJOR version number that is between one and three digits in length with a minimum value of 0 and a maximum value of 999.
- MUST have a `MINOR` version number that is exactly three digits in length.  For `MINOR` version numbers less than 100, zero padding MUST be used.  The `MINOR` version number MUST have a minimum value of 000 and a maximum value of 999.
- MUST NOT include any characters before the `MAJOR` version number nor after the `MINOR` version number

### 3. Version Number Semantics

- The `MAJOR.MINOR` version number SHALL be intended to represent a release milestone that MAY be incompletely implemented in the source that is defined with a  `MAJOR.MINOR` version number. The `MAJOR.MINOR` version number SHALL NOT be intended to represent source code state at build time in build artifacts and MAY NOT be unique across build artifacts as work is carried out to achieve a `MAJOR.MINOR` version milestone.  
- Development source status SHOULD be distinguished from release source status through the use of a Status metadata substring as defined in this specification.
- The `MAJOR` version number SHOULD be set to 0 during the pre-production phase of development before the initial release.  `MAJOR` version number 0 SHALL indicate this pre-production phase of development.
- The `MAJOR` version number MUST be set to 1 at the time of the initial release to end users. The conversion from `MAJOR` version number 0 to `MAJOR` version number 1 SHALL indicate the authors' acknowledgment that source code and build artifacts meet the OpenFV release definition.
- Upon completion of the source code changes to achieve a release milestone, a version number increment SHOULD be performed immediately before making changes to the source code that are intended for a future release milestone.

The semantics for changes to the version number SHALL include an increment by the value of 1 of the:

- `MAJOR` version number for completion of major project-specific milestones and all backward incompatible changes made to the typeface software (e.g., the elimination of support for an entire Unicode code range).
- `MINOR` version number for functionality, hotfix, and dependency changes. Examples include:
  - glyph design changes
  - addition/elimination/modification of hinting/instruction sets
  - addition/removal of glyphs
  - design flaw fixes
  - changes to build processes
  - updated build dependencies

When the `MAJOR` version number is incremented, the `MINOR` version number SHALL be changed to a value of 000.

## Examples

### name Table ID 5 Record

Examples of name table ID 5 record version strings that meet the OpenFV specification include:

##### Version number only
```
Version 1.001
```

##### Version number and development status label
```
Version 1.001; DEV
Version 1.001; RELEASE
```

##### Version number and source code state label

```
Version 1.001; [abcd123]
```

##### Version number, source code state label, and development status label
```
Version 1.001; [abcd123]-dev
Version 1.001; [abcd123]-release
```

##### Version number, source code state label, development status label, and other metadata

```
Version 1.001; [abcd123]-dev; here are metadata
Version 1.001; [abcd123]-release; here are metadata
```

##### Version number and other metadata

```
Version 1.001; here are metadata
Version 1.001; here are metadata; here are more metadata
```

### head table fontRevision Record

Examples of head table fontRevision record version strings that meet the OpenFV specification include:

```
1.001
```

```
10.010
```

```
100.100
```

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
