# simple_icd_10
A simple python library for ICD-10 codes

## Index
* [Introduction](#introduction)
* [What a code is and how it looks like](#what-a-code-is-and-how-it-looks-like)
* [Performance](#performance)
* [Documentation](#documentation)
  * [is_valid_item(code)](#is_valid_itemcode)
  * [is_valid_code(code)](#is_valid_codecode)
  * [is_chapter_or_block(code)](#is_chapter_or_blockcode)
  * [get_descriptions(code)](#get_descriptionscode)
  * [get_descendants(code)](#get_descendantscode)
  * [get_ancestors(code)](#get_ancestorscode)
  * [is_descendant(a,b)](#is_descendantab)
  * [is_ancestor(a,b)](#is_ancestorab)
  * [get_all_codes(keep_dots)](#get_all_codeskeep_dots)
  * [get_index(code)](#get_indexcode)
* [Conclusion](#conclusion)

## Introduction
The scope of this library is to provide a simple instrument for dealing with ICD-10 codes in your Python projects. It provides ways to check whether a code exists, to find its ancestors and descendants, to see its description and much more.  
The codes and their descriptions were taken from [this page](https://icd.who.int/browse10/2019/en#/E15-E16) in the WHO's website and are referred to the **2019 version of ICD-10**.

You can find the all the codes and their descriptions in plain text in the "data" folder.

## What a code is and how it looks like
We need to start by clarifying what a code is for us. The [ICD-10 instruction manual](https://icd.who.int/browse10/Content/statichtml/ICD10Volume2_en_2019.pdf) makes a distinction between **chapters**, **block of categories**, **three-character categories** and **four-character subcategories** (which from now on we'll refer to as chapters, blocks, categories and subcategories), with a few additional five-character subcategories: we will consider all these items as codes.  
That said, this library includes ways to make distinctions between the first two kinds of codes (chapters and blocks) and the other two kinds of codes (categories and subcategories); while making this distinction we may consider only categories and subcategories to be "codes", while we may use "items" as a more general term. However, in every other circumstance the definition of "code" is the more inclusive one.

Generally speaking, the codes of subcategories can be written in two different ways: with a dot (for example "I13.1") and without the dot (for example "I131"). The functions in this library can receive as input codes in both these formats. The codes returned by the functions will always be in the format without the dot.

## Performance
While several steps were taken in order to improve the performance of this library, speed was not the main focus while it was being written. This means that many functions are slower than they could potentially be. Please let me know if the performance makes this library unsuitable for your project.

## Documentation
Here I will list all the functions provided by this library and describe how to use them. If you are interested in a more interactive introduction to simple_icd_10, please take a look at the Jupyter Notebook "Showcase notebook.ipynb"; there you can also find more examples.

Here we suppose we have imported the library as follows:
```python
import simple_icd_10 as icd
```
### is_valid_item(code)
This function takes a string as input and returns True if the string is a valid chapter, block, category or subcategory in ICD-10, False otherwise.
```python
icd.is_valid_item("cat")
#False
icd.is_valid_item("B99")
#True
```
### is_valid_code(code)
This function takes a string as input and returns True if the string is a valid category or subcategory in ICD-10, False otherwise.
```python
icd.is_valid_code("A00-B99")
#False
icd.is_valid_code("B99")
#True
```
### is_chapter_or_block(code)
This function takes a string as input and returns True if the string is a valid chapter or block in ICD-10, False otherwise.
```python
icd.is_chapter_or_block("A00-B99")
#True
icd.is_chapter_or_block("B99")
#False
```
### get_descriptions(code)
This function takes a string as input. If the string is a valid ICD-10 code, it returns a string with its short description, otherwise it raises a ValueError.
```python
icd.is_chapter_or_block("XII")
#"Diseases of the skin and subcutaneous tissue"
icd.is_chapter_or_block("F00")
#"Dementia in Alzheimer disease"
```
### get_descendants(code)
This function takes a string as input. If the string is a valid ICD-10 code, it returns a list containing all its descendants in the ICD-10 classification, otherwise it raises a ValueError. While usually the returned codes are ordered, no particular order is guaranteed.
```python
icd.get_descendants("C00")
#['C000', 'C001', 'C002', 'C003', 'C004', 'C005', 'C006', 'C008', 'C009']
```
### get_ancestors(code)
This function takes a string as input. If the string is a valid ICD-10 code, it returns a list containing all its ancestors in the ICD-10 classification, otherwise it raises a ValueError. The results are ordered from its parent to its most distant ancestor.
```python
icd.get_descendants("H60.1")
#['H60', 'H60-H62', 'VIII']
```
### is_descendant(a,b)
This function takes two strings as input. If both strings are valid ICD-10 codes, it returns True if the first string is a descendant of the second string. If at least one of the strings is not a valid ICD-10 code, it raises a ValueError.
```python
icd.is_descendant("R01.0","XVIII")
#True
icd.is_descendant("M31","K00-K14")
#False
```
### is_ancestor(a,b)
This function takes two strings as input. If both strings are valid ICD-10 codes, it returns True if the first string is an ancestor of the second string. If at least one of the strings is not a valid ICD-10 code, it raises a ValueError.
```python
icd.is_ancestor("XVIII","R01.0")
#True
icd.is_ancestor("K00-K14","M31")
#False
```
### get_all_codes(keep_dots)
This function takes a boolean for input and returns the list of all items in the ICD-10 classification. If the boolean is True the subcategories in the list will have a dot in them, if it's False the subcategories won't have a dot in them.
```python
icd.get_all_codes(True)
#['I', 'A00-A09', 'A00', 'A00.0', 'A00.1', 'A00.9', 'A01', 'A01.0', ...
icd.get_all_codes(False)
#['I', 'A00-A09', 'A00', 'A000', 'A001', 'A009', 'A01', 'A010', ...
```
### get_index(code)
This function takes a string as input. If the string is a valid ICD-10 code, it returns its index in the list returned by `get_all_codes`, otherwise it raises a ValueError.
```python
icd.get_index("P00")
#7159
icd.get_all_codes(True)[7159]
#"P00"
```
## Conclusion
This is everything you needed to know before using the simple_icd_10 library - please contact me if you feel I missed something or there's some passage that you think should be explained better or more. Also contact me if you find any errors in the library or in the documentation.  
I hope this library will save you some time; it definitely would have done it for me if I hadn't had to write it!

If you are feeling particularly thankful and/or generous and feel like leaving me a tip, **don't!**  
Donate instead to *Médecins Sans Frontières* to support their efforts to provides medical care to millions of people caught in crises around the world. Click on the logo below to open their "donate" page.

[![MSF Logo](https://www.msf.org/themes/custom/msf_theme/src/kss/components/icons/assets/logo-white-en.svg)](https://www.msf.org/donate)

If you end up making a donation because of this library, feel free to drop me an email: I'll be happy to read it!

*Stefano Travasci*