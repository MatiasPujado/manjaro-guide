# Graphics

It might not be necessary to install Mesa and Vulkan drivers. These components are primarily used for rendering graphics in applications that support them, such as many modern
games and some professional software. If you don't use these applications, you can skip this section.

## MESA

Mesa, also called Mesa3D and The Mesa 3D Graphics Library, is an open source implementation of OpenGL, Vulkan, and other graphics API specifications. Mesa translates these
specifications to vendor-specific graphics hardware drivers.

We can install Mesa with the following command:

```Bash
sudo pacman -S --needed mesa mesa-utils lib32-mesa-utils mesa-demos lib32-mesa-demos libva-mesa-driver mesa-vdpau lib32-mesa-vdpau glu
```

To check if Mesa is installed correctly, run the following command:

```Bash
glxinfo | grep OpenGL
```

The output should look like this:

```Bash
❯ glxinfo | grep OpenGL
OpenGL vendor string: AMD
OpenGL renderer string: AMD Radeon RX 5500 XT (radeonsi, navi14, LLVM 18.1.8, DRM 3.54, 6.6.41-1-MANJARO)
OpenGL core profile version string: 4.6 (Core Profile) Mesa 24.1.3-manjaro1.1
OpenGL core profile shading language version string: 4.60
OpenGL core profile context flags: (none)
OpenGL core profile profile mask: core profile
OpenGL core profile extensions:
OpenGL version string: 4.6 (Compatibility Profile) Mesa 24.1.3-manjaro1.1
OpenGL shading language version string: 4.60
OpenGL context flags: (none)
OpenGL profile mask: compatibility profile
OpenGL extensions:
OpenGL ES profile version string: OpenGL ES 3.2 Mesa 24.1.3-manjaro1.1
OpenGL ES profile shading language version string: OpenGL ES GLSL ES 3.20
OpenGL ES profile extensions:
```

If in the value of `OpenGL renderer string:` you see `llvmpipe`, that means your system does not use the GPU but the CPU instead to render the computer graphics. If you want to use
the GPU, look at the GraphicsCard page. Sometimes you need to install the proprietary package from the non-free repositories to activate the driver.


### 3D acceleration

To determine whether 3D acceleration is working, use the `glxinfo tool`:

```Bash
glxinfo  | grep rendering
```

### Testing performance

> **Note**
>
> The `gears test` is not very effective, many drivers work very well with a bad FPS in this test.
>
{style="note"}

<br/>

To see how many frames per second your video card is putting out, run the following command:

```Bash
glxgears -info
```

[Learn more about benchmarking options.](references.md#benchmarking)

## Vulkan

Vulkan is a low-level, low-overhead cross-platform API and open standard for 3D graphics and computing. It was intended to address the shortcomings of OpenGL and grant developers
more control over the GPU. It is designed to support a wide variety of GPUs, CPUs and operating systems, and it is also designed to work with modern multicore CPUs.

Install Vulkan with the following command:

```Bash
sudo pacman -S --needed amdvlk qt6-shadertools vulkan-extra-layers vulkan-extra-tools vulkan-html-docs vulkan-tools vulkan-validation-layers python-glfw vkd3d vkmark lib32-amdvlk lib32-vkd3d lib32-vulkan-validation-layers vulkan-mesa-layers
```

To ensure that Vulkan is working with your hardware, run the following command:

```Bash
vulkaninfo | less
```
