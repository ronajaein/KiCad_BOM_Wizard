# KiCad_BOM_Wizard

### Arthur: 
Ronald Sousa http://hashdefineelectronics.com/kicad-bom-wizard/

### Revision:
0

### Repository: 
https://github.com/HashDefineElectronics/KiCad_BOM_Wizard.git

### Project Page: 
http://hashdefineelectronics.com/kicad-bom-wizard/

# Description: 
This is the repository for KiCad_BOM_Wizard. This KiCad plugin can be used to create custom BOM files based on easy configurable templates files. The plugin is writing in JavaScript and has been designed to integrate into KiCad’s BOM plugin manager.

The Idea for this plugin came from our need to generate BOM that are specific to of our clients needs. for example, some of our clients require their product to have document traceability due to their product ATEX certificate requirement. 
With KiCad_BOM_Wizard, We simply made a template that the output file includes the document number, project revision, and manufacture notes.

By default, KiCad_BOM_Wizard comes with two templates, one will generate a stand along HTML file and the other will generate a CSV file.
They are both include to simplify the use of plugin and can be used as an example by those who want to make their own templates. The latter could be due to needing to have your own company or project logo.

KiCad_BOM_Wizard works by scanning through all of the template files and replacing any of the Short Codes with the data that is associated with it. It will then output all of the data it has collected including the file structure
into one file based on the order that it finds the short codes. 

For example, if the KiCad_BOM_Wizard finds the short code <!--TAG_TITLE--> in template.conf then it we replace it with the KiCad project root sheet title. KiCad_BOM_Wizard will also group and sort all components together that have same parts value, the same starting designator reference prefix and the same fields value. 

For example, if your project component list consist of; 
> R1 10K, R2 100K, C1 10pF and R3 10K

then it would be grouped like this;

> | Ref | qty | value |
>>>>>>> fae047601684861fc00338fcde26ded3717d71d5
> |----|-----|-----|
> |C1 | 1 | 10pF |
> | R1 R3 | 2 | 10K|
> | R2| 1 | 100K|

## For more details on how to use and make your own template that KiCad_BOM_Wizard can use, please visit the main project page. http://hashdefineelectronics.com/kicad-bom-wizard/

# The following serves as quick reference.

## installing nodejs in Linux:
```sh
sudo apt-get install nodejs
sudo apt-get install npm
```
## installing nodejs on other system:
    https://nodejs.org/en/download/

## Kicad BOM Plugin Manager Command Line:
#For HTML BOM
    node "SCRIPT_ROOT_DIR/KiCad_BOM_Wizard.js" "%I" "%O.html"
    // This is the same as
    node "SCRIPT_ROOT_DIR/KiCad_BOM_Wizard.js" "%I" "%O.html" "HTML"
    // This is the same as
    node "SCRIPT_ROOT_DIR/KiCad_BOM_Wizard.js" "%I" "%O.html" "SCRIPT_ROOT_DIR/Template/HTML"

#For CSV BOM
    node "SCRIPT_ROOT_DIR/KiCad_BOM_Wizard.js" "%I" "%O.csv" "CSV"
    // This is the same as
    node "SCRIPT_ROOT_DIR/KiCad_BOM_Wizard.js" "%I" "%O.csv" "SCRIPT_ROOT_DIR/Template/CSV"

#Using your own template BOM
    node "SCRIPT_ROOT_DIR/KiCad_BOM_Wizard.js" "%I" "%O.txt" "Path_To_Your_Template_conf/Your_Template"
    // or if you are using the plugin template directory to store your template. "SCRIPT_ROOT_DIR/Template/"
    node "SCRIPT_ROOT_DIR/KiCad_BOM_Wizard.js" "%I" "%O.txt" "Your_Template"

where "%I" in the input kicad xml file and "%O" is the output directory and name for the BOM. This must include your file extension

## short_codes list used by the template files

### for template.conf:
    <!--TITLE-->                        inserts the root sheet title.
    <!--DATE-->                         inserts the root sheet date.
    <!--DATE_GENERATED-->               inserts the date and time the Kicad net file was created
    <!--COMPANY-->                      inserts the root sheet company name
    <!--REVISON-->                      inserts the root sheet revision value
    <!--COMMENT_1-->                    inserts the root sheet comment 1
    <!--COMMENT_2-->                    inserts the root sheet comment 2
    <!--COMMENT_3-->                    inserts the root sheet comment 3
    <!--COMMENT_4-->                    inserts the root sheet comment 4
    <!--TOTAL_NUM_OF_PARTS-->           inserts the number of parts used in the design
    <!--TOTAL_NUM_OF_UNIQUE_PARTS-->    inserts the number of unique parts used in the design. Note, if two similar parts have different fields then it will be register as unique
    <!--CLASS_HEADER_TAG-->       inserts the table headers
    <!--BOM_TABLE-->                    inserts the complete generated BOM table

### for headers.conf:
    <!--HEADER_ROW-->         inserts the column title
    <!--HEADER_CLASS_REF_TAG-->        insert the tag for the part reference. HeadRefTag 
    <!--HEADER_CLASS_QTY_TAG-->        insert the tag for the part qty. HeadQtyTag
    <!--HEADER_CLASS_VALUE_TAG-->        insert the tag for the part value. HeadValueTag

### for group.conf:
    <!--GROUP_ROW_DATA-->       inserts the group of parts row data
    <!--GROUP_CLASS_TAG-->    inserts the group class name. format "group_" + "part ref prefix"
    <!--GROUP_TITLE_TEXT-->    inserts the group title. the part ref prefix

### for row.conf:
    <!--ROW_PART_REF-->            inserts the list of parts reference designator
    <!--ROW_PART_QTY-->            inserts the number of parts grouped together
    <!--ROW_PART_VALUE-->          inserts the part value
    <!--ROW_PART_FIELDS-->          inserts the generator parts fields
    <!--ROW_CLASS_ODD_EVEN_TAG-->    returns RowEvenTag on even rows or RowOddTag for odds rows.
    <!--HEADER_CLASS_REF_TAG-->         insert the tag for the part reference. HeadRefTag 
    <!--HEADER_CLASS_QTY_TAG-->         insert the tag for the part qty. HeadQtyTag
    <!--HEADER_CLASS_VALUE_TAG-->         insert the tag for the part value. HeadValueTag

### for fields.conf:
    <!--FIELD_CLASS_TAG-->    inserts the fields class name
    <!--FIELD-->              inserts the field value
