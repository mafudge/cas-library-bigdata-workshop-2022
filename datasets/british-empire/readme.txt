---------------------------------------------
-- Enslaved Peoples of the British Empore ---
---------------------------------------------

Syracuse University Fall 2020

Professors Tessa Murphy Ph.D
Professor of Practice Michael Fudge 


About The Project
-----------------

Data Source is British Registries of Enslaved Peoples circa 1815 for the island of Saint Lucia. 
Great Britain had recently secured the island in 1814 from the French following the Treaty of Paris 
ending the Napoleonic Wars. Registry is in French. 

Each page in the registry corresponds to a plantation (sometimes running to several pages, based on
number of people). Each plantation is listed by name, owner, and usually location (by parish within
the island) across the top of the page.

Enslaved people are then organized into three groups: “liste generale des familles” (general list
of families; usually female-headed, and often multi-generational); then “liste generale des 
esclaves males” (enslaved men without family on the plantation); then “liste generale des 
esclaves femelles” (enslaved women without family on the plantation).

The purpose of this project is to digitize these records, process them, and present a clean 
dataset for research in the spirit of digital humanities. Analysis can be completed through 
various methods including visual to answer important questions about the lives of these 
individuals. The ultimate goal is to create an artifact such as an API or Website for the 
data which is easily accessible for other researchers.

How The Data Got Here
---------------------

The first step in this process was data collection and input. Tessa Murphy and her team of research
assistants worked on data collection and translation for each page in the registries. Each page is
a scanned photo of the actual page from the registry document. The registy is in French and written
with a quill pen in curvsive script. For this reason, Optical Character Recognition (OCR) does
not perform well on the scanned images, so the entire registry, hundreds of pages, were 
transcribed by hand into a google sheet.

Michael Fudge's team of students worked on Python code to read each of the google sheets, perform
some data quality screens, and then export the data into JSON files. These JSON files are 
in the "individuals" folder and the names of the file correspond to the ID number of the 
google sheet (these are globally unique). 

In addition the plantation information was extracted from each sheet and combined into a single
file in the plantation folder. This is available in two formats CSV and JSON.

The data you see here is just 15 plantations from the registry. The actual registry has close
to 300 plantations, and over 12,000 individuals.

Data quality Issues
-------------------

People make mistakes. There are issues with:

- The original transcription of the registry. Spelling errors, inconsistencies, etc.
- The data tranlated into the google sheets. Language translation errors, typographical errors,
  etc.

The proper approach to data quality is to never overwrite the source data. Instead, you should
dervie new data from the source data. For example in a column of values that are yes/no, you
might see Y,N, Yes, No, YES, No, Oui, Non, etc. Some of these inconsistencies from the original
registry, others are from the human translation and transcription of the data. We data engineers
clean up this data, but its very important to leave the original data intact for data lineage.
The same data is in 3 places: the original registry, the google sheets, and the JSON files. 
Therefore it is important to not lose history of the data in case we need to refer back to the
google sheet or the original registry.

Derived columns in the dataset have an asterisk (*) after the column name. This is to indicate
the data is not part of the original document, but derived from the original data.


Lookup Data
-----------

To improve data quality, we created a lookup tables for various fields like color, employment,
general employment, and parish. These files are in the "lookup-data" folder in JSON format. 
Each file has the corrected field as a key and a list of possible incorrections from dataset.


Data Science in this dataset
----------------------------

There is a lot of data science potential in this dataset:

- With hundreds of transcribed pages, can an approach to building the plantion and individual 
  data be created from the image itself?
- Can we create models to autocorrect common errors in the data?
- Can categorical generation be used to improve the lookups categories?
- Can lineage be derived from the family infomation of the registry? (Right now this is ddone
  manually)


Data Dictionary 
---------------

Below is a data dictionary of column values located in the final dataset. Engineered columns are
denoted with an asterisk following the column name. Tessa Murphy has indicated some thoughts on
information that could potentially be derived from this information below some of the column 
definitions:

Individual information
----------------------

**Individual ID**
    -Unique identifyer for an individual within a particular plantation. 
    -Unique only to plantation. 
    -Use Individual ID along with the File Id* is a globalluy unique identifier for the 
     individual.

**Nom** (first name)

**Surnom** (surname):

**Couleur** (color):
	-Gender (terms ending in ‘esse’ indicate women). Used to get gender.

**Emplois** (employment)
	-Variety of work.

**Age**
	-in years, sometimes in months

**Taille** (height)
	-Usually in Ft and Inches

**Pays** (Country)
	-Place of origin.

**Marques** (Marks)
    -Marks on the body.
	-Disease (e.g. “marques de petite verolle” = smallpox scars).
    -Clusters of scarred people may point to outbreaks at a given moment 
     (e.g. if no one over age x has these scars, we can probably assume an outbreak occurred prior to x date).
	-Violence (e.g. “doigts coupes” = fingers cut off; “brulure” = burn marks).
	-Ritual scarification (e.g. “marques de son pays”) indicates that they reached adulthood initiation 
     in West African; may eventually allow people to pinpoint more precise places of origin 
     (different scarification practices in different tribes).

**Parente** (family relations) **ORIGINAL COLUMN NOT IN FINAL DATASET --> SEE IMMEDIATELY BELOW**
	-The ID of the individual's parent.

**Female Parent** (Individual ID)
    -Directly Corresponding to the Individual ID of the female parent in the Parente column.
    -ID used for the future purpose of creating family trees.

**Male Parent** (Individual ID)
    -Directly Corresponding to the Individual ID of the male parent in the Parente column.
    -ID used for the future purpose of creating family trees.

**Other Relations** (Relationship - First Name, Surname)
    -Contains all other relationships excluding parental for the individual.
    -Relationship listed followed by first name and surname of relations.

**Corrections**
	-Rarely used.
	-Occasionally shows if a person has run away (“marron”), died, or been sold.

**Gender** (M/F)
    -Male or female gender of individual

**Family** (Y/N)
    -Is the individual known to have family on the plantation or is there individually.
    -Yes or No.

**Registry Page Number** (Actual Physical)
    -Eventual link of where the registry image will be stored.
    -Corresponds to Page Reference column.

**Page Reference** (Ancestry Pointer)
    -Registry page where individual information is located.
    -Corresponds to Registry Page Number column.


Plantation Information
----------------------

**Plantation Name**
    -Name of individual's plantation.

**Owner**
    -Name of known plantation owner.

**Manager** (If Applicable)
    -Name of known plantation manager.
    -Many times the manager performed day-to-day plantaion management.

**Location** (Parish)
    -Location of plantation on Saint Lucia.
    -Location denoted as parish the plantation resided in.

**Main Production**
    -Main agricultural or manufacturing product of the plantation.

**Number of Enslaved People**
    -Known number of enslaved people on the plantation.
    -This should match the count of the individuals in the plantation.

**Sex of Owner**
    -Known sex of plantation owner.

**Date of Registry** (If Applicable)
    -Listed date that the registry page was completed for the plantation. 

**Signature**
    -Individual who provided a signature for the registry content of a particular plantation.
    -May be owner, manager, registrar, or other possible individuals.

