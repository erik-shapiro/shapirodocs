# Shapiro MIS Documentation
---

## [Ocean Insights](Ocean Insights/about)

* Shapiro Subscription Program: imp/callOceanInsightsTracking.p 

* [Check OI data](https://shapiro360.shapiro.com/index.php/run-app?app=ocean-insights-request-mbl)

* [OI API Data](https://shapiro360.shapiro.com/index.php/run-app?app=ocean-insights-request-mbl)

* [Carrier List](https://capi.ocean-insights.com/containertracking/v2/carriers)

---


## Ports America

* [Shapiro Subscription URL](https://shapiro360.shapiro.com/index.php/run-app?app=test-ports-america)

* [Shapiro Response URL](https://shapiro360.shapiro.com/index.php/ports-america-response)

---

## Regen The WSDL

* [How to regen the WSDL](regenwsdl)

---

## Database Changes

---

## Crontab Changes

* Change the crontab by typing: crontab -e -u demon1

---

## Reset The Demons

1.	cd to /usr/bin
*	run stopdemons
*	cd to /data3/demon1
*	remove STOPDEMON1
*	run new.start.demon as su

---

## Local Documentation Setup

* Download [Git](https://git-scm.com/downloads).
* Install [Python](https://www.python.org/downloads/release/python-371) (be sure to "Add Python 3.7 to PATH"). It should include pip by default.
* Go into your favorite directory.
* Right-click in whitespace and click "Git Bash Here".
* A command prompt will come up. Run the following:
    * pip install mkdocs (this will install mkdocs, which you need to compile the documentation into HTML pages)
    * git clone //shap-exp-360pro/c$/inetpub/wwwroot/shapiro-cms/templates/rt_metamorph/shapirodocumentaion
    * cd shapirodocumentaion
    * git checkout master
* Now make your changes. Once you are done, commit your raw changes to the master branch:
    * git pull (make sure you have the latest version of the repository)
    * git status (make sure your files show up as edited)
    * git add {filename} (stage your files for a commit)
    * git commit (be sure to add a commit message on the next screen, or else your commit will not take)
    * git push (this pushes your raw changes to the master branch)
    * mkdocs gh-deploy (this compiles the documentation site and syncs it to 360)
* Please remember to do both: push to the master branch and deploy the new site. These are two distinct branches, and updating one will not update the other.

---

## Miscellaneous

* [Tickets](https://shapiro360.shapiro.com/administrator/index.php?option=com_shapiroreports&c=ticket&view=ticket)
* [Reports](report)
* [M+R ASN](mr-asn)
* [New Shapiro 360 Login](360-login)
* [Database Class](db)

---

## Lost ISF

If someone transmits an ISF, and the file gets nuked, find the ABI response using abitoday and the BL number.

```
A1303655ROBERT113018     SN                                          vvvvvvvvv
B  1601655SN                                                         vvvvvvvvv
SF10101ACTEI wwwwwwwwwwww           10xxxxxxxxxxxxxxx    zzzzzzzzzzzz   018
SF15BMyyyyyyyyyyyyyyy
SF9002   ISF ACCEPTED
Y  1601655SN00004
Z1303655      113018
```

* xxxxxxxxxxxxxxx - transaction number
* yyyyyyyyyyyyyyy - BL number
* vvvvvvvvv - batch number

The user will need to create a new file and fill out the ISF module. Don't retransmit the ISF, though.

Put the transaction number in a note on the new file like this:

* Note type - "A"
* Note login - "ABI"
* Note text - "ISF ACCEPTED - " + {transaction number}

And put it in isf-header.isf-trans-no.

---

## [Automated Document Packages](ADP/about)

---

## [Document Indexing](Document Indexing/about)

---

## [Hub 360](Hub360/about)

* [Production Site URL](https://shapiro360.shapiro.com/hub360/)

---

## [ShapTable](ShapTable/about)

---

## [ShapAlert](ShapAlert/about)

A basic replacement of the vanilla JS alert(); function. 

---