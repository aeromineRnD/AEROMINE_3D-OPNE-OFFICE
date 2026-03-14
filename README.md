# Aeromine 3D Open Office

Interactive 3D facility map for the Aeromine open office environment. Built with Three.js and Vite, it renders a GLTF scene and overlays clickable pins on safety equipment, emergency devices, and meeting areas. Clicking a pin smoothly animates the camera to the object and opens an information panel with location, description, and contact details.

## Features

### 3D Viewer
- WebGL rendering with ACES filmic tone mapping and SRGB color space for accurate material representation
- PBR environment lighting via PMREMGenerator with RoomEnvironment, ensuring metallic and specular materials render correctly
- Orbit controls with inertia damping, polar angle limits to prevent camera going below ground, and configurable zoom range
- Automatic camera fit on model load, centering the scene and setting near/far planes relative to model scale
- Smooth camera focus animation with ease-in-out interpolation when navigating to an object
- Per-object configurable camera approach direction, allowing wall-mounted objects to be viewed correctly regardless of orientation
- Reset camera button that smoothly returns to the initial overview from any position

### Interactive Pins
- CSS2D overlay pins rendered above each object in world space, always facing the camera
- Pulsing ring animation on each pin to draw attention without being intrusive
- Color-coded by category: fire safety (red), emergency (orange), first aid (green), meeting rooms (blue)
- Hover tooltip showing the object name before clicking
- Pin visibility and interactivity respond to both search and category filters simultaneously

### Information Panel
- Slide-in panel on desktop (right side, 320px) with cubic-bezier transition
- Responsive bottom sheet on mobile screens, occupying 25% of viewport height with a drag-handle indicator
- Displays object name, category, location, description, contact, and an optional photo
- Closes on outside click or via the close button

### Navigation and Filtering
- Real-time search bar in the header, filtering pins by object name or category as you type
- Category legend with four toggle buttons, allowing individual categories to be shown or hidden
- Search and category filters work together, a pin is visible only if it satisfies both conditions
- All filter state is in-memory with no page reloads or URL changes

## Technology Stack

- [Three.js](https://threejs.org/) v0.169 - 3D rendering, GLTF loading, CSS2D overlays
- [Vite](https://vitejs.dev/) v5.4 - development server and production bundler

## Project Structure

```
├── index.html
├── style.css
├── src/
│   ├── main.js           # Application entry point
│   ├── viewer.js         # Three.js scene, camera, lighting, model loading
│   ├── tag-manager.js    # CSS2D pin placement, filtering, category toggling
│   ├── popup.js          # Info panel UI component
│   └── site-data.js      # Object definitions and camera directions
└── public/
    └── models/
        ├── openOffice.gltf
        ├── openOffice.bin
        └── images/
```

## Setup

Install dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

Build for production:

```bash
npm run build
```

Preview the production build:

```bash
npm run preview
```

## Adding or Updating Objects

All interactive objects are defined in `src/site-data.js`. Each entry maps to a node name in the GLTF scene graph.

```js
{
  meshName:    'NodeNameInGLTF',   // must match the Three.js scene node name exactly
  name:        'Display Name',
  category:    'Fire Safety',      // Fire Safety | First Aid | Emergency Alert | Emergency Egress | Meeting Room
  icon:        '',
  location:    'Zone A - East Wall',
  description: 'Object description shown in the info panel.',
  contact:     'Safety Officer',
  photo:       null,               // relative path to image or null
  camDir:      [0.3, 0.4, -1],    // optional camera approach vector [x, y, z]
}
```

Note: Three.js strips dots from GLTF node names on load. A Blender object named `Cabinet.Body` becomes `CabinetBody` at runtime. Use the browser console to verify node names if a pin does not appear.

## 3D Model

The model files are located in `public/models/`. The GLTF references textures relative to itself inside the `images/` subfolder. If the model is re-exported from Blender, ensure all image URIs in the GLTF use the `images/` prefix, and that any material base color factors are set explicitly for materials that rely solely on texture maps.

## Browser Requirements

WebGL 2.0 capable browser. Tested on Chrome, Firefox, Safari, and Edge.

## Author

Aeromine RnD - [www.aeromine.info](https://www.aeromine.info)
