/**
 * Quick Advance — Claude API Proxy
 * Vercel Edge Function: /api/chat
 *
 * Keeps the Anthropic API key server-side so it's never exposed to the browser.
 *
 * Deploy steps:
 *   1. npm i -g vercel && vercel login
 *   2. vercel --prod  (from the quickadvance/ directory)
 *   3. In Vercel dashboard → Project Settings → Environment Variables:
 *      Add ANTHROPIC_API_KEY = sk-ant-...
 *
 * Usage from dashboard.html:
 *   POST /api/chat
 *   Body: { model, max_tokens, system, messages }
 *   Returns: Anthropic /v1/messages response JSON
 */

export const config = { runtime: 'edge' };

const ALLOWED_ORIGINS = [
  'https://quickadvance.netlify.app',
  'https://quickadvance.com',
  'http://localhost:3000',
  'http://127.0.0.1:5500',  // VS Code Live Server
];

function corsHeaders(origin) {
  const allowed = ALLOWED_ORIGINS.includes(origin) ? origin : ALLOWED_ORIGINS[0];
  return {
    'Access-Control-Allow-Origin': allowed,
    'Access-Control-Allow-Methods': 'POST, OPTIONS',
    'Access-Control-Allow-Headers': 'Content-Type',
  };
}

export default async function handler(req) {
  const origin = req.headers.get('origin') || '';
  const cors = corsHeaders(origin);

  // Handle CORS preflight
  if (req.method === 'OPTIONS') {
    return new Response(null, { status: 204, headers: cors });
  }

  if (req.method !== 'POST') {
    return new Response(JSON.stringify({ error: 'Method not allowed' }), {
      status: 405,
      headers: { ...cors, 'Content-Type': 'application/json' },
    });
  }

  const apiKey = process.env.ANTHROPIC_API_KEY;
  if (!apiKey) {
    console.error('ANTHROPIC_API_KEY environment variable not set');
    return new Response(JSON.stringify({ error: 'API key not configured' }), {
      status: 500,
      headers: { ...cors, 'Content-Type': 'application/json' },
    });
  }

  let body;
  try {
    body = await req.json();
  } catch (e) {
    return new Response(JSON.stringify({ error: 'Invalid JSON body' }), {
      status: 400,
      headers: { ...cors, 'Content-Type': 'application/json' },
    });
  }

  // Whitelist only the fields we need — never let client override the model tier
  const payload = {
    model: 'claude-sonnet-4-20250514',
    max_tokens: Math.min(body.max_tokens || 1000, 2000),  // cap at 2000
    system: body.system || '',
    messages: body.messages || [],
  };

  try {
    const upstream = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': apiKey,
        'anthropic-version': '2023-06-01',
      },
      body: JSON.stringify(payload),
    });

    const data = await upstream.json();

    return new Response(JSON.stringify(data), {
      status: upstream.status,
      headers: { ...cors, 'Content-Type': 'application/json' },
    });
  } catch (err) {
    console.error('Upstream Anthropic error:', err);
    return new Response(JSON.stringify({ error: 'Upstream API error', detail: err.message }), {
      status: 502,
      headers: { ...cors, 'Content-Type': 'application/json' },
    });
  }
}
