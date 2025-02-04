<p align="center">
    <picture>
    <img alt="kiuikit_logo" src="docs/source/_static/logo.png" width="50%">
    </picture>
    </br>
    <b>Kiuikit</b>
    </br>
    <code>pip install kiui</code>
    &nbsp;&nbsp;&bull;&nbsp;&nbsp;
    <a href="https://kit.kiui.moe/">Documentation</a>
</p>

A toolkit for computer vision (especially 3D vision) tasks.

**Features**:
* Collection of *maintained, reusable and trustworthy* code snippets.
* Always using lazy import so the code is not slowed down by `import kiui`.
* Useful CLI tools, such as a GUI mesh renderer.

### Install

```bash
# released
pip install kiui # install the minimal package
pip install kiui[full] # install optional dependencies

# latest
pip install git+https://github.com/ashawkey/kiuikit.git # only the minimal package
```

### Basic Usage

```python
import kiui

### auto import
kiui.env() # os, glob, math, time, random, argparse
kiui.env('data') # above + np, plt, cv2, Image, ...
kiui.env('torch') # above + torch, nn, F, ...

### quick inspection of array-like object
x = torch.tensor(...)
y = np.array(...)

kiui.lo(x)
kiui.lo(x, y) # support multiple objects

kiui.lo(kiui) # or any other object (just print with name)

### io utils
# read image as-is in RGB order
img = kiui.read_image('image.png', mode='float') # mode: float (default), pil, uint8, tensor
# write image
kiui.write_image('image.png', img)

### visualization tools
img_tensor = torch.rand(3, 256, 256) 
# tensor of [3, H, W], [1, H, W], [H, W] / array of [H, W ,3], [H, W, 1], [H, W] in [0, 1]
kiui.vis.plot_image(img)
kiui.vis.plot_image(img_tensor)

### mesh utils
from kiui.mesh import Mesh
mesh = Mesh.load('model.obj')
kiui.lo(mesh.v, mesh.f) # CUDA torch.Tensor suitable for nvdiffrast
mesh.write('new.obj')
mesh.write('new.glb') # support exporting to GLB/GLTF too (texture embedded).

# perceptual loss (from https://github.com/richzhang/PerceptualSimilarity)
from kiui.lpips import LPIPS
lpips = LPIPS(net='vgg').cuda()
loss = lpips(input, target) # [B, 3, H, W] image in [-1, 1]
```

CLI tools:
```bash
# background removal utils
python -m kiui.cli.bg --help
python -m kiui.cli.bg input.png output.png
python -m kiui.cli.bg input_folder output_folder

# openpose detector
python -m kiui.cli.pose --help

# blip2 image captioning
python -m kiui.cli.blip --help

# hed edge detector
python -m kiui.cli.hed --help

# zoe depth estimation (extra dep: pip install timm==0.6.11)
python -m kiui.cli.depth_zoe --help

# midas depth estimation (dpt-large)
python -m kiui.cli.depth_midas --help

# sr (Real-ESRGAN from https://github.com/ai-forever/Real-ESRGAN/tree/main)
python -m kiui.sr --help
python -m kiui.sr image.jpg --scale 2 # save to image_2x.jpg
kisr image.jpg --scale 2 # short cut cmd

# made-in-heaven timer (https://github.com/ashawkey/made-in-heaven-timer)
python -m kiui.cli.timer --help

# mesh format conversion (only for a single textured mesh in obj/glb)
python -m kiui.cli.convert input.obj output.glb
kico input.obj output.glb # short cut cmd
kico mesh_folder/ video_folder --fmt .mp4 # render all meshes into rotating videos

# aesthetic predictor v2 (https://github.com/christophschuhmann/improved-aesthetic-predictor)
python -m kiui.cli.aes --help

# compare content of two directories
python -m kiui.cli.dircmp <dir1> <dir2>

# lock requirements.txt package versions based on current environment
python -m kiui.cli.lock_version <requirements.txt>
```

GUI tools:
```bash
# open a GUI to render a mesh (extra dep: nvdiffrast)
python -m kiui.render --help
python -m kiui.render mesh.obj
python -m kiui.render mesh.glb --pbr # render with PBR (metallic + roughness)
python -m kiui.render mesh.obj --save_video out.mp4 --wogui # save 360 degree rotating video
kire --help # short cut cmd

# open a GUI to render and edit pose (openpose convention, controlnet compatible)
python -m kiui.poser --help
python -m kiui.poser --load 3head # load preset 3 headed skeleton
```

WebGUI tools:
```bash
# open a web GUI to render a mesh (extra dep: viser)
python -m kiui.render_viser --help
vire --help # short cut cmd
```