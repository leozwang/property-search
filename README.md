# Property Search Gemini CLI Extension

This is an MLS extension for the Gemini CLI that allows you to search for real estate properties.

## 🛠️ Installation

1.  **Install the extension from GitHub:**
    ```bash
    gemini extensions install https://github.com/leozwang/property-search
    ```

2.  **Restart your session:**
    Close your current Gemini CLI interactive session and start a new one for the extension to be loaded.

## 📖 How to Use

Once the extension is installed, you can search for properties using natural language. The model will automatically discover the `property_search` tool and use it to answer your questions.

**Example Prompt:**
```text
Can you find me properties with a lot size larger than 1 acre and is for sale?
```

You can also use the custom `/greet` command:

**Example Prompt:**
```text
/greet Gemini
```
