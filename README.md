# Open Font Version Specification

**Version 0.1.0**

## Table of Contents

- Summary
- Definitions
- Introduction and Rationale
- Specification
- License

## Introduction and Rationale

Software version strings commonly include the following semantics:

 - the magnitude and backwards compatibility of source code changes relative to the last version
 - the readiness of the source code for production level release to end users
 - the source code state at build time

The traditional [semantic versioning](https://github.com/semver/semver) (SemVer) approach to software versioning uses the version number format `MAJOR.MINOR.PATCH` and provides a software API-based standard for iteration of these values during software development.  Additional metadata are commonly appended to this semantic versioning syntax to indicate the readiness of the software for general use by the end user as work progresses towards a release milestone (e.g. `v1.2.0-alpha`, `v1.2.0-beta`,`v1.2.0-rc.1`, `v1.2.0-rc.2`, `v1.2.0`).  Projects that create compiled binary artifacts from the source code commonly associate a source code state label with the build artifact in order to identify and record the source code state at build time (e.g. the git commit SHA1 hash string).

Font version strings are compiled in the nameID 5 record of the font OpenType name table.  This name table record is defined as follows:

##### OpenType name table nameID 5 record definition

>Version string. Should begin with the syntax 'Version <number>.<number>' (upper case, lower case, or mixed, with a space between “Version” and the number).
>
>The string must contain a version number of the following form: one or more digits (0-9) of value less than 65,535, followed by a period, followed by one or more digits of value less than 65,535. Any character other than a digit will terminate the minor number. A character such as “;” is helpful to separate different pieces of version information.
>
>The first such match in the string can be used by installation software to compare font versions. Note that some installers may require the string to start with “Version >”, followed by a version number as above.

([Source](https://www.microsoft.com/typography/otspec/name.htm#nameIDs))

The OpenType specification defines a version number as `MAJOR.MINOR`.  Neither a `PATCH` version number nor version number metadata strings are permitted under the OpenType specification as the definition above states that "any character other than a digit will terminate the minor number".  Font versioning deviates further from the SemVer approach as a result of the font compiler convention to include leading zeroes in `MINOR` version numbers.  The interpretation of version numbers with this approach is not intuitive as `Version 1.1`, `Version 1.01`, and `Version 1.001` are all defined as "different" versions though these numbers may all represent the same stage of development (i.e. one iteration beyond the first major release) based upon different conventions established by project author(s) or by the tools that they use to compile fonts from their source code.  The OpenType nameID 5 record definition does not specify a format to indicate the development status of a typeface project relative to its version milestone, nor does it define an approach to maintain information about the source code state at build time within the font build artifact.  The lack of a formal standard for font versioning to address the above issues provided the impetus for this project.

The Open Font Version (OpenFV) Specification represents a compliant extension of the OpenType name table nameID 5 specification that is intended to serve as a typeface software versioning standard for the development, testing, release, and use of typeface source code and the build artifacts (fonts) that are derived from the source.  This specification defines a version string syntax with semantic underpinnings that maintains informative data for both developers and users.


## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
