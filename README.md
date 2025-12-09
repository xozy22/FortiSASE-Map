# SASE Map

An interactive web-based map visualization tool for Fortinet's SASE (Secure Access Service Edge) global data center locations, including compute nodes and edge points of presence (PoPs).

## Overview

SASE Map is a browser-based visualization tool that displays Fortinet's global SASE infrastructure on an interactive map. It allows users to:
- View and explore Fortinet's global network of compute and edge locations
- Filter locations by subscription tier (Standard, Advanced, Comprehensive)
- Upload and visualize customer location data
- Measure distances and calculate network latency between locations
- Toggle visibility of different location types

**DISCLAIMER:** This is not an official solution by Fortinet. This tool is provided as-is for convenience. Use at your own risk.

**Created by:** Alexander Uhlmann

## Features

### Interactive Map
- **Global Visualization**: View all Fortinet SASE locations on an OpenStreetMap-based interactive map
- **Marker Clustering**: Automatically groups nearby locations for better visibility at different zoom levels
- **Location Details**: Click on any marker to view detailed information about that location

### Location Types
The map displays three types of locations, each with distinct color coding:

1. **Customer Locations** (Pink)
   - User-uploaded customer site locations
   - Optional overlay for visualizing your infrastructure

2. **Compute PoPs** (Green)
   - Analytics and endpoint management locations
   - Core SASE infrastructure nodes

3. **Edge PoPs** (Blue)
   - Access locations for SASE services
   - Edge computing and networking nodes

### Filtering & Controls

#### Subscription Tier Filtering
Filter locations based on Fortinet SASE subscription tiers:
- **Standard**: Shows only locations available with any subscription
- **Advanced**: Shows locations available with Standard and Advanced subscriptions
- **Comprehensive**: Shows all locations (Standard, Advanced, and Comprehensive subscriptions)

#### Visibility Toggles
- Show/hide Customer Locations
- Show/hide Compute PoPs
- Show/hide Edge PoPs

#### Additional Controls
- **Center Map**: Automatically centers and zooms the map to show all visible markers
- **Upload Customer Data**: Import your own location data (JSON format)
- **Clear Customer Data**: Remove uploaded customer locations

### Distance & Latency Measurement
- Built-in polyline measurement tool
- Calculates both distance (in kilometers) and estimated network latency
- **Latency Formula**: Round-trip latency calculated as: `distance (km) × 4.9 μs × 2`
- Interactive measurement with point-to-point accuracy

### Session Persistence
The application saves your session preferences:
- Current map view (center position and zoom level)
- Subscription filter selection
- Layer visibility settings (Compute/Edge PoPs)
- Uploaded customer location data (persists during browser session)

## Location Naming Convention

Locations follow a standardized naming pattern: `<IATA-Code>-<Type>`

**Type Indicators:**
- **Fx**: Fortinet Compute Node
- **Gx**: GCP (Google Cloud Platform) Compute Node
- **Ax**: AWS (Amazon Web Services) Compute Node
- **Ex**: Edge Node

**Example:** `IAD-F4` = Ashburn, Virginia (Washington Dulles International Airport) - Fortinet Compute Node #4

## Installation & Usage

### Prerequisites
- A modern web browser (Chrome, Firefox, Safari, Edge)
- Python 3 (for running the local server)

### Quick Start

1. **Clone or download this repository**

2. **Start the local web server**
   ```bash
   ./run-local-server.sh
   ```
   Or manually:
   ```bash
   python3 -m http.server 9000
   ```

3. **Open your browser** and navigate to:
   ```
   http://localhost:9000
   ```

4. **Explore the map**
   - Use your mouse to pan and zoom
   - Click on markers to view location details
   - Use the controls in the top-right corner to filter and customize the view

## File Structure

```
sasemap/
├── index.html                  # Main application file
├── locations.json              # Fortinet SASE location data
├── customer-example.json       # Sample customer location format
├── run-local-server.sh         # Script to start local web server
├── sase-locations.csv          # CSV export of location data
├── sase-locations-new.csv      # Updated CSV export
└── README.md                   # This file
```

## Customer Data Format

To upload your own customer location data, create a JSON file following this structure:

```json
[
  {
    "name": "Office Name or Description",
    "location": {
      "lat": 37.368832,
      "long": -122.036346
    }
  },
  {
    "name": "Another Location",
    "location": {
      "lat": 40.712776,
      "long": -74.005974
    }
  }
]
```

### Required Fields
- `name`: String - Location name or description
- `location.lat`: Number - Latitude coordinate
- `location.long`: Number - Longitude coordinate

### Tips
- Coordinates must be valid numeric values
- Use decimal degrees format (e.g., 37.368832, not 37°22'7.8")
- Reference `customer-example.json` for a complete example with multiple locations

## Technical Details

### Dependencies
All dependencies are loaded via CDN (no local installation required):

- **Leaflet.js** - Core mapping library
- **Leaflet.markercluster** - Marker clustering functionality
- **Leaflet.PolylineMeasure** - Distance and latency measurement
- **OpenStreetMap** - Base map tiles

### Browser Compatibility
- Chrome/Edge (recommended)
- Firefox
- Safari
- Any modern browser with JavaScript enabled

### Data Storage
- Uses browser `sessionStorage` for temporary data persistence
- Customer location data is cleared when the browser session ends
- No data is sent to external servers

## Location Data Fields

Each location in `locations.json` contains:

| Field | Description | Example Values |
|-------|-------------|----------------|
| `name` | Location identifier | "Ashburn - Virginia - USA (IAD-F4)" |
| `location.lat` | Latitude | 39.043757 |
| `location.long` | Longitude | -77.487442 |
| `cloud` | Cloud provider | "Fortinet", "AWS", "GCP" |
| `analyticsAndEndpointManagement` | Analytics availability | "Yes", "No" |
| `endpointManagementLocation` | Endpoint mgmt availability | "Yes", "No" |
| `accessLocation` | Access location type | "Yes", "No" |
| `availability` | Subscription tier | "Any subscription", "Advanced or Comprehensive", "Comprehensive only" |

## Controls Reference

### Map Navigation
- **Pan**: Click and drag the map
- **Zoom**: Scroll wheel or use +/- buttons
- **Marker Click**: View location details

### Distance Measurement Tool
1. Click the ruler icon to activate
2. Click on the map to set start point
3. Click additional points to continue the line
4. View distance (km) and latency (ms) in the tooltip
5. Double-click to finish measurement

### Keyboard Shortcuts (Measurement Tool)
- **CTRL + Click**: Add or resume a measurement line
- **SHIFT + Click**: Delete a point from the line
- **Click + Drag**: Move a point

## Known Limitations

- Requires internet connection for map tiles and CDN resources
- Customer data persists only during the browser session
- Random offsets (up to ±0.1 degrees) are applied to prevent exact marker overlap
- Latency calculation is a simplified estimate based on fiber optic speed

## Support & Documentation

For more information about Fortinet SASE global data centers, visit:
[Fortinet SASE Reference Guide](https://docs.fortinet.com/document/fortisase/latest/reference-guide/663044/global-data-centers)

## License

This project is provided as-is without warranty. Not an official Fortinet product.

---

**Note**: This tool is designed for visualization and planning purposes. Always verify location availability and specifications with official Fortinet documentation before making deployment decisions.
