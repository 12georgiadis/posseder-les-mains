# Field guide: building an AI creation infrastructure for a residency or studio

*A generic blueprint, drawn from two real setups built for CNC-supported creative residencies. The figures are 2026 orders of magnitude: pin them down again before any decision. Companion to the [Manifesto](MANIFESTO.md).*

## 1. The principles that decide everything

1. **Local first**: open models on a machine you own cover most of image, video and text, for free and with no imposed filter.
2. **Rent by the hour, shared**: for overflow, GPUs rented by the hour in datacenters you choose. A released instance serves someone else: nothing runs idle. Never a monthly dedicated server for occasional use.
3. **Proprietary in conscious doses**: closed models are there to compare against, and to cover what open ones can't do yet, with a named cap per person.
4. **Everything gets measured or questioned**: see the carbon method (§6).
5. **Daily use must be trivial**: open up in the morning, find everything where you left it. If you can feel the infrastructure, it has failed.

## 2. The architecture, in four blocks

**Block 1, the machine you own.** A tower with a recent large-memory GPU (in 2026: NVIDIA RTX 5090 32 GB, 16 CPU cores, 96 GB RAM, two NVMe SSDs). It handles day-to-day generation, the training of personal styles, and the real-time uses (assisted painting, where latency rules out anything remote). System kept as a versioned **disk image**: configured once, backed up, restorable identically in minutes.

**Block 2, the participants' own computers (BYOD).** Apple Silicon Mac or laptop with an NVIDIA GPU, both work. They carry the local language models (Ollama, LM Studio), the interfaces, and plug into the rest over the network. Nobody needs a powerful machine: the heavy compute lives elsewhere.

**Block 3, the rented server (the "homemade cloud").** A by-the-hour GPU server at a transparent host, with a full graphical desktop streamed to you: windows, file browser, web browser. Linux does not mean command line; the terminal exists only for the admin. Three mechanisms make it livable:
- the **golden image**: the system configured once (tools, models, paths), snapshotted, versioned (v1, v2, v3); install freely day to day, freeze after each validated change; if it breaks, restore the last good version;
- the **persistent volume**: data lives outside the system; shutting the machine down costs only the storage; bringing it back takes two minutes and everything is there;
- the **data/system separation**: a restore never touches anyone's belongings.

Geography: the interactive server (streamed desktop) close to the users (from France, a French datacenter: 10 to 15 ms); batch compute (generation queues, training runs) can go further away, wherever the electricity is cleanest (Norwegian hydro, Icelandic geothermal, 25 to 65 ms that batch jobs don't care about).

**Block 4, shared storage.** A NAS (36 TB usable in RAID), one personal folder per participant with a quota, mounted everywhere, including in each computer's Finder or Explorer (drag and drop, never a terminal). One physical constraint to respect: models (5 to 30 GB each) live next to the GPU that loads them; the NAS holds the work, not the active checkpoints.

**The golden rule, taught on day one**: what's yours lives in your folder, present everywhere; everything else can be reset without ever touching you.

## 3. The network

- **Prerequisite**: 1 Gbit/s symmetric minimum for a group; 8 Gbit/s symmetric recommended (in France, available as a shared business plan, sometimes without commitment; dedicated 10 Gbit/s guaranteed fiber means lead times and contracts incompatible with a short run).
- **Your own router**, independent of the venue, with an encrypted private mesh (Tailscale) for remote access.
- A multi-gigabit switch; if overflow runs on a rented server, the heavy downloads happen on the datacenter side and spare the local fiber.

## 4. The software stack (open source by default)

| Use | Tools |
|---|---|
| Generation (engine) | ComfyUI in server mode, interface in the browser |
| Simple interface | SwarmUI (a form on top of the same ComfyUI, full graph for advanced users) |
| Painting with AI | Krita + the krita-ai-diffusion plugin (hooks into the server's ComfyUI) |
| Editing | DaVinci Resolve (free, Studio optional) or the 100% open-source chain: Kdenlive, Audacity/Ardour, ffmpeg |
| Open-source all-in-one (lab) | Blender + Pallaidium (generation inside the timeline) |
| Local language models | Ollama, LM Studio, OpenWebUI |
| Agents | an existing harness (Hermes Desktop and the like, with your own key) or your own, built with Claude Code |
| Installation | Pinokio (one click, plus sharing a local app via link or QR code: a laptop becomes a mini platform for the group) |

One caution: the software's license does not cover the license of the model weights. For works headed for distribution, keep a list of models whose weights are genuinely free.

## 5. Credits: named, capped, visible

- **One pool per use**: generation (ComfyUI credits cover remote model calls from the interface itself, with the cost displayed on each node, including locally), text (OpenRouter, which also feeds the agentic harnesses), orchestration (one serious agentic subscription per person, facilitators included: they must experiment at the same level as the participants).
- **A quota per person, alerts, a global cap.** The cost visible on every gesture is a teaching tool, not a punishment.
- Verified in 2026: on identical closed models, the aggregators pass through the same provider price to the cent. Price will not separate the platforms; freedom (custom tools, filters), ecosystem and carbon transparency will.

## 6. The carbon method: measure, calculate, estimate, question

| Level | Scope | Method |
|---|---|---|
| Measured | local machines | direct readout (`nvidia-smi`, CodeCarbon, watt meter) |
| Calculated | transparent rented servers | hours × power draw × published PUE × local carbon intensity (national grid sources) |
| Estimated | opaque APIs | estimation tools (EcoLogits for LLMs), GPU-hour proxies, published assumptions, ranges |
| Questioned | the providers | formal request: location, energy mix, PUE, CO2e per GPU-hour. Silence is a data point. |

Useful orders of magnitude: a month of intensive local production on one workstation burns about 200 kWh, which means a few kilos of CO2e on a deeply decarbonized grid, and ten times more on an average American mix. Where the electricity comes from outweighs everything else. And the manifesto's reminder stands: low-carbon does not mean clean.

## 7. The prerequisites (demand them early)

**From participants**: a decent laptop (Apple Silicon 16 GB or a PC with an 8 GB VRAM RTX), 100 GB free, the free base stack installed before arrival, no paid license required.

**From the host venue**: floor plans before any decisions get made; a power circuit for the workstation (about 1 kW under load); the fiber eligibility test at the exact address; a ventilated room (a workstation runs hot) that locks; evening and weekend access; a video projector.

**On the human side, prerequisite number one**: a dedicated, paid person to build, install and maintain (machine, images, server, network), distinct from whoever guides the creative uses. Without that role, buy the turnkey services and accept their limits. And decide **before purchase** who inherits the hardware afterward.

## 8. The budget, in blocks that cut cleanly

2026 orders of magnitude for a group of ten or so over four to six weeks: the workstation (7 to 8 k€, half of it GPU and RAM, both volatile), the shared equipment (screens, NAS, network: 3 to 4 k€), the named subscriptions and credits (2.5 to 3.5 k€), GPU rental (0.3 to 0.9 k€ for 100 to 300 hours), the connection (negligible on a shared plan), setup and maintenance (2 to 3 k€). Realistic total: **15 to 22 k€** depending on where you set the dials. The two classic mistakes: squeezing the credits (the setup exists but frustrates) and forgetting the human role (the setup dies at the first breakdown).

## 9. The lessons that cost dearly when ignored

1. RAM and GPUs are volatile: lock the buying window, keep a margin.
2. Install everything months in advance and run it in, then **do a full update pass 30 days before the start**: in AI time, six months is an eternity.
3. Never put data inside the system image.
4. Do not confuse cheap spot rental prices (interruptible marketplaces, unknown carbon) with real cost.
5. API content filters apply the same everywhere: if creative freedom matters, it is won in the local open stack, not in the choice of aggregator.
6. Write to the providers. The answers (and the silences) change the choices.

*Ismaël Joffroy Chandoutis. Text under CC BY-SA 4.0: reuse, adapt, credit.*