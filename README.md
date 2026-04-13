# OFAC SDN Advanced Search Application

A robust, fully standalone web application for indexing, querying, and exporting OFAC SDN sanctions data from the Advanced XML schema. Zero external dependencies — runs on pure Python 3 and vanilla HTML/CSS/JavaScript.

## 🚀 Features

- **Zero-Dependency Architecture**: Built entirely with Python standard libraries and vanilla browser APIs. No `npm`, no `pip`, no frameworks.
- **Streaming XML Engine**: Parses 117MB+ `sdn_advanced.xml` files in seconds using memory-efficient `xml.etree.ElementTree.iterparse`, indexing 18,000+ profiles without RAM exhaustion.
- **Deep Reference Resolution**: Automatically resolves all `ReferenceValueSets` (70+ dictionaries including Country, FeatureType, SanctionsType, LegalBasis, etc.) into human-readable values at boot time.
- **SanctionsEntries Linking**: Performs a second parsing pass to match `SanctionsEntry.ProfileID` to `DistinctParty.FixedRef`, enriching each profile with listing dates, legal basis, sanctions programs, and measures.
- **Intelligent Name Assembly**: Uses `NamePartGroups` and `NamePartTypeID` mappings to correctly order name components (First Name → Last Name) and concatenate primary aliases.
- **Unique & Batch Search**: Query individual profiles or upload a CSV file to process thousands of checks, with comprehensive flattened CSV output.
- **Interactive Dataset Explorer**: Browse and search raw XML datasets (Locations, IDRegDocuments, ProfileRelationships, SanctionsEntries, ReferenceValueSets) directly in the browser with streaming query limits.
- **Dark/Light Theme Toggle**: Full theme support with `localStorage` persistence across sessions.
- **Hot-Swappable Datasets**: Upload new XML files via the UI to reload the database without restarting the server.

---

## 🛠️ Quick Start

No installation required beyond a Python 3 runtime.

1. Place `sdn_advanced.xml` in the project root directory.
2. Start the server:
   ```bash
   python server.py
   ```
3. Open `http://127.0.0.1:8000` in your browser.

---

## 📁 Project Structure

| File | Description |
|---|---|
| `server.py` | Backend: HTTP server, XML parsing, REST API endpoints, CSV/ZIP export |
| `index.html` | UI structure with tabs for Unique Search, Batch Search, Datasets, and Upload |
| `script.js` | Frontend logic: API interaction, dynamic rendering, CSV generation |
| `style.css` | Glassmorphic design system with dark/light theme variables |
| `CHANGELOG.md` | Version history of all changes |

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/status` | Database status, profile count, feature types |
| `GET` | `/api/search/unique?q=<query>` | Search profiles by name or ID (max 50 results) |
| `GET` | `/api/search/dataset?type=<name>&q=<query>` | Search raw datasets (max 100 results) |
| `GET` | `/api/export/<dataset>` | Download full dataset as CSV or ZIP |
| `GET` | `/api/template` | Download batch search CSV template |
| `POST` | `/api/search/batch` | Upload CSV for batch matching, returns flattened CSV |
| `POST` | `/api/upload` | Upload new `sdn_advanced.xml` to reload database |

---

## 📊 CSV Export Columns

Both unique and batch search exports include:

| Column Group | Columns |
|---|---|
| **Core** | SearchTerm*, Matched*, OFAC_ID, PrimaryName, Type, PartyComment, Aliases |
| **Sanctions** | SanctionsList, EntryDate, LegalBasis, SanctionsPrograms |
| **Features** | One column per FeatureType (Birthdate, Gender, Nationality Country, Place of Birth, Title, etc.) |

*\* Batch export only*

---

## 🧪 Testing & Validation

- **Boot Performance**: 18,698 profiles + 18,874 sanctions entries indexed in ~5 seconds.
- **Search Accuracy**: Partial text and ID queries return correct profile matches with full alias resolution.
- **Reference Integrity**: All `DetailReferenceID`, `LocationID`, `SanctionsTypeID`, and `LegalBasisID` values resolve to readable strings.
- **Export Completeness**: CSV exports contain all linked data: features, locations, sanctions programs, legal bases, and entry dates.
- **Theme Robustness**: All UI elements maintain proper contrast and visibility in both dark and light modes.
