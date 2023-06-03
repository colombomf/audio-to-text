# Audio-to-Text Extractor

This repository contains a Python script that extracts text from an audio file using the MoviePy, SpeechRecognition, and tqdm libraries. The extracted text is saved to a text file for further analysis or processing.

## Installation

To use this script, you need to have Python installed on your system. You also need to install the required libraries by running the following command:

`pip install moviepy speechrecognition tqdm` 

## Usage

1.  Import the necessary libraries:

`import moviepy.editor as mp
 import speech_recognition as sr
 from tqdm import tqdm` 

2.  Define the function `extract_text_from_audio` that takes the file path of the audio file as input and returns the extracted text:

`def extract_text_from_audio(file_path):
    # Load the video file
    clip = mp.VideoFileClip(file_path)
    
    # Extract the audio from the video
    audio = clip.audio
    
    # Save the audio as a temporary file
    audio_path = "temp_audio.wav"
    audio.write_audiofile(audio_path)
    
    # Initialize the recognizer
    r = sr.Recognizer()
    
    # Transcribe each chunk of audio
    chunk_duration = 30  # Chunk duration in seconds
    total_duration = clip.duration
    chunks = int(total_duration / chunk_duration) + 1
    
    text = ""
    
    # Use tqdm to create a loading bar
    with tqdm(total=chunks, desc="Processing", unit="chunk") as pbar:
        for i in range(chunks):
            start_time = i * chunk_duration
            end_time = min((i + 1) * chunk_duration, total_duration)
            
            with sr.AudioFile(audio_path) as source:
                audio = r.record(source, offset=start_time, duration=end_time - start_time)
                chunk_text = r.recognize_google(audio)
                text += chunk_text + " "
            
            pbar.update(1)  # Update the loading bar
            
    return text` 

3.  Specify the file path of the audio file you want to process:

`file_path = "YOUR_AUDIO_FILE_HERE.mp4"` 

4.  Call the `extract_text_from_audio` function with the file path as an argument to extract the text:

`extracted_text = extract_text_from_audio(file_path)` 

5.  Save the extracted text to a file:

`output_file = "extracted_text.txt"
with open(output_file, "w") as file:
    file.write(extracted_text)` 

6.  Run the script and check the console for the path to which the extracted text is saved:

`Text extracted and saved to: extracted_text.txt` 

## Notes

-   The script assumes that the input audio file is in MP4 format. If your file is in a different format, you may need to modify the code accordingly.
-   The script splits the audio into chunks of 30 seconds for transcription. You can adjust the `chunk_duration` variable in the `extract_text_from_audio` function to change the chunk duration.
-   The script uses the Google Web Speech API through the SpeechRecognition library for audio transcription. You may need an internet connection for the transcription to work.
-   Make sure you have read and write permissions in the directory where the script is located, as well as the output file's location.

## License

This project is licensed under the MIT License. Feel free to use and modify the code as per your needs.