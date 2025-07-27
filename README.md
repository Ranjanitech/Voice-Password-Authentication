# Voice-Password-Authentication
import speech_recognition as sr
import os

# Function to record and save the voice password
def record_voice(filename="password.wav"):
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Speak your password clearly...")
        r.adjust_for_ambient_noise(source)
        audio = r.listen(source)

        with open(filename, "wb") as f:
            f.write(audio.get_wav_data())

        print("Password saved to", filename)

# Function to recognize text from the stored voice password
def recognize_from_file(filename="password.wav"):
    r = sr.Recognizer()
    with sr.AudioFile(filename) as source:
        audio = r.record(source)
    return r.recognize_google(audio)

# Function to authenticate user's voice
def authenticate():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Speak to authenticate...")
        r.adjust_for_ambient_noise(source)
        audio = r.listen(source)

    try:
        stored_text = recognize_from_file()
        input_text = r.recognize_google(audio)

        print("Stored Password:", stored_text)
        print("You Said:", input_text)

        if stored_text.lower() == input_text.lower():
            print("✅ Access Granted")
        else:
            print("❌ Access Denied")

    except sr.UnknownValueError:
        print("Could not understand audio.")
    except sr.RequestError:
        print("API unavailable or network error.")

# Main program
if __name__ == "__main__":
    print("1. Record New Password")
    print("2. Authenticate")
    choice = input("Enter your choice (1/2): ")

    if choice == "1":
        record_voice()
    elif choice == "2":
        authenticate()
    else:
        print("Invalid choice.")
