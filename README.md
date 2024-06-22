# Random_Python_Melody

### Step-by-Step Guide

1. **Set Up the Environment**:
   Ensure you have Python installed. You may also need some additional libraries:
   - `midiutil` to create MIDI files.
   - `random` for generating random elements.

   Install `midiutil` using pip if you haven't already:
   ```bash
   pip install midiutil
   ```

2. **Define Music Theory Basics**:
   Define the notes and chords in different keys.

3. **Create the Melody Generation Logic**:
   Implement the logic to create melodies based on user inputs for meter and key.

4. **Generate and Save the MIDI File**:
   Use `midiutil` to save the generated melody as a MIDI file.

Here’s a complete implementation:

### Python Code

```python
import random
from midiutil import MIDIFile

# Define notes in different keys
keys = {
    'C': ['C', 'D', 'E', 'F', 'G', 'A', 'B'],
    'G': ['G', 'A', 'B', 'C', 'D', 'E', 'F#'],
    'D': ['D', 'E', 'F#', 'G', 'A', 'B', 'C#'],
    'A': ['A', 'B', 'C#', 'D', 'E', 'F#', 'G#'],
    # Add more keys if needed
}

# Define chord progressions (I, V, vi, IV)
chord_progressions = {
    'C': ['C', 'G', 'Am', 'F'],
    'G': ['G', 'D', 'Em', 'C'],
    'D': ['D', 'A', 'Bm', 'G'],
    'A': ['A', 'E', 'F#m', 'D'],
    # Add more progressions if needed
}

# Convert note names to MIDI numbers
note_to_midi = {
    'C': 60, 'C#': 61, 'D': 62, 'D#': 63, 'E': 64, 'F': 65, 'F#': 66,
    'G': 67, 'G#': 68, 'A': 69, 'A#': 70, 'B': 71,
    'Db': 61, 'Eb': 63, 'Gb': 66, 'Ab': 68, 'Bb': 70
}

# Define meters
meters = {
    '4/4': 4,
    '3/4': 3,
    '2/4': 2,
    '6/8': 6
}

# Function to generate a random melody
def generate_melody(meter, key):
    melody = []
    chords = chord_progressions[key]
    key_notes = keys[key]
    beats_per_measure = meters[meter]
    
    for _ in range(4):  # Generate 4 measures
        chord = random.choice(chords)
        root_note = chord[0]
        chord_notes = [root_note, key_notes[(key_notes.index(root_note) + 2) % 7], key_notes[(key_notes.index(root_note) + 4) % 7]]
        
        for _ in range(beats_per_measure):
            note = random.choice(chord_notes)
            melody.append(note)
    
    return melody

# Function to create a MIDI file
def create_midi(melody, filename='melody.mid'):
    midi = MIDIFile(1)  # Create a single track MIDI file
    track = 0
    time = 0
    channel = 0
    volume = 100
    duration = 1  # Each note lasts one beat
    
    midi.addTrackName(track, time, "Track")
    midi.addTempo(track, time, 120)
    
    for note in melody:
        midi_note = note_to_midi[note]
        midi.addNote(track, channel, midi_note, time, duration, volume)
        time += duration
    
    with open(filename, 'wb') as output_file:
        midi.writeFile(output_file)

# Main function
def main():
    meter = input("Choose a meter (4/4, 3/4, 2/4, 6/8): ")
    key = input("Choose a key (C, G, D, A): ")
    
    if meter not in meters:
        print("Invalid meter selected.")
        return
    
    if key not in keys:
        print("Invalid key selected.")
        return
    
    melody = generate_melody(meter, key)
    print("Generated Melody:", melody)
    create_midi(melody)
    print(f"Melody saved to melody.mid")

if __name__ == "__main__":
    main()
```

### Explanation

1. **Notes and Keys**:
   - Define the notes and chords for different keys.
   - Map note names to MIDI numbers for MIDI file creation.

2. **Meters**:
   - Define different meters (4/4, 3/4, 2/4, 6/8).

3. **Generate Melody**:
   - Generate a melody based on the chosen meter and key.
   - Randomly choose chords from the defined progression and notes from those chords.

4. **Create MIDI**:
   - Use `midiutil` to create and save the generated melody as a MIDI file.

### Usage
- Run the script.
- Input the desired meter and key when prompted.
- The generated melody will be saved as a MIDI file named `melody.mid`.

Feel free to expand the program by adding more keys, meters, and more complex melody generation logic.
