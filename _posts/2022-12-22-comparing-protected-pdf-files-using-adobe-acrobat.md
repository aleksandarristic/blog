---
title: "Comparing protected PDF files using Adobe Acrobat"
date: 2022-12-22 13:51:00 +0000
layout: post
tags:
  - "pdf"
  - "howto"
  - "compare pdfs"
  - "adobe"
  - "tips"
---

Reviewing long PDF documents, especially ones with a lot of legalese, is cumbersome and tedious work. It takes a lot of time to just read and understand everything, especially when the document is not written in your native language.

So, you read, and you finish the review, and you have a set of changes you need to do. The other party makes the changes, and then you need to confirm that everything is as you requested.

And what if the old and the new version of your PDF document is protected?

## The Problem

I found myself in a pickle once with protected PDFs. I needed to quickly review and compare two versions of PDF documents, ~100 pages long, with my changes added; turned out that the other side has also added ~150 changes to the document that are less relevant, such as spelling and grammar. And both previous and the new version were protected, thus disabling my ability to simply feed both documents to Adobe Acrobat Pro DC and compare. Due to the protections, I was unable to edit, or even copy the text. Naturally, Acrobat Pro refused to compare such protected documents without a password, which I didn't have.

So, I had to somehow remove the protection from the PDFs in order to be able to use the Acrobat Pro DC for comparison. Or revert to reading the whole document along with comparing important parts with the older version manually. Nuh-uh.

## The Solution

#### Removing PDF protection

**Enter Google Chrome's (or any other chromium based browser really) builtin PDF reader, and the browser's ability to print files to PDF.** Here are the steps that I had used to remove the protection on the PDFs:

1.  open the protected PDF in Google Chrome (or other chromium based browser)
2.  type CMD-P (or CTRL-P) to open browser's print dialog.
3.  proceed to change the target to "save as pdf" (sometimes it's the default option)
4.  save the file to a directory of your choice.

**Voilà!** You now have an unprotected PDF file with the same content as the original! You do this twice, for each version/copy of your protected document, and you are ready to proceed to comparison.

#### Comparing the PDFs

In order to compare the two documents, I've used the Adobe Acrobat Reader Pro DC. The comparison is really easy to do once you have removed the protections from the two files:

1.  Select "TOOLS" in the top bar
2.  Select the "Compare and highlight the differences between 2 files" option

The comparison tool is really intuitive, showing you the report page first, and then on scroll - showing you the two documents side by side with changes highlighted.

Easy, right?
