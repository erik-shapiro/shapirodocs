# Document Hub 360

## - Overview
Built w/ React, the Shapiro Document Portal, aka Hub360, is a one page app that allows the user to easily download, rename, and upload documents to/from the:

* shapiro360-docs-index, 
* glg-docs-index,
* customer-docs-index ( General )
* customer-docs-index ( FPPI Authorization )

---








## - Developer Documenation
#### __Local Setup__

###### If you don't have Git, [download it here](https://git-scm.com/downloads)
```
git clone //shap-exp-360pro/c$/inetpub/wwwroot/shapiro-apps/hub360repo
```

###### You'll need to install the package.json to get the Node_Modules
```
cd hub360repo
npm install
```

###### Spin up a local server - Terminal will run a compile everytime you save a file and alert you to errors. Keep in mind, at the time of writing, all WSDL related calls will fail.
```
npm start
```

###### When ready to create a production build;
```
npm run build
```

---







## - User Guide
#### Shapiro360/ScannedDocs

Document Names **must** follow the following convention:

```
FILEID + NAME OF FILE + EXTENSION
```
ex. 2102748ImportantFile.pdf, 2102748 Important-File.pdf 

#### GLG/TransDocs

Document Names **must** follow the following convention:

```
FILEID + NAME OF FILE + EXTENSION
```
ex. 2102748.tdocs.pdf, 2102748 - TransDocument.pdf

#### Customer Docs

Document Names **should** follow the following convention:

```
CUSTNO + NAME OF FILE + EXTENSION
```
ex. FIVEBMER POA.pdf, AirwayBill.pdf*

#### FPPI Docs

Document Names **should** follow the following convention:

```
CUSTNO + "-FPPI-" + NAME OF FILE + EXTENSION
```
ex. AMTRANS-FPPI-GTS.pdf, GTS AMTRANS FPPI AUTHORIZATION.pdf**

---

*Works, but we should always push for users to include an identifier in the name, in this case, the custno

**Works, but we should maintain consistency in the event we need to gather these files programmatically by name.