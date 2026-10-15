# Data building

Notebooks that download the two public datasets and build the input files used in the experiments.
**No images are included in this repository.** Running the notebooks recreates them locally from the original sources;
please check each dataset's page for its license and terms of use.

| Dataset | Source |
|---|---|
| Tangram abstract shapes (KILOGRAM) | Hugging Face `lil-lab/kilogram`, plus `dataset/dense10.json` from https://github.com/lil-lab/kilogram |
| VERI safe / dangerous image pairs (VERI-Emergency) | Hugging Face `Dasool/VERI-Emergency` |

## Tangram, in order

1. **`tangram_data_download.ipynb`** downloads KILOGRAM, renders the tangram SVGs to PNG (including the images colored by annotation),
   and builds the image index with the shape, color and description columns (`dense10_image_index_with_descriptions.csv` and the intermediate index files).
2. **`tangram_caption_switch.ipynb`** reads `dense10_image_index_with_descriptions.csv`, builds the caption conditions with text-similarity
   measures (scikit-learn TF-IDF and `sentence-transformers/all-MiniLM-L6-v2`), and writes `tangram_captioned.csv`.
3. **`tangram_additional_swaps.ipynb`** edits the swap descriptions in `tangram_believable_swaps.csv`.

## VERI, in order

1. **`veri_image_extractor.ipynb`** downloads VERI-Emergency, saves the images to `VERI_images/`, and writes `VERI_metadata.csv`.
2. **`veri_caption_switcher.ipynb`** reads `VERI_metadata.csv` and builds the caption conditions (switched, random, and most / least similar captions,
   using TF-IDF, Jaccard and `all-MiniLM-L6-v2` similarity), writing `veri_image_captions.csv`.

## Running them

```
pip install datasets huggingface_hub cairosvg pandas numpy scikit-learn sentence-transformers pillow requests tqdm
```

The notebooks read and write relative paths, so run each one from an empty working folder, in the order above.
Their saved outputs were cleared before publishing.
