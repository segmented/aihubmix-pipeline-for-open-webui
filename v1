from typing import List, Union, Generator, Iterator
from pydantic import BaseModel
import os
import requests


class Pipeline:
    class Valves(BaseModel):
        AIHUBMAX_API_KEY: str = ""
        pass

    def __init__(self):
        self.name = "AiHubMix"
        self.valves = self.Valves(
            **{
                "AIHUBMAX_API_KEY": os.getenv(
                    "AIHUBMAX_API_KEY", "your-aihubmix-api-key-here, you can get it at https://aihubmix.com/token."
                )
            }
        )
        pass

    async def on_startup(self):
        # This function is called when the server is started.
        print(f"on_startup:{__name__}")
        pass

    async def on_shutdown(self):
        # This function is called when the server is stopped.
        print(f"on_shutdown:{__name__}")
        pass

    def pipe(
        self, user_message: str, model_id: str, messages: List[dict], body: dict
    ) -> Union[str, Generator, Iterator]:
        # This is where you can add your custom pipelines like RAG.
        print(f"pipe:{__name__}")

        print(messages)
        print(user_message)

        OPENAI_API_KEY = self.valves.AIHUBMAX_API_KEY
        MODEL = "gpt-4o"

        headers = {}
        headers["Authorization"] = f"Bearer {AIHUBMAX_API_KEY}"
        headers["Content-Type"] = "application/json"

        payload = {**body, "model": MODEL}

        if "user" in payload:
            del payload["user"]
        if "chat_id" in payload:
            del payload["chat_id"]
        if "title" in payload:
            del payload["title"]

        print(payload)

        try:
            r = requests.post(
                # AiHubMix DOC: https://doc.aihubmix.com
                url="https://aihubmix.com/v1/chat/completions",
                json=payload,
                headers=headers,
                stream=True,
            )

            r.raise_for_status()

            if body["stream"]:
                return r.iter_lines()
            else:
                return r.json()
        except Exception as e:
            return f"Error: {e}"
