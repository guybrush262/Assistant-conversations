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

3) Create a Long-lived access token.

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

9) Create the Python script in "/home/plinio1/log_service.py" (my folder is called plinio1) remembering to inpu the Long-lived access token:
Move there:
cd /home/plinio1

and

nano log_service.py

9) Execute the script:
cd /home/plinio1/
python3 log_service.py

10) Automate the script execution:
sudo nano /etc/systemd/system/log_service.service

and inside it input the following code:

[Unit]
Description=Flask Log Service
After=network.target

[Service]
ExecStart=/home/plinio1/flask_env/bin/python3 /home/plinio1/log_service.py
WorkingDirectory=/home/plinio1
StandardOutput=inherit
StandardError=inherit
Restart=always
User=plinio1

[Install]
WantedBy=multi-user.target

11) Activate and start the service:
sudo systemctl daemon-reload 
sudo systemctl enable log_service
sudo systemctl start log_service


*Finalize the Home Assistant configuration*
12) Add the Home Assistant script.

13) Add the Home Assistant automation.

14) In configuration.yaml add the Home Assistant sensor.
