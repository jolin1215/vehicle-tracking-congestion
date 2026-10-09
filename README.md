# Vehicle Tracking, Counting and Congestion Estimation

Detects vehicles in surveillance/traffic video with **YOLOv8x**, tracks them across
frames with **ByteTrack**, and derives traffic metrics from a fixed camera:

- **Zone counting** — how many vehicles are inside a user-defined polygon, with
  cumulative entry/exit counts.
- **Speed estimation** — per-vehicle speed (km/h) from frame-to-frame displacement,
  using a pixels-per-metre scale.
- **Congestion level** — an A–F grade derived from the average speed inside the zone.

The annotated result is written out as a video.

## Pipeline

1. Load a clip and run YOLOv8x detection, keeping only vehicle classes (car, bus, truck).
2. Associate detections across frames with ByteTrack to obtain stable track IDs.
3. Feed tracked boxes to `AreaCounter` (zone occupancy, entry/exit) and `SpeedEstimator`
   (per-track speed).
4. Overlay boxes, IDs, speeds and the congestion grade, then export the video.

## Running

Open `vehicle_tracking_congestion.ipynb` in Google Colab with a GPU runtime and run
the cells top to bottom. Set `SOURCE_VIDEO_PATH` to your input clip and adjust
`AREA_BOUNDARY` (the monitored polygon) and `pixel_per_meter` to your camera geometry.

The notebook pins specific versions (`supervision==0.1.0`, `ultralytics<=8.3.40`,
`numpy<1.24`) because it relies on the older `supervision` tracking API that ByteTrack
integrates with.

## Stack

Python, YOLOv8 (Ultralytics), ByteTrack, supervision, OpenCV, NumPy.

## Notes

Part of a final-year project on video analytics for surveillance footage. The sample
paths point at Google Drive locations used during development; repoint them at your own
clips. Model weights (`yolov8x.pt`) download automatically on first run.
