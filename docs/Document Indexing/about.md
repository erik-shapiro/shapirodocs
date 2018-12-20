# Document Indexing Programs

## General Overview

Each index has their own specific rules for matching documents to Files/Customers/Parts.

* Shapiro360 and GLG docs must be placed in their intended CustNo folder with a FileID as the prefix in order to fully match.

* Customer and Profile docs simply need to be in the right CustNo folder, but its suggested to use the CustNo as the prefix.
    * FPPI documents are specialty Customer Docs that need be in the right AgentNo folder and should follow the naming convention; ```AgentNo-FPPI-DocumentName.Ext```. 

* Classification Docs simply need to be in a the right Class PartNo folder. That's about it.

* Damco Docs & MR Docs are both automated and explained in further detail below.

Regardless of Index type, we reject the following extensions;

* MSG
* TMP
* URL
* ZIP
* RAR
* HTML
* XML
* LNK
* DB

We also reject documents that begin with ```~$``` since these are temporary files. But honestly, people need to stop trying to edit files in the Upload Directory. Editing should all occur BEFORE entering the Upload Directory. Not that it will stop people. /shrug

---

## Error Logs
We keep track of error and success(history) logs in ```\\shapiro-fs01\DAILY\docindexer```. 

---

## Upload/Storage Folder Locations

**Windows Paths**

Document Type | Upload Folder | Secure Storage
------------ | ------------- | ------------
Shapiro360/ScannedDocs | ```\\shapiro-fs01\DOC-Index\WEB-SCANNEDDOCS``` | ```\\shapiro-fs01\WEB\Scanneddocs```
GLG/TransDocs | ```\\shapiro-fs01\DOC-Index\WEB-GLGDOCS``` | ```\\shapiro-fs01\GLG-DOCS```
CustomerDocs | ```\\shapiro-fs01\DOC-Index\WEB-CUSTOMERDOCS``` | ```\\shapiro-fs01\WEB\CustomerDocs```
Profile Docs | ```\\shapiro-fs01\DOC-Index\WEB-PROFILES``` | ```\\shapiro-fs01\CORP-COMMON\Profiles```
Classification Docs | ```\\shapiro-fs01\DOC-Index\WEB-ClASSDOCS``` | ```\\shapiro-fs01\WEB\ClassDocs```
Damco Docs | ```\\shapiro-fs01\DOC-Index\DAMCODOCS``` | ```\\shapiro-fs01\WEB\ClassDocs```
MR Docs | ```\\shapiro-fs01\DOC-Index\MRDOCS``` | ```\\shapiro-fs01\WEB\ClassDocs```


**Rashi Paths**

Document Type | Upload Folder | Secure Storage
------------ | ------------- | ------------
Shapiro360/ScannedDocs | ```usr5/shapiro360docs``` | ```usr5/doc-index/WEB-SCANNEDDOCS```
GLG/TransDocs | ```usr5/glg-docs``` | ```usr5/doc-index/WEB-GLGDOCS```
CustomerDocs | ```usr5/webcustomerdocs``` | ```usr5/doc-index/WEB-CUSTOMERDOCS```
Profile Docs | ```usr5/webprofiles``` | ```usr5/doc-index/WEB-PROFILES```
Classification Docs | ```usr5/webclassdocs``` | ```usr5/doc-index/WEB-CLASSDOCS```
Damco Docs | ```usr5/ftproot/mrspedag``` | ```usr5/doc-index/DAMCODOCS```
MR Docs | ```usr5/ftproot/mrspedag/mrspedag/docs``` | ```usr5/doc-index/MRDOCS```

---

## Main Indexer

**imp/docindexer.p**
```
cd /progs/mxp/mxp81e/shapsrc/imp/docindexer.p
```


**imp/mrdocs.p**
```
cd /progs/mxp/mxp81e/shapsrc/imp/mrdocs.p
```
```mrdocs.p``` will move Zip files into the ```DOC-Index\MRDOCS\0-PPROCESSING\``` folder to perform the matching and renaming, then move the completed files into the parent folder, ```DOC-Index\MRDOCS```, to queue them up for proper Indexing. 

Unmatched Zips will move to ```DOC-Index\MRDOCS\0-UNMATCHED\```, where they will sit until they are attempted to match again every other hour by ```/usr5/bin/rerun-nomatch.sh```. This is to try and catch Zips that failed a match because they arrived before the File could be created.

**imp/damcodocs.p**
```
cd /progs/mxp/mxp81e/shapsrc/imp/damcodocs.p
```
Low maintenance and typically smooth. More info will come if needed.

---
