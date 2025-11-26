# ML Challenge: Feature Extraction from Images

In this hackathon, the goal is to create a machine learning model that extracts entity values from product images. This capability is crucial in fields like **healthcare**, **e-commerce**, and **content moderation**, where precise product information is vital. As digital marketplaces expand, many products lack detailed textual descriptions, making it essential to obtain key details directly from images. These images provide important information such as weight, volume, voltage, wattage, dimensions, and more—critical for digital stores.

## Data Description

The dataset consists of the following columns:

| Column          | Description                                                                 |
|-----------------|-----------------------------------------------------------------------------|
| **index**       | Unique identifier (ID) for the data sample.                                 |
| **image_link**  | Public URL where the product image is available for download. <br/> *Example:* `https://m.media-amazon.com/images/I/71XfHPR36-L.jpg` <br/> *Tip:* Use the `download_images` function from `src/utils.py` to download images (see sample code in `src/test.ipynb`). |
| **group_id**    | Category code of the product.                                               |
| **entity_name** | Product entity name (e.g., “item_weight”).                                  |
| **entity_value**| Product entity value (e.g., “34 gram”). <br/> *Note:* This column is absent in `test.csv` as it is the target variable. |

## Output Format

The output file should be a CSV with **2 columns**:

| Column       | Description                                                                 |
|--------------|-----------------------------------------------------------------------------|
| **index**    | The unique identifier (ID) of the data sample. Must match the test record index. |
| **prediction** | A string in the format: `“x unit”` where:<br/>- `x` is a float number in standard formatting.<br/>- `unit` is one of the allowed units (see Appendix).<br/>- The two values are concatenated with a single space.<br/> <br/>**Valid examples:**<br/>- “2 gram”<br/>- “12.5 centimetre”<br/>- “2.56 ounce”<br/> <br/>**Invalid examples:**<br/>- “2 gms”<br/>- “60 ounce/1.7 kilogram”<br/>- “2.2e2 kilogram”<br/> <br/>*Notes:* <br/>- Output a prediction for **all** indices.<br/>- If no value is found, return an empty string: `“”`.<br/>- Ensure the exact number of output samples matches `test.csv`; otherwise, it won’t be evaluated. |

## File Descriptions

### Source Files
- **`src/sanity.py`**: Sanity checker to ensure the final output file passes all formatting checks. <br/>*Note:* This script does **not** check if the number of predictions matches the test file. See sample code in `src/test.ipynb`.
- **`src/utils.py`**: Helper functions for downloading images from `image_link`.
- **`src/constants.py`**: Contains the allowed units for each entity type.
- **`sample_code.py`**: Optional sample dummy code to generate an output file in the required format.

### Dataset Files
- **`dataset/train.csv`**: Training file with labels (`entity_value`).
- **`dataset/test.csv`**: Test file without labels (`entity_value`). Generate predictions here and format to match `sample_test_out.csv`.
- **`dataset/sample_test.csv`**: Sample test input file.
- **`dataset/sample_test_out.csv`**: Sample outputs for `sample_test.csv`. <br/>*Note:* Predictions in this file may not be accurate; focus on formatting.

## Constraints
1. Use the provided sample output file and `sanity.py` to validate formatting. Your output must match `sample_test_out.csv` exactly. <br/>*Success message:* `Parsing successful for file: ...csv`. <br/>*Warning:* Files failing sanity checks will **not** be evaluated.
2. Outputs must use **only** allowed units from `constants.py` (also in Appendix). Other units will be invalid.

## Evaluation Criteria

Submissions are evaluated using the **F1 score**, a standard measure for prediction accuracy in classification and extraction tasks.

Let **GT** = Ground truth value and **OUT** = Model prediction for a sample. Predictions are classified as:

| Class                      | Condition                                      |
|----------------------------|------------------------------------------------|
| **True Positives (TP)**    | OUT ≠ `""` and GT ≠ `""` and OUT == GT         |
| **False Positives (FP)**   | OUT ≠ `""` and GT ≠ `""` and OUT ≠ GT          |
| **False Positives (FP)**   | OUT ≠ `""` and GT == `""`                      |
| **False Negatives (FN)**   | OUT == `""` and GT ≠ `""`                      |
| **True Negatives (TN)**    | OUT == `""` and GT == `""`                     |

**F1 Score** = 2 * (Precision * Recall) / (Precision + Recall)

Where:

- **Precision** = TP / (TP + FP)
- **Recall** = TP / (TP + FN)

## Submission File

Upload `test_out.csv` to the portal with **exact** formatting as `sample_test_out.csv`.

## Appendix: Allowed Units

```python
entity_unit_map = {
  "width": {
    "centimetre",
    "foot",
    "millimetre",
    "metre",
    "inch",
    "yard"
  },
  "depth": {
    "centimetre",
    "foot",
    "millimetre",
    "metre",
    "inch",
    "yard"
  },
  "height": {
    "centimetre",
    "foot",
    "millimetre",
    "metre",
    "inch",
    "yard"
  },
  "item_weight": {
    "milligram",
    "kilogram",
    "microgram",
    "gram",
    "ounce",
    "ton",
    "pound"
  },
  "maximum_weight_recommendation": {
    "milligram",
    "kilogram",
    "microgram",
    "gram",
    "ounce",
    "ton",
    "pound"
  },
  "voltage": {
    "millivolt",
    "kilovolt",
    "volt"
  },
  "wattage": {
    "kilowatt",
    "watt"
  },
  "item_volume": {
    "cubic foot",
    "microlitre",
    "cup",
    "fluid ounce",
    "centilitre",
    "imperial gallon",
    "pint",
    "decilitre",
    "litre",
    "millilitre",
    "quart",
    "cubic inch",
    "gallon"
  }
}
```

---

*This README has been reformatted for better readability: added tables, bold/italic emphasis, consistent headings, and a clean code block for the appendix. Equations have been converted to plain text for compatibility across all Markdown viewers (no LaTeX required). You can copy-paste this directly into your GitHub repo's `README.md` file.*
