# ▲ Vercel AI SDK + Crazyrouter

> Build streaming AI UIs with Next.js and Crazyrouter.

## ⚡ Setup
```bash
npm install ai openai
```

```typescript
import { openai } from '@ai-sdk/openai';
import { generateText } from 'ai';

const result = await generateText({
  model: openai('gpt-4o', { baseURL: 'https://crazyrouter.com/v1' }),
  prompt: 'Hello!',
});
```

## 🔗 [Crazyrouter](https://crazyrouter.com?ref=github) | [Vercel AI SDK](https://sdk.vercel.ai)
## 📄 License: MIT
