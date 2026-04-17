# COMING SOON ...


# **PoLaPro: Political Language Processing**


Our project aims to create a **Retrieval-Augmented Generation (RAG)** system that allows users to interact with structured and contextualized political knowledge about Germany through a conversational AI interface. The idea is to combine large language models with curated political text data from various sources — such as party programs, parliamentary speeches, and governmental press releases — to make political information transparent, interactive, and easily understandable.

Our **motivation** stems from the observation that political data in Germany is publicly available but highly fragmented and often difficult to navigate. Citizens, students, and journalists frequently need to search through long documents or datasets to find relevant facts or statements. We want to simplify this process by offering a system that can retrieve, interpret, and summarize political content on demand.

A **potential user scenario** could be a civic education organization or a news outlet that integrates our chatbot to enable users to ask questions like “What was the position of the Green Party on nuclear energy in 2023?” or “How did the Bundestag debate evolve on migration policies?” Our system would retrieve relevant sources, cite them, and provide a concise, factual response.


### **The main questions we want to explore include:**

* How can heterogeneous political text data be unified and structured efficiently?
* How can we ensure factual correctness and transparency when using LLMs for political topics?
* What are the best retrieval and ranking strategies for political knowledge bases?


### **Data**

We plan to collect textual political data from freely available public sources, including:
* Bundestag Plenary Protocols (official transcripts of parliamentary sessions): https://www.bundestag.de/protokolle
* Party Programs and Manifestos (e.g., from official party websites or archives like https://www.wahlprogramme.de)
* Government Press Releases from https://www.bundesregierung.de
* News and fact databases (e.g., open government datasets or open data portals)
All data will be stored in a structured format and automatically kept up to date to support semantic search and retrieval within the RAG system.


### **Team:**

* Micha Walter: waltermi96328@th-nuernberg.de
* Nico Simon: simonni96318@th-nuernberg.de
* Jan Tischner: tischnerja95752@th-nuernberg.de


---


# **Predecessor project**

In the [preceding project](https://github.com/printjan/NLP-Projekt_Political-Language-Processing), we (different team) analyzed political speeches in the German Bundestag using Natural Language Processing to gain insights into the dynamics and structure of political communication. Our motivation was to reveal and critically question hidden patterns and relationships in political discourse.

The scenario focused on a comprehensive analysis of the plenary protocols of the German Bundestag. As a hypothetical client, we considered, for example, political research institutes, media houses, or parties that are interested in detailed analyses and comprehensible visualizations of political communication.

Specifically, we aimed to answer the following questions:
* Which party speaks how much, and how fair is the allocation of speaking time?
* Is it possible to automatically identify the party affiliation or even the identity of a speaker based solely on the speech transcript?
* Which main topics dominate the political discourse, and how do the parties differ in this regard?


## **Predecessor project: Data Acquisition and Preparation**

- Crawling all publicly available plenary transcripts of the 19th and 20th legislative periods of the German Bundestag in JSON format via the Bundestag’s official API: https://www.bundestag.de/services/opendata
- Before crawling, verify the website’s Terms of Use and robots.txt to ensure the data retrieval is permitted
- Note: For the 20th period, a small number of protocols are missing.
- For downloading, an API client was created and used, it is checked in under the [bundestagsapi](https://github.com/printjan/NLP-Projekt_Political-Language-Processing/tree/main/dataGeneration/bundestagsapi) folder. However since the data only has to be downloaded once, this is not included in any notebook and just kept for reference.
- After crawling, the data was stored in a Pandas dataframe, preprocessed through several stages, and stored in Pickle (*.pkl) files.


## **Predecessor project: Analysis of Speaking Time Distribution – Who Speaks How Much?**

- Approach: purely statistical evaluation of speaking times using word and character counts in speeches  
- Visualization, e.g., using boxplots, distributions, or heatmaps  
- Idea: identify patterns and differences in speaking time between political parties  
- Idea: evaluate fairness in speaking time distribution between genders  
- Idea: compare interjections and comments (who makes the most, who receives the most, who receives the most applause, who receives the least applause)  
- See the corresponding notebook: [statistics](https://github.com/printjan/NLP-Projekt_Political-Language-Processing/tree/main/ST-Task)


## **Predecessor project: Automatic Detection of the Speaker’s Party Affiliation in Transcribed Speeches**

- Approach: build a classification model (e.g., SVM or BERT model) to automatically classify party affiliation
- Extra: identify speakers with the largest speaking share using an additional classification model in a pipeline structure
- Evaluation: precision and recall of the classification results (automated evaluation)
- Idea: perform similarity analysis to infer potential speech authorship overlaps
- See the corresponding notebook: [classification](https://github.com/printjan/NLP-Projekt_Political-Language-Processing/tree/main/ML-Task-1_Classification)


## **Predecessor project: Investigation of the Main Topics Discussed in Bundestag Speeches**

- Approach: use topic modeling algorithms (e.g., LDA or NMF) to identify thematic clusters in the speeches  
- Idea: party-specific comparison — compare topic clusters across parties to reveal differences in communication strategies regarding overarching topics  
- Suggestion from Prof. Albrecht: analyze how parties interpret and use certain terms (e.g., which terms related to migration, environment, or energy are used most frequently by different parties) → implementation still to be defined  
- Evaluation: assess topic coherence and qualitatively evaluate the distinguishability of the identified themes  
- Idea: retrospectively assign speeches to the identified topics  
- Idea: compare party-specific topic clusters with general clusters to illustrate each party’s focus 
- See the corresponding notebook: [topic modeling notebook](https://github.com/printjan/NLP-Projekt_Political-Language-Processing/tree/main/ML-Task-2_Topic-Modeling)



## **Predecessor project: Introduction to the data**

### **Data Scope**

The foundation of the project were the following structured speech datasets from our custom [Data Generation Pipeline's](https://github.com/printjan/NLP-Projekt_Political-Language-Processing/tree/main/dataGeneration/dataGenerationPipeline) final output stage, containing **all available plenary speeches** from the **19th and 20th legislative periods** of the German Bundestag:

```
data/
├── dataFinalStage/
│   ├── speechContentFinalStage/
│   │   ├── speech_content_19_20.pkl
│   │   ├── speech_content_19.pkl
│   │   └── speech_content_20.pkl
│   ├── contributionsSimplifiedFinalStage/
│   │   ├── contributions_simplified_19.pkl
│   │   ├── contributions_simplified_20.pkl
│   │   └── contributions_simplified_19_20.pkl
│   └── factionsAbbreviations.pkl
```

- **Preprocessing-friendly structure:** With each datapoint including the **full speech content**, speaker metadata (e.g., name, role, party affiliation), and timestamp information for one speech, these datasets provide a comprehensive and semantically rich basis for downstream preprocessing, training and evaluation workflows.
    The speeches were extracted from official plenary protocol documents `<session-number>.xml` released for each sitting by the Bundestag and systematically transformed into **machine-readable tabular formats**: All markup has been removed from the speech content and texts are stored in plain UTF-8 format with clear separation of metadata and content.
- **Contributions (interjections, applause, etc.)** have been **systematically extracted** from the main speech text and stored separately during data generation step 4.1. However, **position markers** (e.g., `({1})`, `({2})`) have been inserted into the text at the corresponding positions as placeholders, enabling accurate **re-insertion** at any time.


### **Columns (speech_content_*.pkl):**

| Column            | Description                                         |
|-------------------|-----------------------------------------------------|
| `id`              | Unique ID for each speech block                     |
| `electoral_term`  | Electoral period (e.g., 19, 20)                     |
| `session`         | Session number within the term                      |
| `first_name`      | First name of the speaker                           |
| `last_name`       | Last name of the speaker                            |
| `faction_id`      | Faction ID assigned to the speaker                  |
| `position_short`  | Speaker's role (e.g., MP, Minister)                 |
| `position_long`   | Full position title (e.g., "Bundesminister für...") |
| `politician_id`   | Unique politician ID (ui)                           |
| `speech_content`  | Full speech text (without contributions)            |
| `date`            | Session date (in Unix time format)                  |
| `document_url`    | Link to original PDF of session                     |


### **Label Handling: Faction Encoding**

- During [data generation](https://github.com/printjan/NLP-Projekt_Political-Language-Processing/tree/main/dataGeneration) each speaker in the dataset is assigned a **numerical `faction_id`**, which serves as a unique identifier for their political party.
- While the final visualizations often involve textual party abbreviations, working with numeric identifiers internally offers several advantages, including **faster lookups and comparisons** as well as the elimination of issues related to **spelling variations** or encoding.
- It is important to note that **`faction_id`s are not sequential integers from 0 to N**, but sparsely distributed over the number line:
    - This **non-contiguous mapping** stems from the [Data Generation Pipeline](https://github.com/printjan/NLP-Projekt_Political-Language-Processing/tree/main/dataGeneration/dataGenerationPipeline) being built to support future extensions to **earlier electoral terms**. Historically, many additional parties were represented in the Bundestag, and this ID scheme ensures compatibility and uniqueness across ALL time periods.
    - **Implication for deep learning models:** Since many machine learning libraries (e.g., PyTorch, TensorFlow) expect **zero-based, sequential label indices**, a **`LabelEncoder`** must be applied before feeding `faction_id` values into neural networks such as BERT-based classifiers.
    This encoding step ensures compatibility with loss functions like `CrossEntropyLoss`, which rely on dense class indices.

| Party                    | `faction_id` |
|--------------------------|--------------|
| AfD                      | 0            |
| Bündnis 90/Die Grünen    | 4            |
| DIE LINKE.               | 7            |
| CDU/CSU                  | 5            |
| FDP                      | 15           |
| SPD                      | 25           |
| BSW                      | 3            |
| Fraktionslos             | 18           |


### **Special Cases in Speaker Affiliation during Parsing**

During the data generation process, several specific challenges arose:

- **Handling of Ministerial Speeches:** In the German Parliament, ministers are considered to speak as representatives of the **Federal Government**, not of their respective parties.
    - Although their party affiliation exists, it is not directly stated in the XML files provided by the Bundestag: Their speeches are only labeled with their **official role** (e.g. `<role>Minister of Defense</role>`) instead of a faction (e.g. `<fraktion>DIE LINKE.</fraktion>`).
    - While their statements formally represent the **governments position**, ministers still remain party members and — and their speeches often reflect party-specific semantics useful for classification.

> We **reassigned ministers to their party affiliation** using a dedicated politician metadata lookup table.

- **Handling of Moderation Speeches:**
    - The following speaker groups are responsible for moderating Bundestag sessions:
        - President of the Bundestag (1 person)
        - Vice Presidents (5–6 people)
        - Secretaries (20–40 people)
    - These individuals frequently make short, repetitive and formulaic announcements (e.g., “Next speaker: XYZ”), which can distort analyses:
       **ML classifiers** might learn to associate certain formulaic phrases with a particular party (e.g., that of the President).

> All such entries are marked as `"Presidium of Parliament"` in the `position_short` column during data generation step 4.1 and can be **safely excluded** datasets if necessary.

- **Parsing Errors and Missing Party Affiliation:**
    - In some cases, speeches could not be linked to a party due to:
        - Structural inconsistencies or inconsistencies in the XML plenary protocols,
        - ambiguities in speaker identification (When no unique speaker ID is available, matching is done via fuzzy name and role matching, but that does not always work),
        - or edge cases where the rule-based parser fails.
        - ...

> The between 1000 and 2000 speeches per term that fall into this category are assigned `faction_id = -1`. Based on that, they can be **safely excluded** from datasets used for training and evaluation, ensuring data integrity.



---



# **Project-Structure-Overview:**

```
PoLaPro_dataPrivate/        # Off-git secret server access information
├── config/
│   ├── llm_providers.toml  # provider configs + default generation params
│   ├── servers.toml        # VM/hosting/data-server connection metadata
│   └── rag_defaults.toml   # shared retrieval defaults
├── secrets/
│   ├── ssh.toml            # ssh creds
│   ├── api_keys.toml       # api key registry
│   └── openai_key.toml     # openai api key - legacy (do not use)
├── ssh_multiplexing/
│   ├── control/
│   │   ├── cm_<hash>.sock  # ControlMaster socket
│   │   └── cm_<hash>.json  # pinned session metadata (IP, timestamps)
│
PoLaPro_data/               # Off-git main project data directory
├── dataGeneration/ …       # created by the Data Generation Pipeline
│   │
├── dataFinalStage/         # final result of the Data Generation Pipeline
│   ├── contributionsExtendedFinalStage/
│   │   ├── contributions_extended_19_20.pkl
│   │   ├── contributions_extended_19.pkl
│   │   └── contributions_extended_20.pkl
│   ├── contributionsSimplifiedFinalStage/
│   │   ├── contributions_simplified_19.pkl
│   │   ├── contributions_simplified_20.pkl
│   │   └── contributions_simplified_19_20.pkl
│   ├── speechContentFinalStage/
│   │   ├── speech_content_19_20.pkl
│   │   ├── speech_content_19.pkl
│   │   └── speech_content_20.pkl
│   ├── uniqueDates/        # dates of all Bundestags-sesseions
│   │   ├── unique_dates_19_20.pkl
│   │   ├── unique_dates_19.pkl
│   │   └── unique_dates_20.pkl
│   ├── uniqueSpeakersWithFactions/ # names of all politicians
│   │   └── unique_speakers_with_factions_19_20.pkl 
│   └── factionsAbbreviations.pkl
├── dataExcel/              # data in readable format
│   ├── finalStage/
│   │   ├── contributions_extended_19_20_finalStage.xlsx
│   │   ├── contributions_extended_19_finalStage.xlsx
│   │   ├── contributions_extended_20_finalStage.xlsx
│   │   ├── contributions_simplified_19_20_finalStage.xlsx
│   │   ├── contributions_simplified_19_finalStage.xlsx
│   │   ├── contributions_simplified_20_finalStage.xlsx
│   │   ├── speech_content_19_20_finalStage.xlsx
│   │   ├── speech_content_19_finalStage.xlsx
│   │   ├── speech_content_20_finalStage.xlsx
│   │   ├── unique_dates_19_20.xlsx
│   │   └── factionsAbbreviations.xlsx
│   ├── dataGeneration/
│   │   ├── mgs_wiki_rawData.xlsx
│   │   ├── mps_stage02.xlsx
│   │   ├── mpsFactions_stage03.xlsx
│   │   ├── politicians_stage03.xlsx
│   │   ├── speech_content_19_stage04.xlsx
│   │   ├── speech_content_20_stage04.xlsx
│   │   └── factions_stage02.xlsx
├── dataClassification/…    # predecessor project
│
Political_Language_Processing/  # git project directory
├── pyproject.toml
├── src/
│   ├── polapro/
│   │   ├── core/
│   │   │   ├── __init__.py     # empty
│   │   │   ├── errors.py
│   │   │   ├── logger_setup.py
│   │   │   ├── paths.py
│   │   │   ├── toml_loader.py
│   │   │   ├── llm_clients.py
│   │   │   └── th_nbg_polapro_vm.py
│   │   ├── cli/
│   │   │   ├── main.py
│   │   │   └── th_nbg_polapro_vm.py
│   │   ├── llm/                # provider information resolution, client building, facade, framework adapters, session management
│   │   │   ├── __init__.py
│   │   │   ├── ...
│   │   │   ├── providers/ ...
│   │   │   ├── ports/ ...
│   │   │   └── adapters/ ...
│   │   ├── th_nbg_polapro_vm/  # remote vm management
│   │   │   ├── __init__.py
│   │   │   ├── ssh_multiplexing_helpers.py
│   │   │   ├── vm_ssh_helpers.py
│   │   │   └── vm_ssh_rsync_utility.py # legacy (do not use)
│   │   ├── main.py
│   │   └── __init__.py         # empty
│   └── polapro.egg-info/ ...
├── rag_protoype_1/
│   └── ...
├── rag_protoype_2/
│   └── ...
├── .setup/
│   ├── python_envs/
│   │   ├── env_backups/
│   │   ├── polapro_env_setup_mac.yml
│   │   ├── polapro_env_setup_windows.yml
│   │   ├── requirements_mac.txt
│   │   └── requirements_windows.txt
│   └── ...
├── documentation/
│   └── ...
│
```



---



# **Development Environment Setup**


## **Python Environment**

* All installation files for Python environments are stored in the directory: 
    ```
    ├── .setup/
    │   ├── python_envs/
    ```
* You can use these .yml files to create the corresponding Conda environments with Miniconda by running:
    ```
    conda env create -f .setup/python_envs/<environment_file>.yml
    ```
* In the same directory, you will also find .txt files containing requirements descriptions for pip-based installations.


## PoLaPro Toolset `polapro`


A lot of the projects core functionality (LLM Provider Management, VM Control, Data Generation) is packaged in the installable Python package `polapro`.



---



## Startup - Installation


**Prerequisites for it's installation:**

- You have the off-git private directory next to the repo root.
- You created `PoLaPro_dataPrivate/config/llm_providers.toml` and `PoLaPro_dataPrivate/secrets/api_keys.toml`.

**Install the package by running the following command from the repo root:**

```powershell
python -m pip install -e .
```

**Optional (recommended for an isolated environment):**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -U pip
python -m pip install -e .
```


---


##If you use this repository, please consider citing as:

```
Tischner Jan, Nico Simon, Micha Walter (2026): PoLaPro - Political Language Processing: RAG system for German political knowledge.
```
