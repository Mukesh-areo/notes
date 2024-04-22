import docx
import spacy
from collections import defaultdict

# Load spaCy model with Word2Vec
nlp = spacy.load("en_core_web_lg")

# Function to preprocess and get word vectors
def preprocess_and_get_vectors(text):
    doc = nlp(text)
    tokens = [token for token in doc if not token.is_stop and token.is_alpha]
    return [token.vector for token in tokens]

# Read the Word document
doc = docx.Document("your_document.docx")

# Get the first paragraph as reference
reference_paragraph = doc.paragraphs[0].text
reference_vectors = preprocess_and_get_vectors(reference_paragraph)

# Initialize a dictionary to store similar words for each paragraph
similar_words_dict = defaultdict(set)

# Iterate through subsequent paragraphs
for paragraph in doc.paragraphs[1:]:
    vectors = preprocess_and_get_vectors(paragraph.text)
    for vector in vectors:
        for ref_vector in reference_vectors:
            similarity = vector.similarity(ref_vector)
            if similarity > 0.8:  # Adjust the threshold as needed
                similar_words_dict[paragraph.text].add(vector.text)

# Print similar words for each paragraph
for paragraph, similar_words in similar_words_dict.items():
    print(f"Paragraph: {paragraph}")
    print(f"Similar Words: {similar_words}")






    _______________&&&&&&&&&--------
