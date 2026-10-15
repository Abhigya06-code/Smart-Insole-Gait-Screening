# Smart Insole Gait Screening

Low-cost smart insole with 6 force-sensitive resistors (FSRs) and an ESP32. It records gait at 100 Hz over Bluetooth, analyses it on a web dashboard, and gives a Parkinson-like pattern indicator from a small 1-D CNN.

> Course prototype. Not a medical device. The AI score is only an indicator, not a diagnosis.

## How it works
1. **Firmware** (`firmware/`): an ESP32 samples the six pads at 100 Hz and streams them over Bluetooth.
2. **Recorder** (`recorder/`): `record.py` saves a CSV (`n, t_ms, L_toe, L_mid, L_heel, R_toe, R_mid, R_heel`).
3. **Dashboard** (`dashboard/`): gait metrics (cadence, step and stride variability, asymmetry, step count), a foot pressure map, playback, and the AI indicator.
4. **Model** (`model/`): a CNN with 37,090 parameters trained on the public PhysioNet gaitpdb data. Subject-wise 5-fold macro F1 is 0.759, and 0.706 when a whole study is held out. These are PhysioNet numbers, not accuracy on this insole.

## Run the dashboard
    cd dashboard
    pip install -r requirements.txt
    uvicorn app:app --reload

Open http://127.0.0.1:8000, add a patient and import a recording CSV.

## Record
    python recorder/record.py

## Limitations
- The model was trained on a different sensor and on walking, while our tests are marching in place.
- Worn or clipped pads change the signal shape and can raise the score. Clipped heel pads are repaired before scoring.
- Not validated on patients. No clinical accuracy is claimed.
- Recordings are not included in this repository.

## Layout
    firmware/   ESP32 code
    hardware/   pin map, schematic, photos
    recorder/   recording and sensor-check scripts
    dashboard/  web app
    model/      weights, training and export code
    docs/       project brief
