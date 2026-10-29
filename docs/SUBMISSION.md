# Devpost copy — Zolo × Qloo

## Tagline
A conversational cultural discovery agent that follows your tastes across music, movies, books, TV and podcasts.

## Inspiration
Discovery begins with a favorite, but users should not have to learn an API or trust invented recommendations. We wanted one accessible conversation grounded in Qloo's cultural graph.

## What it does
A person types or dictates a favorite and a desired direction. Workers AI extracts the discovery intent, Qloo Search resolves the entity, and Qloo Insights returns recommendations. The app displays the matched favorite and tool steps, provides optional spoken playback, and supports conversational follow-ups and manual category selection.

## How we built it
A dedicated Cloudflare Worker serves the interface and server endpoints. Workers AI handles language; Qloo supplies discovery evidence. The credential remains a server secret. The existing Zolo codebase used Next.js, React and TypeScript; the hackathon demo is a lightweight independent Worker module.

## Challenges
Cloudflare's old model alias was deprecated and structured responses required normalization. We added regression coverage, bounded the model workflow, and kept result cards grounded in returned Qloo entities. Ambiguous favorite names remain a limitation and are made visible to the user.

## Accomplishments
A live conversational Radiohead-to-movies flow returned eight real Qloo recommendations. The interface combines conversation, dictation, manual discovery and playback. Fixture orchestration tests and a dedicated Worker dry-run passed. The original private assistant was tested separately. See README.md for the scope and limitations.

## What we learned
Cultural affinity is evidence about audiences, not a prediction of personal behavior. Showing the resolved favorite and provenance is central to an understandable recommendation experience.

## What's next
Manual disambiguation, broader category evaluation, durable abuse protection and user-controlled preferences.

Demo: https://zolo-qloo-hackathon.eddyflores100.workers.dev/taste
Public repository: https://github.com/alicelabs-llc/zolo-qloo-agent No final submission has been recorded.
