# 3D Print Pricer - Progressive Web App

## Overview

This is a Progressive Web App (PWA) for calculating 3D print pricing. It supports multi-filament tracking, project management, and can be installed on any device as a standalone application.

## Requirements

All files must be in the same directory:
- print_calculator_pwa.html
- manifest.json
- sw.js
- icon-192.png
- icon-512.png

The application must be served over HTTPS to enable PWA installation features.

## Icon Generation

Before deploying, generate the required icons:

1. Open icon-generator.html in a web browser
2. Download the 192x192 icon and save as icon-192.png
3. Download the 512x512 icon and save as icon-512.png
4. Place both files in the same directory as the other application files

Alternatively, create custom icons with the following specifications:
- Two PNG files: 192x192 pixels and 512x512 pixels
- Square aspect ratio
- File names must be icon-192.png and icon-512.png

## Deployment Options

### Synology NAS

#### Prerequisites

1. Web Station package must be installed on your Synology NAS
   - Open Package Center on your Synology
   - Search for "Web Station"
   - Click Install if not already installed

2. HTTPS must be configured
   - Go to Control Panel > Security > Certificate
   - Verify you have a valid certificate (self-signed is acceptable for personal use)
   - Go to Control Panel > Network > DSM Settings
   - Check "Automatically redirect HTTP connections to HTTPS"

#### File Upload Steps

1. Access File Station on your Synology NAS
   - Open DSM (DiskStation Manager) in your browser
   - Click on File Station

2. Navigate to the web root directory
   - Default location: `/web`
   - If Web Station is configured with a different root, navigate to that location
   - To check Web Station settings: Package Center > Web Station > click on Web Station > Settings

3. Create a new folder for the application (optional but recommended)
   - Right-click in the web directory
   - Select "Create Folder"
   - Name it something like "print-pricer" or "3d-calculator"

4. Upload all application files to this folder
   - Click Upload button in File Station
   - Select all files:
     - print_calculator_pwa.html
     - manifest.json
     - sw.js
     - icon-192.png
     - icon-512.png
   - Wait for upload to complete

#### Finding Your Access URL

Your access URL depends on your Synology configuration:

**Local Network Access:**
```
https://your-nas-ip/print-pricer/print_calculator_pwa.html
```

To find your NAS IP address:
- Go to Control Panel > Info Center > Network
- Look for LAN > IP Address

**If using QuickConnect:**
```
https://your-quickconnect-id.quickconnect.to/print-pricer/print_calculator_pwa.html
```

**If using a custom domain:**
```
https://your-domain.com/print-pricer/print_calculator_pwa.html
```

**If using Synology's default ports:**
- Default HTTPS port is 5001
- URL format: `https://your-nas-ip:5001/print-pricer/print_calculator_pwa.html`
- To check: Control Panel > Network > DSM Settings

#### Testing the Deployment

1. Open the URL in your desktop browser
   - You should see the calculator interface
   - Open browser developer tools (F12)
   - Check Console tab for any errors
   - Look for "Service Worker registered successfully" message

2. Test on mobile device
   - Ensure your phone is on the same network as the NAS
   - Open the URL in Chrome or Safari
   - Verify the page loads correctly

#### Common Synology Issues

**Certificate Warnings:**
- If using a self-signed certificate, browsers will show a warning
- Click "Advanced" and "Proceed anyway"
- This is normal for self-signed certificates on local networks

**Port Access:**
- If accessing from outside your network, ensure ports are forwarded in your router
- Default HTTPS port: 5001
- Alternatively, use QuickConnect for external access without port forwarding

**File Permissions:**
- If files don't load, check permissions in File Station
- Right-click the folder > Properties > Permissions
- Ensure "http" user has Read permission

**Web Station Not Working:**
- Go to Package Center > Web Station
- Ensure it's running (click "Open" to verify)
- Check virtual host settings if using multiple sites

#### Alternative: Using Docker on Synology

For more advanced setups, you can run a dedicated web server in Docker:

1. Install Docker from Package Center
2. Search for "nginx" in Docker Registry
3. Download and configure nginx container
4. Mount your application files as a volume
5. This provides more control but requires Docker knowledge

### GitHub Pages

1. Create a new GitHub repository
2. Upload all files to the repository
3. Enable GitHub Pages in repository settings
4. Access via https://username.github.io/repository-name/print_calculator_pwa.html

### Local Development Server

For testing purposes only (installation features require HTTPS):

```bash
python -m http.server 8000
```

Access via http://localhost:8000/print_calculator_pwa.html

### Alternative Hosting Services

The following services provide free hosting with automatic HTTPS:
- Netlify
- Vercel  
- Firebase Hosting

## Installation Instructions

### Android (Chrome, Edge, Samsung Internet)

1. Navigate to the application URL
2. Open the browser menu
3. Select "Install app" or "Add to Home Screen"
4. The application will appear on the home screen

### iOS/iPadOS (Safari)

1. Navigate to the application URL in Safari
2. Tap the Share button
3. Select "Add to Home Screen"
4. Tap "Add"
5. The application will appear on the home screen

### Windows (Chrome, Edge)

1. Navigate to the application URL
2. Click the install icon in the address bar, or open browser menu and select "Install"
3. Click "Install" in the confirmation dialog
4. The application will be added to the Start Menu

### macOS (Chrome)

1. Navigate to the application URL
2. Open the browser menu
3. Select "Install 3D Print Pricer"
4. The application will be added to the Applications folder

## Verification

After installation, verify the following:
- Application launches as a standalone window without browser UI
- Application functions without an internet connection after initial load
- Data persists between sessions
- Application appears in the device's app launcher

## Updating the Application

To deploy an updated version:

1. Modify sw.js and increment the cache version:
   ```javascript
   const CACHE_NAME = '3d-print-pricer-v2';  // increment version number
   ```
2. Upload updated files to the hosting location
3. Users will receive the update automatically on their next application launch

## Offline Functionality

The service worker caches all application resources on first load. After the initial visit, the application functions completely offline with all features available:
- Price calculations
- Filament management
- Project tracking
- History viewing
- CSV export

## Data Storage

All data is stored locally using browser localStorage. Each device maintains its own independent data store. Data is not synchronized between devices or backed up to any server.

## Troubleshooting

**Install option not available:**
- Verify the application is served over HTTPS
- Localhost testing may not show install prompts on mobile devices
- Try Chrome or Edge browsers

**Service worker not registering:**
- Check browser console for error messages
- Verify sw.js is in the same directory as the HTML file
- Ensure the server is configured to serve .js files with the correct MIME type

**Icons not displaying:**
- Verify icon files exist in the same directory
- Check that file names match exactly (case-sensitive on some systems)
- Clear browser cache and reload

**Data not persisting:**
- Verify localStorage is enabled in browser settings
- Check that the application is running in a normal browser context (not private/incognito mode)

## Features

- Multi-filament cost tracking
- Custom filament presets with automatic usage-based sorting
- Project mode for multi-item jobs
- Complete calculation history
- CSV export functionality
- Offline operation after initial load

## Next Steps

This PWA can be packaged for distribution on the Google Play Store using Capacitor. The process involves wrapping the web application in a native container and building an Android APK file. Google Play Store submission requires a one-time $25 developer account fee.

