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

## Annotation Naming Convention

**CRITICAL RULE:** Annotation folder names must exactly match image base names (without extension).

### How annotations are organized:

The annotation system uses a simple flat structure where each image has a corresponding folder:

| Image File | Annotation Folder | Status |
|------------|------------------|---------|
| `images/chapter1-page01.jpg` | `annotations/chapter1-page01/` | ✅ Correct - exact match |
| `images/chapter1-page01.jpg` | `annotations/chapter1/page01/` | ❌ Wrong - nested folders not supported |
| `images/chapter1-page01.jpg` | `annotations/chapter1-page-01/` | ❌ Wrong - must match exactly |

### Annotation IDs follow the folder structure:

Miiify organizes annotations using a **container/annotation** pattern:

- **Container**: The folder name (e.g., `chapter1-page01`)
- **Annotation**: The JSON filename without extension (e.g., `transcription-1`)
- **Full ID**: `http://localhost:8080/miiify/annotations/{container}/{annotation}`

**Examples:**
```
Folder:        annotations/chapter1-page01/transcription-1.json
Annotation ID: http://localhost:8080/miiify/annotations/chapter1-page01/transcription-1

Folder:        annotations/chapter2-page02/highlight-1.json
Annotation ID: http://localhost:8080/miiify/annotations/chapter2-page02/highlight-1
```

### Target Canvas IDs use slash-separated paths:

When annotations reference canvases, dashes in the image filename become slashes:

| Image Filename | Canvas ID Path |
|----------------|----------------|
| `chapter1-page01.jpg` | `/iiif/canvas/chapter1/page01` |
| `chapter2-page02.jpg` | `/iiif/canvas/chapter2/page02` |

**Note:** The image naming also determines the IIIF Collection/Manifest structure. See the main IIIF-in-a-Box README for details on how dashes create hierarchical collections.

### Quick Reference:

- ✅ **Image**: `chapter1-page01.jpg`
- ✅ **Annotation folder**: `annotations/chapter1-page01/` (matches exactly)
- ✅ **Annotation file**: `transcription-1.json`
- ✅ **Annotation ID**: `/miiify/annotations/chapter1-page01/transcription-1`
- ✅ **Canvas ID**: `/iiif/canvas/chapter1/page01` (dashes → slashes)

## Annotation Example

Here's a complete example showing how the naming convention ties everything together:

### File Structure:
```
images/chapter1-page01.jpg              ← Image file
annotations/chapter1-page01/            ← Folder name MUST match image base name
    transcription-1.json                ← Annotation file
```

### Annotation Content (`annotations/chapter1-page01/transcription-1.json`):
```json
{
  "@context": "http://www.w3.org/ns/anno.jsonld",
  "id": "http://localhost:8080/miiify/annotations/chapter1-page01/transcription-1",
  "type": "Annotation",
  "motivation": "describing",
  "body": {
    "type": "TextualBody",
    "value": "In principio erat verbum...",
    "format": "text/plain",
    "language": "la"
  },
  "target": {
    "source": "http://localhost:8080/iiif/canvas/chapter1/page01",
    "selector": {
      "type": "FragmentSelector",
      "value": "xywh=100,800,1000,200"
    }
  }
}
```

### How the pieces connect:

1. **Image file**: `chapter1-page01.jpg`
2. **Annotation folder**: `chapter1-page01/` (must match image base name exactly)
3. **Annotation file**: `transcription-1.json` (inside the folder)
4. **Miiify ID**: `/miiify/annotations/chapter1-page01/transcription-1` (container/annotation pattern)
5. **Target canvas**: `/iiif/canvas/chapter1/page01` (dashes → slashes for IIIF hierarchy)

**Remember**: The annotation folder name must exactly match the image filename (without the `.jpg` extension).

## How to Use

### 1. Clone the IIIF-in-a-Box repository

```bash
git clone https://github.com/your-org/iiif-in-a-box.git
cd iiif-in-a-box
```

### 2. Build and start the IIIF services

```bash
./bootstrap.sh build --input-dir /home/john/git/example-iiif-in-a-box-project
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
./bootstrap.sh build --input-dir /home/john/git/example-iiif-in-a-box-project
```

## Notes

- All images are synthetic examples created with ImageMagick
- Texts are excerpts from the Gospel of John (Latin Vulgate) and Chaucer's Canterbury Tales
- Annotations follow the W3C Web Annotation standard
- Canvas IDs use the dash-to-slash pattern: `chapter1-page01.jpg` → `/iiif/canvas/chapter1/page01`
