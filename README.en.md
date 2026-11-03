# 3D Model Viewer

English | [日本語](README.md)

## Overview

A single-HTML 3D model viewer that runs entirely in the browser. No installation is required, and it works on any OS with a supported browser, including Windows, macOS, Linux, iPhone, and Android. (An internet connection is required to fetch the external libraries.)

## Key Features

- Single HTML file, no installation
- Runs in the browser regardless of OS or environment
- Models and motions can be loaded independently
- Motion conversion between VRM and MMD
- Touch controls (smartphones and tablets)
- History and playlists saved with IndexedDB

> Note: VRM / VRMA are the primary supported formats. Support for other formats is approximate.

## Supported Formats

### Loading

| Type | Formats |
|---|---|
| Model | `.vrm` `.glb` `.pmx` `.pmd` `.zip` (MMD bundle) `.fbx` `.obj` |
| Motion | `.vrma` `.vmd` |

### Saving

| Type | Formats |
|---|---|
| Model | VRM, GLB, MMD |
| Motion | VRMA, VMD |

## External Modules Used

These are used mainly for 3D rendering, model loading, VRM handling, motion conversion, and ZIP extraction.

### 1. External Modules

| Module | Main function | How it is used in this app |
|---|---|---|
| Three.js 0.160.0 | 3D rendering engine | Model display, camera, lighting, bone manipulation, animation playback |
| three-vrm 3.0.0 | VRM support | Loading VRM, humanoid bones, expressions, hair and clothing physics |
| three-vrm-animation 3.0.0 | VRMA support | Loading VRMA, converting to animations for VRM |
| fflate 0.8.2 | ZIP compression / extraction | Extracting MMD models and textures stored in ZIP files |
| Ammo.js (bundled with Three.js 0.160.0) | Physics engine | Rigid bodies, collisions, and physics for MMD models (loaded only when needed) |

### 2. Extensions Bundled with Three.js

These are not standalone external libraries; they are features included with Three.js.

| Module | Function | Purpose |
|---|---|---|
| GLTFLoader | Loads GLB / glTF | Loading 3D models |
| MMDLoader | Loads PMX / PMD / VMD | Loading MMD models and motions |
| FBXLoader | Loads FBX | Loading FBX models |
| OBJLoader | Loads OBJ | Loading OBJ models |
| MTLLoader | Loads MTL | Setting materials and textures for OBJ |
| MMDAnimationHelper | MMD animation management | Managing VMD playback and physics |
| GLTFExporter | Exports GLB | Converting MMD / FBX / OBJ and others to GLB |

Notes:

- MMDParser is used internally by MMDLoader.
- GLTFExporter is loaded when a save operation is performed.

### 3. Which Module Is Used for What

**Loading models**

| Format | Module(s) |
|---|---|
| VRM | three-vrm |
| GLB | GLTFLoader |
| MMD | MMDLoader + fflate (for ZIP) |
| FBX | FBXLoader |
| OBJ | OBJLoader + MTLLoader |

**Loading motions**

- VRMA → three-vrm-animation
- VMD → MMDLoader
- Applying motions across different model formats → the app's own conversion code

**Playback and physics**

- Playback → Three.js AnimationMixer
- VRM secondary motion → VRM SpringBone
- MMD physics → Ammo.js

**Saving**

- GLB → GLTFExporter

### 4. Requirements and Notes

- External modules are fetched from the jsDelivr CDN.
- An internet connection is required.
- Versions are pinned, which helps avoid sudden behavior changes caused by library updates.
- IndexedDB, localStorage, WebGL, and similar features are standard browser features, not external modules.

## Summary

The core of this app is Three.js. It is combined with the following:

- VRM handling → three-vrm
- Motion handling → three-vrm-animation and MMDLoader
- ZIP handling → fflate
- MMD physics → Ammo.js

In addition, motion conversion between models, ground-contact correction, approximate foot IK, finger bending, and similar features are implemented directly in the HTML.

## Rights

- The rights to all external modules belong to their respective authors.
- The rights and terms of use for the models and motions you load are governed by their respective rights holders. Please follow them.
- This app, including its accompanying files, may be freely modified and redistributed.
- Copyright notices are not required, but they are appreciated.
