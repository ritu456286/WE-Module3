# assignment 4

**You**

The 1980s saw a shift from Natural Language Processing techniques aiming to codify the grammatical rules of natural language towards techniques aiming to use statistical models to generate text. One early idea which technically isn’t “AI” seeing as it is “memorizing” the training data and yet introduces us to the power contained in statistical techniques of text generation is the idea of Markov chains. Write a python function generate(filename: str, start_words: list[str], chain_length: int, num_generated: int) -> str which takes a filename, a chain length, a list of start words which has to be exactly as long as the chain_length (why?), and an integer num_generated and returns a sentence num_generated words long which sounds similar to the text contained in filename.

**ChatGPT**

Here's a Python function that uses Markov chains to generate text:

```python
pythonCopy code
import random

def generate(filename: str, start_words: list[str], chain_length: int, num_generated: int) -> str:
    # Read the text from the file
    with open(filename, 'r', encoding='utf-8') as file:
        text = file.read()

    # Tokenize the text into words
    words = text.split()

    # Create a dictionary to store transitions
    transitions = {}
    for i in range(len(words) - chain_length):
        key = tuple(words[i:i + chain_length])
        value = words[i + chain_length]
        if key in transitions:
            transitions[key].append(value)
        else:
            transitions[key] = [value]

    # Generate text
    current_words = start_words.copy()
    generated_text = ' '.join(current_words)
    for _ in range(num_generated):
        key = tuple(current_words)
        if key in transitions:
            next_word = random.choice(transitions[key])
        else:
            next_word = random.choice(words)  # If key not found, randomly choose a word
        generated_text += ' ' + next_word
        current_words = current_words[1:] + [next_word]

    return generated_text

# Example usage:
filename = 'your_text_file.txt'
start_words = ['The', 'quick', 'brown']
chain_length = 2  # Adjust this according to desired context length
num_generated = 20
generated_sentence = generate(filename, start_words, chain_length, num_generated)
print(generated_sentence)

```

Replace **`'your_text_file.txt'`** with the path to your text file. Adjust the **`chain_length`** parameter according to how many words you want to consider for each step in the Markov chain. This function generates a sentence of length **`num_generated`** starting with the words provided in **`start_words`**.