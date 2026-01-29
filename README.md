# SeaTable Static Catalog Generator

Generate static HTML catalogs from SeaTable database views with images and metadata.

## Features

- Downloads images from SeaTable and stores them locally
- Generates responsive HTML catalog pages
- Supports multiple catalogs from different views using config files
- Images shared across all catalogs (hash-based filenames prevent duplicates)
- Customizable headers and page titles per catalog

## Setup

1. Install dependencies:
```bash
pip install requests
```

2. Create a config file for your catalog (see examples below)

## Usage

**Structure to Generate a Catalog:**

```bash
python3 generate_catalog.py <config_file.json>
```

### Examples:
```bash
# Generate "Available Works" catalog
python3 generate_catalog.py config_produced_works.json

# Generate "Currently Showing" catalog  
python3 generate_catalog.py config_currently_showing.json
```

### Update All Catalogs

```bash
python3 generate_catalog.py config_produced_works.json
python3 generate_catalog.py config_currently_showing.json
git add art/
git commit -m "Update catalogs"
git push
```

## Configuration Files for Catalogs

Each catalog needs a JSON config file with these fields:

```json
{
  "view_name": "SeaTable view name",
  "output_file": "path/to/output.html",
  "header_logo": "path/to/logo.png",
  "header_title": "path/to/title-image.png",
  "page_title": "HTML page title",
  "config:edit this
}
```

### Example: Available Works

**config_available_works.json:**
```json
{
  "view_name": "Produced Works",
  "output_file": "art/catalog.html",
  "header_logo": "page-header-assets/logo.png",
  "header_title": "page-header-assets/available-works.png",
  "page_title": "Available Works - John Woodruff"
}
```

### Example: Currently Showing

**config_currently_showing.json:**
```json
{
  "view_name": "Currently Showing",
  "output_file": "art/currently-showing.html",
  "header_logo": "page-header-assets/logo.png",
  "header_title": "page-header-assets/currently-showing-title.png",
  "page_title": "Currently Showing - John Woodruff"
}
```

## Output Structure

```
art/
├── catalog.html              # Available Works catalog
├── habit-pattern.html        # Habit Pattern catalog
├── page-header-assets/       # Header images
│   ├── logo.png
│   ├── available-works.png
│   └── habit-pattern-title.png
└── images/                   # Shared images (all catalogs)
    ├── a1b2c3d4e5f6.jpg
    ├── b2c3d4e5f6a7.jpg
    └── ...
```

## Image Management

- **Hash-based filenames**: Same image = same filename across all catalogs
- **Shared directory**: All catalogs use `art/images/`
- **No duplicates**: Images downloaded once, reused everywhere
- **Clean unused images**: `rm -rf art/images/* && regenerate all catalogs`

## SeaTable Configuration

The script uses these constants (edit in `generate_catalog.py` if needed):
- **API Token**: Read-only token for SeaTable
- **Server**: https://cloud.seatable.io
- **Table**: "Works & Exhibits"

## Workflow

1. **Edit data** in SeaTable view
2. **Run generator** with appropriate config file
3. **Commit changes**: `git add art/ && git commit -m "Update catalog" && git push`
4. **View live**: https://jofowood.github.io/art/[output-file].html

## Embedding in Website

```html
<iframe src="https://jofowood.github.io/art/catalog.html" 
        width="100%" height="800" frameborder="0" 
        style="border: none;">
</iframe>
```

## Adding New Catalogs

1. Create new config file (e.g., `config_new_series.json`)
2. Set the SeaTable view name
3. Choose output filename
4. Specify header images
5. Run: `python3 generate_catalog.py config_new_series.json`

## Troubleshooting

**"Config file not found"**: Check the config file path  
**"Missing required fields"**: Ensure all 5 required fields are in config  
**Images not loading**: Run the generator to download images  
**Authentication errors**: Verify API token in script
