# Urban Atlas: Two Final Models

Public academic model release by **Vansh Arora, 23B0683**, Civil Engineering,
IIT Bombay. Guide: **Prof. Gopal Patil**.

```text
Urban-Atlas-Final-Models/
  pan_2_5m/
    model_weights.pth
    model_config.json
    README.md
    loader.py / worker.py / runtime/ / requirements.txt
  native_0_6m/
    model_weights.pth
    model_config.json
    README.md
    loader.py / worker.py / runtime/ / requirements.txt
```

| Folder | Input | Frozen final pipeline |
| --- | --- | --- |
| [pan_2_5m](pan_2_5m/) | 2.5 m single-band PAN GeoTIFF | TextureNet teacher/student + SegFormer Roads V9 |
| [native_0_6m](native_0_6m/) | 0.6 m four-band B/G/R/NIR GeoTIFF | Attention U-Net++ + separate CoANet main-road specialist |

Both preserve five class IDs: water, vehicle roads, built-up, barren/empty land,
vegetation. Unknown is 255. TIFF georeferencing is retained; shapefiles are optional.
No source satellite scenes, reviewed labels, private credentials or training data are published.

## Get The Actual Weights

```bash
git lfs install
git clone https://github.com/vansharora665/Urban-Atlas-Final-Models.git
cd Urban-Atlas-Final-Models/native_0_6m
python -m pip install -r requirements.txt
python loader.py --verify --device cpu
```

[Direct weight downloads](https://github.com/vansharora665/Urban-Atlas-Final-Models/releases/tag/v1.0.0)
also work without Git LFS. The loader checks and recovers a missing/pointer weights file.
Git LFS pointer files are references, not trained weights:
[official GitHub explanation](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage).

## Auto-configuration Contract

`display_name`, `architecture` and `overall_accuracy` follow the supplied metadata
example. Extra fields declare resolution, channels, labels, a verified weights URL,
checksum and `loader.py:load_model`. **A host must register the custom loader.**
These pipelines cannot honestly be labelled HRNet or loaded as a plain stock-model
state dictionary. Each folder includes all required matching code.

PAN `overall_accuracy` is null because the reported 92.13% is only partial weak-label
agreement. The four-band 89.73% accuracy / 65.03% mean IoU are Pune development results,
not independent generalisation guarantees. CoANet's 41.25% local development and
48.95% external-pilot road IoU have separate scopes. See the individual model cards.

The bundle preserves original component bytes and hashes. No model was retrained,
no previous candidate was silently promoted, and no accuracy was invented.
Public source/weights are for research review subject to the upstream notices.
