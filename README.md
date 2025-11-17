# PhysiCNet: Physics-Informed Foley Generation

This project is a lightweight, interpretable approach to automating Foley sound generation. Instead of complex "black box" models, PhysiCNet uses a physics-informed pipeline to create dynamically realistic, synchronized sound effects from silent video.

The system analyzes a video, tracks the performer's hand motion using **MediaPipe**, and calculates the **physical deceleration** of the hand at the moment of impact. This kinematic feature is then fed into a trained **Linear Regression model** that predicts the correct audio intensity (loudness).

This project was built for the "Advanced Foundations in Machine Learning" (AFML) course at PES University (November 2025).

## Key Features
* **Physics-Informed:** Predicts sound loudness based on real-world physics (deceleration) rather than just visual semantics.
* **Multi-Tap Detection:** Scans a continuous video and uses `scipy.signal.find_peaks` to detect *all* distinct tap events, not just one.
* **Dynamic Intensity:** Each detected tap is assigned a unique volume based on the model's prediction of the impact force.
* **Automated Sync:** The generated audio is perfectly synchronized with the visual impact events using the timing data from the kinematic analysis.
* **Technologies:** Python, OpenCV, MediaPipe, Librosa, Scikit-learn, MoviePy, Pydub.

## 📉 Proof of Concept
The core of the project is proven by the strong positive correlation found between the visually-derived `max_deceleration` and the ground-truth `peak_rms` (loudness) from the audio. The plot below shows that "soft" taps (low deceleration) cluster in the bottom-left, while "hard" taps (high deceleration) cluster in the top-right.

![PhysiCNet Proof of Concept Plot](https://i.imgur.com/your-plot-image-url.png)
*(You will need to upload your plot to GitHub or an image host like Imgur and paste the URL here)*

##How It Works
The pipeline is a multi-stage process:

1.  **Video Input:** A silent video file is loaded.
2.  **Hand Tracking:** `MediaPipe Hands` processes the video to find the (x, y) coordinates of the wrist in every frame.
3.  **Kinematic Analysis:** The raw trajectory is converted into a continuous deceleration signal.
4.  **Event Detection:** `scipy.signal.find_peaks` scans this signal to find all "tap" events (peaks) that cross a prominence threshold.
5.  **Intensity Prediction:** For each tap, the peak deceleration value is fed into the pre-trained `physicnet_model.pkl`.
6.  **Audio Synthesis:** The model's prediction (RMS) is mapped to a dB gain. A `base_hit.wav` file is loaded, its volume is set, and it's scheduled to play at the exact time of the tap.
7.  **Final Export:** `MoviePy` merges the silent video with all the newly generated sound clips into a final `.mp4` file.

## How to Run
This project was built in Google Colab. The `AFML_FINAL.ipynb` notebook contains the complete, end-to-end code.

1.  **Setup:** Upload the notebook to Google Colab and ensure your Google Drive folder structure matches the paths defined in the notebook (e.g., `AFML_MINIPROJECT/data/curated_dataset/`).
2.  **Install:** Run the first code cell to install all required libraries (NumPy, Librosa, MediaPipe, etc.). **Remember to restart the runtime** after installation.
3.  **Run Pipeline:** Run the subsequent cells in order to:
    * Load and define all helper functions.
    * Load the pre-trained model.
    * Generate the final multi-tap demo video.
