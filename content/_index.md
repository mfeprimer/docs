---
title: Introduction
type: docs
---

# MFEprimer-3.0

## Introduction

Quality control for primers is crucial for a successful PCR reaction. For example, multiplex PCR is an efficient capture method to simultaneously amplify up to thousands of SNPs in one tube. And it requires compatible primer sets with no non-specific amplifications, no dimers etc. Previous versions of MFEprimer (Qu, et al., 2009, Qu, et al., 2012) mainly focused on primer specificity evaluation. In current version, MFEprimer-3.0 was developed as a full-functional primer quality control program.

## What is New?

* Binding sites searching is more sensitive by updating the k-mer algorithm to allow mismatches within the k-mer except the first base at the 3’ end. And the binding sites of each primer with stable 3’ end are also listed in output.
* Implements algorithms to quickly check self-dimers, cross-dimers and hairpins.
* Command-line version has an option of JSON output to make MFEprimer more useful by acting as a quality control step in “primer design → quality control → redesign” pipeline.
* Added a function to check whether the binding sites contain SNPs, which will affect the consistency of binding efficiency among different samples.
* Rewrote all the code by Go language to make the program more reliable, fast and easy to use across multiple platforms.Besides the web server with predefined databases, custom databases upon request and command-line tool for user-defined databases are also supported now.

## Usage

[Online servers and download command-line]({{< relref "/online-server" >}})

## How to cite us

> Kun Wang, Haiwei Li, Yue Xu, Qianzhi Shao, Jianming Yi, Ruichao Wang, Wanshi Cai, Xingyi Hang, Chenggang Zhang, Haoyang Cai, Wubin Qu, MFEprimer-3.0: quality control for PCR primers, Nucleic Acids Research, Volume 47, Issue W1, 02 July 2019, Pages W610–W613, https://doi.org/10.1093/nar/gkz351.