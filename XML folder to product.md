# 1. Data file

Data file `datafiles.xml` use definition data source need load into InDesign. The data file structure have main five attributes are `id`, `path`, `alias`, `description` and `enable`
```xml
<datafiles>
	<datafile>
		<id></id>
		<path></path>
		<alias></alias>
		<description></description>
		<enabled></enabled>
	</datafile>
<datafiles>
```
- `id` : start at 1 and always unique
- `path` : path link to data product definition
- `alias` : short name represent path, it's also variable name using query select product statement
- `description` : description data file(option)
- `enable` : accepted two values are `1` stand for enable and `0` stand for disable

#### Example:
```xml
<datafiles>
    <datafile>
		<id>0</id>
		<path></path>
		<alias></alias>
		<description>no</description>
		<enabled>0</enabled>
    </datafile>
	<datafile>
		<id>1</id>
		<path>uits.ml</path>
		<alias>FRUITS</alias>
		<description>Data source Vietnam cuisine</description>
		<enabled>1</enabled>
	</datafile>
	<datafile>
		<id></id>
		<path>images</path>
		<alias>image</alias>
		<description>Picture Vietnam cuisine</description>
		<enabled>1</enabled>
	</datafile>
</datafiles>
``` 
#### Result after define data file
![result after define data file](https://i.imgur.com/gs34Ezb.png)


# 2. Define product

StackEdit stores your files in your browser, which means all your files are automatically saved locally and are accessible **offline!**

#### example
```xml
<fruits>
	<fruitGroup>
		<fruitGroupID></fruitGroupID>
		<i></r>
		<rt>
			<name></name>
		<description></description>
		<uitID></fruID>
		<fruit>
			<name></name>
			<image></image>
			<country></country>
			<description></description>
			<unit></unit>
			<ordernumber></ordernumber>
			<price></price>
		</fruit>
		...one or more fruit...
	</fruitGroup>
	...one or more group...
</fruits>
```

# 3. Panel statement

Panel statement `panelstatements.xml` contain query statement select product load into InDesign
```xml
<panelstatements>
	<panelstatement>
		<id></id>
		<usage></usage>
		<domain></domain>
		<statement></statement>
		<in_parameters></in_parameters>
		<out_parameters></out_parameters>
		<description></description>
		<hasstringids></hasstringids>
	</panelstatement>
<panelstatements>
```
#### 3.1 Query statement in the panel statement id `55` for loading of all top level objects.
```xml
"$FRUITS"
select fruitGroupID, 0, 0,
3,
2069,
name, '',
0,
0,
0, "",
"", "", 0, 0,
0
node fruits.fruitGroup
```
##### Panel tatement after encode
```xml
<panelstatement>
		<id>55</id>
		<usage>Get fruitGroupID level 1</usage>
		<domain>Select all fruit group at level 1 by fruitGroupID and name</domain>
		<statement>&#34;$FRUITS&#34;&#10;select fruitGroupID, 0, 0,&#10;3,&#10;2069,&#10;name, '',&#10;0,&#10;0,&#10;0, &#34;&#34;,&#10;&#34;&#34;, &#34;&#34;, 0, 0,&#10;0&#10;node fruits.fruitGroup</statement>
		<in_parameters></in_parameters>
		<out_parameters></out_parameters>
		<description>!! Es werden max. 100 Produkte geladen !!</description>
		<hasstringids>0</hasstringids>
</panelstatement>
```
##### Result after first panel statement query
![](https://i.imgur.com/wx0jzoG.png)

#### 3.2 Query statement in the panel statement for loading of all child of top level product.
```xml
"$FRUITS"
select fruitID, <parent.ID>, 0,
3,
2022,
name, '',
toDelete,
0,
0, "",
"", "", 0, 0,
pageitemid
node fruits.fruitGroup
where fruitGroupID = <parent.ID>
node fruit
orderby fruitID
```
##### Panel statement after encode
```html
<panelstatement>
		<id>10000</id>
		<usage>Level 2, fruit</usage>
		<domain>Select all fruit, the child of fruitGroup by Id</domain>
		<statement>&#34;$FRUITS&#34;&#10;select fruitID, &lt;parent.ID&gt;, 0,&#10;3,&#10;2022,&#10;name, '',&#10;toDelete,&#10;0,&#10;0, &#34;&#34;,&#10;&#34;&#34;, &#34;&#34;, 0, 0,&#10;pageitemid&#10;node fruits.fruitGroup&#10;where fruitGroupID = &lt;parent.ID&gt;&#10;node fruit&#10;orderby fruitID</statement>
		<in_parameters></in_parameters>
		<out_parameters></out_parameters>
		<description>Ebene 2, Product</description>
		<hasstringids>0</hasstringids>
</panelstatement>
```
#### 3.3 Query statement point panel statement at `3.1` to `3.2`
```xml
"$FRUITS"
select fruitGroupID, 0, 0,
3,
2069,
name, '',
0,
10000,
0, "",
"", "", 0, 0,
0
node fruits.fruitGroup
```
##### Panel statement after encode
```xml
<panelstatement>
		<id>55</id>
		<usage>Level 1, fruitGroup</usage>
		<domain>Select all fruit group at level 1 by fruitGroupID and Name</domain>
		<statement>&#34;$FRUITS&#34;&#10;select fruitGroupID, 0, 0,&#10;3,&#10;2069,&#10;name, '',&#10;0,&#10;10000,&#10;0, &#34;&#34;,&#10;&#34;&#34;, &#34;&#34;, 0, 0,&#10;0&#10;node fruits.fruitGroup</statement>
		<in_parameters></in_parameters>
		<out_parameters></out_parameters>
		<description>!! Es werden max. 100 Produkte geladen !!</description>
		<hasstringids>0</hasstringids>
</panelstatement>
```

#### 3.4 Final result
![](https://i.imgur.com/PbMEfC4.png)

**Caution:** don't modifies default `<panelstatement>` if you don't clearly understand what you do.
# 4. Find statement 

Find statement `findstatements.xml` select panel statement load to InDesign panel.
```html
<?xml version="1.0" encoding="utf-8"?>
<findstatements>
	<findstatement>
		<id>1</id>
		<description/>
		<classid>3</classid>
		<userid>0</userid>
		<statement>
		</statement>
		<label>Show me cuisine name comon baby</label>
		<sequencenr>10</sequencenr>
		<hasstringids>0</hasstringids>
		<candelete>0</candelete>
	</findstatement>
</findstatements>
```

## Rename a file

You can rename the current file by clicking the file name in the navigation bar or by clicking the **Rename** button in the file explorer.

## Delete a file

You can delete the current file by clicking the **Remove** button in the file explorer. The file will be moved into the **Trash** folder and automatically deleted after 7 days of inactivity.

## Export a file

You can export the current file by clicking **Export to disk** in the menu. You can choose to export the file as plain Markdown, as HTML using a Handlebars template or as a PDF.


# Synchronization

Synchronization is one of the biggest features of StackEdit. It enables you to synchronize any file in your workspace with other files stored in your **Google Drive**, your **Dropbox** and your **GitHub** accounts. This allows you to keep writing on other devices, collaborate with people you share the file with, integrate easily into your workflow... The synchronization mechanism takes place every minute in the background, downloading, merging, and uploading file modifications.

There are two types of synchronization and they can complement each other:

- The workspace synchronization will sync all your files, folders and settings automatically. This will allow you to fetch your workspace on any other device.
	> To start syncing your workspace, just sign in with Google in the menu.

- The file synchronization will keep one file of the workspace synced with one or multiple files in **Google Drive**, **Dropbox** or **GitHub**.
	> Before starting to sync files, you must link an account in the **Synchronize** sub-menu.

## Open a file

You can open a file from **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Open from**. Once opened in the workspace, any modification in the file will be automatically synced.

## Save a file

You can save any file of the workspace to **Google Drive**, **Dropbox** or **GitHub** by opening the **Synchronize** sub-menu and clicking **Save on**. Even if a file in the workspace is already synced, you can save it to another location. StackEdit can sync one file with multiple locations and accounts.

## Synchronize a file

Once your file is linked to a synchronized location, StackEdit will periodically synchronize it by downloading/uploading any modification. A merge will be performed if necessary and conflicts will be resolved.

If you just have modified your file and you want to force syncing, click the **Synchronize now** button in the navigation bar.

> **Note:** The **Synchronize now** button is disabled if you have no file to synchronize.

## Manage file synchronization

Since one file can be synced with multiple locations, you can list and manage synchronized locations by clicking **File synchronization** in the **Synchronize** sub-menu. This allows you to list and remove synchronized locations that are linked to your file.


# Publication

Publishing in StackEdit makes it simple for you to publish online your files. Once you're happy with a file, you can publish it to different hosting platforms like **Blogger**, **Dropbox**, **Gist**, **GitHub**, **Google Drive**, **WordPress** and **Zendesk**. With [Handlebars templates](http://handlebarsjs.com/), you have full control over what you export.

> Before starting to publish, you must link an account in the **Publish** sub-menu.

## Publish a File

You can publish your file by opening the **Publish** sub-menu and by clicking **Publish to**. For some locations, you can choose between the following formats:

- Markdown: publish the Markdown text on a website that can interpret it (**GitHub** for instance),
- HTML: publish the file converted to HTML via a Handlebars template (on a blog for example).

## Update a publication

After publishing, StackEdit keeps your file linked to that publication which makes it easy for you to re-publish it. Once you have modified your file and you want to update your publication, click on the **Publish now** button in the navigation bar.

> **Note:** The **Publish now** button is disabled if your file has not been published yet.

## Manage file publication

Since one file can be published to multiple locations, you can list and manage publish locations by clicking **File publication** in the **Publish** sub-menu. This allows you to list and remove publication locations that are linked to your file.


# Markdown extensions

StackEdit extends the standard Markdown syntax by adding extra **Markdown extensions**, providing you with some nice features.

> **ProTip:** You can disable any **Markdown extension** in the **File properties** dialog.


## SmartyPants

SmartyPants converts ASCII punctuation characters into "smart" typographic punctuation HTML entities. For example:

|                |ASCII                          |HTML                         |
|----------------|-------------------------------|-----------------------------|
|Single backticks|`'Isn't this fun?'`            |'Isn't this fun?'            |
|Quotes          |`"Isn't this fun?"`            |"Isn't this fun?"            |
|Dashes          |`-- is en-dash, --- is em-dash`|-- is en-dash, --- is em-dash|


## KaTeX

You can render LaTeX mathematical expressions using [KaTeX](https://khan.github.io/KaTeX/):

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$

> You can find more information about **LaTeX** mathematical expressions [here](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference).


## UML diagrams

You can render UML diagrams using [Mermaid](https://mermaidjs.github.io/). For example, this will produce a sequence diagram:

```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

And this will produce a flow chart:

```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMwODk5NDQwMyw3OTc4OTQ4MDMsNTI5MD
Q1ODg0LDY3MzMzMzU0MywtODE4NjM4MjMyLC0xNDgwODQ2NjY4
LC0xMDI1NjkwMTYzLDIwOTYwOTAzMTcsLTE1MTI5NTYxMTYsLT
E5NTg1NDkwOTEsMTAyNjcwMTg1OSwtMzMyNDU1MzYzXX0=
-->