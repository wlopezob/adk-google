## Example Agents
```python
# Example: Adding instructions
capital_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="capital_agent",
    description="Answers user questions about the capital city of a given country.",
    instruction="""You are an agent that provides the capital city of a country.
When a user asks for the capital of a country:
1. Identify the country name from the user's query.
2. Use the `get_capital_city` tool to find the capital.
3. Respond clearly to the user, stating the capital city.
Example Query: "What's the capital of France?"
Example Response: "The capital of France is Paris."
""",
    # tools will be added next
)
```


## Example: Adding tools
```python
# Example: Adding tools
# Define a tool function
def get_capital_city(country: str) -> str:
  """Retrieves the capital city for a given country."""
  # Replace with actual logic (e.g., API call, database lookup)
  capitals = {"france": "Paris", "japan": "Tokyo", "canada": "Ottawa"}
  return capitals.get(country.lower(), f"Sorry, I don't know the capital of {country}.")

# Add the tool to the agent
capital_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="capital_agent",
    description="Answers user questions about the capital city of a given country.",
    instruction="""You are an agent that provides the capital city of a country... (previous instruction text)""",
    tools=[get_capital_city], # Provide the function directly
    #Fine-Tuning LLM Generation
    generate_content_config=types.GenerateContentConfig(
        temperature=0.2, # More deterministic output
        max_output_tokens=250
    )
)
```

## Structuring Data (input_schema, output_schema, output_key)
- input_schema (Optional): Define a Pydantic BaseModel class representing the expected input structure. If set, the user message content passed to this agent must be a JSON string conforming to this schema. Your instructions should guide the user or preceding agent accordingly.

- output_schema (Optional): Define a Pydantic BaseModel class representing the desired output structure. If set, the agent's final response must be a JSON string conforming to this schema.

    * Constraint: Using output_schema enables controlled generation within the LLM but disables the agent's ability to use tools or transfer control to other agents. Your instructions must guide the LLM to produce JSON matching the schema directly.
    
- output_key (Optional): Provide a string key. If set, the text content of the agent's final response will be automatically saved to the session's state dictionary under this key (e.g., session.state[output_key] = agent_response_text). This is useful for passing results between agents or steps in a workflow.

```python
# Example: Structuring data
# Define input and output schemas
input_schema = {
    "country": str,
    "question": str
}
output_schema = {
    "capital": str
}
output_key = "found_capital"
# Define the agent with structured data
capital_agent = LlmAgent(
    model="gemini-2.0-flash",
    name="capital_agent",
    description="Answers user questions about the capital city of a given country.",
    instruction="""You are an agent that provides the capital city of a country... (previous instruction text)""",
    input_schema=input_schema,
    output_schema=output_schema, # Enforce JSON output
    output_key=found_capital  # Store result in state['found_capital']
     # Cannot use tools=[get_capital_city] effectively here
)
```

## RUNNER
La clase Runner en ADK de Google se utiliza para ejecutar agentes (Agent o LlmAgent) dentro de una aplicación, gestionando la interacción entre el agente, la sesión y el usuario. Se encarga de:

Recibir las entradas del usuario.
Pasar el contexto de sesión al agente.
Ejecutar el agente y obtener la respuesta.
Mantener el historial y estado de la conversación.
En resumen, Runner facilita la orquestación y manejo de la conversación entre el usuario y el agente, usando la sesión correspondiente.