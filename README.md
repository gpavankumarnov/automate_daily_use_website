# automate_daily_use_website

#!/opt/homebrew/bin/python3.11

import speech_recognition as sr
import subprocess

# Map of voice commands to shell scripts
COMMAND_MAP = {
    "open browser": "/Users/g.pavankumar/Desktop/auto-browser.sh"
}

def execute_action(command_text):
    command_text = command_text.lower().strip()

    for key_command, script_path in COMMAND_MAP.items():
        if key_command in command_text:
            print(f"Recognized command: '{key_command}'.")
            print(f"Executing script: {script_path}")

            # Execute the .sh file
            try:
                subprocess.run(script_path, shell=True, check=True)
            except subprocess.CalledProcessError as e:
                print(f"Execution failed: {e}")
            except FileNotFoundError:
                print(f"Script not found: {script_path}")

            return True

    print("Command not recognized.")
    return False


def listen_and_act():

    r = sr.Recognizer()

    print("Listening... Speak now.")

    with sr.Microphone() as source:
        r.adjust_for_ambient_noise(source)
        audio = r.listen(source, timeout=5, phrase_time_limit=10)

    try:
        spoken_text = r.recognize_google(audio)
        print(f"You said: {spoken_text}")
        execute_action(spoken_text)

    except Exception as e:
        print(f"Error: {e}")


# Start program
if __name__ == "__main__":
    listen_and_act()
