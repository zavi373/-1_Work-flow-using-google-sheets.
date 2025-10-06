****AI Agent**
that receives chat messages
Utilizes a Google Gemini language model for its intelligence
Access to a short-term memory
An OpenWeatherMap tool
A Google Calendar tool.
 <img width="1062" height="366" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/a86e6736-940e-4a3e-96c3-398bd0559182" />


Let's break down this n8n workflow step by step, along with detailed documentation.

**n8n Workflow Documentation:**
 AI Assistant with Chat, Weather, and Calendar Capabilities

This n8n workflow sets up an AI Assistant using Google Gemini that can respond to chat messages, provide weather information, and create events in Google Calendar.

**Workflow Overview:**

The core of this workflow is an AI Agent that receives chat messages, utilizes a Google Gemini language model for its intelligence, and has access to a short-term memory, an OpenWeatherMap tool, and a Google Calendar tool.

1. When chat message received (Trigger Node)

Node Type:chatTrigger

Purpose: This node acts as the entry point for the workflow. It listens for incoming chat messages (likely from a connected chat service like a custom chatbot frontend, Telegram, Slack, etc., depending on how it's deployed) and triggers the AI Agent to process the message.

Configuration:

Webhook ID: [ ] (This unique ID is used for the webhook URL that receives chat messages).

Input: Receives a chat message from an external source.

Output: Passes the received chat message content to the connected AI Agent node.

**2. AI Agent (Core AI Logic Node)**

Node Type: agent

Purpose: This is the central intelligence of the workflow. It leverages a Large Language Model (LLM) and a set of tools to understand user requests and formulate responses or perform actions.

Configuration:

It's connected to:

**Google Gemini Chat Model (as its language model)

Simple Memory (for retaining conversation context)

OpenWeatherMap (as a tool to get weather data)

Create an event in Google Calendar (as a tool to manage calendar events)**

Input: Receives the chat message from the "When chat message received" node. Also receives context from "Simple Memory" and has access to the capabilities of its connected tools.

Output: Based on the input and its access to tools, it decides on an action. This could be:

Responding directly to the user (e.g., "Hello, how can I help you?").

**Calling the OpenWeatherMap tool to get weather information.**

Calling the Google Calendar tool to create an event.

Responding with information retrieved from a tool.

How it works: The AI Agent uses the LLM (Google Gemini) to reason about the user's intent. If the intent aligns with one of its available tools (e.g., "What's the weather in London?" or "Schedule a meeting tomorrow at 3 PM"), it will call that tool with the necessary parameters. If not, it will respond directly using its general knowledge.

**3. Google Gemini Chat Model (Language Model Node)**

Node Type: ChatGoogleGemini

Purpose: Provides the underlying intelligence for the "AI Agent" node. This node connects to the Google Gemini API to process natural language, understand user intent, and generate human-like text responses.

Configuration:

Credentials: Google Gemini( ) Api account ( ). This credential stores your API key for accessing the Google Gemini service.

**4. Simple Memory (Memory Node)**

Node Type: .memoryBufferWindow

Purpose: Allows the AI Agent to remember previous parts of the conversation, providing continuity and context. This is crucial for natural, multi-turn interactions.

Configuration:

Context Window Length: 10 (This means the memory will store the last 10 exchanges (input/output pairs) to provide context to the AI Agent. 
**5. OpenWeatherMap (Tool Node)**

Node Type: openWeatherMapTool

Purpose: Equips the AI Agent with the ability to fetch real-time weather information from the OpenWeatherMap API.

Configuration:

City Name: lahore (This is set as a default or example. The AI Agent can dynamically override this based on user input, e.g., "What's the weather in New York?").

Language: en (English for weather descriptions).

Credentials: OpenWeatherMap  ( ). This credential holds your API key for the OpenWeatherMap service.

**6. Create an event in Google Calendar (Tool Node)**

Node Type: googleCalendarTool

Purpose: Grants the AI Agent the functionality to create new events in a Google Calendar.

Configuration:

Description Type: manual

Tool Description: "Create an event in Google Calendar" (This is how the AI Agent understands what the tool does).

Calendar: (The specific Google Calendar where events will be created).

End: ={{ $now.plus(7, 'hour') }} (Default end time for events, 7 hours after the current time. The AI Agent can dynamically set start/end times based on user input).

Use Default Reminders: ={{ true }} (Uses the default reminders set for the calendar).

Credentials: Google Calendar  (). This credential holds your OAuth2 authentication for Google Calendar.

Input: The AI Agent will call this tool, dynamically providing event details like summary, start time, end time, and description based on the user's request.
