

# viEMO: Advancing Emotion Recognition in Vietnamese Language through Multimodel Learning

## Overview
viEMO is a project focused on developing an emotion recognition system for Vietnamese using multimodal learning techniques. The project leverages data from text, audio, and images to improve the accuracy and reliability of emotion recognition.

## Dataset Construction Process

### 1. Text Data
- **Source**: YouTube video transcripts.
- **Method**: Automatically collect character utterances from transcripts, extracting the start time of each dialogue segment. Then, manually annotate the end time for each segment.

### 2. Audio Data
- **Source**: Audio from YouTube videos.
- **Method**: Use the start and end times of each dialogue segment to extract the corresponding audio clip.
- **Quality Control**: Manually review each audio segment, removing those with multiple speakers or excessive noise.

### 3. Image Data
- **Source**: Frames from YouTube videos.
- **Method**: Manually capture the speaker's face in each dialogue segment and save to separate folders.

## Folder Structure
- `Dataset/`
  - `Audio/`: Contains extracted audio segments, organized by video or character.
  - `File csv/`: Contains CSV files with metadata, text, timestamps, emotion labels, etc.
  - `Image/`: Contains folders of face images, organized by video or character.
- `Face_Emotion_Recognition-master/`: Source code and resources for face emotion recognition.
- `Speech_Emotion_Detection-master/`: Source code and resources for speech emotion recognition.
- `Text_Emotion_Recognition-master/`: Source code and resources for text emotion recognition.
- `Fusion_Method/`: Notebooks for multimodal fusion methods.

## Modalities and Data Sources
Each emotion recognition modality uses different data sources and models:

- **Face Emotion Recognition**
  - Uses the `emotiondetector.h5` model to classify emotions from face images.
  - Input data: Face images in `Dataset/Image/`.
  - Source code: `Face_Emotion_Recognition-master/`.

- **Speech Emotion Recognition**
  - Uses the TESS Toronto dataset for training and evaluation.
  - Input data: Audio segments in `Dataset/Audio/`.
  - Source code: `Speech_Emotion_Detection-master/`.

- **Text Emotion Recognition**
  - Uses the UIT-VSMEC dataset for Vietnamese text.
  - Input data: Dialogue sentences and metadata in `Dataset/File csv/`.
  - Source code: `Text_Emotion_Recognition-master/`.


## Multimodal Fusion
The results from the three modalities (face, speech, text) are combined using algorithms in the `Fusion_Method/` folder, specifically:

- **Product Fusion (Probability Multiplication):**
  - The predicted emotion probabilities from each modality (face, speech, text) are multiplied together for each emotion class, creating a fused probability vector.
  - The final emotion label is the one with the highest fused probability.
  - This method leverages agreement across modalities for more reliable predictions.

- **Weighted Average Fusion:**
  - The predicted probabilities from each modality are combined using the formula:
    - `fused_probs = audio_probs * 0.5 + image_probs * 0.35 + text_probs * 0.15`
  - The weights can be adjusted, for example, to prioritize audio over other sources.
  - The final result is the emotion label with the highest combined probability.

Both methods are common ensemble/fusion techniques that improve accuracy compared to using a single modality. These algorithms are implemented in Python notebooks in the `Fusion_Method` folder.

## Project Goals
- Develop robust Vietnamese emotion recognition models based on text, audio, and image data.
- Research and apply multimodal fusion techniques to improve recognition performance.
- Provide a high-quality, manually verified dataset for the research community.

## Notes
- Data was collected from publicly available YouTube videos.
- Emotion labels and quality assurance were manually performed. Label consistency was assessed using Cohen's Kappa to ensure annotation agreement.

