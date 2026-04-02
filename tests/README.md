# Markdown Object Language Offical Tests

STATUS: INCOMPLETE

## Overview

These are the official tests for MOL. Any implementor must pass all tests.

Tests are grouped into subfolders by topic.

## Test Files

For each test, there are two file versions:
 - .mol: the MOL versions of the JSON version
 - .json: the JSON version of the MOL version

Note: Since there are multiple ways to produce a MOL from JSON and vice-versa, any test can define
multiple results using [test].[#].mol, etc. For example mytest.1.mol, mytest.2.mol.

A test is successful if it matches any of the resulting formats.
