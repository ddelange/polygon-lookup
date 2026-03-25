<p align="center">
  <img height="100" src="https://raw.githubusercontent.com/pelias/design/master/logo/pelias_github/Github_markdown_hero.png">
</p>
<h3 align="center">A modular, open-source search engine for our world.</h3>
<p align="center">Pelias is a geocoder powered completely by open data, available freely to everyone.</p>
<p align="center">
<a href="https://github.com/pelias/polygon-lookup/actions"><img src="https://github.com/pelias/polygon-lookup/workflows/Continuous%20Integration/badge.svg" /></a>
<a href="https://en.wikipedia.org/wiki/MIT_License"><img src="https://img.shields.io/github/license/pelias/polygon-lookup?style=flat&color=orange" /></a>
<a href="https://gitter.im/pelias/pelias"><img src="https://img.shields.io/gitter/room/pelias/pelias?style=flat&color=yellow" /></a>
</p>
<p align="center">
	<a href="https://github.com/pelias/docker">Local Installation</a> ·
        <a href="https://geocode.earth">Cloud Webservice</a> ·
	<a href="https://github.com/pelias/documentation">Documentation</a> ·
	<a href="https://gitter.im/pelias/pelias">Community Chat</a>
</p>
<details open>
<summary>What is Pelias?</summary>
<br />
Pelias is a search engine for places worldwide, powered by open data. It turns addresses and place names into geographic coordinates, and turns geographic coordinates into places and addresses. With Pelias, you're able to turn your users' place searches into actionable geodata and transform your geodata into real places.
<br /><br />
We think open data, open source, and open strategy win over proprietary solutions at any part of the stack and we want to ensure the services we offer are in line with that vision. We believe that an open geocoder improves over the long-term only if the community can incorporate truly representative local knowledge.
</details>

# polygon-lookup

[![Greenkeeper badge](https://badges.greenkeeper.io/pelias/polygon-lookup.svg)](https://greenkeeper.io/)

[![NPM](https://nodei.co/npm/polygon-lookup.png)](https://nodei.co/npm/polygon-lookup/)

A data-structure for performing fast, accurate point-in-polygon intersections against (potentially very large) sets of
polygons. `PolygonLookup` builds an [R-tree](http://en.wikipedia.org/wiki/R-tree), or bounding-box spatial index, for its
polygons and uses it to quickly narrow down the set of candidate polygons for any given point. If there are any
ambiguities, it'll perform point-in-polygon intersections to identify the one that *really* intersects. `PolygonLookup`
operates entirely in memory, and works best for polygons with little overlap.

## API

##### `PolygonLookup(featureCollection)`
  * `featureCollection` (**optional**): A GeoJSON collection to optionally immediately load with `.loadFeatureCollection()`.

##### `PolygonLookup.search(x, y, limit)`
Narrows down the candidate polygons by bounding-box, and then performs point-in-polygon intersections to identify the first n container polygon (with n = limit, even if more polygons really do intersect).

  * `x`: the x-coordinate to search for
  * `y`: the y-coordinate to search for
  * `limit` **optional**: the upper bound for number of intersecting polygon found (default value is 1, -1 to return all intersecting polygons)
  * `return`: the intersecting polygon if one was found; a GeoJson FeatureCollection if multiple polygons were found and limit > 1; otherwise, `undefined`.

##### `PolygonLookup.loadFeatureCollection(featureCollection)`
Stores a feature collection in this `PolygonLookup`, and builds a spatial index for it. The polygons and rtree can be
accessed via the `.polygons` and `.rtree` properties.

  * `featureCollection` (**optional**): A GeoJSON collection containing some Polygons/MultiPolygons. Note that
    MultiPolygons will get expanded into multiple polygons.

## example usage

```javascript
var PolygonLookup = require( 'polygon-lookup' );
var featureCollection = {
	type: 'FeatureCollection',
	features: [{
		type: 'Feature',
		properties: { id: 'bar' },
		geometry: {
			type: 'Polygon',
			coordinates: [ [ [ 0, 1 ], [ 2, 1 ], [ 3, 4 ], [ 1, 5 ] ] ]
		}
	}]
};
var lookup = new PolygonLookup( featureCollection );
var poly = lookup.search( 1, 2 );
console.log( poly.properties.id ); // bar
```
