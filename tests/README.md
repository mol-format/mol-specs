# Markdown Object Language Offical Tests

STATUS: INCOMPLETE

## Overview

These are the official tests for MOL. Any implementor must pass all tests.

Tests are grouped into subfolders by topic.

## Test Files

For each test, there are two file versions:
 - .mol: the MOL versions of the JSON version
 - .json: the JSON version of the MOL version

Since there are multiple ways to write a MOL, any test can define
multiple results using [test].[#].mol, etc. For example mytest.1.mol, mytest.2.mol.

A test is successful if it matches any of the resulting formats.

A test file should be written in lower-dash case and not include the ``.``, as this is reserved
for the variation number.
