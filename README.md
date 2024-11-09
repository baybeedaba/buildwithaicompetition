# buildwithaicompetition
A simple code for a chatbot that has google gemini in it that will serve as a go to place for menopause women to help answer their question that will provide answers to help relieve the pain their suffering
from langchain.llms import HuggingFaceHub
from langchain.chat_models import ChatOpenAI
from langchain.chains import RetrievalQA
from langchain.embeddings import HuggingFaceEmbeddings

# Replace with your Hugging Face Hub API token
hf_token = "YOUR_HUGGING_FACE_HUB_API_TOKEN"

# Load the Gemini model
llm = HuggingFaceHub(repo_id="google/flan-t5-xxl", token=hf_token)

# Create a RetrievalQA chain
embeddings = HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")
# Assuming you have a knowledge base (e.g., a collection of articles or a database)
# You would load it here and create a DocumentStore. For simplicity, we'll use a list of strings.
docs = [
    "Menopause is a natural phase in a woman's life...",
    "Hot flashes are a common symptom of menopause...",
    # ... more information on menopause
]
docsearch = VectorStore.from_texts(docs, embeddings)
qa = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=docsearch.as_retriever()
)

def chatbot(user_input):
    response = qa.run(user_input)
    return response

# Example usage:
while True:
    user_query = input("Enter your question about menopause: ")
    if user_query.lower() == "quit":
        break
    response = chatbot(user_query)
    print(response)
