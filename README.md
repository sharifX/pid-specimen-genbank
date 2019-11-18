# pid-specimen-genbank

# Introduction 

A page to demonstrate various identifiers and linkages. 

Natural history collections held in museums preserve and archive various types of biological and geological specimens that provide a physical record of the biodiversity in the natural world. Together, these specimens and associated data (such as publications and molecular sequence data) represent an unparalleled resource to study the world's biodiversity. However, the current ecosystem of specimens and collection management globally is highly fragmented with discipline-specific data and metadata standards and institutional repositories. A consistent and sustainable approach to linking these physical specimens with other identifiable entities is then crucial for findability, accessibility, interoperability, and reproducibility i.e., achieving data ‘FAIRness’. 

Within the [DiSSCo](https://dissco.eu) project, we are proposing the idea of [Digital Specimens](https://doi.org/10.3897/biss.3.37033) that act as representations of physical specimens. We are envisioning a ‘unified knowledge graph’ where Digital Specimens sit as nodes in an interconnected graph of connections between specimens, and connections from specimens to third-party sources of data/information related to those specimens. 


# Background information 

* Persistent Identfiers (PID) 

"A persistent identifier (PI or PID) is a long-lasting reference to a document, file, web page, or other object. The term "persistent identifier" is usually used in the context of digital objects that are accessible over the Internet"

* Digital Object 
https://www.dona.net/sites/default/files/2018-11/DOIPv2Spec_1.pdf

"A digital object (DO) is a sequence of bits, or a set of sequences of bits, incorporating a work or portion of a work or other information in which a party has rights or interests, or in which there is value, each of the sequences being structured in a way that is interpretable by one or more computational facilities. Each DO has, as an essential element,
an associated unique persistent identifier, known as digital object identifier (referred to informally as a handle)."

* Physical and Digital Specimen ID 

This is a physical specimen id (often times maintained in the collection management system by the collection holding institute) : B100586893. The URL associated with this physical specimen: http://herbarium.bgbm.org/object/B100586893

This is a Digital Specimen ID (20.5000.1025/c4942d87a9f89d8929c1 which is a handle): http://hdl.handle.net/20.5000.1025/c4942d87a9f89d8929c1

And links to the physical specimen id 

```
curl -s -X GET http://nsidr.org:8080/objects/20.5000.1025/c4942d87a9f89d8929c1?jsonPointer=/physicalSpecimenId|jq
" B 10 0586893"
```


# Example Use cases 

## Which specimens have associated sequences in GenBank? 

Physical specimen/catalog id: 
```
RMNH 6001 
RMNH D 38033
RMNH:MOL:256470
RMNH:MOL:47312
````
### Search in the ENA database 

String search "RMNH*". We only grab a few fields and the first 5 records in tsv format. 
```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "result=sequence_release&query=specimen_voucher%3D%22RMNH*%22&fields=specimen_voucher%2Caccession%2Cscientific_name%2Ctax_id&limit=5&format=tsv" "https://www.ebi.ac.uk/ena/portal/api/search"

```
Output in tsv format: 

```
                                                                                                                        |           |                                   |        | 
|------------------------------------------------------------------------------------------------------------------------|-----------|-----------------------------------|--------| 
| specimen_voucher                                                                                                       | accession | scientific_name                   | tax_id | 
| RMNH 6001 Type specimen, Nationaal Natuurhistorisch Museum, Leiden (formerly Rijksmuseum van der Natuurlijke Historie) | AF371257  | Cylindraspis vosmaeri             | 180178 | 
| RMNH D 38033                                                                                                           | AM076944  | Stylodactylus serratus            | 342640 | 
| RMNH D42542 (Naturalis Museum Leiden)                                                                                  | AM180692  | Palicus caronii                   | 364890 | 
| JWS700;RMNH:MOL:256470                                                                                                 | AY382068  | Albinaria puella puella           | 256890 | 
| SK616;RMNH:MOL:47312                                                                                                   | AY382076  | Isabellaria isabellina isabellina | 127721 | 
```

How can I verify that these records are indeed currently housed in the Naturalis Biodiversity Center formerly Rijksmuseum van der Natuurlijke Historie? 

EMBL record details https://www.ebi.ac.uk/ena/browser/api/embl/AM076944.1?lineLimit=1000


OLD ENA browser https://www.ebi.ac.uk/ena/data/view/AM076944


```
RN   [2]
RA   Cleva R., Van Wormhoudt A.E.;
RT   "On two rare and poorly known species, Stylodactylus discissipe Bate, 1888,
RT   and S. serratus A Milne Edwards, 1881 (crustacea decapoda :
RT   Styodactylydae)";
RL   Unpublished.
XX
DR   MD5; 3cc4a3ac26715ebcc7bcdb8d4352b881.
XX
FH   Key             Location/Qualifiers
FH
FT   source          1..300
FT                   /organism="Stylodactylus serratus"
FT                   /organelle="mitochondrion"
FT                   /mol_type="genomic DNA"
FT                   /country="Atlantic Ocean:Caribbean Islands St Vincent"
FT                   /isolation_source="sea water, trawl at -800m"
FT                   /specimen_voucher="RMNH D 38033"
FT                   /db_xref="taxon:342640"
FT   rRNA            1..>300
FT                   /gene="16S rRNA"
FT                   /product="16S ribosomal RNA"
```

After some manual investigation (via gbif record https://www.gbif.org/occurrence/2434390604) we find this: 

https://bioportal.naturalis.nl/specimen/RMNH.CRUS.D.38033

Looks like the ENA record is missing the full physical specimen id. How can we do this in a more efficient way? 

### Digital Specimen 

Only if we had a digital object connecting all these links:  

```
curl -s -X GET http://145.136.243.81:8080/objects/test/7ae4c781214bda021013|jq
{
  "id": "test/7ae4c781214bda021013",
  "scientificName": "Stylodactylus serratus Milne-Edwards, 1881",
  "physicalSpecimenId": "RMNH.CRUS.D.38033",
  "stableIdentifier": "https://data.biodiversitydata.nl/naturalis/specimen/RMNH.CRUS.D.38033",
  "accession": "https://identifiers.org/ena.embl:AM076944",
  "tax_id": "https://identifiers.org/taxonomy:342640"
}
```

## Can I track down all the specimens in this paper? 

Tenorio, Manuel J. & Castelin, Magalie, 2016, Genus Profundiconus Kuroda, 1956 (Gastropoda, Conoidea): Morphological and molecular studies, with the description of five new species from the Solomon Islands and New Caledonia, European Journal of Taxonomy 173, pp. 1-45: 19-21

http://dx.doi.org/10.5852/ejt.2016.173

Plazi link: http://treatment.plazi.org/id/24768796-CD1F-FFC6-FDED-1609FD27FE12

RDF LINK http://treatment.plazi.org/id/24768796-CD1F-FFC6-FDED-1609FD27FE12.rdf 


This uses darwin core terms 
```
grep 'dwc:specimenCode>' 24768796-CD1F-FFC6-FDED-1609FD27FE12.rdf 
<dwc:specimenCode>IM-2007-34878</dwc:specimenCode>
<dwc:specimenCode>IM-2007-30935</dwc:specimenCode>
<dwc:specimenCode>IM-2007-30338</dwc:specimenCode>
<dwc:specimenCode>IM-2007- 34880</dwc:specimenCode>
<dwc:specimenCode>IM-2007-34879</dwc:specimenCode>

grep 'dwc:httpUri' 24768796-CD1F-FFC6-FDED-1609FD27FE12.rdf 
<dwc:httpUri>http://coldb.mnhn.fr/catalognumber/mnhn/im/2007-34878</dwc:httpUri>
<dwc:httpUri>http://coldb.mnhn.fr/catalognumber/mnhn/im/2007-30935</dwc:httpUri>
<dwc:httpUri>http://coldb.mnhn.fr/catalognumber/mnhn/im/2007-30338</dwc:httpUri>
<dwc:httpUri>http://coldb.mnhn.fr/catalognumber/mnhn/im/2007-34880</dwc:httpUri>
<dwc:httpUri>http://coldb.mnhn.fr/catalognumber/mnhn/im/2007-34879</dwc:httpUri>

```

Other identifications 

```
grep 'dwc:accessionNumber' 24768796-CD1F-FFC6-FDED-1609FD27FE12.rdf 
<dwc:accessionNumber>KJ550352</dwc:accessionNumber>
```

Reading the pdf we also find several BOLD process ids which takes us to the barcode number (BIN) 

http://www.boldsystems.org/index.php/Public_RecordView?processid=CONO296-08 


```
processid	sampleid	recordID	catalognum	fieldnum	institution_storing	collection_code	bin_uri	phylum_taxID	phylum_name	class_taxID	class_name	order_taxID	order_name	family_taxID	family_name	subfamily_taxIsubfamily_name	genus_taxID	genus_name	species_taxID	species_name	subspecies_taxID	subspecies_name	identification_provided_by	identification_method	identification_reference	tax_note	voucher_status	tissue_type	collection_event_id	collectors	collectiondate_start	collectiondate_end	collectiontime	collection_note	site_code	sampling_protocol	lifestage	sex	reproduction	habitat	associated_specimens	associated_taxa	extrainfo	notes	lat	lon	coord_source	coord_accuracy	elev	depth	elev_accuracy	depth_accuracy	country	province_state	region	sector	exactsite	image_ids	image_urls	media_descriptors	captions	copyright_holders	copyright_years	copyright_licenses	copyright_institutions	photographers
CONO296-08	MNHN-IM-2007-17921	723406	MNHN_MOLL_17921	Panglao 05_96	Museum National d'Histoire Naturelle, Paris		BOLD:AAD3581	23	Mollusca	92	Gastropoda	84899	Neogastropoda	866	Conidae			1900	Conus	126909	Conus orbignyi			Nicolas Puillandre		Audouin, 1831					MNHN/IRD	FAO-71		9.49	123.74	
```

We need to link: 
Specimen ID: MNHN-IM-2007-34878 / KJ550376 / AJ409610 / CONO1409-14
https://science.mnhn.fr/institution/mnhn/collection/im/item/2007-34878

http://www.boldsystems.org/index.php/Public_RecordView?processid=CONO1409-14



```
{
  "id": "test/cae42177c14a8fcdeb14",
  "scientificName": "Profundiconus maribelae",
  "physicalSpecimenId": "MNHN-IM-2007-34878",
  "stableIdentifier": "https://science.mnhn.fr/institution/mnhn/collection/im/item/2007-34878",
  "accession": "https://identifiers.org/ena.embl:AJ409610",
  "tax_id": "https://identifiers.org/taxonomy:58944"
  "BIN" : "http://www.boldsystems.org/index.php/Public_BarcodeCluster?clusteruri=BOLD:AAO3751"
}
