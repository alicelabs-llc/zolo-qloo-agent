# Zolo × Qloo — Conversational cultural discovery

Live demo: https://zolo-qloo-hackathon.eddyflores100.workers.dev/taste

## Problem and behavior
People know what they like but may not know where to explore next. Zolo accepts natural-language requests, resolves a favorite with Qloo Search, queries Qloo Insights, and displays real returned entities. Workers AI extracts intent and summarizes the evidence. Follow-ups include the last six messages. The user can instead choose categories manually, dictate input, and play a spoken summary.

## Reproduce the hackathon demo
Node.js 22.19+ and a Cloudflare account with Workers AI are required. Install dependencies with npm install. Run node scripts/test_agent.mjs. Deploy the dedicated worker with npx wrangler deploy --config wrangler.agent.jsonc. Configure the personal organizer-issued QLOO_API_KEY using npx wrangler secret put QLOO_API_KEY --config wrangler.agent.jsonc. Never commit it. Enable the AI binding in the supplied configuration. Workers AI is metered; hosting and Qloo usage depend on account limits. The application calls https://hackathon.api.qloo.com.

The production hackathon entrypoint is worker/agent.mjs. This public repository contains the standalone hackathon module, tests and setup instructions; it does not contain the unrelated private legacy assistant.

## Architecture and provenance
1. Workers AI (@cf/meta/llama-3.3-70b-instruct-fp8-fast) extracts favorite, sourceType and targetType, or asks a question.
2. GET /search resolves the favorite. The seed name is displayed for review.
3. GET /v2/insights uses signal.interests.entities with the resolved ID and the selected filter.type.
4. Only Qloo result names become result cards. A bounded model summary is supplementary; it must not be treated as additional Qloo evidence.
5. Browser speech APIs provide optional dictation and playback. Typed input is always available.

The custom server tooling is used directly; this project does not use the Qloo harness chat/plan/build modes.

## Verified on 2026-10-28 (Ecuador)
The deployed natural-language request “I love Radiohead. Find me movies.” returned Radiohead as the seed and eight movies, including Children of Men, Being John Malkovich and Moon. Manual Search → Insights also returned eight entities. Automated tests use fixture upstream responses and cover orchestration, structured model output, invalid input, missing configuration, cross-origin requests and redacted errors. A dedicated Wrangler dry-run also passed.

## Privacy and limitations
No API key reaches the browser. User text and recent conversation are processed by Cloudflare; only the resolved cultural favorite is sent to Qloo. Do not input personal data. History is in tab memory, not application storage. Provider/platform request logs and browser speech services have their own policies. Rate limiting is best-effort per isolate (12 requests/minute/IP), not a global abuse guarantee. No CORS access is provided to other origins; nonbrowser callers can still call the API. Search chooses an exact-name match or the first candidate, so ambiguity is possible. Categories are artist/movie/book/tv_show/podcast. Availability for all pairs is not guaranteed. The model may misparse intent or produce imperfect summaries. Results describe audience affinity, not individual behavior or causation. No taste profile is saved. An unavailable provider produces an error rather than invented recommendations.

## Hackathon-period changes
Zolo existed before this event. The hackathon branch added Qloo discovery. The completion adds an independent lightweight Worker entrypoint, real model binding, conversational intent extraction and clarification, follow-up context, a unified text/voice interface, visible tool provenance, optional manual controls, playback, request validation and reproducible tests. No claim is made that the pre-existing assistant was created during the event.

## Submission status
Demo deployed and source prepared. Devpost submission is NOT confirmed. Public source: https://github.com/alicelabs-llc/zolo-qloo-agent (MIT). No credentials are included.

## License
MIT — see LICENSE. Cloudflare's selected model and Qloo's API/data remain subject to their respective provider terms. They are not relicensed by this repository.
