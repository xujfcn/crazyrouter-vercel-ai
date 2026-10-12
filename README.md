# Vercel AI SDK with Crazyrouter

Updated 2026-10-12. Requires Node.js 22+. This example explicitly uses Chat Completions through a configured provider.

```bash
npm install ai@7.0.137 @ai-sdk/openai@4.0.91
```

Set `CRAZYROUTER_API_KEY` on your server. Do not use a `NEXT_PUBLIC_` variable for the key.

## Next.js App Router endpoint

Create `app/api/chat/route.ts`:

```typescript
import { createOpenAI } from '@ai-sdk/openai';
import { streamText } from 'ai';

export async function POST(req: Request) {
  const { prompt } = await req.json();
  if (typeof prompt !== 'string' || !prompt.trim()) {
    return new Response('prompt must be a non-empty string', { status: 400 });
  }
  const apiKey = process.env.CRAZYROUTER_API_KEY;
  if (!apiKey) return new Response('API key is not configured', { status: 503 });
  const provider = createOpenAI({ baseURL: 'https://crazyrouter.com/v1', apiKey });
  const result = streamText({ model: provider.chat('gpt-4o-mini'), prompt });
  return result.toTextStreamResponse();
}
```

The endpoint returns a plain text stream, suitable for a `fetch` client. It is not the UI-message stream expected by the React `useChat` hook. Add application authentication, request limits and appropriate input limits before exposing a billable endpoint publicly.

## Browser request

```javascript
const response = await fetch('/api/chat', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ prompt: 'Explain API gateways in three sentences.' }),
});
if (!response.ok) throw new Error(await response.text());
console.log(await response.text());
```

For a hook-based UI, use the current `@ai-sdk/react` package and its documented UI-message transport end to end. The previous `ai/react` and `toDataStreamResponse` examples have been retired. Calling `provider(model)` may select Responses in current provider versions; `provider.chat(model)` deliberately selects Chat Completions here.

[AI SDK documentation](https://ai-sdk.dev/docs) · [Current models](https://crazyrouter.com/en/models) · [Current prices](https://crazyrouter.com/en/pricing)
