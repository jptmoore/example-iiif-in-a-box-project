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

## How to Use

### 1. Navigate to the IIIF-in-a-Box directory

```bash
cd /home/john/git/iiif-in-a-box
```

### 2. Create the Docker network (one-time setup)

```bash
docker network create iiif-network
```

### 3. Build and start the IIIF services

```bash
./bootstrap.sh build --input-dir /tmp/example-iiif-project
```

This command will:
- Process the images and create IIIF-compliant image tiles
- Generate IIIF Collection and Manifest JSON files
- Import annotations into the Miiify annotation server
- Index annotations for full-text search with AnnoSearch
- Start all Docker services (nginx, IIPImage, Miiify, AnnoSearch, Quickwit, Tamerlane viewer)

### 4. View in your browser

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

### 5. Explore the features

In the Tamerlane viewer, you can:
- **Zoom and pan** the high-resolution images
- **View annotations** overlaid on the images
- **Search** the transcriptions using full-text search
- **Filter annotations** by type (transcriptions, comments, tags, highlights)
- **Navigate** between pages and chapters

### 6. Test the search feature

The annotations are fully searchable. Try searching for:
- "verbum" (Latin text)
- "pilgrimages" (Middle English text)
- "Eucharist" (tags)

## Annotation Examples

This project demonstrates different types of W3C Web Annotations:

### Transcription (motivation: describing)
```json
{
  "motivation": "describing",
  "body": {
    "type": "TextualBody",
    "value": "Transcribed text with translation...",
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

### Comment (motivation: commenting)
Scholarly notes and analysis attached to specific regions of the image.

### Tags (motivation: tagging)
Subject classifications and keywords for discovery and organization.

### Highlight (motivation: highlighting)
Visual emphasis on important passages or features.

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

## Customizing This Example

### Add More Images

1. Add new images to the `images/` folder using the naming pattern:
   ```
   chapter3-page01.jpg
   chapter3-page02.jpg
   ```

2. Create matching annotation folders:
   ```
   annotations/chapter3-page01/
   annotations/chapter3-page02/
   ```

3. Rebuild:
   ```bash
   ./bootstrap.sh build --input-dir /tmp/example-iiif-project
   ```

### Add More Annotations

1. Create new JSON files in the appropriate annotation folder
2. Follow the W3C Web Annotation format (see examples)
3. Make sure the `target.source` matches the canvas ID pattern:
   - Format: `http://localhost:8080/iiif/canvas/{chapter}/{page}`
   - Example: `http://localhost:8080/iiif/canvas/chapter1/page01`
4. Rebuild to import the new annotations

### Modify Metadata

Edit `config.yml` to change:
- Project name and title
- Descriptive metadata
- Provider information
- Rights statements

Then rebuild to apply changes.

## Learning More

- **IIIF-in-a-Box Documentation**: See the main README.md in `/home/john/git/iiif-in-a-box`
- **W3C Web Annotation Model**: https://www.w3.org/TR/annotation-model/
- **IIIF Presentation API**: https://iiif.io/api/presentation/3.0/

## Notes

- All images are synthetic examples created with ImageMagick
- Texts are excerpts from the Gospel of John (Latin Vulgate) and Chaucer's Canterbury Tales
- Annotations follow the W3C Web Annotation standard
- Canvas IDs use the dash-to-slash pattern: `chapter1-page01.jpg` → `/iiif/canvas/chapter1/page01`
