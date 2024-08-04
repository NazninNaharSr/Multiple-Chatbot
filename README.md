
# Setting Up Multiple Chatbots with GenAI Stack

This document provides a step-by-step guide to set up and run multiple chatbots using the GenAI Stack. 
Follow the instructions below to configure and start the chatbots.

## Step 1: Clone the GenAI Stack Repository
Open your terminal and run the following command to clone the GenAI Stack repository:

```bash
git clone https://github.com/docker/genai-stack.git
cd genai-stack
```

## Step 2: Create .env Files for Each Chatbot
In the project directory, create separate `.env` files for each chatbot instance. 
For example, create `.env.chatbot1` and `.env.chatbot2` with the following content:

### .env.chatbot1
```plaintext
OPENAI_API_KEY=your_api_key_for_chatbot1
NEO4J_URI="neo4j+s://AURADB-INSTANCE:7687"
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=password
NEO4J_DATABASE=neo4j
LLM=gpt-4
EMBEDDING_MODEL=openai
```

### .env.chatbot2
```plaintext
OPENAI_API_KEY=your_api_key_for_chatbot2
NEO4J_URI="neo4j+s://AURADB-INSTANCE:7687"
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=password
NEO4J_DATABASE=neo4j
LLM=gpt-4
EMBEDDING_MODEL=openai
```

## Step 3: Modify docker-compose.yml
To run multiple chatbot instances, you need to modify the `docker-compose.yml` file. 
Create separate services for each chatbot. For example:

```yaml
version: '3.8'

services:
  chatbot1:
    env_file: .env.chatbot1
    build: .
    ports:
      - "8502:8502"
    depends_on:
      - neo4j

  chatbot2:
    env_file: .env.chatbot2
    build: .
    ports:
      - "8503:8502" # Note the port change for the second chatbot
    depends_on:
      - neo4j

  neo4j:
    image: neo4j:latest
    environment:
      - NEO4J_AUTH=neo4j/password
    ports:
      - "7687:7687"
      - "7474:7474"
```

## Step 4: Start Docker Containers
Run the following command to start the containers:

```bash
docker compose up
```

This command will start all containers and you should be able to access the chatbots at:
- Chatbot 1: http://localhost:8502
- Chatbot 2: http://localhost:8503

## Step 5: Install and Start Neo4j
Install Neo4j using Homebrew and start the Neo4j service:

```bash
brew install neo4j
neo4j start
```

## Step 6: Run Sample Queries in Neo4j
Open the Neo4j browser at http://localhost:7474 and execute the following sample queries:

```cypher
CREATE (a:Person {name: 'Alice', age: 30})
CREATE (b:Person {name: 'Bob', age: 25})
CREATE (c:Person {name: 'Charlie', age: 35})

CREATE (a)-[:FRIENDS_WITH]->(b)
CREATE (b)-[:FRIENDS_WITH]->(c)
```

## Step 7: Create a Python Script to Connect to Neo4j
Create a file named `connect_neo4j.py` with the following content:

```python
from neo4j import GraphDatabase

# Set up the Neo4j connection parameters
uri = "bolt://localhost:7687"
username = "neo4j"
password = "your_password"  # Replace with your actual password

# Create a driver instance
driver = GraphDatabase.driver(uri, auth=(username, password))

def query_neo4j():
    with driver.session() as session:
        result = session.run("MATCH (n) RETURN n LIMIT 10")
        for record in result:
            print(record)

if __name__ == "__main__":
    query_neo4j()
```

## Step 8: Run the Python Script
Execute the script to connect to Neo4j and run a sample query:

```bash
python connect_neo4j.py
```
