# Mantella-Local for [Mantella](https://github.com/art-from-the-machine/Mantella/releases) v0.14
Status: ALPHA 
- Remaking for `Mantella 0.14`. (older versions exist in tags for earlier versions of mantella)

### Descriptioon
**Mantella-LocalLlm v0.14.x** is a drop-in augmentation of Mantella v0.14, targeting Fallout 4 only, with local-only LLM and TTS — no cloud APIs, no external model servers required.

### Features
- **LLM**: Replaced cloud/Ollama/LM Studio dependency with `llama-server.exe` (Vulkan binary), launched automatically as a subprocess. Any Qwen3-era GGUF model is loaded directly; Mantella's existing OpenAI-compatible client talks to it at `127.0.0.1:8080/v1` unchanged.
- **TTS**: Replaced xVASynth/Piper/XTTS with Kokoro TTS (~326 MB model, ~10 distinct English voices). FO4 voice types are mapped to Kokoro speaker IDs via a generated CSV. Two new files added to `src/tts/`: `kokoro_tts.py` and entries in the modified `tts_definitions.py` / `tts_factory.py`.
- **Launcher**: Replaces the old batch+Python launcher with `Mantella-LocalLlm.bat` and `src/local_inference.py` — an interactive menu for GGUF model selection, context size, CPU thread count, Vulkan GPU layer offload, and default voice selection.
- **Installer**: New `installer.py` handles venv creation, PyTorch CPU, Kokoro, llama-server download, and voice mapping CSV generation in one step.
- **Skyrim**: Not supported in this variant (Fallout 4 only).
- **Unchanged**: All Mantella core logic — HTTP server, conversation flow, STT, memory/summaries, actions, character CSV data, Gradio config UI — untouched.

### Preview
- The `Pre-Launch Configuration` options of `Mantella-Launcher.Bat`...
```
========================================================================================================================
    Pre-Launcher Configuration
========================================================================================================================









    1. Mantella-Local-Launcher

    2. Just Run Mantella

    3. Installer-Setup









========================================================================================================================
Selection; Menu Options = 1-3, Exit Batch = X:

```
- The `Mantella-Local-Launcher` provides simplified, configuration and optimization...
```
=======================================================================================================================
    MaNTella-Local-Launcher
-----------------------------------------------------------------------------------------------------------------------

    1. Select Game Used
        (Fallout4VR)
    2. Prompt Optimization
        (Regular)
    3. Model Token Count
        (4096)
    4. Voice Input Cutoff
        (2s)
    5. Launch WebUI Config
        (False)

-----------------------------------------------------------------------------------------------------------------------

    Fallout4VR_Path:
        C:\Games\Steam\steamapps\common\Fallout4VR
    xVAsynth Path:
        C:\Games\Steam\steamapps\common\xVASynth\xVASynth.exe
    LLM API:
        OpenRouter
    Model Loaded:
        uncensored-frank-llama-3-8b.q6_k

=======================================================================================================================
Selection, Program Options = 1-5, Refresh Display = R, Begin Mantella/xVASynth/Fallout4 = B, Exit and Save = X:

```
- The `Installer-Setup`, to install requirements compitently...
```
========================================================================================================================
    Installer-Setup
========================================================================================================================


    1. Install `.\requirements.txt`

    2. First Run Setup (Run Once)

    3. Fix Ollama Non-Cuda/Non-Rocm GPU

    4. Upgrade Pip Version

    5. Check Dependency Conflicts

    6. File Integrity Test

------------------------------------------------------------------------------------------------------------------------

    Python Path:
        C:\Users\Mastar\AppData\Local\Programs\Python\Python311\python.exe

    Config File:
        F:\Documents\My Games\Mantella\config.ini


========================================================================================================================
Selection; Menu Options = 1-6, Back to Main = B:

```

## Requirements
1. **Powerful Computer**: Running Fallout 4/Skyrim and interference on models can be intensive.
4. **Operating System**: Compatible with Windows 7 through Windows 11; administrative privileges may be needed.
2. **Python Environment**: "Mantella-Local-Launcher requires Python 3.11", but Ensure that it was installed to the default directory or the default directory for all users.
3. **Python Libraries**: The same libraries as Mantella listed in `requirements.txt`.
3. **LM Studio or Ollama**: Obtain model folder/name and foolproofs api config, Mantella-Local-Launcer will not work on non-local model host services.
4. **Language Model**: Search [HuggingFace](https://huggingface.co) for a `.Gguf` model such as, llama 3 or llama 3.1, 4.xGB VRam Free for Q3 or 5.xGB VRam free for Q4, and for shared GPU this is in addition to whatever Fallout 4 consumes of your Vram.
5. **Mantella Installation**: It was made for the v12 version on [Github](https://github.com/art-from-the-machine/Mantella/releases), other versions will work likely unless they change the key names. The files from Nexus are likely always going to be compatible, heres the link for, [Fallout 4](https://www.nexusmods.com/fallout4/mods/79747) and [Skyrim](https://www.nexusmods.com/skyrimspecialedition/mods/98631).
5. **xVASynth Installation**: Must be installed and correctly configured in the specified directory.

# Usage / Install
1. Ensure the, [Fallout 4 Mantella Mod](https://www.nexusmods.com/fallout4/mods/79747) or [Skyrim SE/AE Mantella Mod](https://www.nexusmods.com/skyrimspecialedition/mods/98631), is installed for from the Nexus mods site to relevantly, fallout 4 or skyrim SE/AEm, and ensure to skim through the relating guides, this will require install [Mantella v12](https://github.com/art-from-the-machine/Mantella/releases/tag/v0.12) to a suitable directory.
2. Download the most recent version of [Mantella-Local-Launcher](https://github.com/wiseman-timelord/Mantella-Local-Launcher/releases), and drag the, file(S) and folder(s), from the zip into the main Mantella folder.
3. Run `Mantella-Launcher.Bat`, and ensure to select `3. Installer-Setup`, to assist in the processes of installing requirements compitently, or if you had issues with the instructions in the guide, this may help you with certain parts of the process. Also ensure that you have ran `First Run Setup`, and filled in the `Mantella Webui Configuration` appropriately in the browser that will pop up. 
<br>3a. For Ollama, if, you are getting Torch errors and/or you do not have suitable GPU, then install `Torch[Cpu]`, to ensure Ollama uses CPU libraries instead. 
<br>3b. For LM Studio, to Quote "In config.ini, set llm_api = http://localhost:1234/v1/. This is the URL that LM Studio is running your model on.".
<br>3c. For Other methods of hosting models, they are detailed in the Official guides, I cannot confirm they will work, they probably wont. 
5. Go back to the main menu in `Mantella-Launcher.Bat`, select `1. Mantella-Local-Launcher` for the Game Launcher, therein, configure your preferences from options 1-4, and when done then select "B" for begin. 
- If for some reasoning there are issues with mantella or mantella crashes, then you can at any point, close the mantella window, and run `Mantella-Launcher.Bat`, and instead select `2. Just Run Mantella`, to re-start mantella, which I found works to fix issues of mantella having issues where the User is somehow locked into a conversation, and unable to start new conversations.
  
## Notes
- all options for Optimization are shown...
```
Default: max_tokens = 250, max_response_sentences = 999, temperature = 1
Faster: max_tokens = 100, max_response_sentences = 1, temperature = 0.4
Regular: max_tokens = 150, max_response_sentences = 2, temperature = 0.5
Quality: max_tokens = 200, max_response_sentences = 3, temperature = 0.6
```
- The System Prompt, on your Model Server (LM Studio) or that you used to install the model (Ollama), should be something like "You are, to requirement, an AI, role-player and text processor, you will be instructed to either be, roleplaying with the Player/NPCs."...
<img src="./media/lm_studio_prompt_mantella.jpg" align="center" alt="no image" width="313" height="315">.
- If you are using one graphics card for, game and model, then ensure to use nVidia/Amd control panels to monitor free ram when fallout 4 is running, the amount of free space with small additional amount for runnings, determines what size of model you will be using.
- a Llama 3 Q3_m model with fallout 4 dlc & ~300 mods including PhyOp performance texture pack, utilizes all of the 8GB on a single card, if you want to use =>Q4 and/or hd textures, then 12GB GPU is suggested, or maybe you have heard of things like PhyOp AI Enhanced Textures.
- There is also my mod collection specifically designed for Mantella, [MWO2 - Mantella Wasteland Operator](https://www.youtube.com/watch?v=ayZmcOqZ8iY), in that, initially the mods from MWO1 that had issues with Mantella were removed.
- If you are looking for the v11.4 files, there is a specific, setup-install and run-mantella, batches, that are separate to the launcher, the latest versions of those are the files I intended for Mantella team to incorporate into Mantella Mod, as I could not push batches to main...which explains why so many programs on github dont have simple batches to take care of basic issues for windows users.  

# Development
- Project restart, updating local launcher to become Mantalla-Local, this will be a Llama.cpp built-in streamlined/enhanced version, specific to Qwen 3 variants in GGUF, though other models may be added later, but not newer than Qwen 3 level/dated models (because otherwise this requires compiling the wheel, and other complication). 
- Skyrim does not include an equivalent to "FO4_Voice_folder_XVASynth_matches.csv", so the project will not be supporting Skyrim, this project will be for Fallout4NG_984 ONLY ONLY. `FO4_Voice_folder_XVASynth_matches.csv` will be used in the project in order to enumerate which accent to use with CorquiTTS. This will be highly inaccurate to begin with, but later be improved through web research into accent/unique voice types in fallout 4, and including some kind of lookup table, in order to have something similar to the original. While this is not optimal, it will  greatly simplify the installation of tts to just install alongside the python requirements. ???
- For now the plan is to make a custom Mantella Experience tuned to local models, with streamlined install/operation, that can be used with a NG compilation I am working on for `MWO6: Mantella Wasteland Operator`, with pre-configuration via the mantella v0.14 pre-configuration popup GUI, this should be configurable via option ion the batch menu.


## Disclaimer
This software is subject to the terms in License.Txt, covering usage, distribution, and modifications. For full details on your rights and obligations, refer to License.Txt.
