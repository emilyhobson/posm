# End-to-End Status

This attempts to capture the current status of POSM's usability from end-to-end,
both in terms of software installed and configured and data made available.

## Legend

:computer: - automated

:hand: - manual

:sunny: - good to go

:cloud: - partially working

:bomb: - not working / unknown

## Misc. Cloud Services

Description | Status | Notes
----------- | ------ | -----
1. Data gatherer | :bomb: | **TBD**, HOT Export Tool?

## Misc. Device Services

Description | Status | Notes
----------- | ------ | -----
1. Static iD assets | :bomb: |
2. Field Papers | :cloud: |
3. OMK Server | :cloud: |
4. OSM API | :bomb: | macrocosm
5. Captive Portal | :bomb: | **TBD**
6. WiFi | :bomb: | `hostapd`
7. MBTiles server | :cloud: | `tessera`
8. Tile renderer | :cloud: | `tessera`

## Initial Data Gathering

Description | Automated | Status | Notes
----------- | --------- | ------ | -----
1. PBF Extract | :hand: | :bomb: | Can be done directly or using the [HOT Export Tool](http://export.hotosm.org/) with an arbitrary file format
1a. Overpass → XML | :hand: | :bomb: |
1b. XML → PBF conversion | :hand: | :bomb: | Osmosis
2. MBTiles creation | :hand: | :bomb: | [tl](https://github.com/mojodna/tl) or [mbtiles-generate](https://github.com/AmericanRedCross/mbtiles-generate) (which uses `tl` internally)

### Outputs

Description | Status | Notes
----------- | ------ | -----
1. AOI data package | :bomb: | HTTP-accessible, contains PBF + MBTiles (styles TBD) for the AOI

## Initial Device Data Provisioning

Description | Automated | Status | Notes
----------- | --------- | ------ | -----
1. Download data package | :hand: | :bomb: | Target path: **TBD**
2. Extract MBTiles | :hand: | :bomb: | Target path: **TBD**
3. Extract PBF | :hand: | :bomb: | Target path: **TBD**
4. Populate APIDB<sup>1</sup> | :hand: | :bomb: | Osmosis
5. Populate rendering DB | :hand: | :cloud: | `osm2pgsql`
6. Restart tile server(s) | :hand: | :bomb: |

1. To populate the APIDB (using scripts from [macrocosm](https://github.com/americanredcross/macrocosm)):

```bash
createdb macrocosm
psql -d macrocosm -f db-server/script/macrocosm-db.sql
osmosis \
  --read-pbf-fast delaware-latest.osm.pbf \
  --log-progress \
  --write-apidb database=macrocosm
```

### Outputs

Description | Status | Notes
----------- | ------ | -----
1. HTTP-accessible tiles | :bomb: | From MBTiles archive(s)
2. HTTP-accessible tiles | :bomb: | From MBTiles archive(s)

## Field Papers

Description | Automated | Status | Notes
----------- | --------- | ------ | -----
1. Generate PDF | :hand: | :bomb: | fp-legacy / fp-tasks
2. Download, print, and annotate | :hand: | :bomb: |
3. Upload | :hand: | :bomb: | fp-legacy / fp-tasks
4. Trace in iD | :hand: | :cloud: | static iD, fp-tiler

### Outputs

Description | Status | Notes
----------- | ------ | -----
1. HTTP-accessible PDF | :bomb: |
2. HTTP-accessible snapshot GeoTIFF | :bomb: |
3. HTTP-accessible snapshot tiles | :bomb: | `fp-tiler`

## OSM Editing

Description | Automated | Status | Notes
----------- | --------- | ------ | -----
1. Load data | :hand: | :bomb: | macrocosm
2. Save changes | :hand: | :bomb: | macrocosm

### Outputs

Description | Status | Notes
----------- | ------ | -----
1. Changesets | :bomb: | Inherently in the APIDB, but maybe we want files at this stage too

## OpenMapKit

**TBD**

## Sync ↓

Description | Automated | Status | Notes
----------- | --------- | ------ | -----
1. Regenerate AOI data package | :hand: | :bomb: |
2. Download data package | :hand: | :bomb: | Target path: **TBD**
3. Extract PBF | :hand: | :bomb: | Target path: **TBD**
4. Extract MBTiles | :hand: | :cloud: | Target path: **TBD**

## Sync ↑

Description | Automated | Status | Notes
----------- | --------- | ------ | -----
1. Generate diff(s) from APIDB<sup>1</sup> | :hand: | :bomb: | Osmosis / `osmconvert` to go from DB → PBF and to generate `.osm` and/or `.osc` files.
2. Update diffs | :hand: | :bomb: | Staged for manual merging. Target: **TBD**

1. Affected features in locally applied changesets need to be identified and renumbered if they did not previously exist.

## Periodic Tasks

Description | Automated | Status | Notes
----------- | --------- | ------ | -----
1. Sync APIDB → rendering DB| :hand: | :bomb: | Osmosis / `osm2pgsql`
2. Flush tile cache | :hand: | :bomb: | Restart `tessera`
