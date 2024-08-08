
A simple LangChain example pulling database records. The idea is that you can create a chain of events and invoke them serially or in parallel.

![langchainarch](https://github.com/user-attachments/assets/d3e3dbca-07b6-4999-baa1-284d869ffc5e)

# Pseudo-code
```python
# Create a runnable (inherits Runnable) that calls your database for external records
class GetMyRunnable(Runnable):
  def invoke(self, userinput, callback):
    return call_to_database_to_get_data()

# Instantiate it
my_runnable = GetMyRunnable()

# Create a prompt
prompt = ChatPromptTemplate.from_template("""Given {external_data} and objective {userinput}""")

# Retrieval (note the passthrough)
setup_and_retrieval = RunnableParallel(
  {"external_data": my_runnable, "userinput": RunnablePassthrough()}
)

# Create a chain
chain = setup_and_retrieval | prompt | llm | output_parser

# Call invoke on the chain
response = chain.invoke(userinput)
```

# Actual-code
```python
from openai import OpenAI
from langchain_openai import ChatOpenAI
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import Runnable, RunnableParallel, RunnablePassthrough

# Define a runnable for your specific function
class GetTrailsRunnable(Runnable):
    def invoke(self, description, callback):
        results = vectorquery_RS_gettrails_from_description(description)
        return results

# LANGCHAIN HELPER RETRIEVAL FUNCTION
def vectorquery_RS_gettrails_from_description(description):
    # Using a query lambda API call to the database
    query_url = "https://api.usw2a1.rockset.com/v1/orgs/self/ws/testing/lambdas/confluentkafka_local_trailsvectorsearchL5_QL2/tags/latest"
    try:
        client = OpenAI(api_key=YOUR_OPENAI_API_KEY)
        response = client.embeddings.create(
            model="text-embedding-ada-002",
            input=description,
            encoding_format="float"
        )
        embedding_data = [embedding.embedding for embedding in response.data]
    except Exception as e:
        print(f"Error converting text to vector: {e}")
        return jsonify({"error": "OpenAI API call failed"}), 500

    array_as_string = json.dumps(embedding_data[0])
    payload = {
        "parameters": [
            {
                "name": "input_embedding",
                "type": "array",
                "value": array_as_string
            }
        ]
    }
    response = requests.post(query_url, headers={
        'Authorization': 'ApiKey ' + YOUR_ROCKSET_API_KEY,
        'Content-Type': 'application/json'
    }, data=json.dumps(payload))

    if response.status_code == 200:
        json_data = response.json()
        results = json_data.get('results', [])
        print("vectorquery_RS_gettrails_from_description -> " + str(results))
        return results
    else:
        print(f"Error from Rockset: {response.text}")
        return []

# LANGCHAIN - step 1 submitting
@app.route('/submitlangchainstep1', methods=['POST'])
def submitlangchainstep1():
    status_message = request.args.get('status_message', '')
    status_class = request.args.get('status_class', '')
    json_message = request.args.get('json_message')

    input_llmmodel = 'gpt-4-0125-preview' if 'check_latestAImodel' in request.form else 'gpt-3.5-turbo-0125'
    input_prompt1 = request.form['input_prompt1'].strip()
    description = input_prompt1 + " Also, please create a packing list and tell me where I can buy those items."

    try:
        trails_runnable = GetTrailsRunnable()
        output_parser = StrOutputParser()
        llm = ChatOpenAI(openai_api_key=YOUR_OPENAI_API_KEY, model_name=input_llmmodel)
        
        print("****Setting up prompt")
        chat_sysmsg = "You are a helpful assistant. Please respond as quickly as possible. If you provide a list, only give me 3 items please."
        template = chat_sysmsg + """ Given the following trails information that I want to ride, {trails} and stated objective as: {description}. Also, please do tell me how many trails you picked out of the ones provided to you."""
        prompt = ChatPromptTemplate.from_template(template)

        print("****Setting up retrieval")
        setup_and_retrieval = RunnableParallel(
            {"trails": trails_runnable, "description": RunnablePassthrough()}
        )

        print("****Creating chain")
        chain = setup_and_retrieval | prompt | llm | output_parser

        print("****Invoking chain")
        response = chain.invoke(description)
        json_message = response
    except Exception as e:
        print(f"Error LangChanging: {e}")
        status_class = "danger"
        status_message = f"Ouch. LangChanging threw an error. Please try again!"

    return redirect(url_for('viewlangchainstep2', status_message=status_message, json_message=json_message, status_class=status_class))
```
