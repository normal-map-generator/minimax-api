# MiniMax API: a practical guide

*Unofficial community guide for MiniMax API. Not affiliated with MiniMax. All trademarks belong to their owners.*

The MiniMax API is the developer platform for MiniMax's model families: the M-series text models (MiniMax M3, M2.7, M2.5 and older), the H3 video models, Speech 2.8 for text-to-speech, and Music 3.0. You can call the models directly through MiniMax's own platform or through OpenRouter, which lists 14 MiniMax models behind its unified API. This guide is written from the public pages of minimax.io, the platform documentation and the OpenRouter provider page, and covers the model line-up, the two access routes, the prices that are actually published, and the details that matter before you commit to one route.

> If what you need is image, video and audio generation behind one endpoint with a Python SDK, [try Synexa - hosted FLUX, video and audio models, pay per run](https://synexa.ai?utm_source=github&utm_medium=ugc&utm_campaign=minimax-api&utm_content=readme-top&utm_term=tier-r). Comparison below.

## What it is

On the text side, the current flagship is MiniMax M3, described as a frontier coding and agentic model with a 1M-token context built on MiniMax Sparse Attention (MSA), a sparse attention design that replaces full attention with KV-block selection to cut per-token compute at long context. OpenRouter's listing adds that M3 accepts text, image and video inputs with text output. Below it sit MiniMax M2.7 and M2.7-highspeed, then the historical M2.5, M2.5-highspeed, M2.1, M2.1-highspeed and M2. The highspeed variants are documented as the same quality as the base model at much higher throughput. The platform docs file the text API under an Anthropic-compatible API reference page, which is useful if you already have Anthropic-style client code.

On the media side, MiniMax H3 is an open-weight, general-purpose omni-modal video model supporting text-to-video, image-to-video, first-and-last-frame and multimodal reference input, at 768P or 2K and 4 to 15 seconds. MiniMax H3 Max is a faster variant post-trained by fal.ai on top of H3, limited to text and image (first or last frame) input at 480P or 768P, 5 to 15 seconds. Older video models are Hailuo 2.3, Hailuo 2.3 Fast and Hailuo 02 (native 1080p). Speech 2.8 comes in HD and Turbo tiers and accepts arbitrary MiniMax voice IDs. Music 3.0 is an open-weights music model. The docs also list image models and an MCP guide and a local-run and self-hosting guide.

## Getting started

There are two routes.

**Direct (MiniMax platform)**

1. Go to [platform.minimax.io](https://platform.minimax.io/contact-us) for the international platform, or [platform.minimax.cn](https://platform.minimax.cn/docs) for the Chinese-language docs; the .cn docs are the more complete set that the crawl landed on.
2. Decide between pay-as-you-go and the [Token Plan](https://platform.minimax.io/subscribe/token-plan) subscription.
3. Read the [API overview](https://platform.minimax.cn/docs/api-reference/api-overview) and the [text API reference](https://platform.minimax.cn/docs/api-reference/text-anthropic-api) for chat, or the [video generation v2 reference](https://platform.minimax.cn/docs/api-reference/video-generation-v2-create) for H3.
4. Create a key in the console and keep it in an environment variable.

**Via OpenRouter**

1. Open the [MiniMax provider page on OpenRouter](https://openrouter.ai/minimax) and pick a model slug, for example `minimax/minimax-m3` or `minimax/hailuo-3`.
2. Follow the [OpenRouter quickstart](https://openrouter.ai/docs/quickstart) with an OpenRouter key. One key then covers every provider OpenRouter hosts.

## Pricing and limits

MiniMax's own [pricing overview](https://platform.minimax.cn/docs/pricing/overview) is the source of truth for direct access. The OpenRouter provider page publishes these numbers, which are OpenRouter's prices, not necessarily MiniMax's:

| Model (OpenRouter) | Price shown |
| --- | --- |
| MiniMax H3 Max | from $0.05 / second |
| MiniMax H3 | from $0.13 / second |
| Speech 2.8 HD | $100 / M characters |
| Speech 2.8 Turbo | $60 / M characters |

Text model prices are on the respective OpenRouter model pages and on the MiniMax pricing page. Rate limits are not on the pages covered here; check the [API FAQ](https://platform.minimax.cn/docs/faq/about-apis).

## Practical notes

- Two platforms, two docs sets. platform.minimax.io is the international console; platform.minimax.cn hosts the Chinese-language docs, which carry the full model table. Do not assume a key from one works on the other.
- The highspeed variants (M2.7-highspeed, M2.5-highspeed) are the same model at higher throughput. For agent loops with many short calls, they are usually the right default.
- H3 and H3 Max are not interchangeable: H3 has 2K output and reference-guided editing; H3 Max is faster, tops out at 768P and drops the multimodal reference mode.
- Video is priced per second of output. A 15-second H3 clip at $0.13 / s on OpenRouter is about $1.95 before retries.
- Speech is priced per million characters, so long scripts are cheap but many short calls still count characters, not requests.
- H3 and Music 3.0 are open weights and the docs have a self-hosting guide, so you can prototype hosted and move to your own GPUs later.

## Comparison

| | MiniMax API (direct) | OpenRouter | Synexa |
| --- | --- | --- | --- |
| Models | M3, M2.7, M2.5, H3, H3 Max, Speech 2.8, Music 3.0, image | 14 MiniMax models plus other providers | FLUX, video and audio models |
| Billing | Pay-as-you-go or Token Plan | Per-token and per-second, one key | Pay per run |
| Client | Anthropic-compatible text API, video v2 API | OpenRouter unified API | REST endpoint, Python SDK |
| Video pricing example | See MiniMax pricing page | H3 from $0.13 / s | See Synexa pricing |

## FAQ

**Which MiniMax model should I start with for coding?** MiniMax M3 is the current flagship for coding and agentic work with 1M context; M2.7-highspeed is the cheaper high-throughput option.

**Is the MiniMax API compatible with existing SDKs?** The text API is documented as Anthropic-compatible on the platform, and OpenRouter exposes the models through its unified API.

**Can I generate video?** Yes, via H3 (768P or 2K, 4 to 15 s) or H3 Max (480P or 768P, 5 to 15 s), and the older Hailuo models.

**Is any of it open weights?** H3 and Music 3.0 are described as open-weight, and the docs include a local-run and self-hosting guide.

**Direct or OpenRouter?** Direct if you want MiniMax-specific features and pricing; OpenRouter if you want one key across providers and do not mind its markup.

## Try Synexa

MiniMax is a strong choice when you need its text models. When the job is image, video or audio generation in a backend, [Synexa](https://synexa.ai?utm_source=github&utm_medium=ugc&utm_campaign=minimax-api&utm_content=readme-top&utm_term=tier-r) offers one REST endpoint and a Python SDK across FLUX, video and audio models, billed per run, so you do not manage separate consoles for each media type.

[Try Synexa - one API for FLUX, video and audio models](https://synexa.ai?utm_source=github&utm_medium=ugc&utm_campaign=minimax-api&utm_content=readme-top&utm_term=tier-r)


_Last reviewed: 2026-09-22_
