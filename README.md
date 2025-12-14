# YakaLLM 
A smaller, faster and cooler version of LangChain. the repo without `.git` is only 74k. Its like preact for react, but nothing is backward compatible.


## how to install
pip:
```
pip install yaka-llm
# or 
pip3 install yaka-llm
```
uv:
```
uv add yaka-llm
# or 
uv pip install yaka-llm
```

## Usage 
```
from typing import List
from yaka_llm.open_ai import ChatGPTModel
# from yaka_llm import GeminiModel 
# ( warn : this example code is made with ai!)

gm = ChatGPTModel(
    model="openai/gpt-oss-20b:free",
    url=".../v1/chat/completions",
    api_key="",
)


# gm = GeminiModel("gemini-2.5-flash", "")
def run_chat_system():

    # 2. Register tools 
    @gm.tool
    def add_numbers(a: float, b: float):
        """Add two numbers a and b. Use this for all arithmetic addition requests."""
        return {"result": a + b}

    print("🤖 LLM System Initialized. Type 'quit' or 'exit' to stop.")
    print("-" * 30)

    # 3. Initialize History
    history: List[str] = []

    # 4. Enter the Repeating Loop
    while True:
        try:
            # Read user input
            user_prompt = input("You: ")

            # Check for exit commands
            if user_prompt.lower() in ("quit", "exit"):
                print("👋 Goodbye!")
                break

            if not user_prompt.strip():
                continue

            # Call the model
            assistant_response = gm.call(
                history=history, prompt=user_prompt, role="user"
            )

            # Print the response and update history
            print(
                f"Gemini: {assistant_response or '... (No response or an error occurred)'}"
            )

            # Update history for the next turn
            # The current `call` method implementation uses the history for context,
            # so we update it with the conversation turn.
            if assistant_response:
                # Add user prompt and assistant response to history
                # Note: The `call` method already formats the history internally,
                # so we just append the raw text of the turn.
                history.append(user_prompt)
                history.append(assistant_response)

            print("-" * 30)

        except KeyboardInterrupt:
            print("\n👋 Goodbye!")
            break
        except Exception as e:
            print(f"An error occurred: {e}")
            break


if __name__ == "__main__":
    run_chat_system()
```
