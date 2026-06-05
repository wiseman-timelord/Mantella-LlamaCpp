# Mantella-LlamaCpp for [Mantella](https://github.com/art-from-the-machine/Mantella/releases) v0.14
Status: ALPHA 
- Remaking for `Mantella 0.14`. (older versions exist in tags for earlier versions of mantella)

### Descriptioon
**Mantella-LocalLlm v0.14.x** is a drop-in augmentation of Mantella v0.14, targeting Fallout 4 only, with local-only LLM and TTS — no cloud APIs, no external model servers required. While older versions exist in the tags, that are more true to mantella, the version for v0.14 will be highly experimental, where the voices are done compoletely differently, and may not sound much like the original. The concept is to enable a simpler version of TTS, enabling requirement of only, Mantella mod AND Mantella v0.14 and then dropping in the scripts from Manatella-Local. So no requirements for XVASynth, Model Server, etc, instead we are using Kokoro, which will also result in much faster speech generation, and Llama.Cpp for integrated model handling.

### Features
- **LLM**: Replaced cloud/Ollama/LM Studio dependency with `llama-server.exe` (Vulkan binary), launched automatically as a subprocess. Any Qwen3-era GGUF model is loaded directly; Mantella's existing OpenAI-compatible client talks to it at `127.0.0.1:8080/v1` unchanged.
- **TTS**: Replaced xVASynth/Piper/XTTS with Kokoro TTS (~326 MB model, ~10 distinct English voices). FO4 voice types are mapped to Kokoro speaker IDs via a generated CSV. Two new files added to `src/tts/`: `kokoro_tts.py` and entries in the modified `tts_definitions.py` / `tts_factory.py`.
- **Launcher**: Replaces the old batch+Python launcher with `Mantella-LocalLlm.bat` and `src/local_inference.py` — an interactive menu for GGUF model selection, context size, CPU thread count, Vulkan GPU layer offload, and default voice selection.
- **Installer**: New `installer.py` handles venv creation, PyTorch CPU, Kokoro, llama-server download, and voice mapping CSV generation in one step.
- **Skyrim**: Not supported in this variant (Fallout 4 only).
- **Unchanged**: All Mantella core logic — HTTP server, conversation flow, STT, memory/summaries, actions, character CSV data, Gradio config UI — untouched.

### Preview
- T.B.A....
```
    T.B.A.
```

### Requirements
- **Powerful Computer**: Running Fallout 4 and interference on models can be intensive.
- **Operating System**: T.B.A. 
- **Python Environment**: T.B.A. - It will use a venv.
- **Language Model**: Testing with some form of Qwen v3.
- **Mantella**: It was made for the v0.14 version on [Github](https://github.com/art-from-the-machine/Mantella/releases), other versions will likely not work. The Mantella Mod for Fallout 4 is [here](https://www.nexusmods.com/fallout4/mods/79747).

### Usage / Install
1. Download the most recent version of [Mantella-LocalLlm](https://github.com/wiseman-timelord/Mantella-LocalLlm-Launcher/releases), and drag the, file(S) and folder(s), from the zip into the main Mantella folder.
- Under Construction
  
### Notes
- all options for Optimization are shown...
```
Default: max_tokens = 250, max_response_sentences = 999, temperature = 1
Faster: max_tokens = 100, max_response_sentences = 1, temperature = 0.4
Regular: max_tokens = 150, max_response_sentences = 2, temperature = 0.5
Quality: max_tokens = 200, max_response_sentences = 3, temperature = 0.6
```
- Mantealla-Local v0.14+ has GPU selection, so if you have multiple GPUs ensure to correctly configure your second GPU (if thats optimal) in the configuration.
- Mods like PhyOp performance texture pack, they will allow one to reduce texture memory footprint, enabling loading of models to GPU larger VRAM spaces on single card.

### Early Development
- Project restart, updating local launcher to become Mantalla-Local, this will be a Llama.cpp built-in streamlined/enhanced version, specific to Qwen 3 variants in GGUF, though other models may be added later, but not newer than Qwen 3 level/dated models (because otherwise this requires compiling the wheel, and other complication). 
- FO4_Voice_folder_XVASynth_matches.csv`, but later be improved through web research into accent/unique voice types in fallout 4, and including some kind of lookup table, in order to have something similar to the original. While this is not optimal, it will  greatly simplify the installation of tts to just install alongside the python requirements. ???
- For now the plan is to make a custom Mantella Experience tuned to local models, with streamlined install/operation, that can be used with a NG compilation I am working on for `MWO6: Mantella Wasteland Operator`, with pre-configuration via the mantella v0.14 pre-configuration popup GUI, this should be configurable via option ion the batch menu.

### Phase 1 Structure (dropin scripts plan)
```
.\Mantella-LocalLlm.bat - Batch launcher 
.\main.py - Replacement entry point 
.\installer.py - Installer 
.\requirements.txt - Replacement requirements 
.\src\local_inference.py - Interactive launcher menu 
.\src\tts/kokoro_tts.py - New TTS backend 
.\src\tts\tts_definitions.py - Modified (adds KOKORO enum) 
.\src\tts\tts_factory.py - Modified (registers KokoroTTS) 
```

### Phase 2 pre-install Structure
```
| New path | Replaces | What it does |
| `launcher.py` | `main.py` | Entry point, starts HTTP server + UI |
| `installer.py` | `installer.py` | STANDALONG Installer script |
| `scripts/inference.py` | `src/local_inference.py` | Interactive launcher menu |
| `scripts/utility.py` | `src/utils.py` | Core utilities, **telemetry stripped out** |
| `scripts/kokoro.py` | `src/tts/kokoro_tts.py` | Kokoro TTS backend |
| `scripts/speech.py` | `src/tts/tts_definitions.py` | TTS enum (KOKORO added) |
| `scripts/displays.py` | `src/tts/tts_factory.py` | TTS factory (registers Kokoro) |
| `scripts/profiler.py` | `src/model_profile_manager.py` | Model profile manager |
| `scripts/selector.py` | `src/random_llm_selector.py` | Random LLM selector |
| `scripts/manager.py` | `src/game_manager.py` | Game state manager |
| `scripts/biotempl.py` | `src/bio_template_manager.py` | Bio template manager |
| `scripts/charter.py` | `src/character_manager.py` | Character dataclass |
| `scripts/charlist.py` | `src/characters_manager.py` | Characters collection |
| `scripts/colors.py` | `src/color_formatter.py` | Logging colour formatter |
| `scripts/cfgedit.py` | `src/config_editor.py` | Tkinter config editor |
| `scripts/startapp.py` | `src/setup.py` | MantellaSetup (telemetry stubbed) |
| `data/requirements.txt` | `requirements.txt` | Updated requirements |
```

### Late Development
1. Streamlining scripts, because the dropin files would have removed requirement for a good number of files/functions, also making specific to fallout 4 and built-in TTS, etc, all adds up to less functions/scripts.

### Nice to haves
2. Put back in, TTS/Voice and/or local LLm, options from Mantella v0.14, then rename to "Mantella-Local". 

### Disclaimer
This software is subject to the terms in License.Txt, covering usage, distribution, and modifications. For full details on your rights and obligations, refer to License.Txt.
