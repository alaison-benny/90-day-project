**Vercel AI SDK** പ്രധാനമായും വെബ് ഡെവലപ്പർമാർക്ക് (Next.js, React, Vue) വേണ്ടിയുള്ളതാണ്. ഇതിന്റെ ഏറ്റവും വലിയ പ്രത്യേകത AI നൽകുന്ന മറുപടികൾ ഓരോ വാക്കായി വെബ്‌സൈറ്റിൽ തത്സമയം കാണിക്കാൻ (Streaming) സഹായിക്കുന്നു എന്നതാണ്.

ഇതിന്റെ ഘടനയിൽ പ്രധാനമായും രണ്ട് ഭാഗങ്ങളാണുള്ളത്: **Server-side (Route Handler)**, **Client-side (Frontend Component)**.

---

### 1. Server-side: AI റൂട്ട് നിർമ്മിക്കുക (Route Handler)

നിങ്ങളുടെ `Pet Cart` വെബ്‌സൈറ്റിൽ നിന്ന് ഒരു ചോദ്യം വരുമ്പോൾ അത് കൈകാര്യം ചെയ്യുന്ന ഭാഗമാണിത്.

```typescript
// app/api/chat/route.ts (Next.js App Router)
import { google } from '@ai-sdk/google'; // Gemini ഉപയോഗിക്കാൻ
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { messages } = await req.json();

  // 1. AI മോഡൽ സെറ്റ് ചെയ്യുക (Gemini Pro)
  const result = await streamText({
    model: google('models/gemini-1.5-pro'),
    system: 'നിങ്ങൾ Pet Cart-ലെ ഒരു സഹായിയാണ്.',
    messages,
  });

  // 2. മറുപടി സ്ട്രീം ആയി തിരിച്ചയക്കുന്നു
  return result.toDataStreamResponse();
}

```

---

### 2. Client-side: ചാറ്റ് ഇന്റർഫേസ് (Frontend)

യൂസർ ടൈപ്പ് ചെയ്യുന്നതും AI മറുപടി നൽകുന്നതും കാണിക്കുന്ന ഭാഗമാണിത്.

```tsx
// components/ChatComponent.tsx
'use client';
import { useChat } from 'ai/react';

export default function PetCartChat() {
  // useChat ഹുക്ക് വഴി മെസ്സേജുകളും ഇൻപുട്ടും എളുപ്പത്തിൽ കൈകാര്യം ചെയ്യാം
  const { messages, input, handleInputChange, handleSubmit } = useChat();

  return (
    <div className="flex flex-col w-full max-w-md py-24 mx-auto">
      {messages.map(m => (
        <div key={m.id} className="whitespace-pre-wrap mb-4">
          {m.role === 'user' ? 'You: ' : 'PetBot: '}
          {m.content}
        </div>
      ))}

      <form onSubmit={handleSubmit}>
        <input
          className="fixed bottom-0 w-full max-w-md p-2 mb-8 border border-gray-300 rounded shadow-xl"
          value={input}
          placeholder="നായക്കുട്ടികൾക്കുള്ള ബെഡ് ഉണ്ടോ?"
          onChange={handleInputChange}
        />
      </form>
    </div>
  );
}

```

---

### Vercel AI SDK ആർക്കിടെക്ചർ എങ്ങനെയാണ്?

1. **Client (Browser):** `useChat` എന്ന ഹുക്ക് വഴി യൂസർ നൽകുന്ന ചോദ്യങ്ങൾ സർവറിലേക്ക് അയക്കുന്നു.
2. **Server (Edge Function):** സർവർ ഈ ചോദ്യം എടുത്ത് Gemini അല്ലെങ്കിൽ GPT പോലുള്ള മോഡലുകൾക്ക് നൽകുന്നു.
3. **Streaming:** AI മറുപടി തയ്യാറാക്കുമ്പോൾ തന്നെ ഓരോ വാക്കായി സർവർ ക്ലയന്റിലേക്ക് അയക്കുന്നു. ഇത് വെബ്‌സൈറ്റ് വളരെ വേഗതയുള്ളതാണെന്ന തോന്നൽ നൽകുന്നു.

### പെറ്റ് കാർട്ടിൽ ഇത് എവിടെ ഉപയോഗിക്കാം?

നിങ്ങൾ ഒരു **Full-stack developer** ആകാൻ ആഗ്രഹിക്കുന്നുണ്ടെങ്കിൽ, നിങ്ങളുടെ `Pet Cart` വെബ്‌സൈറ്റിൽ ഒരു **Floating Chat Bubble** നിർമ്മിക്കാൻ ഏറ്റവും എളുപ്പമുള്ള വഴി ഇതാണ്.

* **Scrum Master** എന്ന നിലയിൽ പറയുകയാണെങ്കിൽ, ഒരു സ്പ്രിന്റിനുള്ളിൽ (Sprint) തന്നെ നിങ്ങൾക്ക് ഇതിന്റെ ഒരു വർക്കിംഗ് മോഡൽ തയ്യാറാക്കാം.
* **DevOps** കാഴ്ചപ്പാടിൽ, ഇത് Vercel പ്ലാറ്റ്‌ഫോമിൽ ഒരൊറ്റ ക്ലിക്കിൽ വിന്യസിക്കാനും (Deploy) സാധിക്കും.

നിങ്ങളുടെ പ്രോജക്റ്റിൽ ഇത് പരീക്ഷിക്കാൻ ഒരു **Gemini API Key** മാത്രം മതിയാകും. ഇതിന്റെ സ്റ്റൈലിംഗിനായി (UI Design) എന്തെങ്കിലും സഹായം വേണോ?
