# pid-specimen-genbank

# Introduction 

Natural history collections held in museums preserve and archive various types of biological and geological specimens that provide a physical record of the biodiversity in the natural world. It has been estimated that natural history collections around the world are housing 2-4 billion specimens. Together, these specimens and associated data (such as publications and molecular sequence data) represent an unparalleled resource to study the world's biodiversity. However, the current ecosystem of specimens and collection management globally is highly fragmented with discipline-specific data and metadata standards and institutional repositories. A consistent and sustainable approach to linking these physical specimens with other identifiable entities is then crucial for findability, accessibility, interoperability, and reproducibility i.e., achieving data ‘FAIRness’. 

# Background information 

"A persistent identifier (PI or PID) is a long-lasting reference to a document, file, web page, or other object. The term "persistent identifier" is usually used in the context of digital objects that are accessible over the Internet"

# Use case 

"“Which specimens have associated sequences in GenBank?” 

"Can I track down all the specimens in this paper"? 

# Physical Specimens 

Specimen id 
CETAF stable identifiers 

```
RMNH:MOL:256470
RMNH 16 XIV 1892
RMNH-Jo641 
RMNH Coel. 32050_1542
````

# ENA 

String search "RMNH*" 
```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "result=sequence_release&query=specimen_voucher%3D%22RMNH*%22&fields=scientific_name%2Ctax_id%2Ccollection_date%2Cspecimen_voucher%2Caccession&format=tsv" "https://www.ebi.ac.uk/ena/portal/api/search"
```
# Digital Object 

JSON: 
```
{
  "id": "test/11c9358ccf74a1e8717b",
  "scientificName": "Albinaria puella puella",
  "physicalSpecimenId": "RMNH:MOL:256470",
  "stableIdentifier": "https://bioportal.naturalis.nl/specimen/RMNH.MOL.256470",
  "accession": "https://identifiers.org/ena.embl:AY382068",
  "tax_id": "https://identifiers.org/taxonomy:256890"
}
```

# Publication 

ZENODO 
http://doi.org/10.5281/zenodo.1210134

treatment / plazi 
https://doi.org/10.5852/ejt.2016.173

https://science.mnhn.fr/institution/mnhn/collection/im/item/2007-34878

Specimen ID: MNHN-IM-2007-34878 / KJ550376 / AJ409610

