













# 
```

```
# 
```

```
# 
```
Before implementing the next sprint, perform a COMPLETE IMPLEMENTATION AUDIT of the current repository.

Do NOT write any code.

Do NOT modify any file.

Audit only.

This audit must be based ONLY on the actual source code currently in the repository.

Ignore previous conversations, plans, promises, prompts, or roadmap unless they are already implemented in code.

For every feature determine whether it is:

✅ Fully Implemented
🟡 Partially Implemented
❌ Not Implemented

Verify implementation by inspecting the real source code, not class names or comments.

Audit the following areas:

1. Architecture
2. OpenAI Compatibility
3. Provider Manager
4. Provider Adapters
5. Multi Provider
6. Multi API Keys
7. API Key Rotation
8. API Key Health
9. Provider Health
10. Provider Failover
11. API Key Failover
12. Model-aware Failover
13. Smart Routing
14. Routing Strategies
15. Cooldown
16. Circuit Breaker
17. Model Registry
18. Model Alias
19. Dynamic Model Discovery
20. Request Executor
21. Retry
22. Streaming
23. Responses API
24. Embeddings
25. Images
26. Audio
27. Tool Calling
28. Metrics
29. Logging
30. Admin Dashboard
31. Admin APIs
32. Hot Reload
33. Runtime Persistence
34. Configuration
35. Security
36. Rate Limiting
37. Testing
38. CI/CD
39. Performance
40. Documentation

For every partial or missing item include:

- reason
- affected files
- affected classes/modules
- estimated implementation effort
- implementation priority

Also detect:

- dead code
- duplicated code
- obsolete code
- TODO
- FIXME
- unreachable code
- memory leaks
- race conditions
- security vulnerabilities
- performance bottlenecks
- architecture violations

Finally generate:

# Overall Completion (%)

# Architecture Score (/100)

# Production Readiness (/100)

# Fully Implemented

# Partially Implemented

# Missing Features

# Security Findings

# Performance Findings

# Technical Debt

# Highest Priority Tasks

# Recommended Next Sprint

IMPORTANT:

If every item required for the next sprint is already fully implemented, explicitly say:

"Ready to proceed to the next sprint."

Otherwise list exactly what must be completed before continuing.
```

# 
```
Sprint 10 – Intelligent Model Registry & Routing Engine

Preserve the existing architecture and all completed features.

Do NOT break compatibility with the OpenAI API.

The objective of this sprint is to build an intelligent Model Registry and Routing Engine.

Requirements

1. Global Model Registry

Create a central Model Registry.

Each model record should contain:

- model id
- aliases
- provider
- provider id
- endpoint
- capabilities
- context length
- supports streaming
- supports tools
- supports vision
- supports reasoning
- supports embeddings
- supports images
- supports audio
- health
- latency
- success rate
- priority

2. Automatic Provider Discovery

When a provider is added:

Automatically query:

GET /v1/models

and populate the registry.

Support manual refresh.

Support scheduled refresh.

No server restart required.

3. Model Alias System

Support aliases.

Example:

Alias:
gpt-5

Available Providers:

OpenAI
OpenRouter
Azure
Provider X

Alias:

claude-sonnet

Available Providers:

Anthropic
OpenRouter

Alias:

kimi-k3

Available Providers:

Moonshot
Provider X

The client always sends the alias.

Gateway resolves the best provider automatically.

4. Capability Detection

Automatically detect provider capabilities.

Examples:

Streaming

Tool Calling

Vision

Reasoning

Embeddings

Responses API

Images API

Audio API

Store capabilities inside the registry.

5. Intelligent Routing Rules

Support routing rules such as:

IF model == gpt-5

THEN

Priority:

OpenAI

↓

OpenRouter

↓

Provider X

Another example:

IF model == kimi-k3

Priority:

Moonshot

↓

Provider X

↓

Provider Y

Every model can have its own provider order.

6. Routing Conditions

Allow routing by:

priority

latency

provider health

API key health

least used

weighted

round robin

random

success rate

response time

7. Rule Engine

Create a routing rule engine.

Support:

IF

ELSE

AND

OR

NOT

Example:

IF provider latency > 3000 ms

Switch provider.

Example:

IF API key cooldown active

Skip key.

Example:

IF provider unhealthy

Skip provider.

8. Admin Dashboard

Add pages:

Model Registry

Aliases

Routing Rules

Capabilities

Discovery Status

Provider Priority per Model

Refresh Models

Refresh Capabilities

9. Persistence

Persist:

Providers

Models

Aliases

Capabilities

Routing Rules

API Keys

Health History

10. APIs

Implement admin APIs:

GET /admin/models

GET /admin/aliases

GET /admin/routing

POST /admin/discover

POST /admin/refresh-models

POST /admin/refresh-capabilities

PUT /admin/routing

11. Tests

Add comprehensive unit and integration tests covering:

Provider discovery

Model registry

Alias resolution

Routing rules

Capability detection

Persistence

Dashboard

Backward compatibility

12. Final Goal

The gateway must become a self-learning OpenAI-compatible gateway where providers can be added dynamically, models are discovered automatically, routing adapts based on health and performance, and clients continue using standard OpenAI-compatible requests without changing their configuration.
```




# 
```
Sprint 9 – Multi Provider Gateway Architecture

Do NOT rewrite the existing architecture.

Preserve all completed features including:
- OpenAI-compatible APIs
- Provider Adapter architecture
- Model Registry
- Retry
- Fallback
- Streaming
- Metrics
- Admin Dashboard
- Tests

The next milestone is implementing a production-grade Multi Provider Gateway.

Requirements:

1. Multiple Providers
Each provider can be enabled or disabled independently.

Example:
- OpenAI
- OpenRouter
- Anthropic
- Gemini
- DeepSeek
- NVIDIA
- Databricks
- Any future OpenAI-compatible provider

2. Multiple API Keys per Provider

Each provider must support unlimited API keys.

Example:

OpenRouter
- Key 1
- Key 2
- Key 3

DeepSeek
- Key 1
- Key 2

Gemini
- Key 1
- Key 2
- Key 3
- Key 4

3. API Key Rotation

Support:
- Priority
- Round Robin
- Random
- Least Used
- Weighted

4. Automatic API Key Failover

If a key returns:
- 401
- 403
- 429
- quota exceeded
- rate limited
- timeout
- connection error
- upstream unavailable

Automatically switch to the next healthy API key without failing the client request.

5. Automatic Provider Failover

If every API key of a provider fails, automatically switch to the next provider.

6. IMPORTANT

Provider failover is ONLY allowed if the next provider supports the SAME requested model.

Never silently replace the requested model with another model.

Example:

Client requests:
model = gpt-5

Gateway may only fail over to providers that also support gpt-5.

If no provider supports the requested model, return an appropriate OpenAI-compatible error.

7. Smart Routing

Support routing modes:

- Priority
- Fastest Response
- Lowest Latency
- Round Robin
- Least Used
- Weighted
- Random

Routing strategy must be configurable.

8. Health Monitoring

Track health for every:

- Provider
- API Key

Store:

- latency
- success rate
- error rate
- last success
- last failure
- cooldown status
- request count
- token usage

9. Cooldown

Failed API keys should enter cooldown.

Cooldown duration must be configurable.

Healthy keys should automatically rejoin rotation after cooldown.

10. Admin Dashboard

Add full management UI for:

Providers
API Keys
Routing Strategy
Health Status
Latency
Success Rate
Cooldown
Priority
Weights
Usage
Model Support

11. Configuration

Support hot reload.

No restart required after changing:

- Providers
- API Keys
- Routing
- Priorities

12. Backward Compatibility

Existing OpenAI-compatible clients such as Codex CLI, Aider, Continue, Roo Code, Cline, OpenCode, Open WebUI, and other OpenAI-compatible tools must continue to work without any configuration changes.

13. Testing

Implement comprehensive unit, integration, and failover tests covering:

- API key rotation
- Provider failover
- Smart routing
- Model-aware failover
- Health monitoring
- Cooldown
- Dashboard integration

Implement this incrementally without breaking the existing architecture. At the end, provide a summary of all new modules, APIs, database/config changes, and tests added.
```



# 
```
Act as a senior Staff Software Engineer and AI Gateway architect.

First, DO NOT implement any new feature.

Your first task is to perform a complete audit of this repository.

Analyze the entire project including:

- folder structure
- architecture
- code quality
- scalability
- maintainability
- security
- performance
- API compatibility
- OpenAI compatibility
- provider architecture
- routing architecture
- admin dashboard
- frontend
- backend
- testing
- CI/CD
- documentation

Specifically verify:

1. Which features are fully implemented.
2. Which features are partially implemented.
3. Which features are only UI placeholders.
4. Which files are dead code.
5. Which files are duplicated.
6. Which modules violate clean architecture.
7. Potential race conditions.
8. Memory leaks.
9. Blocking I/O.
10. Error handling.
11. Retry logic.
12. Streaming implementation.
13. Provider abstraction.
14. Model registry.
15. API key management.
16. Metrics.
17. Health monitoring.
18. Admin Dashboard functionality.
19. Security issues.
20. Performance bottlenecks.

Then produce a report with this exact structure:

## Overall Score (/100)

## Architecture

## Completed Features

## Partially Completed Features

## Missing Features

## Bugs

## Technical Debt

## Security Issues

## Performance Issues

## Code Smells

## Suggested Refactoring

## Highest Priority Fixes

## Recommended Development Roadmap

Do not modify any code yet.

Only analyze the repository and produce a detailed audit report.
```



# 
```
Continue with the ROADMAP from the first unchecked task.

Rules:
- Do NOT ask for approval after every task.
- Continue automatically until the current Phase is 100% complete.
- Stop only when the entire current Phase is finished or if you encounter a real blocking issue that cannot be resolved safely.
- Follow SPEC.md exactly.
- Reuse existing components, hooks, stores, utilities, design tokens, and services. Do not duplicate code.
- Maintain the current architecture and coding standards.
- Do not introduce breaking changes.
- Keep TypeScript strict with zero lint errors.
- Update ROADMAP.md immediately after each completed task.
- Update CHANGELOG.md after each completed task.
- Create a separate commit for every completed task and push to the repository.
- Verify that the implementation matches the ROADMAP before moving to the next task.
- At the end of the phase, provide a concise summary of all completed tasks, modified files, commits, and the next phase to be implemented.
- Do not restart previous phases or re-audit the project unless inconsistencies are found.
- If a minor inconsistency is found, fix it immediately and continue without stopping.
- Continue until the current phase is fully completed.
``
# 
```
Backend AI Gateway sudah selesai dan berjalan normal. Endpoint /health dan server sudah berfungsi, tetapi Admin Dashboard masih belum selesai. Fokus hanya menyelesaikan Admin Dashboard, jangan mengubah arsitektur gateway, provider manager, request executor, retry, fallback, routing, auth, metrics, rate limiting, ataupun fitur backend yang sudah selesai.

Target utama adalah membuat /admin menjadi dashboard produksi yang sepenuhnya interaktif.

Selesaikan seluruh frontend dan backend yang diperlukan agar semua menu benar-benar bekerja.

Persyaratan:

1. Semua menu dapat diklik:
- Overview
- Providers
- API Keys
- Models
- Logs
- Health
- Config

Gunakan SPA (single page), tanpa reload halaman.

2. Overview
- Total request
- Success
- Error
- Active provider
- Active API Keys
- Uptime
- Version
- Average latency
- Request per second
- Grafik realtime yang otomatis refresh.

3. Providers
- Menampilkan seluruh provider.
- Enable/Disable provider.
- Priority.
- Adapter.
- Endpoint.
- Health.
- Test connection.
- Reload provider.
- Simpan perubahan tanpa restart server.

4. API Keys
- CRUD lengkap.
- Enable/Disable.
- Restriksi provider.
- Restriksi model.
- Expired date.
- Role.
- Usage.
- Search.
- Tanpa reload halaman.

5. Models
- Ambil data langsung dari registry gateway.
- Refresh model.
- Provider.
- Capability.
- Context.
- owned_by.
- Endpoint support.

6. Logs
- Live log viewer.
- Auto refresh.
- Pause.
- Search.
- Filter.
- Copy.
- Clear.

7. Health
- Provider health.
- Circuit breaker.
- Retry status.
- Latency.
- Memory.
- Request count.
- Uptime.
- Update realtime.

8. Config
- Edit konfigurasi provider.
- Validasi sebelum save.
- Reload tanpa restart.
- Rollback jika konfigurasi tidak valid.

9. Frontend
Pastikan:
- Tidak ada JavaScript error.
- Tidak ada CSS atau overlay yang membuat menu tidak bisa diklik.
- Semua tombol berfungsi.
- Semua fetch menuju endpoint admin yang benar.
- Loading indicator.
- Toast notification.
- Error handling.
- Responsive desktop dan mobile.

10. Backend
Jika endpoint admin masih belum ada, implementasikan endpoint yang diperlukan tanpa mengubah arsitektur gateway yang sudah selesai.

11. Testing
Verifikasi:
- Semua tab dapat diklik.
- Semua tombol bekerja.
- Tidak ada error di browser console.
- Tidak ada request 404.
- Tidak ada JavaScript exception.
- Tidak ada endpoint admin yang gagal.
- Dashboard menampilkan data nyata dari gateway, bukan data dummy.

Setelah selesai, jalankan seluruh test yang ada, perbaiki semua kegagalan hingga seluruh test lulus, kemudian tampilkan ringkasan fitur yang berhasil diselesaikan beserta daftar endpoint admin yang digunakan.
```

# Prompt: Production Admin Dashboard
```
Implement a production-grade Admin Dashboard.

Requirements

Backend

Create authenticated admin API.

Frontend

Responsive web UI.

Dashboard

Overview cards:

- Requests
- Tokens
- Cost
- Active Providers
- Healthy Providers
- Active API Keys
- Rate Limit
- Average Latency

Provider Management

Allow:

- Enable/Disable provider
- Priority
- Weight
- Timeout
- Retry
- Fallback
- API Base URL
- API Keys
- Health status
- Manual reload
- Test provider

API Key Management

Allow:

- Create
- Delete
- Enable/Disable
- Expire
- Restrict providers
- Restrict models
- Usage statistics

Model Registry

Display:

- Provider
- Model
- Capabilities
- Health
- Status

Monitoring

Charts:

- Requests
- Tokens
- Cost
- Latency
- Success rate
- Retry
- Fallback
- Rate-limit events

Logs

Realtime request log.

Filtering by:

- Provider
- API Key
- Model
- Status

Health

Display:

- Circuit breaker
- Health monitor
- Consecutive failures
- Last success
- Last failure

Configuration

Support live edits.

Use existing Hot Reload.

No restart.

Security

Reuse authentication.

Role:

Admin only.

Architecture

Reuse existing services.

No duplicated logic.

Testing

Add integration tests.

Update README.

Finish only after all tests pass.
```
# Prompt selanjutnya: Provider Configuration & Hot Reload
```
Implement production-grade Provider Configuration & Hot Reload.

Requirements

Create a centralized ProviderConfigManager.

Goals

Allow providers to be managed without restarting the gateway.

Support:

- enable/disable provider
- priority
- weight
- timeout
- retry policy
- fallback policy
- API base URL
- API keys
- model mapping
- provider metadata

Configuration Sources

Support:

- .env
- config/providers/*.json

Hot Reload

Watch configuration files.

Reload provider configuration automatically.

No process restart.

ProviderManager

ProviderManager must react to configuration changes automatically.

Existing requests continue normally.

New requests use the updated configuration.

Validation

Validate:

- duplicated provider ids
- duplicated priorities
- invalid URLs
- invalid model mapping
- missing API keys
- unsupported capabilities

Rollback

If a configuration reload fails:

- keep previous configuration
- log validation errors
- continue serving requests

Metrics

Expose:

- config reload count
- reload failures
- active providers
- disabled providers

Testing

Add integration tests for:

- enable/disable
- hot reload
- rollback
- validation
- priority changes
- provider removal
- provider addition

Documentation

Update README.

Maintain clean architecture.

No duplicated logic.

Finish only after every test passes.
```
# Prompt berikutnya: Rate Limiting & Quotas
```
Implement production-grade Rate Limiting & Quota Management.

Requirements

Create centralized RateLimiter.

Reuse existing architecture.

Do not duplicate logic.

Support:

- Global rate limit
- Per API Key rate limit
- Per Provider rate limit
- Per Model rate limit
- Concurrent request limit
- Burst limit
- Daily request quota
- Daily token quota
- Monthly token quota

Window Types

Support:

- Fixed Window
- Sliding Window
- Token Bucket

Configuration

Allow configuration from:

- .env
- config/rateLimit.json

Headers

Return OpenAI-compatible headers:

X-RateLimit-Limit
X-RateLimit-Remaining
X-RateLimit-Reset

Retry-After

Return HTTP 429 with OpenAI-compatible error envelope.

Integration

Reuse existing:

- Authentication
- Metrics
- RequestExecutor
- ApiKeyManager

Metrics

Expose:

- rejected requests
- quota usage
- burst usage
- provider throttling

Testing

Add integration tests for:

- burst limit
- concurrency limit
- per-key limit
- per-provider limit
- quota exhaustion
- retry-after
- metrics integration
- OpenAI compatibility

Documentation

Update README.

Maintain clean architecture.

Finish only after all tests pass.
```


# Prompt selanjutnya: Metrics & Monitoring
```
Implement production-grade Metrics & Monitoring.

Requirements

Create internal metrics subsystem.

Expose endpoints:

GET /metrics
GET /stats
GET /health/providers

Metrics

Track globally and per provider:

- total requests
- successful requests
- failed requests
- retry count
- fallback count
- average latency
- p50 latency
- p95 latency
- p99 latency
- total prompt tokens
- total completion tokens
- total cost (if provider exposes usage)
- active API keys
- active providers

Provider Health

Track for every provider:

- online/offline
- last success
- last failure
- average latency
- success rate
- consecutive failures
- circuit state (closed/open/half-open)

Health Checks

Support automatic periodic health checks.

Providers that recover should automatically become available again.

Caching

Cache metrics snapshots efficiently.

Avoid blocking request execution.

Architecture

Reuse RequestExecutor hooks.

Do not duplicate logic.

Metrics collection must be centralized.

Testing

Add tests for:

- metrics updates
- provider health
- retries
- fallback
- latency calculation
- health recovery
- endpoint responses

Documentation

Update README.

Maintain clean architecture.

Finish only after all tests pass.
```
# Prompt berikutnya: Authentication & API Keys
```
Implement production-grade authentication and authorization.

Requirements

Implement middleware-based authentication.

Support:

- Bearer API Keys
- Multiple API Keys
- Key metadata
- Key status (active/inactive)
- Optional expiration
- Optional provider restriction
- Optional model restriction
- Request usage tracking

Configuration

Support API keys from:

- .env
- config/apiKeys.json

Authentication middleware

Protect every API endpoint except:

- /
- /health
- /ready

Error responses must be OpenAI-compatible.

Usage

Track per-key:

- total requests
- total tokens
- provider usage
- model usage
- last used
- created time

Key rotation

Allow multiple active keys simultaneously.

Architecture

Reuse existing architecture.

Do not duplicate logic.

Testing

Add tests for:

- valid key
- invalid key
- disabled key
- expired key
- missing key
- provider restriction
- model restriction

Documentation

Update README.

Maintain clean architecture.

Finish only after all tests pass.
```

# Prompt selanjutnya: Models API & Provider Discovery
```
Implement a production-ready OpenAI-compatible Models API.

Requirements

Implement:

GET /v1/models
GET /v1/models/:id

Architecture

Reuse:

- ProviderManager
- ProviderAdapter
- RequestExecutor
- Logger

Do not duplicate logic.

Provider Discovery

Each ProviderAdapter must expose:

- listModels()
- supportsModel()
- capabilities

The ProviderManager aggregates all providers into one unified model registry.

Deduplicate identical model IDs.

Return provider metadata internally while exposing OpenAI-compatible output externally.

Capabilities

Track capabilities for every model:

- chat
- responses
- embeddings
- images
- audio
- tools
- streaming
- reasoning

OpenAI Compatibility

Return the standard OpenAI models response.

Support retrieving a single model.

Caching

Cache model lists with configurable TTL.

Allow manual refresh.

Fallback

If one provider fails, continue collecting models from remaining providers.

Testing

Add tests for:

- aggregation
- deduplication
- provider failure
- cache
- refresh
- OpenAI compatibility

Documentation

Update README.

Maintain clean architecture.

No duplicated code.

Finish only after all tests pass.
```
# Prompt: Tool Calling / Function Calling
```
Implement full OpenAI-compatible Tool Calling / Function Calling.

Requirements

Implement complete support for:

- tools
- tool_choice
- parallel_tool_calls
- tool_calls
- function calling

Maintain compatibility with OpenAI Chat Completions API.

Architecture

Reuse existing architecture only.

Use:

- ProviderManager
- ProviderAdapter
- RequestExecutor
- HttpClient
- Retry
- Fallback
- ApiKeyManager
- Logger

Do not duplicate logic.

Provider Capability

Add:

supportsTools

Automatically filter providers that do not support tool calling.

Return OpenAI-compatible behaviour.

Provider Mapping

Support providers that expose native tool/function calling.

Map provider-specific formats into a single internal representation.

Adapters are responsible only for request/response translation.

Streaming

Support tool calls during SSE streaming.

Support incremental tool_call deltas exactly like OpenAI.

Normalization

Normalize every provider into OpenAI Chat Completion format.

Support:

- assistant tool_calls
- function.name
- function.arguments
- tool_call_id
- finish_reason = tool_calls

Parallel Tool Calls

Support multiple tool calls in one assistant response.

Validation

Validate:

- tools schema
- tool_choice
- function definitions
- JSON schema

Return OpenAI-compatible validation errors.

Retry/Fallback

Reuse current RequestExecutor retry and fallback.

Documentation

Update README:

- tools examples
- function examples
- streaming examples
- provider compatibility
- limitations

Testing

Add integration tests for:

- single tool call
- multiple tool calls
- streaming tool calls
- validation errors
- unsupported providers
- fallback
- retry
- OpenAI compatibility
- finish_reason=tool_calls

Maintain clean architecture.

No duplicated code.

No provider-specific logic outside ProviderAdapter.

Finish only after all tests pass.
```
# Prompt: Audio API
```
Implement full OpenAI-compatible Audio API.

Requirements

Implement all OpenAI Audio endpoints:

POST /v1/audio/speech
POST /v1/audio/transcriptions
POST /v1/audio/translations

Architecture

Reuse existing architecture only.

Use:

- ProviderManager
- ProviderAdapter
- RequestExecutor
- HttpClient
- Retry
- Fallback
- ApiKeyManager
- Logger

No duplicated code.

Provider Capability

Add:

supportsAudio

Automatically filter unsupported providers.

Return OpenAI-compatible error if unsupported.

Speech API

Support:

- model
- input
- voice
- response_format
- speed

Support binary audio responses.

Normalize provider responses.

Transcriptions

Support multipart upload.

Support:

- file
- model
- language
- prompt
- response_format
- temperature

Translations

Support multipart upload.

Translate into English following OpenAI behavior.

Multipart

Reuse existing multipart utilities from Images API where possible.

Streaming

Reject stream=true for unsupported operations.

Retry/Fallback

Reuse current retry and fallback pipeline.

Response Normalization

Normalize all providers into OpenAI format.

Provider Support

Support providers that expose audio APIs.

Unsupported providers must be skipped automatically.

Testing

Add integration tests for:

- speech
- transcription
- translation
- multipart upload
- validation
- fallback
- retry
- normalization
- unsupported providers
- OpenAI compatibility

Documentation

Update README:

- endpoints
- request examples
- response examples
- provider matrix
- limitations

Maintain clean architecture.

No duplicated code.

Finish only after every test passes.
```



# Prompt: Images API
```
Implement full OpenAI-compatible Images API.

Requirements:

1. Add endpoints:

POST /v1/images/generations
POST /v1/images/edits
POST /v1/images/variations

Maintain full OpenAI API compatibility.

2. Support request fields:

- model
- prompt
- image
- mask
- n
- size
- quality
- style
- response_format
- user

Validate all requests.

3. Integrate into existing architecture only.

Reuse:

- ProviderManager
- ModelRouter
- RequestExecutor
- ProviderAdapter
- HttpClient
- Retry
- Fallback
- Logging
- ApiKeyManager

Do not duplicate logic.

4. Provider capability

Add supportsImages to ProviderAdapter.

Automatically filter unsupported providers.

Return OpenAI-compatible error if unsupported.

5. Provider support

OpenAI-compatible:

- OpenAI
- OpenRouter
- TokenFaucet (if available)
- Gemini/OpenAI-compatible
- NVIDIA
- Databricks

Providers without image capability must be skipped automatically.

6. Response

Support both:

- url
- b64_json

Normalize all provider responses into OpenAI format.

7. Streaming

Images never use streaming.

Reject stream=true.

8. Retry/Fallback

Reuse existing retry/fallback.

9. Tests

Add integration tests for:

- image generation
- edits
- variations
- unsupported provider
- fallback
- validation
- response normalization
- OpenAI compatibility

10. Documentation

Update README with:

- supported endpoints
- examples
- provider support
- limitations

Keep clean architecture.

No duplicated code.

No provider-specific logic outside ProviderAdapter.

Finish only when all tests pass.
```



# 
```
Implement full OpenAI-compatible Embeddings API.

Requirements:

1. Add endpoint:

POST /v1/embeddings

Compatible with OpenAI request/response format.

2. Request

Support:

- input (string)
- input (array)
- model
- encoding_format
- dimensions (if provider supports)

Validate request.

3. Provider Integration

Use existing architecture:

ProviderManager
ModelRouter
RequestExecutor
ProviderAdapter

Do not duplicate provider logic.

ProviderAdapter must expose embeddings capability.

If provider does not support embeddings, return a proper OpenAI-compatible error.

4. Provider Mapping

OpenAI-compatible providers:

- OpenAI
- OpenRouter
- TokenFaucet
- DeepSeek
- NVIDIA
- Gemini (OpenAI compatible)
- Databricks

Anthropic should return "Embeddings not supported".

5. Response

Return OpenAI-compatible JSON:

{
  "object": "list",
  "data": [
    {
      "object": "embedding",
      "embedding": [...],
      "index": 0
    }
  ],
  "model": "...",
  "usage": {
    "prompt_tokens": ...,
    "total_tokens": ...
  }
}

6. Streaming

Embeddings must never use streaming.

7. Retry

Reuse existing retry/fallback system.

8. Logging

Reuse existing logging.

9. Tests

Add integration tests for:

- successful embedding
- multiple inputs
- unsupported provider
- validation errors
- provider fallback
- OpenAI compatibility

10. Documentation

Update README with:

- endpoint
- request examples
- response examples
- provider support table

Do not modify existing architecture.

Keep clean architecture.

Reuse existing abstractions.

Do not duplicate code.

Finish only after all tests pass.
```


# Tahap berikutnya: Integrasi Provider Nyata
```


Tahap berikutnya adalah implementasi Provider Adapter.

Core gateway sudah selesai.

Sekarang fokus membuat adapter provider nyata.

Buat folder:

src/providers/

Setiap provider berada pada file terpisah.

Minimal support:

- OpenAI
- OpenRouter
- TokenFaucet
- Anthropic
- Google Gemini (OpenAI compatible jika tersedia)
- DeepSeek
- Databricks
- NVIDIA

Buat interface provider yang sama untuk seluruh adapter.

Setiap provider bertugas:

- membangun endpoint
- mapping payload jika diperlukan
- mapping response jika diperlukan
- mendeteksi capability provider
- tidak mengandung retry
- tidak mengandung fallback
- tidak mengandung API key rotation

Retry, fallback, logging dan HttpClient tetap berasal dari core gateway.

Tambahkan capability seperti:

supportsChat

supportsResponses

supportsStreaming

supportsEmbeddings

supportsImages

supportsAudio

supportsTools

supportsReasoning

ProviderManager harus otomatis memilih adapter berdasarkan konfigurasi.

Jika provider OpenAI compatible, gunakan GenericOpenAIAdapter.

Provider yang membutuhkan mapping khusus menggunakan adapter sendiri.

Tambahkan integration test menggunakan mock provider.

Tambahkan contoh konfigurasi minimal untuk setiap provider.

Pastikan tidak ada duplikasi kode antar adapter.

Pertahankan clean architecture.



```
# Streaming (Server-Sent Events / SSE)
```

Tahap berikutnya adalah implementasi Streaming (Server-Sent Events / SSE).

Tujuan:

Menambahkan dukungan streaming OpenAI Compatible untuk:

- POST /v1/chat/completions
- POST /v1/responses

Jika request memiliki:

"stream": true

Gateway harus mengembalikan response streaming menggunakan format OpenAI Compatible SSE.

Persyaratan:

- Jangan membuat endpoint baru.
- Reuse seluruh service yang sudah ada.
- Gunakan RequestExecutor yang sama.
- Gunakan HttpClient yang sama.
- Gunakan Retry dan Fallback yang sudah ada.

Flow:

1. Validasi request.
2. Cari provider.
3. Ambil API key.
4. Jika stream=false gunakan flow lama.
5. Jika stream=true gunakan HttpClient stream mode.
6. Forward seluruh SSE event ke client.
7. Jika provider mengirim [DONE], teruskan ke client lalu tutup koneksi.

Tambahkan:

- StreamingResponseAdapter
- StreamParser
- SSEWriter

Logging:

- request id
- provider
- model
- stream started
- stream ended
- latency
- bytes sent

Error:

Jika error terjadi sebelum stream dimulai:
kembalikan JSON OpenAI error.

Jika error terjadi saat stream berlangsung:
kirim event error sesuai format OpenAI lalu tutup stream.

Belum membuat:

- Dashboard
- Database
- Metrics
- Authentication
- Embeddings
- Images
- Audio

Tambahkan integration test menggunakan mock SSE provider.

Tambahkan contoh curl:

curl ... -d '{"stream":true}'

Pastikan implementasi tetap mengikuti clean architecture dan tidak menduplikasi kode non-streaming.




```

# 
```


Tahap berikutnya adalah implementasi OpenAI Responses API.

Tujuan:

Menambahkan endpoint:

POST /v1/responses

dengan tetap menggunakan arsitektur yang sudah ada.

Persyaratan:

- Jangan menduplikasi kode Chat Completions.
- Reuse seluruh komponen yang sudah ada:
  - ProviderManager
  - ModelRouter
  - HttpClient
  - ApiKeyManager
  - Retry
  - Fallback
  - Logger

Responses API harus menjadi adapter di atas service yang sudah ada.

Validasi request:

- model wajib ada
- input wajib ada
- support string maupun array input
- metadata optional
- instructions optional
- temperature optional
- max_output_tokens optional

Flow:

1. Validasi request.
2. Cari provider menggunakan ModelRouter.
3. Ambil API Key dari ApiKeyManager.
4. Kirim request menggunakan HttpClient.
5. Gunakan Retry jika retryable.
6. Gunakan Provider Fallback jika provider gagal.
7. Normalisasi response menjadi format OpenAI Responses API.

Error harus konsisten dengan Chat Completions.

Tambahkan logging:

- request id
- provider
- model
- latency
- retry count
- fallback count

Tambahkan integration test.

Tambahkan contoh curl.

Pastikan seluruh endpoint berikut berbagi service yang sama:

GET /v1/models

POST /v1/chat/completions

POST /v1/responses

Jangan membuat endpoint lain.

Jangan membuat Dashboard.

Jangan membuat Database.

Jangan membuat Authentication.

Jangan membuat Embeddings.

Jangan membuat Images.

Jangan membuat Audio.

Pastikan clean architecture tetap dipertahankan dan tidak ada duplikasi kode.



```

# 
```


Tahap berikutnya adalah membangun API Key Manager.

Jangan membuat Dashboard.

Jangan membuat Responses API.

Jangan membuat Streaming.

Fokus hanya pada sistem manajemen API Key.

Tujuan:

Semua provider dapat memiliki banyak API key.

Contoh:

Provider OpenAI
- key1
- key2
- key3

Provider TokenFaucet
- key1
- key2

Provider OpenRouter
- key1
- key2
- key3
- key4

Buat ApiKeyManager dengan fitur:

- memuat seluruh API key dari konfigurasi
- mendukung banyak API key per provider
- round robin
- next available key
- skip disabled key
- temporary disable key jika gagal
- cooldown key
- re-enable otomatis setelah cooldown selesai

Status API key:

ACTIVE

RATE_LIMITED

UNAUTHORIZED

QUOTA_EXCEEDED

DISABLED

COOLDOWN

Tambahkan service:

getNextKey(providerId)

reportSuccess(providerId,key)

reportFailure(providerId,key,error)

disableKey(providerId,key)

enableKey(providerId,key)

getKeyStatus(providerId)

Tambahkan statistik:

jumlah request

jumlah sukses

jumlah gagal

last used

last error

cooldown until

Belum implementasi retry.

Belum implementasi fallback provider.

Belum implementasi dashboard.

Belum implementasi database.

Semua data masih boleh berada di memory.

Pastikan HttpClient menggunakan ApiKeyManager saat mengambil Authorization Bearer.

Tambahkan unit test.

Tambahkan integration test.

Pastikan clean architecture tetap terjaga.

Setelah selesai jelaskan alur kerja ApiKeyManager dan bagaimana nanti akan digunakan oleh Retry dan Fallback.



```


# Prompt: Build Complete Folder Management for Telegram Drive

Tujuan:
Bangun sistem Folder Management yang modern seperti Google Drive, MEGA, dan Dropbox. Semua file harus dapat dikelompokkan ke dalam folder. Sistem harus siap digunakan oleh file yang berasal dari Website maupun Telegram Downloader Bot.

## Folder List

Buat halaman Folder yang menampilkan:

- Folder Card/Grid
- Folder List View
- Nama Folder
- Jumlah File
- Total Ukuran
- Tanggal Dibuat
- Tanggal Update Terakhir
- Icon Folder

## Folder Actions

Saat menekan tombol (⋮) tampilkan:

- Open
- Rename
- Move
- Share
- Favorite
- Delete

## Create Folder

Tambahkan tombol:

+ New Folder

Saat ditekan tampilkan dialog:

- Nama Folder
- Tombol Cancel
- Tombol Create

Validasi:

- Nama tidak boleh kosong.
- Tidak boleh ada folder dengan nama yang sama pada lokasi yang sama.

## Rename Folder

Dialog Rename Folder.

## Delete Folder

Jika folder kosong:
- Hapus langsung setelah konfirmasi.

Jika folder berisi file:
Tampilkan pilihan:

- Pindahkan file ke folder lain
- Hapus seluruh isi folder
- Batal

## Move File

Saat memilih Move pada file:

Tampilkan Folder Picker.

User dapat memilih folder tujuan.

## Breadcrumb

Contoh:

Home
>
My Files
>
Photos
>
Vacation

Breadcrumb harus dapat ditekan.

## Search Folder

Search Folder secara realtime.

## Sort Folder

- Nama A-Z
- Nama Z-A
- Terbaru
- Terlama

## Favorite Folder

Folder dapat ditandai Favorite.

## Empty State

Jika belum ada folder:

Tampilkan ilustrasi.

Pesan:

"Belum ada folder."

"Tekan New Folder untuk membuat folder pertama."

## Loading

Gunakan Skeleton Loading.

## Error

Jika folder tidak ditemukan tampilkan Folder Not Found.

## Mobile

Gunakan Bottom Sheet.

## Desktop

Gunakan Context Menu.

## API

Semua folder menggunakan API.

Jangan menggunakan data dummy.

Gunakan service/repository yang sudah ada.

## Persiapan Telegram Downloader Bot

Saat integrasi selesai:

- File hasil Telegram Downloader Bot dapat langsung disimpan ke folder pilihan user.
- Upload Website juga dapat memilih folder.
- Upload Telegram Bot juga menggunakan struktur folder yang sama.

Folder menjadi struktur utama penyimpanan Telegram Drive.

## Target

Folder Management harus siap dipakai sebagai fondasi Telegram Drive sehingga seluruh file dari Website maupun Telegram Downloader Bot memiliki struktur penyimpanan yang rapi, modern, responsif, dan mudah dikelola.
# 
```


Tahap berikutnya adalah implementasi endpoint OpenAI Compatible Chat Completions.

Fokus hanya pada endpoint:

POST /v1/chat/completions

Tujuan:

Menerima request OpenAI Compatible, memilih provider menggunakan Model Router, lalu mengirim request menggunakan HttpClient.

Persyaratan:

- Gunakan ProviderManager.
- Gunakan ModelRouter.
- Gunakan HttpClient.
- Jangan hardcode provider.
- Jangan hardcode model.
- Semua provider berasal dari konfigurasi.

Validasi request:

- model wajib ada
- messages wajib ada
- messages harus berupa array
- minimal satu message

Flow:

1. Validasi request.
2. Cari provider berdasarkan model.
3. Bangun payload OpenAI Compatible.
4. Kirim request menggunakan HttpClient.
5. Kembalikan response provider dalam format OpenAI Compatible.

Belum implementasi:

- Streaming
- Retry
- API Key Rotation
- Fallback Provider
- Responses API
- Embeddings
- Images
- Audio

Tambahkan logging:

- model
- provider
- durasi request
- status code
- request id

Normalisasi seluruh error menjadi format OpenAI Compatible.

Tambahkan integration test menggunakan mock provider.

Tambahkan contoh request curl.

Pastikan endpoint dapat bekerja tanpa mengubah arsitektur yang sudah ada.



```

# 
```


Tahap berikutnya adalah membangun HTTP Client.

Jangan membuat endpoint Chat Completions atau Responses API terlebih dahulu.

Fokus hanya membuat lapisan komunikasi ke provider AI.

Buat HttpClient yang reusable untuk semua provider.

Persyaratan:

- Gunakan axios.
- Semua request menggunakan timeout dari konfigurasi provider.
- Semua header dibangun secara otomatis.
- Mendukung Authorization Bearer.
- Mendukung custom header provider.
- Mendukung POST dan GET.
- Mendukung query parameter.
- Mendukung request body JSON.
- Mendukung streaming di tahap berikutnya (siapkan struktur, tetapi belum implementasi).

Tambahkan:

- Request Logger
- Response Logger
- Error Logger

Normalisasi seluruh error menjadi format internal gateway.

Misalnya:

- Timeout
- 401
- 403
- 404
- 429
- 500
- 502
- 503
- Connection refused
- DNS error
- Invalid JSON

Semua error harus menghasilkan objek error internal yang konsisten.

HttpClient harus dapat dipanggil seperti:

sendRequest(provider, endpoint, payload)

Belum perlu melakukan retry.

Belum perlu API Key Rotation.

Belum perlu Streaming.

Belum perlu Chat Completions.

Belum perlu Responses API.

Tambahkan unit test sederhana atau contoh penggunaan HttpClient.

Pastikan desain mengikuti clean architecture sehingga seluruh provider nantinya menggunakan HttpClient yang sama.



```


# 
```
Tahap berikutnya adalah membangun Model Router.

Jangan membuat request HTTP ke provider terlebih dahulu.

Jangan membuat Chat Completions atau Responses API.

Fokus hanya pada sistem routing model.

Buat komponen Model Router yang bertugas memilih provider berdasarkan model yang diminta.

Persyaratan:

- Model Router menggunakan ProviderManager.
- Tidak boleh ada hardcode provider di dalam router.
- Semua konfigurasi model berasal dari file konfigurasi.
- Mendukung banyak provider.
- Mendukung satu model tersedia di lebih dari satu provider.
- Menggunakan priority provider sebagai urutan pemilihan.
- Jika provider disabled, jangan digunakan.
- Jika model tidak ditemukan, kembalikan error yang jelas.

Buat service berikut:

- getProviderForModel(model)
- hasModel(model)
- listModels()
- listProvidersForModel(model)

Tambahkan validasi:

- model kosong
- model tidak dikenal
- provider tidak aktif
- konfigurasi model rusak

Tambahkan unit helper agar mudah dipakai endpoint nanti.

Belum perlu melakukan HTTP request.

Belum perlu API Key Rotation.

Belum perlu Retry.

Belum perlu Streaming.

Belum perlu Responses API.

Belum perlu Chat Completions.

Tambahkan logging sehingga saat aplikasi dijalankan akan tercetak:

- jumlah provider
- jumlah model
- model yang dimiliki masing-masing provider

Pastikan desain mengikuti clean architecture.

Setelah selesai tampilkan struktur folder terbaru dan jelaskan alur kerja Model Router.





```

# 
```

Tahap berikutnya adalah membangun Provider Manager.

Jangan membuat endpoint AI terlebih dahulu.

Fokus hanya pada sistem manajemen provider.

Buat struktur berikut jika belum ada:

config/
providers/

Tambahkan file konfigurasi yang mendukung banyak provider AI.

Setiap provider harus memiliki konfigurasi seperti:

- id
- name
- enabled
- baseURL
- apiKeys
- supportedModels
- priority
- timeout

Buat ProviderManager yang bertugas:

- memuat konfigurasi provider saat aplikasi dijalankan
- memvalidasi konfigurasi
- mengambil provider berdasarkan model
- hanya mengembalikan provider yang enabled
- mendukung lebih dari satu provider
- mudah ditambah provider baru tanpa mengubah source code

Jangan hardcode provider di dalam kode.

Semua provider harus dibaca dari file konfigurasi.

Tambahkan service untuk:

- listProviders()
- getProviderByModel(model)
- getEnabledProviders()

Tambahkan logging jika konfigurasi provider tidak valid.

Belum perlu membuat request HTTP ke provider.

Belum perlu membuat Chat Completions.

Belum perlu membuat Responses API.

Belum perlu API Key Rotation.

Belum perlu Streaming.

Setelah selesai tampilkan struktur folder terbaru dan jelaskan desain arsitektur Provider Manager.




```

# 
```

Audit seluruh repository dan perbaiki struktur proyek tanpa mengubah fungsi aplikasi.

Terjadi kesalahan saat pembuatan file. Beberapa teks dari README atau contoh output terminal salah dibuat menjadi file di root project.

Lakukan langkah berikut:

1. Scan seluruh root project.
2. Identifikasi file yang bukan bagian dari proyek, misalnya file yang namanya berupa kalimat, output terminal, atau contoh dokumentasi.
3. Hapus file-file tersebut dengan aman.
4. Pindahkan informasi yang masih berguna ke README.md atau .env.example sesuai fungsinya.
5. Pastikan root project hanya berisi file dan folder yang memang diperlukan.

Root project setelah dirapikan minimal berisi:

.git
src/
config/
logs/
package.json
package-lock.json (jika ada)
README.md
.gitignore
.env.example

Pastikan folder src tetap menggunakan clean architecture.

Jangan menghapus source code yang valid.

Jangan mengubah endpoint, logika aplikasi, atau struktur source code yang sudah benar.

Setelah selesai:

- tampilkan daftar file yang dihapus,
- jelaskan alasannya,
- tampilkan struktur proyek terbaru menggunakan `tree -L 3`,
- pastikan proyek masih dapat dijalankan tanpa error.

Jangan membuat fitur baru. Fokus hanya membersihkan dan merapikan struktur repository.




```
