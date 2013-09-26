

CSW Webservice NGR
==================

De developer pagina van het Nationaal Georegister (NGR) bevat een aantal tips om met data en services uit het register aan de slag te gaan. Deze pagina bevat meer informatie over de CSW interface van het NGR.

Het register is een catalogus met informatie over vooral ruimtelijke datasets en services die door voornamelijk overheden ter beschikking gesteld worden. OGC:CSW is de catalogus standaard van OGC. De CSW requests kunnen via HTTP GET of XMl over HTTP POST verstuurd worden.

De CSW specificatie is vrij groot en bescrhijft veel functionaliteit. Deze pagina beschrijft hoe je de meest voorkomende operaties kan uitvoeren. Meer info over CSW vind je op (http://www.opengeospatial.org/standards/cat).


CSW requests
============
**Operaties**
De belangrijkste operaties om metadata op te vragen zijn:

1. GetCapabilities: voor het bekijken van de mogelijkheden van de service. Deze operatie kan handig zijn om te zien welke filters ondersteund worden bijvorrbeeld.

2. GetRecords: om meerdere metadata documenten (records) op te vragen, bijvoorbeeld met een zoekfilter.

3. GetRecordById: om 1 metadata document op te vragen, via het metadata ID.

De CSW requests kunnen via HTTP GET of XMl over HTTP POST verstuurd worden. Deze requests worden hieronder behandeld.

Op de website van GeoNetwork is meer informatie te vinden over CSW: 

http://www.geonetwork-opensource.org/manuals/2.8.0/eng/developer/xml_services/csw_services.html

Hieronder volgen praktische voorbeelden voor het NGR.

GetCapabilities: opvragen wat de server kan
===========================================
Door het volgende HTTP GET request te versturen, kan je zien wat de server aan fucntionaliteit biedt:

http://nationaalgeoregister.nl/geonetwork/srv/dut/csw?request=GetCapabilities&service=CSW

Het antwoord is een XML document, conform de CSW 2.0.2 specificatie. Het somt de operaties en filters op die de CSW ondersteunt en de URLs voor het versturen van de requests. Daarnaast bevat het document de beschikbare formaten om metadata op te vragen. Het voert te ver de gehele structuur van het document uit te leggen.

Opvragen van metadata records
=============================

**Omgaan met veel metadata**
Opvragen in batches / pagineren. Gebruik de MaxRecords parameter en de XXXXXX parameter, om te voorkomen dat alles terug komt. Dit is namelijk veel. Voorbeeld van MaxRecords:

En vervolgens de volgende XXX.

**Aantal zoekresultaten opvragen**
Via parameter hits.

**Voorbeelden**


**Request parameters**

**Vragen om specifieke metadata formaten**
Vraag om ISO-metadata
Vraag om Dublin Core metadata (beperkte set)


Zoeken via filters
==================
Het NGR staat allerlei zoekopdrachten toe, ook via de CSW. De meestgebruikte is alle tekst doorzoeken.

Voor heel gerichte zoekopdrachten, gebruik een van de filters uit de CSW ():
<todo: lijst filters>


INSPIRE metadata
================
Op zoek naar alleen INSPIRE metadata? Gebruik dan in plaats van de URL:
http://nationaalgeoregister.nl/geonetwork/srv/dut/csw

http://nationaalgeoregister.nl/geonetwork/srv/dut/inspire

Dit laatste (eindigend op inspire) is het zoenaanmde INSPIRE endpoint. Dit is een gewone CSW, maar de inhoud betreft alleen de metadata voor INSPIRE.

Tooling met CSW ondersteuning 
=============================

In veel gevallen ben je echter sneller af als je een bestaande bibliotheek gebruikt om CSW te ontsluiten: OpenLayers(http://dev.openlayers.org/docs/files/OpenLayers/Protocol/CSW/v2_0_2-js.html) of GXP(http://gxp.opengeo.org/master/examples/catalogue.html) of Geonetwork Widgets(http://nationaalgeoregister.nl/geonetwork/apps/js/GeoNetwork/examples).
Ook Esri(https://github.com/Esri/geoportal-server/tree/master/components/desktop/CswClient/trunk) en QGIS(http://hub.qgis.org/projects/cswclient) kennen CSW-ondersteuning via het laden van een plug-in, beide plug-ins zijn beschikbaar als opensource.
