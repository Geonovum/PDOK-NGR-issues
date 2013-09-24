..Developers page Nationaal Georegister
..Licence CC

Nationaal GeoRegister Developer pagina
======================================

De developer pagina van het nationaal georegister bevat een aantal tips om met data en services uit het register aan de slag te gaan.

Het register is een catalogus met informatie over vooral ruimtelijke datasets en services die door voornamelijk overheden ter beschikking gesteld worden.

In het zoekresultaat wordt middels iconen aangegeven welke API's er beschikbaar zijn voor een bepaalde dataset.

Het register maakt gebruik van open standaarden (api-beschrijvingen) die door dan wel [W3C](http://www.w3c.org), [OGC](http://www.opengeospatial.org), ISO, [JRC](http://ec.europa.eu/dgs/jrc) of [geonovum](http://geonovum.nl) beheerd worden. 
Hieronder een lijst van standaarden met korte omschrijving en links om verder te lezen.

**W3C:RDF**
De catalogus kan geindexeerd worden middels Linked Data RDF, ten behoeve van semantic web applicaties. Het register biedt momenteel (nog) geen SPARQL end-point. Het RDF endpoint is http://www.nationaalgeoregister.nl/geonetwork/srv/dut/rdf.search.

**W3C:RSS**
De catalogus kan ontsloten worden middels RSS. Diverse RSS endpoints zijn gepubliceerd in het open search description document op http://www.nationaalgeoregister.nl/geonetwork/srv/dut/portal.opensearch. Inspire heeft een aantal additionele vereisten opgesteld ten behoeve van interoperabiliteit rond file-downloads via Atom.

**OGC:CSW**
OGC:CSW is de catalugus standaard van OGC. Meer info over CSW vind je op (http://www.opengeospatial.org/standards/cat). In veel gevallen ben je echter sneller af als je een bestaande bibliotheek gebruikt om CSW te ontsluiten: [OpenLayers](http://dev.openlayers.org/docs/files/OpenLayers/Protocol/CSW/v2_0_2-js.html) of [GXP](http://gxp.opengeo.org/master/examples/catalogue.html) of [Geonetwork Widgets](http://nationaalgeoregister.nl/geonetwork/apps/js/GeoNetwork/examples).
Ook [Esri](https://github.com/Esri/geoportal-server/tree/master/components/desktop/CswClient/trunk) en [QGIS](http://hub.qgis.org/projects/cswclient) kennen CSW-ondersteuning via het laden van een plug-in, beide plug-ins zijn beschikbaar als opensource.

**OGC:WMS**
Web Map Services is een rest-api om kaartbeelden uit te wisselen als bitmap. Een volledige beschrijving van WMS staat hier (http://www.opengeospatial.org/standards/wms). WMS kent (minimaal) 3 operaties: GetCapabilities (retourneert een lijst van beschikbare kaartlagen, projecties, formaten enz), GetMap (retourneert een kaart afbeelding) en getFeatureInfo (geeft attribuutgegevens van het aangeklikte object). Daarnaast wordt optioneel ondersteund: GetLegendGraphic. WMS is eenvoudig te consumeren met bibliotheken als [OpenLayers](http://www.openlayers.org), Flamingo, Leaflet. Ook is WMS standaard te openen in QGIS, Esri, Udig, Mapwindow enz.

**OGC:WMTS**
Web Map Tile Services zijn vergelijkbaar met WMS, echter in dit geval is het kaartbeeld opgeknipt in tegels volgens een gedefinieerd grid. De tegels worden al dan niet gecached aan serverzijde voor hergebruik.
http://www.opengeospatial.org/standards/wmts. De operaties zijn vergelijkbaar, echter het capabilities document is uitgebreid met het gebruikte grid (per projectie). WMTS wordt ondersteund door het overgrote deel van de hierboven genoemde WMS clients. Voor Esri is er een plug-in beschikbaar voor wmts [arcbrutile](http://arcbrutile.codeplex.com/). Geonovum heeft ten behoeve van interoperabiliteit binnen Nederland een tiling richtlijn voor RD_new (epsg:28992) vastgesteld (http://www.geonovum.nl/sites/default/files/Nederlandse_richtlijn_tiling_-_versie_1.0.pdf).

**OGC:WFS**
Web Feature Services is een rest-api om vector data uit te wisselen. Een volledige beschrijving van WFS staat hier (http://www.opengeospatial.org/standards/wfs). Belangrijke operaties zijn GetCapabilities, DescribeFeatureType, GetFeature. Een getFeature request initieert de feature download. Deze operatie kan gecombineerd worden met ruimtelijke of attribuut filters en een output-formaat keuze (afhankelijk van implementatie: bv GML, Shp, json, kml, csv)

**iso:19139**
[19139](http://www.iso.org/iso/catalogue_detail.htm?csnumber=32557) is een schema wat de inhoud van metadata documenten beschrijft die middels CSW uitgewisseld worden. Een alternatief voor iso19139 is Dublic Core. Bij het uitlezen van een CSW antwoord dient nagegaan te worden welk schema gebruikt wordt (of in de request dent een target schema meegegeven te worden). Middels het gebruikte schema is het CSW antwoord om te zetten naar bruikbare concepten als titel, auteur, datum. Iso19139 wordt gebruikt om metadata van een dataset (iso19115) of metadata van een service (iso19119) vast te leggen. Geonovum heeft een aantal Nederlandse profielen voor metadata vastgesteld om interoperabiliteit in het Nederlandse domein te faciliteteren (http://www.geonovum.nl/geostandaarden/metadata/destandaarden).

