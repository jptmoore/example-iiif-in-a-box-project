# Example IIIF Project: Medieval Manuscript Collection

A sample input project for [IIIF-in-a-Box](https://github.com/jptmoore/iiif-in-a-box). Use it to verify your install or as a template for your own project.

## What's here

- `config.yml` — project metadata (title, provider, etc.)
- `images/` — 4 sample manuscript pages (`book-page01.jpg` … `book-page04.jpg`)
- `annotations/book-pageNN/` — W3C Web Annotations (transcriptions, comments, tags, highlights) keyed to each image

## Usage

From your IIIF-in-a-Box checkout:

```bash
./bootstrap.sh build --input-dir /path/to/example-iiif-in-a-box-project
```

See the IIIF-in-a-Box README for build options, service management, and the rules governing image naming, annotation folders, and canvas IDs.
