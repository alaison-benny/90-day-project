Semantic Kernel-ലെ **Planner** എന്നത് ഒരു മസ്തിഷ്കം പോലെയാണ്. ഉപഭോക്താവിന്റെ ചോദ്യം അനുസരിച്ച് കയ്യിലുള്ള ടൂളുകളിൽ (Plugins) നിന്ന് ഏതാണ് ഉപയോഗിക്കേണ്ടതെന്ന് ഇത് സ്വയം തീരുമാനിക്കും.

നിങ്ങളുടെ **Pet Cart** പ്രോജക്റ്റിൽ സ്റ്റോക്ക് പരിശോധിക്കാനും ഉപദേശം നൽകാനും സഹായിക്കുന്ന ഒരു ലളിതമായ പ്ലാനർ കോഡ് താഴെ നൽകുന്നു.

### Semantic Kernel Planner പൈത്തൺ കോഡ്

```python
import asyncio
import semantic_kernel as sk
from semantic_kernel.connectors.ai.google_palm import GooglePalmChatCompletion
from semantic_kernel.planning import SequentialPlanner

async def pet_cart_planner():
    # 1. കെർണൽ സെറ്റ് ചെയ്യുക
    kernel = sk.Kernel()

    # 2. AI സർവീസ് (Gemini/GPT) കണക്ട് ചെയ്യുക
    api_key = "YOUR_GOOGLE_API_KEY"
    kernel.add_chat_service("models/gemini-pro", GooglePalmChatCompletion("models/gemini-pro", api_key))

    # 3. പ്ലഗിനുകൾ (Plugins) ലോഡ് ചെയ്യുക
    # ഉദാഹരണത്തിന്: സ്റ്റോക്ക് നോക്കാനുള്ള ടൂൾ, പ്രോഡക്റ്റ് വിവരം നൽകാനുള്ള ടൂൾ
    # (ഇവിടെ നമ്മൾ 'PetCartPlugin' എന്നൊരു ഫോൾഡറിൽ ഫംഗ്ഷനുകൾ ഉണ്ടെന്ന് കരുതുന്നു)
    product_plugin = kernel.import_semantic_skill_from_directory("./plugins", "PetCartPlugin")

    # 4. പ്ലാനർ നിർമ്മിക്കുക
    planner = SequentialPlanner()

    # 5. ഉപഭോക്താവിന്റെ ചോദ്യം (Goal)
    ask = "എന്റെ നായയ്ക്ക് നൽകാൻ പറ്റിയ ബെൽറ്റ് ഉണ്ടോ? ഉണ്ടെങ്കിൽ അതിന്റെ ഗുണങ്ങൾ പറയൂ."

    # 6. പ്ലാനർ ഒരു പ്ലാൻ തയ്യാറാക്കുന്നു
    sequential_plan = await planner.create_plan_async(goal=ask, kernel=kernel)

    # 7. പ്ലാൻ പ്രവർത്തിപ്പിക്കുന്നു
    result = await sequential_plan.invoke_async()

    print(f"മറുപടി: {result}")

# റൺ ചെയ്യാൻ
if __name__ == "__main__":
    asyncio.run(pet_cart_planner())

```

---

### ഈ പ്ലാനർ എങ്ങനെയാണ് പ്രവർത്തിക്കുന്നത്?

1. **Intent Recognition:** ഉപഭോക്താവ് 'ബെൽറ്റ്' തിരയുകയാണെന്നും അതിന്റെ 'ഗുണങ്ങൾ' അറിയണമെന്നും AI മനസ്സിലാക്കുന്നു.
2. **Function Selection:** സ്റ്റോക്ക് പരിശോധിക്കാൻ ഒരു ഫംഗ്ഷനും വിവരങ്ങൾ നൽകാൻ മറ്റൊരു ഫംഗ്ഷനും വേണമെന്ന് പ്ലാനർ തീരുമാനിക്കുന്നു.
3. **Step-by-Step Execution:** ആദ്യം സ്റ്റോക്ക് ഉണ്ടോ എന്ന് നോക്കുന്നു. ഉണ്ടെങ്കിൽ മാത്രം അടുത്ത സ്റ്റെപ്പിലേക്ക് കടക്കുന്നു.
4. **Final Output:** എല്ലാ വിവരങ്ങളും ചേർത്ത് ഉപഭോക്താവിന് കൃത്യമായ മറുപടി നൽകുന്നു.

### Pet Cart-ൽ ഇത് എവിടെ വെക്കാം?

നിങ്ങളുടെ വെബ്‌സൈറ്റിലെ സെർച്ച് ബാറിലോ ചാറ്റ് വിൻഡോയിലോ ഇത് ഇന്റഗ്രേറ്റ് ചെയ്യാം. നിങ്ങൾ ഒരു **Scrum Master** കൂടി ആയതുകൊണ്ട്, ഒരു ടീം വർക്ക് പോലെ എങ്ങനെ വ്യത്യസ്ത ഫംഗ്ഷനുകൾ ഒന്നിച്ച് പ്രവർത്തിക്കുന്നു എന്ന് പ്ലാനറിലൂടെ നിങ്ങൾക്ക് എളുപ്പത്തിൽ മനസ്സിലാക്കാം.

ഇതിന്റെ **Plugin** ഫോൾഡർ എങ്ങനെ സെറ്റ് ചെയ്യണം എന്ന് കൂടി കാണിച്ചു തരേണ്ടതുണ്ടോ? (അതായത് പ്ലാനർ ഉപയോഗിക്കുന്ന ആ ചെറിയ ഫംഗ്ഷനുകൾ എങ്ങനെ എഴുതണം എന്നത്).
