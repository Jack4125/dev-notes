---
title: "Errors"
tags: "npm, npx"
---

# Errors

- Arbitrary File Overwrite from Package: tar

  > Fix: Go into package-lock.json, search for all references of "tar" package, update all to at least version "4.4.8"

- Regular Expression Denial of Service from Package: braces

  > Fix: Go into package-lock.json, search for all references of "braces" package, update all to at least version "2.3.2"

  > "npm update" and "npm install braces@2.3.2" does not seem to work on all references.

- Favicon not displaying:

  > Problem 1: not serving as static asset

  > Problem 2: cache not clear. Add '?v1' at the end of href link

- `npx` not working because OS user file has [space in name](https://github.com/zkat/npx/issues/146).
