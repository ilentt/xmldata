# 1. Define product

To defined product you should organization your product have one or many parent and child. Each parent or child always have one or many attribute depend on your product or business requirement. The product maybe Book, Fashion, Food, Machine compo

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
- `path` : path link to data product definition at `step 1`
- `alias` : short name represent path, it's also variable name using query select product statement
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
		<description>Data source Vietnam cuisine</description>
		<enabled>1</enabled>
	</datafile>
	<datafile>
		<id>10</id>
		<path>images</path>
		<alias>image</alias>
		<description>Picture Vietnam cuisine</description>
		<enabled>1</enabled>
	</datafile>
</datafiles>
``` 
#### Result after define data file
![result after define data file](https://i.imgur.com/gs34Ezb.png)

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
		<description>!! There will be max. 100 products loaded !!</description>
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
		<description>Level 2, Product</description>
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
		<description>!! There will be max. 100 products loaded !!</description>
		<hasstringids>0</hasstringids>
</panelstatement>
```
#### 3.4 Final result
![](https://i.imgur.com/PbMEfC4.png)

**Caution:** don't modifies default `<panelstatement>` if you don't clearly understand what you do. More detail document at `InDesign/Plugins/products.html#Rueckgabewerte`
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
		<statement></statement>
		<label>Filter product by condition(id,label,...)</label>
		<sequencenr>10</sequencenr>
		<hasstringids>0</hasstringids>
		<candelete>0</candelete>
	</findstatement>
</findstatements>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU3MjkzNDMsLTE3MDY3MjQ3NTYsLTE3MD
gxNzYyODksMTc4NTE3Mjk0NCwxOTEzNDU4NDE4LC0xMzg4MTM0
NjYsLTk2NjE3MzkyNiwxMDI2MzIxMTUxLC05NjYxNzM5MjYsMT
AyNjMyMTE1MSwtOTY2MTczOTI2LDEwMjYzMjExNTEsMTUzMDE1
MzQzMSwtMTkxODAxMTgwNiwxOTUzNTM4MTc1XX0=
-->