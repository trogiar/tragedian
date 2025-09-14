---
created_at: 
modified_at: 
---

# Tragedian

## Principles

- The goal is to maximize intellectual productivity while minimizing the human effort required to operate and maintain notes.
	- Avoid the need to install numerous plugins or perform complex customizations.
	- Ensure versatility to accommodate casual usage patterns that don't require high levels of commitment.
		- Famous note organization methodologies like Zettelkasten, Evergreen Notes, and SECOND BRAIN are often aimed at discovering new insights from accumulated notes, actively refining note information by users, and ultimately creating outputs for external publication. However, these "high-commitment" methods do not tolerate the existence of low-quality information and force users to brush up such information through their own efforts.
		- The above work requires considerable energy, so very few users can handle it perfectly. As seen prominently in Zettelkasten and other methods, neglecting note refinement reduces the utility of notes as information sources. To prevent this, users must continuously allocate resources to maintain notes, leading to many users dropping out due to exhaustion.
		- General Obsidian and Zettelkasten guides tend to introduce such "high-commitment" methodologies, but these are often unsuitable for casual markdown editor use cases. Considering this, this Vault is designed to accommodate casual use cases where users don't want to allocate resources to note improvement work.
		- Specifically, the Vault's directory structure ensures that notes are automatically organized to a minimum extent at creation time, making it easy to maintain visibility of the overall Vault structure even when note organization is neglected. Additionally, the directory structure has been made as flexible and comprehensive as possible to cover all use cases of memo text files I've created throughout my life.
		- Even with notes classified by directories, random exploration of notes and construction of internal links are still possible, allowing for note refinement when the mood strikes.
		- Furthermore, the design considers enabling processes similar to multiple renowned note organization methods within this Vault, within a range that doesn't compromise the user experience for casual use cases mentioned above. For example, the Summary header established as a Reserved Header is also intended for use as a summary section utilized in note information compression advocated by Progressive Summarization.
	- Conversely, this approach may underperform compared to other existing methods in high-commitment, advanced intellectual production, but this is acceptable.
- Note classification and other metadata are incorporated into the directory structure as much as possible. This is because consolidating metadata management into Frontmatter increases the maintenance burden for each note and makes it difficult to change note operation rules.
- For similar reasons, notes are designed to be as "stateless" as possible. Notes with state concepts require maintenance work such as metadata updates or moving to different directories corresponding to the note's state.
	- Therefore, notes created in this Vault rarely undergo Frontmatter updates or file movements between directories (except for automatically updated items like the modified_at property).
- Operations and thinking required when creating notes are also minimized as much as possible.
	- All note formats are unified using the community plugin Linter.
	- MOCs that comprehensively aggregate note links and other dependent notes that must be edited in conjunction with creating new notes are avoided as much as possible, and automated using mechanisms like Dataview.
		- In particular, the `inbox/` directory assumes cases where notes are added spontaneously, so additional work and considerations for note creation are kept to a minimum.
	- The aforementioned rule of "avoiding metadata and states" also aims to reduce the mental overhead of thinking about genres and statuses to set for each note when creating new notes.

## Plugins

It is desirable to have the community plugins Dataview and Linter installed.
Dataview can automate the maintenance of notes that reference other notes, including the dossier.md mentioned later.
Linter can unify note layouts to improve readability while automating the insertion and updating of elements like Frontmatter.

## Vault Structure

(Used [tree.nathanfriend.com](https://tree.nathanfriend.com/?s=%28%2527options%21%28%2527fancy%21true%7EfullPath%21false%7EtrailingSlash%21true%7ErootDot%21false%29%7EK%28%2527K%2527TragedianQassetGattachmentG9plaOGscriptsQinbox3C3I3%2A5B5recent7ideaGminuOGpoemGreviewG9p_memoN6model3%28topic1%2938L8U84FLFUF4%28topic2%293%25E2%258B%25AE6READMEN%2527%29%7Eversion%21%25271%2527%29%2A%2520%252006%2A3%252F04J27HBHdossier75I-DD_C7%2A6%255Cn%2A7N08%2Ainquiry9OmB%2A%25E2%258B%25AE0Cdaily_noOsF%2AlistGs3H%2A%2AI%2Ayyyy-MMJ_i9_Ksource%21L3Hi9GHN.mdOteQ%252F6UJ17H%2501UQONLKJIHGFCB98765430%2A))

```
Tragedian/
├── assets/
│   ├── attachments/
│   ├── templates/
│   └── scripts/
├── inbox/
│   ├── daily_notes/
│   │   ├── yyyy-MM/
│   │   │   ├── yyyy-MM-DD_daily_notes.md
│   │   │   └── ⋮
│   │   ├── yyyy-MM-DD_daily_notes.md
│   │   └── recent.md
│   ├── ideas/
│   ├── minutes/
│   ├── poems/
│   ├── reviews/
│   └── temp_memo.md
├── model/
│   ├── {topic1}/
│   │   ├── inquiry/
│   │   │   ├── items/
│   │   │   │   ├── inquiry_item_1.md
│   │   │   │   ├── inquiry_item_2.md
│   │   │   │   └── ⋮
│   │   │   └── dossier.md
│   │   └── list/
│   │       ├── items/
│   │       │   ├── list_item_1.md
│   │       │   ├── list_item_2.md
│   │       │   └── ⋮
│   │       └── dossier.md
│   ├── {topic2}/
│   └── ⋮
└── README.md
```

### assets

#### attachments

Stores all files that are not `.md` files, such as images and PDFs. These files are referenced as attachments from within .md files.

#### templates

Stores templates used by core plugins like daily_notes and community plugins like Templater.

#### scripts

Stores scripts used by Templater and similar tools.

### inbox

Named "inbox" because most new notes are created here.
In principle, information consisting of one topic per note is placed here.
While notes under model may have information added or modified later, notes in inbox are generally not modified or added to after creation. When such information addition or modification is desired, new notes are added following a Zettelkasten-like operation.

#### daily_notes

Area for storing diary entries.
When the number of files increases and the file explorer becomes vertically long, create year-month folders in `yyyy-MM` format as needed and archive files there.

##### yyyy-MM-DD_daily_notes.md

Diary content. One note file is created per day.
Layout templates are managed by the core plugin Daily notes.
The note contains a Dataview to check notes created or edited on that day, a Journal section for writing diary content, and a ToDo section to check incomplete tasks as of that day.
The Journal records factual events that occurred that day, thoughts arising from those events, and ToDo checkboxes.
However, if thoughts or information are somewhat organized when trying to record in the diary and can be extracted as a single note with a clear title, create notes in poems or reviews to enhance reusability.
Similarly, atomic information that seems reusable should be created in ideas from the start, and records with main topics and considerable information should be created in minutes from the start. Information that can be extracted as notes outside daily_notes is basically not recorded within daily_notes.
Conversely, content written in daily_notes Journal is limited to information with extremely low information value or reusability that doesn't warrant extraction to each inbox domain, or trivial miscellaneous notes. Days with zero Journal content are frequently expected.
For days with important events where the day itself can be labeled, edit the `daily_notes` part of the note filename to something like `yyyy-MM-DD_Hawaii_trip_day_one.md` to improve visibility.

##### recent.md

Auxiliary note for listing daily_notes from recent months across dates.
Simple content with just a block that displays a list of notes filtered by recent dates using Dataview.
Using Dataview's default query function only shows a list of links to notes, and the content can't be seen without hovering for preview or clicking links, which is somewhat inconvenient. A better display method should be considered.

#### ideas

Ideas, reminders of things that came to mind.
Atomic notes with extremely little text content are placed here for now.

#### minutes

Meeting minutes, logs.

#### poems

Information for reflecting on one's thoughts and internal aspects.
Among notes containing endogenous information without clear source information, those that are somewhat structured and appropriate for titles or labels are placed here.
Not limited to literal poems or emotional essays, but includes all outputs without sources such as memos based on facts from daily life, thought experiments, considerations, and short stories.
For information where placement in poems is questionable, write in daily_notes if no clear label or title can be assigned, or in ideas if the information volume is too small, then consider extracting to poems later or integrating with other ideas or daily_notes descriptions.

#### reviews

Placement for notes originating from external sources such as internet information, videos, books, or actions of others.
Used for copying internet information for reference purposes or writing opinions and impressions from viewing information.
If the volume of endogenous thoughts arising from external sources is very large, place in poems instead.

#### temp_memo.md

Note for casually writing information that doesn't warrant creating a new note file and is planned to be deleted soon.
Conversely, if information remains in this file indefinitely, it indicates inappropriate placement and should be extracted as some individual note.

### model

Places relatively large information consisting of multiple notes (or single notes with information volume equivalent to multiple inbox notes).
While inbox separates directories by note format, model uses a topic-based structure where top-level directories are separated by note topics, with subdirectories for each note format below.
Format-specific directories basically place smaller note groups with common themes in `items` and place meta `dossier.md` notes for bird's-eye view in the parent directory of items.
However, no strict rules are set for file sizes within items.
This prevents operational overhead such as needing to decompose information to meet size requirements when placing notes or external copied information in items, or needing to decompose information and split notes when items notes become larger than expected.
Furthermore, regarding dossier.md, when the number of notes in items is sufficiently small and bird's-eye management can be adequately achieved just by viewing the note list in the file explorer, dossier.md becomes obstructive. Therefore, no clear rules are set for the presence or absence of dossier.md.

#### {topic}

Under topic, there are subdirectories that classify notes by nature, where items directories, dossier.md, or .md files that don't follow that format are placed.
Since there are various motivations for grouping notes, no particular rules are set for classification subdirectory names.
For example, configurations like the following could be considered:

```
model/
└── PC/
    ├── Custom_PC_parts_consideration/
    │   ├── items/
    │   │   ├── 2025-09-01_Memory_manufacturer_research.md
    │   │   ├── 2025-09-02_Motherboard_requirements_study.md
    │   │   └── 2025-09-03_Local_PC_parts_shop_search.md
    │   └── dossier.md
    └── Security_software_selection/
        ├── items/
        │   ├── 2025-08-01_Security_software_budget_consideration.md
        │   └── 2025-08-02_Required_security_software_features_identification.md
        └── dossier.md
```

When classification subdirectory names are not clearly determined, use one of the following as default values:

- list
  Place for listing and managing parallel information such as reviews, ideas, records, materials, diaries about common themes.
  Main purpose is to aggregate items of the same genre to improve accessibility without considering mutual relationships between notes.
- inquiry
  Place for organizing outcomes from thinking, deep investigation about one main theme with complex structure or context.
  Groups items where content is sequentially connected (next note written based on previous note content) or frequent cross-references occur between notes, or places files intended to grow into large notes with frequent review and information addition/updates.

These subdirectories can accumulate notes and be renamed to any name when classification becomes clear, or continue operating with the original name.
When unclear whether to use list or inquiry, use list for now.
When you want to write additional information about notes such as context or relationships between notes beyond simple lists of items in dossier.md under list, rename to inquiry.

##### items

Place to store note groups.
When notes about specific common topics become scattered in inbox and you want to group them, you can move notes from inbox to items to consolidate them.

##### dossier.md

Note for listing, classifying, managing, and overviewing information in `items/`.
Place interfaces in the note such as internal link lists and Dataview blocks to reference all notes in items in an operationally friendly manner.
Inquiry dossiers become relatively complex content recording various contexts about notes such as background of note creation and relationships between notes, expecting users to write internal links and comments.
List dossiers are expected to have simple content just enumerating list items, so there may be cases with only simple automatic indexing by Dataview.

## Note Rules

### File Name

- For files under inbox, timestamps of file creation are often important information, so files generally take the form of unique notes with yyyy-MM-DD format dates at the beginning of filenames.
- For files under model, presence or absence of dates at the beginning of filenames is optional. For example, for movie review lists where the date of note creation is operationally important, add dates similar to inbox.

### Content

#### Front Matter

Record the following properties in front matter.
created_at and modified_at are automatically inserted by the Linter plugin when saving.
For reasons stated in Principles, information entered in Frontmatter is reduced to the absolute minimum.

| Property Name | Type | Required | Description    |
| ------------- | ---- | -------- | -------------- |
| created_at    | Date | Yes      | Date and time when file was created   |
| modified_at   | Date | Yes      | Date and time when file was modified   |
| tags          | Tags |          | Tags for classifying files |

##### Tags

Tags set in the tags property are generally created freely.
However, only the following tags are used commonly across all notes for search and filtering purposes.

- `#archived`: Applied when marking notes as no longer referenced due to reasons such as discovered incorrect information, outdated information, or complete duplication with another note that fully encompasses the content. Intended for operations such as excluding notes with this tag from Dataview list displays when necessary.

#### Reserved Headers

While maintaining high freedom in note composition, some headers are unified in name to assist in cross-note search and display.
These are merely rules for aligning names and do not force creation of corresponding header items in all notes. They need not be created when unnecessary.

##### Summary

Record note abstracts and summaries.
This item is intended for use as content summaries when displaying note lists in Dataview.
Therefore, text entered in this item should be as short as possible (approximately 3 lines as a guideline).

##### Reference

Record information about external resources such as sources, references, and supplementary materials.
Resource information should ideally enable unique identification of original sources.
Therefore, for web resources like web pages, enter URLs; for books, ideally enter publisher, title, author, referenced pages, etc. However, since tracing sources can be difficult for some information, this is not strictly ruled.

#### ToDo

##### Create Task

Record ToDos arising while reading, adding, or editing notes within related notes, and ToDos arising in daily life within daily_notes by creating checkboxes as appropriate.
There is no dedicated layer or note for aggregating ToDo tasks.
Since daily_notes contains Dataview that displays checkboxes created throughout the Vault organized by note, use daily_notes for cross-note progress management.
Note genres and purposes handled in Obsidian are diverse, so contexts where ToDo tasks arise cannot be systematized into manageable rules.
Therefore, task creation operates under loose rules allowing writing in any note whenever tasks arise.
(There are no clear rules for task behavior after creation such as completion or postponement, but some rules might be beneficial here)

##### Task Status

Tasks have only two states: "completed" and "incomplete." Special states like "postponed," "in progress," or "rejected" do not exist.
These special states have ambiguous definitions of what constitutes postponed or in progress, easily causing operational issues such as:

- Tasks with almost zero progress but minimal or perceived initiation are updated to "in progress" status, leading to satisfaction and no further task advancement
- Tasks that are partially completed with subtle remaining work are left in "in progress" status
- Tasks that end differently than expected become ambiguous whether to consider them "completed" or "discarded"
- Tasks that must be interrupted or deprioritized for reasons are updated to "postponed" status and subsequently forgotten

Long-term or large-scale tasks that require "in progress" status can be managed by decomposing content into subtasks, with progress visualized by remaining subtasks.
States like "postponed" that are variations of "incomplete" can be handled as incomplete with context about why the task is unstarted and when it can be started clearly stated in the task name. This allows expression of various nuanced incomplete states beyond postponement and enables versatile operation.
States like "rejected" that are variations of completion should simply be marked "completed" if the task no longer needs execution. When circumstances prevent simple completion, the task context has essentially changed, requiring restructuring into task configurations that can express context through creating necessary subtasks, completing existing tasks and creating new ones, or other means.
