PhysiCNet: Physics-Informed Foley Generation
This project is a lightweight, interpretable approach to automating Foley sound generation. Instead of complex "black box" models, PhysiCNet uses a physics-informed pipeline to create dynamically realistic, synchronized sound effects from silent video.

The system analyzes a video, tracks the performer's hand motion using MediaPipe, and calculates the physical deceleration of the hand at the moment of impact. This kinematic feature is then fed into a trained Linear Regression model that predicts the correct audio intensity (loudness).

This project was built for the "Advanced Foundations in Machine Learning" (AFML) course.

Key Features
Physics-Informed: Predicts sound loudness based on real-world physics (deceleration) rather than just visual semantics.

Multi-Tap Detection: Scans a continuous video and uses scipy.signal.find_peaks to detect all distinct tap events, not just one.

Dynamic Intensity: Each detected tap is assigned a unique volume based on the model's prediction of the impact force.

Automated Sync: The generated audio is perfectly synchronized with the visual impact events using the timing data from the kinematic analysis.

Technologies: Python, OpenCV, MediaPipe, Librosa, Scikit-learn, MoviePy, Pydub.

Proof of Concept
The core of the project is proven by the strong positive correlation found between the visually-derived max_deceleration and the ground-truth peak_rms (loudness) from the audio.
