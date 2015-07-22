# tdwg-challenge-rdf
Datasets to explore use of RDF in biodiversity informatics

These datasets where described in my blog post [TDWG Challenge - what is RDF good for?](http://iphylo.blogspot.co.uk/2011/10/tdwg-challenge-what-is-rdf-good-for.html), see also [Reflections on the TDWG RDF “Challenge”](http://iphylo.blogspot.co.uk/2011/10/reflections-on-tdwg-rdf.html).

The core files are:

* uniprot.rdf Uniprot RDF for frogs in GenBank
* ion.rdf Index of Organism Names (ION) RDF for taxonomic names for frogs (filtered to just those names that are also in GenBank, the RDF comes from ION LSIDs)
* crossref.rdf CrossRef RDF for DOIs for publications that published new frog names (obtaining using CrossRef’s support for Linked Data for DOIs)
* dbpedia.rdf Dbpedia RDF for frogs in GenBank (most fields removed to keep file size manageable)

Because these files have little to link them, I also created some “glue” files:

* linkout.rdf The list of links between NCBI and Dbpedia, based on mapping in iPhylo LinkOut http://dx.doi.org/10.1371/currents.RRN1228
* ion_doi.rdf A subset of publications listed in ION have DOIs, this file links the corresponding ION LSIDs to those DOIs (this file came from a project which eventually became [BioNames](http://bionames.org)

In the blog posts I explore what can be done with these files. For example, in [Reflections on the TDWG RDF “Challenge”](http://iphylo.blogspot.co.uk/2011/10/reflections-on-tdwg-rdf.html) I created a table listing the name, conservation status, publication DOI and date, and (where available) image from Wikipedia for frog species with sequences in GenBank. The SPARQL for this is:

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbpowl: <http://dbpedia.org/ontology/>
PREFIX uniprot: <http://purl.uniprot.org/core/>
PREFIX tname: <http://rs.tdwg.org/ontology/voc/TaxonName#>
PREFIX tcommon: <http://rs.tdwg.org/ontology/voc/Common#>
PREFIX dcterms: <http://purl.org/dc/terms/>

SELECT ?name ?status ?doi ?date ?thumbnail WHERE {
?ncbi uniprot:scientificName ?name .
?ncbi rdfs:seeAlso ?dbpedia .
?dbpedia dbpowl:conservationStatus ?status .
?ion tname:nameComplete ?name . 
?ion tcommon:publishedInCitation ?doi .
?doi dcterms:date ?date .

OPTIONAL
{
 ?dbpedia dbpowl:thumbnail ?thumbnail
}

} 
ORDER BY ASC(?status)
```

|?name|?status|?doi|?date|?thumbnail|
|-----|-------|----|-----|----------|
|Churamiti maridadi|CR|http://dx.doi.org/10.1080/21564574.2002.9635467|2002||
|Phrynopus kauneorum|CR|http://dx.doi.org/10.2307/1565993|2002||
|Leptodactylus silvanimbus|CR|http://dx.doi.org/10.2307/1563691|1980-10-31Z||
|Eleutherodactylus thorectes|CR|http://dx.doi.org/10.2307/1445381|1988-08-03Z||
|Eleutherodactylus sciagraphus|CR|http://dx.doi.org/10.2307/1563010|1973-08-06Z||
|Eleutherodactylus mariposa|CR|http://dx.doi.org/10.2307/1466962|1992||
|Eleutherodactylus lamprotes|CR|http://dx.doi.org/10.2307/1563010|1973-08-06Z||
|Eleutherodactylus fowleri|CR|http://dx.doi.org/10.2307/1563010|1973-08-06Z||
|Eleutherodactylus eunaster|CR|http://dx.doi.org/10.2307/1563010|1973-08-06Z||
|Eleutherodactylus amadeus|CR|http://dx.doi.org/10.2307/1445557|1987-12-09Z||
|Eleutherodactylus apostates|CR|http://dx.doi.org/10.2307/1563010|1973-08-06Z||
|Ptychohyla hypomykter|CR|http://dx.doi.org/10.2307/3672060|1993||
|Atelopus nanay|CR|http://dx.doi.org/10.1655/0018-0831(2002)058[0229:TNSOAA]2.0.CO;2|2002||
|Bufo chavin|CR|http://dx.doi.org/10.1643/0045-8511(2001)001[0216:NSOBAB]2.0.CO;2|2001||
|Rhacophorus catamitus|DD|http://dx.doi.org/10.1655/0733-1347(2002)016[0046:NAPKPF]2.0.CO;2|2002||
|Huia melasma|DD|http://dx.doi.org/10.1643/CH-04-137R3|2005||
|Amolops bellulus|DD|http://dx.doi.org/10.1643/0045-8511(2000)000[0536:ABANSO]2.0.CO;2|2000||
|Boophis periegetes|DD|http://dx.doi.org/10.1111/j.1096-3642.1995.tb01427.x|1995-12-28Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/4/4b/Boophis_periegetes.jpg/200px-Boophis_periegetes.jpg)|
|Boophis liami|DD|http://dx.doi.org/10.1163/156853803322440772|2003-07-01Z||
|Telmatobius vilamensis|DD|http://dx.doi.org/10.1655/0018-0831(2003)059[0253:ANSOTA]2.0.CO;2|2003||
|Proceratophrys cururu|DD|http://dx.doi.org/10.2307/1447712|1998-02-03Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/1/1c/Proceratophrys_cururu.jpg/200px-Proceratophrys_cururu.jpg)|
|Proceratophrys concavitympanum|DD|http://dx.doi.org/10.2307/1565412|2000||
|Phrynopus bufoides|DD|http://dx.doi.org/10.1643/CH-04-278R2|2005||
|Phrynopus pesantesi|DD|http://dx.doi.org/10.1643/CH-04-278R2|2005||
|Paratelmatobius cardosoi|DD|http://dx.doi.org/10.2307/1447976|1999-12-17Z||
|Gastrotheca galeata|DD|http://dx.doi.org/10.2307/1443617|1978-08-10Z||
|Phyllomedusa duellmani|DD|http://dx.doi.org/10.2307/1444649|1982-08-10Z||
|Litoria kumae|DD|http://dx.doi.org/10.1071/ZO03008|2004||
|Hyalinobatrachium ignioculus|DD|http://dx.doi.org/10.1670/0022-1511(2003)037[0091:ANSOHA]2.0.CO;2|2003||
|Centrolene bacatum|DD|http://dx.doi.org/10.2307/1564528|1994||
|Hyla suweonensis|DD|http://dx.doi.org/10.2307/1444138|1980-02-01Z||
|Callulina kisiwamsitu|EN|http://dx.doi.org/10.1670/209-03A|2004||
|Aglyptodactylus laticeps|EN|http://dx.doi.org/10.1111/j.1439-0469.1998.tb00775.x|1998-03-27Z||
|Telmatobius sibiricus|EN|http://dx.doi.org/10.1655/0018-0831(2003)059[0127:ANSOTF]2.0.CO;2|2003||
|Phrynopus bracki|EN|http://dx.doi.org/10.2307/1445826|1990-03-06Z||
|Gastrotheca trachyceps|EN|http://dx.doi.org/10.2307/1564375|1987||
|Eleutherodactylus melacara|EN|http://dx.doi.org/10.2307/1466962|1992||
|Eleutherodactylus glaphycompus|EN|http://dx.doi.org/10.2307/1563010|1973-08-06Z||
|Eleutherodactylus glamyrus|EN|http://dx.doi.org/10.2307/1565664|1997||
|Eleutherodactylus grahami|EN|http://dx.doi.org/10.2307/1563929|1979-04-30Z||
|Eleutherodactylus amplinympha|EN|http://dx.doi.org/10.1139/z94-297|1994||
|Plectrohyla glandulosa|EN|http://dx.doi.org/10.2307/1441046|1964-06-30Z||
|Cochranella mache|EN|http://dx.doi.org/10.1655/03-74|2004||
|Bufo tacanensis|EN|http://dx.doi.org/10.2307/1439700|1952-09-26Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/d/d6/Bufo_tacanensis_distribution.svg/200px-Bufo_tacanensis_distribution.svg.png)|
|Arthroleptis nikeae|EN|http://dx.doi.org/10.1080/21564574.2003.9635486|2003||
|Philautus aurifasciatus|LC|http://dx.doi.org/10.1163/156853887X00036|1987-01-01Z||
|Rana yavapaiensis|LC|http://dx.doi.org/10.2307/1445338|1984-12-18Z||
|Uperoleia inundata|LC|http://dx.doi.org/10.1071/AJZS079|1981||
|Gephyromantis boulengeri|LC|http://dx.doi.org/10.1111/j.1096-3642.1919.tb02128.x|1919-12-21Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Mantidactylus_boulengeri_map-fr.svg/200px-Mantidactylus_boulengeri_map-fr.svg.png)|
|Mantidactylus argenteus|LC|http://dx.doi.org/10.1111/j.1096-3642.1919.tb02128.x|1919-12-21Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Mantidactylus_argenteus02.jpg/200px-Mantidactylus_argenteus02.jpg)|
|Boophis lichenoides|LC|http://dx.doi.org/10.1163/156853898X00025|1998-01-01Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/4/45/Boophis_lichenoides01.jpg/200px-Boophis_lichenoides01.jpg)|
|Aglyptodactylus securifer|LC|http://dx.doi.org/10.1111/j.1439-0469.1998.tb00775.x|1998-03-27Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/9/94/Aglyptodactylus_securifer.jpg/200px-Aglyptodactylus_securifer.jpg)|
|Leptobrachium nigrops|LC|http://dx.doi.org/10.2307/1440966|1963-12-31Z||
|Megistolotis lignarius|LC|http://dx.doi.org/10.1071/ZO9790135|1979||
|Proceratophrys avelinoi|LC|http://dx.doi.org/10.1163/156853893X00156|1993-01-01Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Proceratophrys_avelinoi.jpg/200px-Proceratophrys_avelinoi.jpg)|
|Crossodactylus caramaschii|LC|http://dx.doi.org/10.2307/1446907|1995-05-03Z||
|Pseudis cardosoi|LC|http://dx.doi.org/10.1163/156853800507264|2000-03-01Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/Podonectes_cardosoi.jpg/200px-Podonectes_cardosoi.jpg)|
|Pseudis tocantins|LC|http://dx.doi.org/10.1590/S0101-81751998000400011|1998||
|Osteocephalus mutabor|LC|http://dx.doi.org/10.1163/156853802320877609|2002-01-01Z||
|Osteocephalus deridens|LC|http://dx.doi.org/10.1163/156853800507525|2000-07-01Z||
|Litoria pronimia|LC|http://dx.doi.org/10.1071/ZO9930225|1993||
|Litoria longirostris|LC|http://dx.doi.org/10.2307/1443159|1977-11-25Z||
|Litoria paraewingi|LC|http://dx.doi.org/10.1071/ZO9760283|1976||
|Litoria havina|LC|http://dx.doi.org/10.1071/ZO9930225|1993||
|Crinia riparia|LC|http://dx.doi.org/10.2307/1440794|1965-09-30Z||
|Ansonia kraensis|NE|http://dx.doi.org/10.2108/zsj.22.809|2005||
|Ansonia endauensis|NE|http://dx.doi.org/10.1655/0018-0831(2006)62[466:ANSOAS]2.0.CO;2|2006||
|Arthroleptella landdrosia|NT|http://dx.doi.org/10.2307/1565359|2000||
|Phrynobatrachus phyllophilus|NT|http://dx.doi.org/10.2307/1565925|2002||
|Litoria jungguy|NT|http://dx.doi.org/10.1071/ZO02069|2004|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/d/da/Litoria_jungguy.jpg/200px-Litoria_jungguy.jpg)|
|Spicospina flammocaerulea|VU|http://dx.doi.org/10.2307/1447757|1997-05-13Z|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Spicospina_distribution.png/200px-Spicospina_distribution.png)|
|Philautus ingeri|VU|http://dx.doi.org/10.1163/156853887X00036|1987-01-01Z||
|Phrynobatrachus uzungwensis|VU|http://dx.doi.org/10.1163/156853883X00030|1983-01-01Z||
|Boophis sambirano|VU|http://dx.doi.org/10.1080/21564574.2005.9635520|2005||
|Oreolalax multipunctatus|VU|http://dx.doi.org/10.2307/1564828|1993||
|Telmatobufo australis|VU|http://dx.doi.org/10.2307/1563086|1972-03-31Z||
|Stefania coxi|VU|http://dx.doi.org/10.1655/0018-0831(2002)058[0327:EDOSAH]2.0.CO;2|2002||
|Gastrotheca dendronastes|VU|http://dx.doi.org/10.2307/1445088|1983-12-14Z||
|Eleutherodactylus guantanamera|VU|http://dx.doi.org/10.2307/1466962|1992||
|Cycloramphus acangatan|VU|http://dx.doi.org/10.1655/02-78|2003||
|Hyperolius cystocandicans|VU|http://dx.doi.org/10.2307/1443911|1977-05-25Z||
|Ansonia torrentis|VU|http://dx.doi.org/10.1163/156853883X00021|1983-01-01Z||
|Leiopelma pakeka|VU|http://dx.doi.org/10.1080/03014223.1998.9517554|1998|![image](http://upload.wikimedia.org/wikipedia/commons/thumb/7/79/Leiopelma_pakeka01.jpg/200px-Leiopelma_pakeka01.jpg)|
|Rana okaloosae|VU|http://dx.doi.org/10.2307/1444847|1985-05-03Z|