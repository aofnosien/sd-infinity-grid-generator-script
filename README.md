# Stable Diffusion Infinity Grid Generator

Extension for the [AUTOMATIC1111 Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) that generates infinite-dimensional grids.

An "infinite axis grid" is like an X/Y plot grid, but with, well, more axes on it. Of course, monitors are 2D, so this is implemented in practice by generating a webpage that lets you select which two primary axes to display, and then choose the current value for each of the other axes.

This design was partially inspired by the "XYZ Plot" script by "xrypgame".

Part of the goal of this system is to develop educational charts, to provide a universal answer to the classic question of "what does (X) setting do? But what about with (Y)?".

There is a built in ability to add description text to fields, for the specific purpose of enhancing educational page output.

The advantage of this design is it allows you to rapidly compare the results of different combinations of settings, without having to wait to generation times.

The disadvantage is that time to generate a grid is exponential - if you have 5 samplers, 5 seeds, 5 step counts, 5 CFG scales... that's 5^4, or 625 images. Add another variable and now it's 3125 images. You can see how this quickly jumps from a two minute render to a two hour render.

### Table of Contents

- [#Examples](Examples)
- [#Installation](Installation)
- [#Usage](Usage)
- [#License](License)

### Examples

TODO

### Installation

- You must have the [AUTOMATIC1111 Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) already installed and working. Refer to that project's readme for help with that.
- Open the WebUI, go to to the `Extensions` tab.
- Click on `Install from URL`
- Copy/paste this project's URL into the `URL for extension's git repository` textbox: `https://github.com/mcmonkeyprojects/sd-infinity-grid-generator-script`
- Click `Install`
- Restart or reload the WebUI

### Usage

Usage comes in three main steps:
- 1: Build a grid definition
- 2: Generate its contents
- 3: Make use of the generated output viewer

#### 1: Grid Definition File

- Grid information is defined by YAML files, in the extension folder under `assets`. Find the `assets/short_example.yml` file to see an example of the full format.
- You can create new files in the assets directory (as long as the `.yml` extension stays), or copy/paste an example file and edit it. I recommend you do not edit the actual example file directly to avoid git issues.
- I recommend editing with a good text editor, such as *VS Code* or *Notepad++*. Don't use *MS Word* or *Windows Notepad* as those might cause trouble.
- All text inputs allow for raw HTML, so, be careful. You can use `&lt;` for `<`, and `&gt;` for `>`, and `&amp;` for `&`.
- The file must have key `grid`, with subkey `title` and `description` to define the file data.
    - It can optionally also have `params` to specify any default parameters.
- The file must have key `axes` to define the list of axes. This is a map-list key - meaning, add subkeys to create a list of each axis.
    - Each axis must have a `title`, and `values`. It can optionally have a `description`.
        - There are two ways to do a value in the value list:
            - Option 1: just do like `steps=10` ... this will set title to `10`, and param `steps` to value `10`, with no description.
            - Option 2: Add a submapping with key `title`, and optional `description`, and then `params` as a sub map of parameters like `steps: 10`

Micro example:
```yml
grid:
    title: Tiny example
    description: This is just to show core format rules. View the example `.yml` files in assets for better examples.
axes:
    1:
        title: Sampler
        values:
            1: sampler=Euler
```

- Names and descriptions can always be whatever you want, as HTML text.
- Settings supported for parameters:
    - `sampler`, `seed`, `steps`, `CFGscale`, `Model`, `VAE`, `Width`, `Height`, `Hypernetwork`, `HypernetworkStrength`, `Prompt`, `NegativePrompt`, `VarSeed`, `VarStrength`, `ClipSkip`, `Denoising`, `ETA`, `SigmaChurn`, `SigmaTmin`, `SigmaNoise`, `ETANoiseSeedDelta`
    - All names are case insensitive and spacing insensitive. That means `CFG scale`, `cfgscale`, `CFGSCALE`, etc. are all read as the same.
    - Inputs where possible also similarly insensitive, including model names.
    - Inputs have error checking at the start, to avoid the risk of it working fine until 3 hours into a very big grid run.

#### 2: Grid Content Generation via WebUI

- Open the WebUI
- Go to the `txt2img` or `img2img` tab
- At the bottom of the page, find the `Script` selection box, and select `Generate Infinite-Axis Grid`
- Select options at will. You can hover your mouse over each option for extra usage information.
- Select your grid definition file from earlier.
    - If it's not there, you might just need to hit the Refresh button on the right side.
    - If it's still not, there double check that your file is in the `assets/` folder of the extension, and that it has a proper `.yml` extension.
- Hit your `Generate` button, and wait.

#### 3: Using The Output

TODO

----------------------

### Licensing pre-note:

This is an open source project, provided entirely freely, for everyone to use and contribute to.

If you make any changes that could benefit the community as a whole, please contribute upstream.

### The short of the license is:

You can do basically whatever you want, except you may not hold any developer liable for what you do with the software.

### The long version of the license follows:

The MIT License (MIT)

Copyright (c) 2022 Alex "mcmonkey" Goodwin

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.