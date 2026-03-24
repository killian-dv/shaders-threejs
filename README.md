# Shaders — Three.js Journey

Quick recap of the **Shaders** lesson from [Three.js Journey](https://threejs-journey.com/) by Bruno Simon.

---

## What this lesson covers

**Shaders** are small programs that run on the GPU: they describe how to transform vertices (**vertex shader**) and how to color each pixel (**fragment shader**). In Three.js, you wire them up with `ShaderMaterial` by passing GLSL code and **uniforms** (parameters from JavaScript).

---

## What I learned

### 1. Vertex → fragment pipeline

- The **vertex shader** receives each vertex (`position`, `uv`, etc.) and must output `gl_Position` (projected screen coordinates).
- You rebuild the usual chain: `modelMatrix` → `viewMatrix` → `projectionMatrix`, i.e. from model space to clip space.
- The **fragment shader** runs for each fragment (candidate pixel) and outputs a color (`gl_FragColor` in GLSL 1.0).

### 2. Varyings: passing data from vertex to fragment

- Variables declared in both shaders with the same type and name (e.g. `varying vec2 vUv`, `varying float vElevation`).
- The GPU **interpolates** these values across the triangle: useful for texture UVs or effects that vary over the surface (here, elevation).

### 3. Deforming geometry in the vertex shader

- By changing position before projection (e.g. offsetting `z` with `sin` on `x` and `y`), you get a **wavy surface** without changing the mesh in JavaScript.
- The **uniforms** `uFrequency` and `uTime` control wave frequency and time-based animation.

### 4. Uniforms and hooking them up in Three.js

- **Uniforms** are constant inputs for the whole draw call: frequency, time, color, texture, etc.
- With `ShaderMaterial`, you declare them in the `uniforms` object; you update `uTime` in the animation loop to drive the shader.

### 5. Textures in the fragment shader

- Use type `sampler2D` and `texture2D(uTexture, vUv)` to sample an image on the plane.
- You can combine the texture with varyings (e.g. darken or lighten based on `vElevation`) for a richer look.

### 6. Project tooling

- **Vite** imports `.glsl` files as strings so they can be passed to `ShaderMaterial`.
- **lil-gui** tweaks `uFrequency` live for quick experimentation.

---

## Run the project

```bash
npm install
npm run dev
```

---

_Part of the Three.js Journey course by Bruno Simon._
