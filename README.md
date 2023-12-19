# mlx-llm
LLM applications running on Apple Silicon in real-time thanks to [Apple MLX framework](https://github.com/ml-explore/mlx).

![Alt Text](static/mlx-llm-demo.gif)


Here's also a [Youtube Video](https://www.youtube.com/watch?v=vB7tk6W6VIw).


## **How to install 🔨**
```
git clone https://github.com/riccardomusmeci/mlx-llm
cd mlx-llm
pip install .
```

## **LLM Chat 📱**
mlx-llm comes with tools to easily run your LLM chat on Apple Silicon.

### **Supported models**

| Model Family | Weights | Supported Models |
|----------|----------|----------|
|   LLaMA-2  |  [link](https://ai.meta.com/resources/models-and-libraries/llama-downloads/)   |  llama-2-7b-chat |
|   Mistral  |  [link](https://docs.mistral.ai/models)  |   Mistral-7B-v0.2-Instruct  |
|   OpenHermes-Mistral  |  [link](https://huggingface.co/teknium/OpenHermes-2.5-Mistral-7B/tree/main)  |   OpenHermes-2.5-Mistral-7B  |


### **How to run**
Once downloaded the weights, you need to convert the tokenizer to Apple MLX format (.npz file) with the following command:
```python
from mlx_llm.utils import weights_to_npz, hf_to_npz

# if weights are original ones (from raw sources)
weights_to_npz(
    ckpt_paths=[
        "path/to/model_1.bin", # also support safetensor
        "path/to/model_2.bin",
    ]
    output_path="path/to/model.npz",
)

# if weights are from HuggingFace (e.g. OpenHermes-Mistral)
hf_to_npz(
    model_name=ckpt_paths=[
        "path/to/model_1.bin", # also support safetensor
        "path/to/model_2.bin",
    ]
    output_path="path/to/model.npz",
    n_heads=32,
    n_kv_heads=8
)
```

Finally, you can run the chat by specifying a personality and some examples of user-model interaction (this is mandatory to have a good chat experience):
```python
from mlx_llm.llm import LLM

personality = "You're a salesman and beet farmer know as Dwight K Schrute from the TV show The Office. Dwight replies just as he would in the show. You always reply as Dwight would reply. If you don't know the answer to a question, please don't share false information."

# examples must be structured as below
examples = [
    {
        "user": "What is your name?",
        "model": "Dwight K Schrute",
    },
    {
        "user": "What is your job?",
        "model": "Assistant Regional Manager. Sorry, Assistant to the Regional Manager.",
    }
]

llm = LLM.build(
    model_name="llama-2-7B-chat",
    weights_path="path/to/llama.npz",
    tokenizer_path="path/to/llama.tokenizer",
    personality=personality,
    examples=examples,
)
    
llm.chat(max_tokens=500)
```

## **Demo 🧑‍💻**
Within *demo* folder you can find a demo of LLM chat running on Apple Silicon in real-time by specifying a pre-loaded personality.

Supported personalities:
- Dwight K Schrute (The Office)
- Michael Scott (The Office)
- Kanye West (Rapper)
- Astro (An astrophysicist that likes to keypoints)

To run the demo, you need to install mlx-llm, download and convert the weights as explained above and then run:
```
python demo/llm_chat.py \
    --personality dwight|michael|kanye|astro
    --model llama-2-7b-chat|Mistral-7B-v0.2-instruct
    --weights path/to/weights.npz
    --tokenizer path/to/tokenizer.model
    --max_tokens 500
```

## 📧 Contact

If you have any questions, please email `riccardomusmeci92@gmail.com`

