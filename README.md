# Kombi
[![Build Status](https://travis-ci.org/kombiHQ/kombi.svg?branch=master)](https://travis-ci.org/kombiHQ/kombi)

<p align="center">
    <img src="data/ui/icons/kombi.png" with="256" height="256"/>
</p>

Kombi is a library & tool focused in processing data across different applications and libraries.

This is done by providing an API that simplifies the process of grabbing whether the partial or full data generated as output by a task (application/library) and use them as input to another task during the execution of nested tasks.

Such as during the ingestion of files, versioning data, creating different variations for the same data, (etc). Where these processes may look simple at first glance they grow in complexity overtime, making them hard to maintain specially when different applications are involved to accomplish the task.

In order to avoid writing boilerplate code, Kombi provides high-level declarative definitions that can be expressed through:

### YAML

<details open="1"><summary>Expand</summary>
<p>


```yaml
---
vars:
  prefix: "/tmp"
tasks:
- run: gafferScene
  metadata:
    match.types:
    - png
    match.vars:
      imageType:
      - sequence
  options:
    scene: "{configDirectory}/scene.gfr"
  target: "{prefix}/gafferBlurImageSequence/(newver <parent> as <ver>)/{name}_<ver>.(pad {frame} 6).exr"
  tasks:
  - run: ffmpeg
    options:
      frameRate: 23.976
      sourceColorSpace: bt709
      targetColorSpace: smpte170m
    target: "(dirname {filePath})/{name}.mov"
```
</p>
</details>

### TOML
<details><summary>Expand</summary>
<p>

```toml
[vars]
prefix = "/tmp"

[[tasks]]
run = "gafferScene"
target = "{prefix}/gafferBlurImageSequence/(newver <parent> as <ver>)/{name}_<ver>.(pad {frame} 6).exr"

  [tasks.metadata]
  "match.types" = [
    "png"
  ]

    [tasks.metadata."match.vars"]
    imageType = [
      "sequence"
    ]

  [tasks.options]
  scene = "{configDirectory}/scene.gfr"

  [[tasks.tasks]]
  run = "ffmpeg"
  target = "(dirname {filePath})/{name}.mov"

    [tasks.tasks.options]
    frameRate = 23.976
    sourceColorSpace = "bt709"
    targetColorSpace = "smpte170m"
```
</details>

### JSON
<details><summary>Expand</summary>
<p>

```json
{
  "vars": {
    "prefix": "/tmp"
  },
  "tasks": [
    {
      "run": "gafferScene",
      "metadata": {
        "match.types": [
          "png"
        ],
        "match.vars": {
          "imageType": [
            "sequence"
          ]
        }
      },
      "options": {
        "scene": "{configDirectory}/scene.gfr"
      },
      "target": "{prefix}/gafferBlurImageSequence/(newver <parent> as <ver>)/{name}_<ver>.(pad {frame} 6).exr",
      "tasks": [
        {
          "run": "ffmpeg",
          "options":{
            "frameRate": 23.976,
            "sourceColorSpace": "bt709",
            "targetColorSpace": "smpte170m"
          },
          "target": "(dirname {filePath})/{name}.mov"
        }
      ]
    }
  ]
}
```
</details>

### GAFFER (node-based)
```
coming soon
```

### Supported platforms
- LINUX
- windows

### Requirements
- CMAKE 2.8+
- PYTHON 3.5+/2.7+
- OPEN IMAGE OI 1.7+

### Optional
- OPEN COLOR IO
- GAFFER
- FFMPEG
- PYSIDE2
- nuke
- maya

## Installing
```bash
cd <SRC_LOCATION>
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=<TARGET_LOCATION> -G "Unix Makefiles" ..
make all install
```

## Running 
The launchers are provided inside of the "bin" directory found inside of the installation.

Kombi command-line:
```bash
kombi --help
```

Kombi graphical user interface (requires Qt5/PySide2):
```bash
kombi-gui
```

## Licensing
Kombi is free software; you can redistribute it and/or modify it under the terms of the MIT License
