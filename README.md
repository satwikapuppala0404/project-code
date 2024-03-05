# project-code
import os
import random
from pydub import AudioSegment
import filetype
from flask import Flask, request, jsonify, render_template
from genre_rec_service import Genre_Recognition_Service
app = Flask(__name__)
@app.route("/predict", methods=["POST"])
def predict():
# get audio file
audio_file = request.files["UploadedAudio"]
kind = filetype.guess(audio_file)
x=kind.extension
if(x=='mp3'):
music_file= "C:/Users/91939/Desktop/music_clf-master/converted.wav"
sound = AudioSegment.from_mp3(audio_file)
sound.export(music_file,format="wav")
#invoke the genre recognition service
grs = Genre_Recognition_Service()

43

# make prediction
prediction = grs.predict(music_file)
os.remove(music_file)
# message to be displayed on the html webpage
prediction_message = f"""
The Genre of song is {prediction}.
"""
return render_template("index.html", prediction_text=prediction_message)
@app.route("/")
def index():
return render_template("index.html")
if __name__ == "__main__":
app.run(debug=False)
