# Project Analysis: 3D Interactive Portfolio

This document summarizes the technical architecture and key features of the Rahees Portfolio website.

## 🚀 Tech Stack
- **Framework:** React with TypeScript
- **3D Engine:** Three.js (Core)
- **React Bridge:** @react-three/fiber & @react-three/drei
- **Animations:** GSAP (GreenSock) with ScrollTrigger & ScrollSmoother
- **Build Tool:** Vite

## 🧩 Key Components

### 1. 3D Character Interaction (`Character/`)
- **Loading:** Uses `GLTFLoader` with `DRACOLoader` for compressed models.
- **Security:** Implements a `decryptFile` utility to load encrypted `.enc` model files, preventing direct asset theft.
- **Interactivity:**
    - Tracks mouse and touch movements.
    - Uses `lerp` (Linear Interpolation) for smooth bone rotation.
    - Specifically targets the `Head/Neck` bones (e.g., `spine006`) to create a "look at mouse" effect.
- **Optimization:** Each `Scene` instance is carefully cleaned up (`renderer.dispose()`, `scene.clear()`, `cancelAnimationFrame()`) to prevent memory leaks and duplicate rendering in React Strict Mode.

### 2. High-End Animations (`GSAP`)
- **Scroll Effects:** `ScrollTrigger` is used to sync 3D animations and UI transitions with the user's scroll position.
- **Smooth Scrolling:** `ScrollSmoother` (GSAP Trial) provides a premium inertia-based scrolling experience.

### 3. Responsive 3D Design
- **Resize Handling:** Custom `resizeUtils` update the camera aspect ratio and renderer size dynamically.
- **Touch Support:** Specialized touch event listeners translate finger movements into the same interaction logic used for the mouse.

## 🛠️ Lessons Learned for Future Projects
1. **Always handle cleanup:** Three.js objects aren't automatically garbage collected by React; manual disposal is mandatory.
2. **Bone Mapping:** Different GLTF models have different bone hierarchies; always verify bone names (e.g., Mixamo vs. Custom) before applying rotations.
3. **Asset Protection:** Encrypting `.glb` files and decrypting them on the fly is a clever way to protect custom 3D work.
4. **Performance First:** Use `DRACO` compression and `Suspense` for loading heavy 3D assets to keep the UX snappy.
