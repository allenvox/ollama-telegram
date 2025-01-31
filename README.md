## Installation (Non-Docker)
+ Install latest [Python](https://python.org/downloads)
+ Clone Repository
    ```
    git clone https://github.com/ruecat/ollama-telegram
    ```
+ Install requirements from requirements.txt
    ```
    pip install -r requirements.txt
    ```
+ Enter all values in .env.example

+ Rename .env.example -> .env

+ Launch bot

    ```
    python3 run.py
    ```
## Installation (Docker Image)
The official image is available at dockerhub: [ruecat/ollama-telegram](https://hub.docker.com/r/ruecat/ollama-telegram)

+ Download [.env.example](https://github.com/ruecat/ollama-telegram/blob/main/.env.example) file, rename it to .env and populate the variables.
+ Create `docker-compose.yml` (optionally: uncomment GPU part of the file to enable Nvidia GPU)

    ```yml
    version: '3.8'
    services:
      ollama-telegram:
        image: ruecat/ollama-telegram
        container_name: ollama-telegram
        restart: on-failure
        env_file:
          - ./.env
      
      ollama-server:
        image: ollama/ollama:latest
        container_name: ollama-server
        volumes:
          - ./ollama:/root/.ollama
        
        # Uncomment to enable NVIDIA GPU
        # Otherwise runs on CPU only:
    
        # deploy:
        #   resources:
        #     reservations:
        #       devices:
        #         - driver: nvidia
        #           count: all
        #           capabilities: [gpu]
    
        restart: always
        ports:
          - '11434:11434'
    ```

+ Start the containers

    ```sh
    docker compose up -d
    ```


## Installation (Build your own Docker image)
+ Clone Repository
    ```
    git clone https://github.com/ruecat/ollama-telegram
    ```

+ Enter all values in .env.example

+ Rename .env.example -> .env

+ Run ONE of the following docker compose commands to start:
    1. To run ollama in docker container (optionally: uncomment GPU part of docker-compose.yml file to enable Nvidia GPU)
        ```
        docker compose up --build -d
        ```

    2. To run ollama from locally installed instance (mainly for **MacOS**, since docker image doesn't support Apple GPU acceleration yet):
        ```
        docker compose up --build -d ollama-tg
        ```

## Environment Configuration
|          Parameter          |                                                      Description                                                      | Required? | Default Value |                        Example                        |
|:---------------------------:|:---------------------------------------------------------------------------------------------------------------------:|:---------:|:-------------:|:-----------------------------------------------------:|
|           `TOKEN`           | Your **Telegram bot token**.<br/>[[How to get token?]](https://core.telegram.org/bots/tutorial#obtain-your-bot-token) |    Yes    |  `yourtoken`  |             MTA0M****.GY5L5F.****g*****5k             |
|         `ADMIN_IDS`         |                     Telegram user IDs of admins.<br/>These can change model and control the bot.                      |    Yes    |               | 1234567890<br/>**OR**<br/>1234567890,0987654321, etc. |
|         `USER_IDS`          |                       Telegram user IDs of regular users.<br/>These only can chat with the bot.                       |    Yes    |               | 1234567890<br/>**OR**<br/>1234567890,0987654321, etc. |
|         `INITMODEL`         |                                                      Default LLM                                                      |    No     |   `llama2`    |        mistral:latest<br/>mistral:7b-instruct         |
|      `OLLAMA_BASE_URL`      |                                                  Your OllamaAPI URL                                                   |    No     |               |          localhost<br/>host.docker.internal           |
|        `OLLAMA_PORT`        |                                                  Your OllamaAPI port                                                  |    No     |     11434     |                                                       |
|            `TIMEOUT`        |                                    The timeout in seconds for generating responses                                    |    No     |     3000      |                                                       |
| `ALLOW_ALL_USERS_IN_GROUPS` |                Allows all users in group chats interact with bot without adding them to USER_IDS list                 |    No     |       0       |                                                       |
