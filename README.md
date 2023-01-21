# KDI-2021-Project-Template
This repository is a template for each KDI-Project developed during the KDI2021-22 course

##### Academic year 2021/2022, University of Trento
alessia.artusi@studenti.unitn.it emma.salti@unitn.it
<br>
<br>

### Project Description
There is an increasing interest across European Union (EU) institutions, government and healthcare systems to digitalise the healthcare ecosystem. As it was proven by the COVID-19 pandemic, a digital healthcare ecosystem would facilitate across-borders processes, creating homogeneity across Europe. There is not a single common European Electronic Health Record (EHR) system for all EU member states. The objective of this project is to develop an easy-to-use knowledge graph that,  integrated with EHR data, will result in a tool that facilitates healthcare data visibility and transfer.
The aim of the project is to build a knowledge graph (KG) concerning healthcare heterogeneous data about patients, professionals, facilities and other entities from European countries with different healthcare systems. This result could be the foundation to create a service which help the citizens to handle their own healthcare data, in a context in which the citizens moves around Europe. <br> <br>
In this project we used syntetic (alrady anonymized) data to create a prototype integration serving this purpose. We followed the iTelos development methodology and improved our soulution incrementaly - starting from hypothetical use cases all the way to the final, exploitable knowledge graph while evaluating each step of the procedure. <br> <br>
This solution can potentially be used in clinics, pharmacies, hospitals and research institutions by various personel. Security and data access aspects have not been covered in this project - the application level exploiting this KG should take care of these.
<br>
<br>

### Resources
#### Knowledge Resources:
- [FHIR](https://www.hl7.org/fhir)
- [Schema](https://schema.org)

#### Data Resources  
- [Synthea](https://synthea.mitre.org/) - 500 patients
- [EMRBOTS](http://www.emrbots.org) - 100 patients, 372 admissions and 111483 laboratory observations

All datasets have been retrieved in .csv format. They vaired greatly in the number of object and data properties with Synthea being the richest one and FHIR SMART data being the poorest one. These datasets have all been synthetically generated, therefore there was no need for anonymization.
<br>
<br>

### Informal modeling phase

In the informal modeling phase we integrated the knowledge layer with the data layer.
For the knowledge layer we classified our resources into common core and contextual for each competency query. From this classification we defined 10 ETypes and the relationships between them. Then we integrate it by adding the data properties for each etype. <br>

From this classification of resources we defined our <b>ETypes</b> and their <b>properties</b> as shown below. \par In our ER model we started with two entities, namely Patient and Doctor, since they are our ideal users of the KG; we noticed they both shared most of the data properties, such as: name, address, date of birth, gender, race and language, hence we decided to create the superclass <b>Person</b> with the data properties listed, that will be inherited by its subclasses <b>Patient</b> and <b>Doctor</b>.
We continued by modeling the data properties of the Person, since it is the entity that is linked with most data resources. We decided to model its connections as ETypes that have their own data properties.

<br>

This diagram was defined by looking at the available data, the project’s purpose and the competency questions.

<br>
<img src="Teleologies/Formal Modeling/Logic_model.jpg" width="300" align="center">

<img src="Teleologies/Informal Modeling/EER_model.pdf" width="300" align="center">
<br>

<br>

After that the dataset previously collected are filtered and handled to obtain more suit-able sets of data for our EER model.

<br>
<br>

### Formal modeling phase
 
During this phase we proceed with the creation of an ontology using Protégé, the language alignment and an initial setup for the data integration step. 

<br>

Starting from the informal schema described in the previous section, we built the ontology in Protégé. To define entities in accordance with the FHIR standard we used the resource to search our entities and the related data properties that were to be added. Next, we uploaded the base-schema to KOS in order to integrate it with the framework ontology provided by UKC (a multilingual lexico-semantic resource - Universal Knowledge
Core).
This procedure resulted in a fully formal ETG. Its structure is shown below:
<img src="">

<br>

we mapped 7 different dataset with our ETG using karma linker. The problem of identity was overcome as we basically had one dataset for each etype. While for the unique identifier problem we used identifying sets (composed uris) to determine the uri of a class.

To integrate the data accordingly we modified the datasets using python, and in particular we generated a unique table for the person EType, which does not actually exist in the original dataset. We merged the three datasets one coming from synthea patients, one coming from emrbots patients and the last one from synthea doctor data.


Due to a very low number of instances, it was decided for it to be removed from downstream
integration since it will cause sparsity. We then resolved all data type and format misalignments amont the datasets. All of our
simulated data was in English, therfore no language misalignments existed. After these steps, we have nine separete csv tables each
representing an eType with its properties.
<br>

The data integration process started by loading the ontology in owl format and the data sets into Karma. Once everything has been successfully loaded, we associated to each column its semantic type to align the data with the schema. The sub classes Patient and Doctor where not recognisedby  GraphDB, hence the data properties that were originally connected to them are now linked to the EType Person. The same goes for Hospital, which links are now on the EType Building.
The same process was applied to the five remaining data sets. No issue was encountered during this phase, except for the modification needed after noticing the error in GraphDB. After creating the models in karma for each data set we exported the files in .ttl format and
loaded them into GraphDB.
<br>

