---
title: High Level Programming
date: 2024-01-09 10:55:50
tags:
  - college
  - high level programming
  - review
categories: Review
description: 前半程大概会是个四不像的 F# 入门指南吧，后面是Issie项目记录。
---

<p style="opacity: 0.7;">Start Notes: 

<small style="opacity: 0.7;">

F# 入门：[cheatsheet](https://intranet.ee.ic.ac.uk/t.clarke/hlp/images/cheatsheet/index.html)

Issie：[GitHub repo](https://github.com/tomcl/issie)

</small>

---

## Intro

## Type

## Functional Programming

---

## Individual Project

### 1. Code Analysis


### 2. DrawBlock Test & Improvement


### 3. RotateScale


### 4. SheetBeatifyHelper Functions

| Key | Type | Difficulty | Value read or written |
| --- | ---- | ---------- | --------------------- |
| B1R, B1W | RW | Low | The dimensions of a custom component symbol |
| B2W | W | Medium | The position of a symbol on the sheet |
| B3R, B3W | RW | Medium | Read/write the order of ports on a specified side of a symbol |
| B4R, B4W | RW | Low | The reverses state of the inputs of a MUX2 |
| B5R | R | Low | The position of a port on the sheet. It cannot directly be written. |
| B6R | R | Low | The Bounding box of a symbol outline (position is contained in this) |
| B7R, B7W | RW | Low | The rotation state of a symbol |
| B8R, B8W | RW | Low | The flip state of a symbol |
| T1R | R | Low | The number of pairs of symbols that intersect each other. See Tick3 for a related function. Count over all pairs of symbols. |
| T2R | R | Low | The number of distinct wire visible segments that intersect with one or more symbols. See Tick3.HLPTick3.visibleSegments for a helper. Count over all visible wire segments. |
| T3R | R | Low | The number of distinct pairs of segments that cross each other at right angles. Does not include 0 length segments or segments on same net intersecting at one end, or segments on same net on top of each other. Count over whole sheet. |
| T4R | R | Medium | Sum of wiring segment length, counting only one when there are N same-net segments overlapping (this is the visible wire length on the sheet). Count over whole sheet. |
| T5R | R | Low | Number of visible wire right-angles. Count over whole sheet. |
| T6R | R | High | The zero-length segments in a wire with non-zero segments on either side that have Lengths of opposite signs lead to a wire retracing itself. Note that this can also apply at the end of a wire (where the zero-length segment is one from the end). This is a wiring artifact that should never happen but errors in routing or separation can cause it. Count over the whole sheet. Return from one function a list of all the segments that retrace, and also a list of all the end of wire segments that retrace so far that the next segment (index = 3 or Segments.Length – 4) - starts inside a symbol. |

One helper function has been written in TestDrawBlock.HLPTick3 for you to use.

| Key | Type | Difficulty | Value read or written |
| --- | ---- | ---------- | --------------------- |
| n/a | R | High difficulty | See TestDrawBlock.HLPTick3.visibleSegments The visible segments of a wire, as a list of vectors, from source end to target end. Note that a zero length segment one from either end of a wire is allowed which if present causes the end three segments to coalesce into a single visible segment. |




## Group Work

### 



| Function | Adjust on sheet | Primary Optimization | Test using random sample inputs |
| -------- | --------------- | -------------------- | ------------------------------- |
| D1. sheetAlignScale | Custom component scaling. Positioning of all components | Reduce wire segments without increasing wire crossings. Align sets of components | Reduction in wire segments. Visual alignment of same-type components: metric that corresponds to overall visual quality |
| D2. sheetOrderFlip | Port order on custom components, flip components, flip MUX input order | Reduce wire crossings. | Reduction in wire crossings, other quality measures. | 
| D3. sheetWireLabelSymbol | Add or remove wire labels (swapping between long wires and wire labels). Improve symbol rendering. | Reduce wiring complexity (symbol rendering is judged manually on appearance) | Test ability to add labels without overlap in presence of adjacent components with visually helpful placement. |
| X1. sheetRouteSeparate* | Change existing routing or separation code to improve it and reduce artifacts | Reduce special case problems, make operation idempotent | Test for idempotence and corner case problem reduction. Test for over-long wires |
| X2. sheetReplace* | All component placement: topology recreated ignoring existing placement | All of the above | All of the above |
|D4. | Integrate (as much as possible) all deliverables into a single function which works well. Provide tests which demonstrate that it works. |||
