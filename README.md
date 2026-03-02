# ▲ Vercel AI SDK + Crazyrouter

> Build streaming AI UIs with Next.js and Crazyrouter — Save 45% on API costs.

[Crazyrouter](https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community?ref=github) — One API key, 300+ models.

## 💰 Price Comparison

| Model | Official (In/Out per 1M tokens) | Crazyrouter | Savings |
|-------|--------------------------------|-------------|---------|
| Claude Opus 4 | $15 / $75 | $8.25 / $41.25 | **45%** |
| GPT-4o | $2.50 / $10 | $1.38 / $5.50 | **45%** |
| Gemini 2.5 Pro | $1.25 / $10 | $0.69 / $5.50 | **45%** |

## ⚡ Quick Start

### Installation
```bash
npm install ai @ai-sdk/openai
```

### Environment Variables
```bash
# .env.local
OPENAI_API_KEY=sk-your-crazyrouter-key
OPENAI_BASE_URL=https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community/v1
```

### API Route (Next.js App Router)
```typescript
// app/api/chat/route.ts
import { openai } from "@ai-sdk/openai";
import { streamText } from "ai";

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = streamText({
    model: openai("gpt-4o"),
    messages,
  });

  return result.toDataStreamResponse();
}
```

### Chat UI Component
```typescript
// app/page.tsx
"use client";
import { useChat } from "ai/react";

export default function Chat() {
  const { messages, input, handleInputChange, handleSubmit } = useChat();

  return (
    <div>
      {messages.map((m) => (
        <div key={m.id}>
          <strong>{m.role}:</strong> {m.content}
        </div>
      ))}
      <form onSubmit={handleSubmit}>
        <input value={input} onChange={handleInputChange} />
        <button type="submit">Send</button>
      </form>
    </div>
  );
}
```

### Generate Text (Non-streaming)
```typescript
import { openai } from "@ai-sdk/openai";
import { generateText } from "ai";

const { text } = await generateText({
  model: openai("claude-sonnet-4-20250514"),
  prompt: "Explain API gateways",
});
```

### Structured Output
```typescript
import { openai } from "@ai-sdk/openai";
import { generateObject } from "ai";
import { z } from "zod";

const { object } = await generateObject({
  model: openai("gpt-4o"),
  schema: z.object({
    name: z.string(),
    description: z.string(),
    price: z.number(),
  }),
  prompt: "Generate a product listing for an AI API service",
});
```

## 🚀 Deploy to Vercel

```bash
npx create-next-app@latest my-ai-app
cd my-ai-app
npm install ai @ai-sdk/openai
# Add your code, then:
vercel deploy
```

Set environment variables in Vercel dashboard:
- `OPENAI_API_KEY` = your Crazyrouter key
- `OPENAI_BASE_URL` = `https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community/v1`

## ❓ FAQ

**Q: Does Vercel AI SDK work with Crazyrouter?**
A: Yes, fully compatible. Just set the base URL.

**Q: Can I use Claude/Gemini models?**
A: Yes! Use model IDs like `claude-sonnet-4-20250514` through the OpenAI provider.

**Q: Does streaming work?**
A: Yes, streaming is a core feature of Vercel AI SDK.

## 🔗 Links
- 🌐 [Crazyrouter](https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community?ref=github)
- ▲ [Vercel AI SDK](https://sdk.vercel.ai)
- 💬 [Telegram](https://t.me/crzrouter)

## 📄 License
MIT

