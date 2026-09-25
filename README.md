from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI(title="Nova AI")


class ChatRequest(BaseModel):
    message: str
    language: str = "en"


@app.get("/")
def home():
    return {"message": "Nova AI is running!"}


@app.post("/chat")
def chat(request: ChatRequest):
    message = request.message.lower().strip()

    # Desktop commands
    if message in ["open instagram", "instagram kholo", "instagram öffnen"]:
        return {
            "type": "command",
            "command": "open_instagram"
        }

    # Demo AI response
    responses = {
        "en": f"You said: {request.message}",
        "hi": f"आपने कहा: {request.message}",
        "pt": f"Você disse: {request.message}",
        "de": f"Du hast gesagt: {request.message}"
    }

    return {
        "type": "text",
        "reply": responses.get(request.language, responses["en"])
    }


@app.post("/chat")
def chat(request: ChatRequest):
