# Multiple-Chatbot
from docx import Document

# Create a new Document
doc = Document()

# Title
doc.add_heading('Setting Up Multiple Chatbots with GenAI Stack', 0)

# Introduction
doc.add_paragraph(
    "This document provides a step-by-step guide to set up and run multiple chatbots using the GenAI Stack. "
    "Follow the instructions below to configure and start the chatbots."
)

# Step 1: Clone the GenAI Stack Repository
doc.add_heading('1. Clone the GenAI Stack Repository', level=1)
doc.add_paragraph(
    "Open your terminal and run the following command to clone the GenAI Stack repository:"
)
doc.add_paragraph(
    'git clone https://github.com/docker/genai-stack.git\n'
    'cd genai-stack',
    style='Normal'
)

# Step 2: Create .env Files for Each Chatbot
doc.add_heading('2. Create .env Files for Each Chatbot', level=1)
doc.add_paragraph(
    "In the project directory, create separate `.env` files for each chatbot instance. "
    "For example, create `.env.chatbot1` and `.env.chatbot2` with the following content:"
)

doc.add_paragraph(".env.chatbot1", style='Normal')
doc.add_paragraph(
    "OPENAI_API_KEY=your_api_key_for_chatbot1\n"
    'NEO4J_URI="neo4j+s://AURADB-INSTANCE:7687"\n'
    "NEO4J_USERNAME=neo4j\n"
    "NEO4J_PASSWORD=password\n"
    "NEO4J_DATABASE=neo4j\n"
    "LLM=gpt-4\n"
    "EMBEDDING_MODEL=openai",
    style='Normal'
)

doc.add_paragraph(".env.chatbot2", style='Normal')
doc.add_paragraph(
    "OPENAI_API_KEY=your_api_key_for_chatbot2\n"
    'NEO4J_URI="neo4j+s://AURADB-INSTANCE:7687"\n'
    "NEO4J_USERNAME=neo4j\n"
    "NEO4J_PASSWORD=password\n"
    "NEO4J_DATABASE=neo4j\n"
    "LLM=gpt-4\n"
    "EMBEDDING_MODEL=openai",
    style='Normal'
)

# Step 3: Modify docker-compose.yml
doc.add_heading('3. Modify docker-compose.yml', level=1)
doc.add_paragraph(
    "To run multiple chatbot instances, you need to modify the `docker-compose.yml` file. "
    "Create separate services for each chatbot. For example:"
)
doc.add_paragraph(
    "version: '3.8'\n\n"
    "services:\n"
    "  chatbot1:\n"
    "    env_file: .env.chatbot1\n"
    "    build: .\n"
    "    ports:\n"
    "      - \"8502:8502\"\n"
    "    depends_on:\n"
    "      - neo4j\n\n"
    "  chatbot2:\n"
    "    env_file: .env.chatbot2\n"
    "    build: .\n"
    "    ports:\n"
    "      - \"8503:8502\" # Note the port change for the second chatbot\n"
    "    depends_on:\n"
    "      - neo4j\n\n"
    "  neo4j:\n"
    "    image: neo4j:latest\n"
    "    environment:\n"
    "      - NEO4J_AUTH=neo4j/password\n"
    "    ports:\n"
    "      - \"7687:7687\"\n"
    "      - \"7474:7474\"",
    style='Normal'
)

# Step 4: Start Docker Containers
doc.add_heading('4. Start Docker Containers', level=1)
doc.add_paragraph(
    "Run the following command to start the containers:"
)
doc.add_paragraph(
    "docker compose up",
    style='Normal'
)
doc.add_paragraph(
    "This command will start all containers and you should be able to access the chatbots at:\n"
    "- Chatbot 1: http://localhost:8502\n"
    "- Chatbot 2: http://localhost:8503"
)

# Step 5: Install and Start Neo4j
doc.add_heading('5. Install and Start Neo4j', level=1)
doc.add_paragraph(
    "Install Neo4j using Homebrew and start the Neo4j service:"
)
doc.add_paragraph(
    "brew install neo4j\n"
    "neo4j start",
    style='Normal'
)

# Step 6: Run Sample Queries in Neo4j
doc.add_heading('6. Run Sample Queries in Neo4j', level=1)
doc.add_paragraph(
    "Open the Neo4j browser at http://localhost:7474 and execute the following sample queries:"
)
doc.add_paragraph(
    "CREATE (a:Person {name: 'Alice', age: 30})\n"
    "CREATE (b:Person {name: 'Bob', age: 25})\n"
    "CREATE (c:Person {name: 'Charlie', age: 35})\n\n"
    "CREATE (a)-[:FRIENDS_WITH]->(b)\n"
    "CREATE (b)-[:FRIENDS_WITH]->(c)",
    style='Normal'
)

# Step 7: Create a Python Script to Connect to Neo4j
doc.add_heading('7. Create a Python Script to Connect to Neo4j', level=1)
doc.add_paragraph(
    "Create a file named `connect_neo4j.py` with the following content:"
)
doc.add_paragraph(
    "from neo4j import GraphDatabase\n\n"
    "# Set up the Neo4j connection parameters\n"
    'uri = "bolt://localhost:7687"\n'
    'username = "neo4j"\n'
    'password = "your_password"  # Replace with your actual password\n\n'
    "# Create a driver instance\n"
    "driver = GraphDatabase.driver(uri, auth=(username, password))\n\n"
    "def query_neo4j():\n"
    "    with driver.session() as session:\n"
    '        result = session.run("MATCH (n) RETURN n LIMIT 10")\n'
    "        for record in result:\n"
    "            print(record)\n\n"
    'if __name__ == "__main__":\n'
    "    query_neo4j()",
    style='Normal'
)

# Step 8: Run the Python Script
doc.add_heading('8. Run the Python Script', level=1)
doc.add_paragraph(
    "Execute the script to connect to Neo4j and run a sample query:"
)
doc.add_paragraph(
    "python connect_neo4j.py",
    style='Normal'
)

# Save the document
file_path = "/mnt/data/GenAI_Stack_Multiple_Chatbots_Setup.docx"
doc.save(file_path)

file_path
