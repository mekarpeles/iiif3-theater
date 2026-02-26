# Archive.org IIIF Theater

## Overview

This project implements a theater viewer for archive.org items using IIIF (International Image Interoperability Framework) and OpenSeaDragon for deep-zoom image viewing.

## Archive.org Basics

- **Item Identifier**: Each item on archive.org has a unique identifier
- **Example URL**: `https://archive.org/details/img-8664_202009` → identifier is `img-8664_202009`

## IIIF Integration

Archive.org implements the [IIIF 3.0 image standard](https://iiif.io/api/image/3.0/) which works with OpenSeaDragon.

### Simple Items (Single Image)

```
https://iiif.archive.org/iiif/img-{identifier}/info.json
```

Example: `https://iiif.archive.org/iiif/img-8664_202009/info.json`

### Multi-file Items (Multiple Images)

When an item contains multiple files, the full path to the image is used with `%2f` to URL-encode `/`:

```
https://iiif.archive.org/image/iiif/3/{item}%2f{filename}/info.json
```

Example: `https://iiif.archive.org/image/iiif/3/michael-karpeles-genealogy%2fcarl-karpeles-naturalization.jpg/info.json`

## Archive.org API

### Get Files Metadata

```
https://archive.org/metadata/{item}/files
```

Returns JSON with all files in an item.

### Download Image

```
https://archive.org/download/{item}/{filename}
```

### Filter Images

```javascript
const images = data.result
  .filter(f =>
    f.name.endsWith(".jpg") &&
    !f.name.includes("_thumb") &&
    !f.name.includes(".thumbs/") &&
    f.name !== "__ia_thumb.jpg"
  )
  .sort((a, b) => a.name.localeCompare(b.name));
```

## OpenSeaDragon Integration

OpenSeaDragon can load IIIF info.json directly as a tile source.

```javascript
OpenSeaDragon({
  tileSources: "https://iiif.archive.org/iiif/img-{identifier}/info.json"
});
```

## Demo References

- Working demo: https://iiif.gdmrdigital.com/openseadragon/index.html?image=https://iiif.archive.org/iiif/img-8664_202009/info.json
- OpenSeaDragon IIIF tilesource docs: https://openseadragon.github.io/examples/tilesource-iiif/
- CodePen for fetching archive.org files: https://codepen.io/mekarpeles/pen/emzVdpQ

## Test Item

Use for testing: `2025-highland-house-walkthrough-ma`
