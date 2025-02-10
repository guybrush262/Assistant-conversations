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


*Preliminary steps and saving Assist conversations*

1) Use the File integration (https://www.home-assistant.io/integrations/file/) to write the responses to a file with path (e.g. "/config/log/assistant_responses.log") in order to store all responses permanently
 
2) Add the Home Assistant script

3) Add the Home Assistant automation

4) Add the following to configuration.yaml:

homeassistant:
  allowlist_external_dirs:
    - /config/www
        
6) Create a Long-lived access token in Home Assistant

 
*Configure the Rapsberry Pi*
   
6) Install Raspberry Pi OS Lite 64-bit (Bookworm) on a SD card

7) Update packages:
   
sudo apt update && sudo apt upgrade -y

9) Install Python 3 and pip:
    
sudo apt install python3 python3-pip -y

10) Create a virtual environment:
    
sudo apt install python3-venv -y

python3 -m venv flask_env

source flask_env/bin/activate

11) Install Flask and Request:
    
pip install flask

pip install requests

12) Create the Python script in "/home/jarvis/log_service.py" (my folder is called "jarvis") remembering to inpu the Long-lived access token:

Move there:

cd /home/jarvis

and

nano log_service.py

13) Execute the script:
    
cd /home/jarvis/

python3 log_service.py

14) Automate the script execution:
    
sudo nano /etc/systemd/system/log_service.service

and inside it input the Service code.

15) Activate and start the service:
    
sudo systemctl daemon-reload 

sudo systemctl enable log_service

sudo systemctl start log_service

*Finalize the Home Assistant configuration*

16) In configuration.yaml add the Home Assistant sensor.

17) In OpenAI Conversation (or whatever conversation agetn you chose) add the Prompt to the instructions/prompt template.


