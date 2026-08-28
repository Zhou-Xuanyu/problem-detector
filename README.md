# problem-detector

An object detection model that finds every question on a page of an exam paper.

Given a scanned or exported page from a past paper, the model draws a box around
each question and each sub-question. The point is to skip the tedious part of
building topic-based worksheets: instead of cropping questions out of a PDF by
hand, you detect them and cut automatically.

Two classes are detected:

| class | meaning |
| ----- | ------- |
| `m_quesiton` | a main, top-level question |
| `s_question` | a sub-question nested inside a main question |

## Data

The dataset is built from CAIE A-Level physics past papers and homework sets.

- `OD-yolov8/data/pre_pdf/` — 18 source PDFs the pages come from
- `OD-yolov8/data/images/train/` — 241 page images rendered from those PDFs
- `OD-yolov8/data/labels/` — matching YOLO-format bounding box annotations

Paths and class names are declared in `OD-yolov8/config.yaml`.

## Training

The model is a YOLOv8-nano trained from scratch with
[Ultralytics](https://github.com/ultralytics/ultralytics), 100 epochs at batch
size 32 on Apple Silicon (`device="mps"`).

```sh
pip install ultralytics
cd OD-yolov8
python new_trainv8.py      # train from a fresh yolov8n
python resume_trainv8.py   # resume from the last checkpoint
python yolov8_test.py      # run the trained weights on an image
```

> [!NOTE]
> `resume_trainv8.py` and `yolov8_test.py` point at absolute checkpoint and
> image paths from the machine they were written on. Edit the paths at the top
> of each script before running them.

Runs are written to `runs/detect/` by Ultralytics and are not tracked in this
repository, so the trained weights are not included.
