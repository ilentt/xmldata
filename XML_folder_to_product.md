# 1. Define product

To defined product you should organization your product have one or many parent and child. Each parent or child always have one or many attribute depend on your product or business requirement. The product maybe Book, Fashion, Food, Machine component, etc.

Below example I defined my product is the fruit. The fruit have two type or more type are fruit from Tropical and other from Temperate Regions or Frigid Regions. Each fruit have some attribute such as id, name, image, country, description, unit, order number, price. The fruit store in the file `fruit.xml`

#### Example:
```xml
<fruits>
	<fruitGroup>
		<fruitGroupID></fruitGroupID>
		<name></name>
		<description></description>
		<fruit>
			<fruitID></fruitID>
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
# 2. Data file
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
- `path` : path link to data product definition at step define product
- `alias` : short name represent path, it's also variable name using in query select product statement
- `description` : description data file(option)
- `enable` : accepted two values are `1` stand for enable and `0` stand for disable

#### Example:
```xml
<?xml version="1.0"?>
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
		<path>cuisine.xml</path>
		<alias>CUISINE</alias>
		<description>Data source Vietnam cuisine</description>
		<enabled>0</enabled>
	</datafile>
	<datafile>
		<id>5</id>
		<path>fruits.xml</path>
		<alias>FRUITS</alias>
		<description>Data source the fruit</description>
		<enabled>1</enabled>
	</datafile>
	<datafile>
		<id>10</id>
		<path>images</path>
		<alias>image</alias>
		<description>the picture of fruit</description>
		<enabled>1</enabled>
	</datafile>
</datafiles>
``` 
**Note:** Data file have two type data are `xml data` and `image`. You must define exactly path link to data. If you configuration correctly the result will be same with below result. Gray icon meaning data is disabled, blue icon meaning enable and already to use. References document `InDesign/Plugins/w2plugins.html` section `Settings` for more detail.

#### Result after define data file correctly
![result after define data file](https://i.imgur.com/gs34Ezb.png)

# 3. Panel statement

Panel statement `panelstatements.xml` contain query `<statement>` select product load into InDesign
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
### 3.1 Query statement in the panel statement id `55` for loading of all top level objects.
To select all top level my fruit product already define at `step 1` you need declare data source first. In this case data source is `FRUITS` in `datafile.xml` at `step 2` this value and the value in InDesign panel `Settings` are one. The data source begin by `$` and surround by double character `"` finally the data source will be similar `"$FRUITS"`

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
To query all top product fruit in `datafile.xml` in this case you need select `fruitGroupID`, `name` from node `fruits.fruitGroup`. The query statement like above example, at this step you should skip other value in the query statement, just using it like default value.

#### Panel statement after encode
Always define panel statement with id `55` for loading all top level product. After write query statement you need encode to avoid suddenly error when InDesign reading your define. The `<statement>` after encode in `<panelstatement` like example below. References document `InDesign/Plugins/products.html` section `Treeview Panel statements` for more detail.
```xml
<panelstatement>
		<id>55</id>
		<usage>Get fruitGroupID level 1</usage>
		<domain>Select all fruit group at level 1 by fruitGroupID and name</domain>
		<statement>&#34;$FRUITS&#34;&#10;select fruitGroupID, 0, 0,&#10;3,&#10;2069,&#10;name, '',&#10;0,&#10;0,&#10;0, &#34;&#34;,&#10;&#34;&#34;, &#34;&#34;, 0, 0,&#10;0&#10;node fruits.fruitGroup</statement>
		<in_parameters></in_parameters>
		<out_parameters></out_parameters>
		<description>!! There will be max. 100 products loaded !!</description>
		<hasstringids>0</hasstringids>
</panelstatement>
```
#### Result after first panel statement query

![](https://i.imgur.com/wx0jzoG.png)

### 3.2 Query statement in the panel statement for loading of all child of top level product.
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
#### Panel statement after encode
```html
<panelstatement>
		<id>10000</id>
		<usage>Level 2, fruit</usage>
		<domain>Select all fruit, the child of fruitGroup by Id</domain>
		<statement>&#34;$FRUITS&#34;&#10;select fruitID, &lt;parent.ID&gt;, 0,&#10;3,&#10;2022,&#10;name, '',&#10;toDelete,&#10;0,&#10;0, &#34;&#34;,&#10;&#34;&#34;, &#34;&#34;, 0, 0,&#10;pageitemid&#10;node fruits.fruitGroup&#10;where fruitGroupID = &lt;parent.ID&gt;&#10;node fruit&#10;orderby fruitID</statement>
		<in_parameters></in_parameters>
		<out_parameters></out_parameters>
		<description>Level 2, Product</description>
		<hasstringids>0</hasstringids>
</panelstatement>
```
### 3.3 Query statement point panel statement at `3.1` to `3.2`
The query statement at `3.1` point to `3.2` by `<id>10000</id>` The final query statement will be similar below example.
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
		<description>!! There will be max. 100 products loaded !!</description>
		<hasstringids>0</hasstringids>
</panelstatement>
```
### 3.4 Final result
![](https://i.imgur.com/PbMEfC4.png)

**Caution:** don't modifies default `<panelstatement>` if you don't clearly understand what you do. More detail document at `InDesign/Plugins/products.html#Rueckgabewerte`

# 5. Create place holder
The place holder define in `placeholder.xml` we have may type of place holder. But in this tutorial I just introduce two type are image place holder  is `imageframe` and text place holder is `text`

### 5.1 Image place holder
To get image of product you need two step.  

**Step 1:**  Define the place holder using place holder type `imageframe`<br/>
in `<placeholder>` need define and initial value some attribute such as
- `id` : id of place holder
- `name` : name of place holder
- `type` : type of place holder
- `domain` : using group one or many place holder
- `domainid` : 
- `load` : xml query select attribute product in `datafile.xml`
- `active` : `1` is enable and `0` is disable<br/>
The rest other attributes are also requirement but in this step maybe blank or using default value is `0`
```xml
<?xml version="1.0" encoding="utf-8"?>
<metadata>
	<placeholder>
		<id>100</id>
		<name>Fruit image</name>
		<description></description>
		<type>imageframe</type>
		<domain>ILen Tutorial</domain>
		<domainid>0</domainid>
		<class>0</class>
		<load>200</load>
		<sync>0</sync>
		<store>0</store>
		<lov>0</lov>
		<objectnameid>0</objectnameid>
			<color name="">
				<id>0</id>
			</color>
		<style></style>
		<styleid>0</styleid>
		<syncstateinvisible>0</syncstateinvisible>
		<loadconstraint>0</loadconstraint>
		<active>1</active>
	</placeholder>
</metadata>
```
Above example show how to define a place holder image frame. If everything is correctly place holder will display in inDesign place holder panel similar below image.

![place holder](https://i.imgur.com/x64uxb1.png)

**Step 2:**  Define the xml query select attribute of product in `datafile.xml`, in this case you will select image of fruit from datasource `fruit.xml` <br/>
1. In `Place Holder Values` panel open script editor at `load: 200` you have define in previous step
2. Write xml query select `image` attribute of `fruit.xml` product. The query similar below example
```
"$FRUITS"
xmlget "$COMETDATA" || "/" || image, 5
node fruits.fruitGroup
where fruitGroupID = <ID2>
node fruit where fruitID = <ID>
```
- `$FRUITS` : data source have already define in `datafile.xml`
- `$COMETDATA` : syntax must to have
- `image` : attribute of product have already defined in `datafile.xml`
- `5` : position image in frame, value `5` meaning middle center, you can try another value such as `4`, `6` etc.
- `<ID> & <ID2>` : are current selected product in Indesign product panel
3. After write xml query you need save it, the script will be encrypt and stored in folder `actions`. In this example the script will named `200.crpt`. To update this script you have to declare in `Place Holder Values` at `load` input text position.

References document `cscript/xmlquery.html#XML_Query_Language` for more detail.

After all your definition will be similar below example

![](https://i.imgur.com/4kuqr8j.png)

**Finally:** checking your work, if everything is correctly your result will be similar below example.
![](https://i.imgur.com/dBo3480.jpg)

# x. Find statement 

Find statement `findstatements.xml` select panel statement load to InDesign panel.
```html
<?xml version="1.0" encoding="utf-8"?>
<findstatements>
	<findstatement>
		<id>1</id>
		<description/>
		<classid>3</classid>
		<userid>0</userid>
		<statement></statement>
		<label>Filter product by condition(id,label,...)</label>
		<sequencenr>10</sequencenr>
		<hasstringids>0</hasstringids>
		<candelete>0</candelete>
	</findstatement>
</findstatements>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjgzNDkzMDkwLDE5MTEyNzc4ODcsLTMzND
QzNTU1MiwxNzk5NzA1MDQ3LDQzOTI3NDgyNSwtMTkwMzE2MzIz
NywtNzQ2MzY0NDM4LDE3NDE3NTc4MzcsMTcwOTMwMTY3MCw1ND
Q3MTAyNDAsMTYwNzI1MTk3NywxNDI1NjY4MjY2LC0yMTQ0MzU5
ODMsLTE3MDY3MjQ3NTYsLTE3MDgxNzYyODksMTc4NTE3Mjk0NC
wxOTEzNDU4NDE4LC0xMzg4MTM0NjYsLTk2NjE3MzkyNiwxMDI2
MzIxMTUxXX0=
-->