# Example IIIF Project: Medieval Manuscript Collection

This is a complete example project demonstrating how to use IIIF-in-a-Box to publish images with annotations.

## Contents

This project contains:
- **4 sample manuscript images** - Two chapters with two pages each
- **8 W3C Web Annotations** - Including transcriptions, comments, tags, and highlights
- **Complete metadata** - Configured in config.yml

## Structure

```
example-iiif-project/
├── config.yml                          # Project configuration with metadata
├── images/                             # Sample manuscript images (1200x1600 JPG)
│   ├── chapter1-page01.jpg            # Latin religious text
│   ├── chapter1-page02.jpg            # Continuation of Latin text
│   ├── chapter2-page01.jpg            # Middle English (Chaucer) - opening
│   └── chapter2-page02.jpg            # Middle English (Chaucer) - continuation
└── annotations/                        # W3C Web Annotation JSON files
    ├── chapter1-page01/               # Annotations for page 1
    │   ├── transcription-1.json       # Latin transcription with translation
    │   └── comment-1.json             # Scholarly comment
    ├── chapter1-page02/               # Annotations for page 2
    │   ├── transcription-1.json       # Latin transcription with translation
    │   └── tags-1.json                # Subject tags
    ├── chapter2-page01/               # Annotations for page 3
    │   ├── transcription-1.json       # Middle English transcription
    │   └── comment-1.json             # Literary analysis
    └── chapter2-page02/               # Annotations for page 4
        ├── transcription-1.json       # Middle English transcription
        └── highlight-1.json           # Highlighted passage of interest
```

## Naming Convention

**IMPORTANT:** File and folder names use a hierarchical naming convention with dashes (`-`) to define the IIIF collection and manifest structure.

### How it works:
- **Dashes (`-`) represent hierarchy levels**: `collection-manifest` or `collection-subcollection-item`
- **Names must match exactly** across images and annotation folders
- The system automatically creates collections and manifests based on the dash-separated structure

### Examples from this project:

| File Name | Creates | Hierarchy |
|-----------|---------|-----------|
| `chapter1-page01.jpg` | Manifest `chapter1`, Canvas `page01` | Collection: chapter1 → Item: page01 |
| `chapter2-page02.jpg` | Manifest `chapter2`, Canvas `page02` | Collection: chapter2 → Item: page02 |

### Matching requirements:
- Image: `images/chapter1-page01.jpg`
- Annotations: `annotations/chapter1-page01/` (folder name must match image base name)
- Result: `/iiif/canvas/chapter1/page01`

### For your own projects:
- Use descriptive names: `album-photo`, `book-chapter-page`, `collection-item`
- Keep names URL-friendly (lowercase, no spaces, use dashes)
- Ensure image filenames match their annotation folder names exactly

## How to Use

### 1. Clone the IIIF-in-a-Box repository

```bash
git clone https://github.com/your-org/iiif-in-a-box.git
cd iiif-in-a-box
```

### 2. Build and start the IIIF services

```bash
./bootstrap.sh build --input-dir /tmp/example-iiif-project
```

This command will:
- Process the images and create IIIF-compliant image tiles
- Generate IIIF Collection and Manifest JSON files
- Import annotations into the Miiify annotation server
- Index annotations for full-text search with AnnoSearch
- Start all Docker services (nginx, IIPImage, Miiify, AnnoSearch, Quickwit, Tamerlane viewer)

### 3. View in your browser

Once the build completes, open:

**Main viewer page:**
```
http://localhost:8080/pages/medieval-manuscript.html
```

**IIIF Manifest:**
```
http://localhost:8080/iiif/medieval-manuscript.json
```

**Collection structure:**
- Chapter 1 Manifest: `http://localhost:8080/iiif/chapter1.json`
- Chapter 2 Manifest: `http://localhost:8080/iiif/chapter2.json`

## Managing Your Services

```bash
# View status
cd /home/john/git/iiif-in-a-box
./bootstrap.sh status

# View logs
./bootstrap.sh logs

# Stop services
./bootstrap.sh stop

# Restart services
./bootstrap.sh restart

# Rebuild with changes
./bootstrap.sh build --input-dir /tmp/example-iiif-project
```

## Notes

- All images are synthetic examples created with ImageMagick
- Texts are excerpts from the Gospel of John (Latin Vulgate) and Chaucer's Canterbury Tales
- Annotations follow the W3C Web Annotation standard
- Canvas IDs use the dash-to-slash pattern: `chapter1-page01.jpg` → `/iiif/canvas/chapter1/page01`
