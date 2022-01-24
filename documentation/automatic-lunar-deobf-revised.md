---
title: (Revised) Automatic Debofuscation of the Lunar Client - A Proposal
wip: true
lastUpdated: "24 January 2022"
---
# Preface
I have been theorizing on-and-off for some time now, and I feel that now is a good time to propose my solution to the issue of Lunar Client deobfuscation. 

## The Problem
Firstly, what's the problem? Simply put, the issue lies in the fact that Lunar Client obfuscation is fickle and changes with every update, and across every version. They update quite frequently, as well. This causes mappings to be impermanent, inefficient, and, worse of all, near-impossible to maintain.

## The Solution
Previously, I proposed a sort of probing algorithm which would programmatically determine classes based off of unique features, references, etcetera. This was a terrible idea.

Bestowed upon me now is new knowledge. With the use of [FabricMC's fork of Matcher](https://github.com/FabricMC/Matcher), we can instead generate tiny-format mappings that can be used to transfer mappings between different versions of `.jar`s.

## The Implementation
With the ability to use Matcher, it is is simple as manually controlling inputs and creating new mappings, which can be used to transfer existing mappings to newer versions. There is no need for a unique, complicated implementation.
