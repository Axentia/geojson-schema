# GeoJSON Schema

This repository provides tools for building [JSON Schema](https://json-schema.org/) docs for [GeoJSON](https://geojson.org/).  A schema is generated for each of the GeoJSON object types, and each individual doc is complete (without `$ref`s to other documents).  So applications can consume just what they need and avoid extra fetching.

See JSON Schema docs for the GeoJSON types below:
 * [`FeatureCollection`](https://geojson.org/schema/FeatureCollection.json)
 * [`Feature`](https://geojson.org/schema/Feature.json)
 * [`Geometry`](https://geojson.org/schema/Geometry.json)
 * [`GeometryCollection`](https://geojson.org/schema/GeometryCollection.json)
 * [`MultiPolygon`](https://geojson.org/schema/MultiPolygon.json)
 * [`MultiLineString`](https://geojson.org/schema/MultiLineString.json)
 * [`MultiPoint`](https://geojson.org/schema/MultiPoint.json)
 * [`Polygon`](https://geojson.org/schema/Polygon.json)
 * [`LineString`](https://geojson.org/schema/LineString.json)
 * [`Point`](https://geojson.org/schema/Point.json)

The overall GeoJSON schema can be found at
 * [`GeoJSON`](https://geojson.org/schema/GeoJSON.json)

The schema files are also published to the [`geojson-schema` package](https://www.npmjs.com/package/geojson-schema) on npm.  You can add them as a dependency to your project with the following:

    npm install geojson-schema

## Limitations

The schema can not be used to validate that linear rings are closed or that they follow the right-hand rule.  These two elements of the [GeoJSON specification](https://datatracker.ietf.org/doc/html/rfc7946) cannot be represented in JSON Schema.  To enforce these two elements of linear ring validation, parsers must implement their own logic on top of JSON Schema validation.

# Development

Install dependencies:

    npm install

To build the schema docs:

    make

The output will be in the `build` directory.

A CI job builds commits pushed to `main`.  After a successful build, artifacts are pushed to the `gh-pages` branch and will be accessible at geojson.org/schema/<name>.json (e.g. https://geojson.org/schema/Point.json).

![Status](https://github.com/geojson/schema/actions/workflows/test.yml/badge.svg)

# Publishing

To publish the [`geojson-schema` package](https://www.npmjs.com/package/geojson-schema), run the following:

    npm version minor # or major or patch
    make build
    git push --tags origin main
    pushd build && npm publish && popd

## Publishing this fork to GitHub Packages

GitHub Packages for npm requires a scoped package name.  This repository includes a workflow that publishes on version tags (`v*.*.*`) and rewrites the built package name to:

    @<github-owner>/geojson-schema

### One-time setup

1. Enable Actions in your fork.
2. Ensure the default branch is `main`.
3. Make sure your fork has package write access for `GITHUB_TOKEN` (the workflow sets `packages: write`).

### Release

Create and push a semver tag like `v1.0.6`:

    git tag v1.0.6
    git push origin v1.0.6

The workflow `.github/workflows/publish-github-packages.yml` will build and publish:

    @<github-owner>/geojson-schema

to:

    https://npm.pkg.github.com

### Install from GitHub Packages

Add the registry mapping for your scope in `.npmrc`:

    @<github-owner>:registry=https://npm.pkg.github.com

Then install:

    npm install @<github-owner>/geojson-schema
