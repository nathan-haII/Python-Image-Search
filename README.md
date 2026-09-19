# Document Scanner

A Python command-line tool that turns a photo of a document (page, form, etc.) into a clean, black-and-white scanned image.

Given a photo like the one below, the script detects the document's edges, corrects the perspective, and applies an adaptive threshold to produce a crisp, high-contrast scan.

| Original Photo | Scanned Output |
| Angled photo of a receipt on a table | Cropped, perspective-corrected, black-and-white receipt |

## How It Works

The pipeline runs in three steps:

1. **Edge Detection** — The image is resized, converted to grayscale, blurred, and passed through Canny edge detection to highlight boundaries.
2. **Contour Detection** — The script finds contours in the edge map, sorts them by area, and looks for the largest one that approximates a four-sided shape (the document itself).
3. **Perspective Transform & Thresholding** — A four-point perspective transform "flattens" the document into a top-down view, which is then converted to grayscale and run through adaptive (Gaussian) thresholding to produce the classic black-and-white scanned look.

## Project Structure

```
.
├── scan.py               # Main script: runs the full scanning pipeline
├── transform.py          # Perspective transform utilities (order_points, four_point_transform)
└── __init__.py           # Makes the folder importable as a package
```

## Requirements

- Python 3
- [OpenCV](https://pypi.org/project/opencv-python/) (`opencv-python`)
- [NumPy](https://pypi.org/project/numpy/)
- [imutils](https://pypi.org/project/imutils/)
- [scikit-image](https://pypi.org/project/scikit-image/)

Install everything with:

```bash
pip install opencv-python numpy imutils scikit-image
```

> **Note:** `scan.py` imports `four_point_transform` from `pyimagesearch.transform`. Either rename your project folder to `pyimagesearch` (with `__init__.py` and `transform.py` inside it) or update the import in `scan.py` to match your folder name.

## Usage

Run the script and pass in the path to the image you want to scan:

```bash
python scan.py --image path/to/your/photo.jpg
```

The script will open a series of windows showing each step of the process:

1. **Edge Detection** — original resized image + Canny edges (press any key to continue)
2. **Find Contours** — the detected document outline drawn in green (press any key to continue)
3. **Apply Perspective Transform** — original vs. final scanned result (press any key to finish)

The final scanned image is saved to disk as `scanned.jpg` in the current directory.

## Tips for Best Results

- Use a photo with **good contrast** between the document and the background (e.g., a light document on a dark surface, or vice versa).
- Make sure all **four corners** of the document are visible in the frame.
- Avoid cluttered backgrounds — the script assumes the document is the largest four-sided shape in the image.
- If the script raises an error saying it couldn't find a 4-point contour, try retaking the photo with better lighting, a plainer background, or more contrast.

## Credits

This project is based on the classic document scanner pipeline popularized by [PyImageSearch](https://pyimagesearch.com/).
