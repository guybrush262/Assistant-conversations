# Assistant Conversations


# Introduction
The project aim to save conversations with Assist so that future conversations can be contextualized:
1.	Saving conversations held with Assist to a database
2.	Recalling past conversations by the Conversation Agent reading the database via indication in the integration service prompt

Requirements:
- File integration
- Conversation agent integration (e.g. OpenAi Conversation)
- Raspberry Pi hardware (I use Raspberry Pi 4)


# Tutorial
*Preliminary steps*
1) Add to configuration.yaml:
  homeassistant:
    allowlist_external_dirs:
      - /config/www

2) Use the File integration (https://www.home-assistant.io/integrations/file/) to write the responses to a file with path (e.g. "/config/log/assistant_responses.log") in order to store all responses permanently.

3) Create a Long-lived access tokens.

*Configure the Rapsberry Pi*
4) Install Raspberry Pi OS Lite 64-bit (Bookworm)

5) Update packages:
sudo apt update && sudo apt upgrade -y

6) Install Python 3 and pip:
sudo apt install python3 python3-pip -y

7) Create a virtual environment:
sudo apt install python3-venv -y

and

python3 -m venv flask_env
source flask_env/bin/activate

8) Install Flask and Request:
pip install flask

and

pip install requests

9) Create a Python script in "/home/plinio1/log_service.py" (my folder is called plinio1):
Move there:
cd /home/plinio1

and

nano log_service.py



*Finalize the Home Assistant configuration*
3) Create the following script:
   sequence:
  - metadata: {}
    data:
      message: >-
        | {{ now().strftime('%Y-%m-%d') }} | {{ now().strftime('%H:%M:%S') }} |
        {{message}}
    target:
      entity_id: notify.file_2
    action: notify.send_message
alias: AI_Add conversations to log
mode: single
fields:
  message:
    selector:
      text: null
    name: message
    description: >-
      The given message will be added in the log file and it will have date and
      time as prefix
description: ""

4) Create the following automation:
alias: AI-Saving Assist conversations
description: ""
triggers:
  - trigger: conversation
    command: "{question}"
conditions: []
actions:
  - action: conversation.process
    metadata: {}
    data:
      agent_id: conversation.openai_jarvis
      text: "{{ trigger.slots.question }}"
    response_variable: chatgpt
  - set_conversation_response: "{{ chatgpt.response.speech.plain.speech }}"
  - action: script.ai_add_conversations_to_log
    metadata: {}
    data:
      message: >-
        User: '{{ trigger.slots.question }}' | Assistant: '{{
        chatgpt.response.speech.plain.speech }}'
mode: single
