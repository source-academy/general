# Source Academy Game - Storywriter guide

## Contents

* [Getting Started](#getting-started)

* [Story Simulator](#story-simulator)
  * [Checkpoint Simulator](#checkpoint-simulator)
  * [Object Placement](#object-placement)
  * [Asset Uploader](#asset-uploader)
  * [Chapter Simulator](#chapter-simulator)

* [Txt Guide](#txt-guide)
  * [Getting Started](#getting-started)
  * [What's in a Checkpoint?](#whats-in-a-checkpoint)
  * [Key Concepts](#key-concepts)
  * [Default checkpoint](#default-checkpoint)
  * [Txt Format Guide](#txt-format-guide)
  * [Main .txt](#main-txt)
  * [Location Guide](#location-guide)
  * [Actions](#headerActions)
  * [Objects](#headerObjects)
  * [BoundingBoxes](#headerBoundingBoxes)
  * [Characters](#headerCharacters)
  * [Bgm](#headerBgm)
  * [Sfx](#headerSfx)
  * [Dialogue Guide](#dialogue-guide)
  * [Dialogue Header](#dialogue-header)
  * [Dialogue Body](#dialogue-body)
    * [Basic rule](#basic-rule)
    * [Dialogue Actions](#dialogue-actions)
    * [Speakers](#speakers)
    * [Parts and goto's](#parts-and-gotos)
    * [Prompts](#prompts)
    * [Interpolation](#interpolation)
  * [Action Guide](#action-guide)
  * [Action execution order](#action-execution-order)
  * [Entities that can trigger actions](#entities-that-can-trigger-actions)
  * [Actions API](#actions-api)
  * [Action Repeatability](#action-repeatability)
  * [Action Conditions](#action-conditions)
    * [Conditions API](#conditions-api)
  * [Entity Guide](#entity-guide)

* [Sample Checkpoint](#sample-checkpoint)

## Getting Started

Important keywords to know as a story writer

* Source Academy Game
  * Interactive visual novel to complement students' learning
  * Comprised of [chapters](#chapters)

* Chapters
  * Main division of the Source Academy game
  * Released on specified open date (can be specified in [Chapter Simulator](#chapter-simulator))
  * Comprised of several [checkpoints](#checkpoints)

* Checkpoints
  * Within a chapter, checkpoints are played in progression as previous checkpoint is completed.
  * Basic 'programmable' unit of Source Academy story; every checkpoint is specified using a text-based CSV
  * Checkpoint is a storywriter's tool to place the student in an entirely new scenario within the course of a single chapter.
  * Enables writers to provide students with different set of objectives and also a new map with a different set of locations/objects/dialogues to interact with.

If you're looking to write your own checkpoint txt:

* Refer to the following [Sample Checkpoint](https://github.com/source-academy/frontend/wiki/Game-:-Sample-Checkpoint) or download this [sample txt](https://source-academy-assets.s3-ap-southeast-1.amazonaws.com/stories/1-settling-in.txt) for reference as you read the [txt guide](#txt-guide).

* You may download the following [Source Academy Game Txt Syntax highlighter](https://github.com/source-academy/sa-game-syntax-highlighter) vs code extension to highlight your txt under the **Sa** language

## Authoring Workflow

These are the steps in authoring a story:

1. Use [Object Placement](#object-placement) to design a scene, print the generated text file from the browser console log
2. Add dialogues and actions onto your checkpoint text file by following the [Txt Guide](#txt-guide)
3. Try out your checkpoint using [Checkpoint Simulator](#checkpoint-simulator)
4. When you're happy with your checkpoint text, upload your checkpoint text as an asset on S3 using [Asset Uploader](#asset-uploader)
5. After making 1 or more checkpoint text files, you may wrap and publish your chapter by providing title, open date, publish status, and component checkpoint files through the [Chapter Simulator](#chapter-simulator).

## Story Simulator

Story Simulator provides a set of tools to help writers generate the .txt file and simulate a single checkpoint, which they can then upload.
Story Simulator has various tools that you can use by clicking on their names on the Story Simulator Main Menu.

### Object Placement

Enables you to rearrange objects, bounding boxes and background in the scene and capture their coordinates and asset paths.

#### Tools available

* At side panel:
  1. **Asset Selection** - The Asset Selection on the side panel is a place to look for assets you'd like to place on scene. You can click to open/close folders and click on assets to select it.
  2. **Asset Viewer** - Displays image that you selected with Asset Selection to let you visualise it before placing it on scene.

* At game screen's Object Placement:
  1. **Background Paint Bucket** - Paints background on scene. To use, select an image from the `/location` folder of the Asset Selection section and click on the paint bucket to see the new background.
  2. **Add Selected Object** - Adds more foreground images to the scene. To use, select an object from any folder of the Asset Selection section and click on the paint bucket to see the new background.
  3. **Bounding box** - Lets you draw bounding boxes. To use, click on the tool and drag to draw rectangles on scene.
  4. **Drag or resize** - Lets you drag things around the scene. To use, click on the tool and start dragging objects and bounding boxes. ⌨️ Hint: Use the `[` and `]` keys to resize images and bounding boxes.
  5. **Print coordinates** - Lets you see coordinates on hover and print them out in the console. To use, click and look at console to see the .txt
  6. **Erase all** - Erases everything on scene. Be careful when using this as there is no Undo button.

### Checkpoint Simulator

Runs a checkpoint txt file using actual game engine. You may either load your local text files to the browser or use existing S3 files

   1. Choose a default (i.e. base) checkpoint file (from local or S3)
   2. Choose a checkpoint txt file (from local or S3)
   3. Click on 'Simulate Checkpoint' and play your story that combines these two text files.

Note: Do note that your game state is not saved to backend and txts are not uploaded. You may upload them once you're satisfied using the [asset uploader](#asset-uploader).

### Asset uploader

Enables you to upload and delete assets (images, sounds, and checkpoint .txts) that that are used by the game.

#### Uploading files

1. Choose a folder that you'd like to upload your asset into.
2. Choose file or files that you want to upload.
3. Click on Upload image.
4. Refresh page to see your new asset in Asset Selection.

Note: You may not overwrite existing files, but you can always delete them and then reupload them. This step is made so that you don't accidentally overwrite files.

#### Deleting files

1. Click on the trash can icon to delete a file.
2. Confirm whether you actually want to delete the file.

### Chapter Simulator

A place to control and simulate stories which are published to students.

#### Editing/Creating a chapter

1. Use dropdown to choose a chapter you want to edit/   create.
2. Give a title for chapter
3. Choose open date for chapter
4. Choose a background image for the chapter
5. Choose and order the checkpoint text files that should make up that chapter (Possibly give your full chapter a go?)
6. Adjust whether or not this story should be published.
7. Hit Save to make changes to your chapter.

## Txt Guide

A guide to writing the .txt read by the Source Academy game engine.

## What's in a Checkpoint?

Definition and structure of a checkpoint

As you may already know, checkpoint is a part of chapter, which are played in progression once the previous one has finished.

A checkpoint is made up of (1) set of objectives and (2) a game map.

**Objectives** 📝

Refers to a set of goals that a player must achieve in order to clear the checkpoint.

It can be a simple list such as:

```
talkToHartin
goToClassRoom
```

Explanation: We define two **objectiveIds**, `talkToHartin` and `goToClassroom`. As the player interacts with various entities in the game, these objectives may be fulfilled through [actions](#action-guide). Once both these tasks are fulfilled by the player, the player can proceed to the next checkpoint.

**Game Map** 🗺️

Refers to all the locations in the checkpoint and how each location is configured. Storywriters have great control when it comes to customising locations, such as which other locations it is connected to, what objects are present in it, or even game modes available, just to name a few customiseable aspects of a location.

## Key Concepts

**Entities**

Entity refers to an element in the game, such as a specific location, object, or dialogue. Refer to [Entity Guide](#entity-guide) to learn more about types of entities and what they mean in the game.

**Actions** 🎬

Actions are the main drivers of a story. They are the storywriters' tool or "Source"😉 to enable the game to progress based on player choices.

* How actions are triggered: Storywriters can attach actions to entities. For example, attaching action to a location causes the action to be performed when location is visited while attaching actions to objects causes action to be performed when object is clicked. Look out for 🎬 to see whether objects can have actions.
* What actions do: Using actions, storywriters can cause new objects and dialogues to be added to the map, move the player to a different location, mark objectives as "complete" and much more.
* Storywriters can specify whether the action occurs just once or many times.
* See [Action Guide](#action-guide) for more information.

**Ids**

Each entity such has its own id. Storywriters typically provide these in order to refer to a specific entity.

* Example: `mainDoor1` could refer to one particular instance of a door image that is located in the center of the Hallway.
* Typically written in camelCase.
* Ids can have types such as **locationId**, **objectId**, etc. depending on the type of entity it is referring to.
* Warning❗ Ids are unique across the entire checkpoint. Even though id's have different types, you cannot have a character with **characterId** of `car` and an object with **objectId** of `car` too.

## Default checkpoint

All checkpoints inherit the `/stories/defaultCheckpoint.txt` in S3, sample [here](https://source-academy-assets.s3-ap-southeast-1.amazonaws.com/stories/defaultCheckpoint.txt).

This reduces the need to redeclare locations and items that are present throughout the game.

You may overwrite this by uploading with the `defaultCheckpoint.txt` filename.

## Txt Format Guide

A little guide on the unique .txt syntax

### 1. Types of listing

There are two types of listing

#### point-listing

Vertical listing of items separated by newlines

Example:

```
item1
item2
item3
```

#### comma-listing

Horizontal listing of items separated by commas

Example:

```
item1, item2, item3
```

### 2. Paragraphs

The .txt uses Python-like paragraph format

A paragraph is made up of a **header** and a **body**.

```
header
    body
    body
    body
```

Storywriters can create paragraph bodies by indenting with 4 spaces or 1 tab

Example:

```
header1
    line3
    line4
header5
    line6
        line7
        line8
    line9
    line10
        line11
    line12
```

In the above example, there are two main headers, `header1` and `header5`, each containing a body of text underneath.

* `header1`'s body is a point-listing made up of just 2 points: `line3` and `line4`
* `header5`'s body is a point-listing 4 points: `line6`, `line9`, `line10`, and `line12`
* We can also nest further things underneath one point. `line6`'s body is a point-listing of 2 points: `line7` and `line8`, while `line10`'s body is a point-listing of 1 points: `line11`.

### 3. Txt Lines

Lines refer to all non-empty lines in the text file, whichever paragraph they are in. A txt line is typically one of the following:

* An id such as `studentRoom`
* A category such as `characters`
* A CSV (see [CSV format](#4-CSV-format))
* A configuration line (see [Configuration Line](#5-configuration-line))

### 4. CSV format

This format specifies properties for an entity.

Examples:

```
cheiftain, Chieftain, happy, left
```

```
myChair, /objects/chair.png, 100, 100
```

CSV's describes an entity (e.g. character or object)'s properties. The order of properties matters. The proper CSV format must be followed in order to accurately describe an entity.

### 5. Configuration line

This format specifies the value(s) for a certain key, using a `:` symbol.

Examples:

```
startingLoc: studentRoom
```

```
modes: talk, move, explore
```

This line describes the value for a particular configuration such as the value of `startingLoc` (starting location) or the list of game modes in a location. In case the value is a comma-listing, the order of values typically don't matter.

### 6. Comments

As in most programming languages, you can comment out lines or sections of lines using `//` and `/* */`.

#### single-line comments

`//` ignores all characters occuring after it in the same line.

```
objectives
    checkedScreen
    // talkedToLokKim1
    // talkedToLokKim2
    // talkedToLokKim3
```

#### multi-line comments

`/* */` ignores characters between `/*` and `*/`, possibly spanning multiple lines.

```
objectives
    checkedScreen
    /* talkedToLokKim1
    talkedToLokKim2
    talkedToLokKim3 */
```

## Main .txt

The following must be specified in the main outermost layer of the .txt

### Main Txt Configuration lines

|Key|Value(s)| Requirements for value |
|:--|:--|:--|
|`startingLoc`| The starting location of the player upon entering the chapter | Must be an existing location id |

### Main Txt Paragraphs

The following are paragraphs present in the outermost layer of .txt

#### Header:`objectives`

#### Body

Point-listing of **objectiveIds** that players must accomplish to complete the checkpoint.

Example:

```
objectives
    talkToHartin
    solveThePuzzle
```

Explanation: We define that players must accomplish the tasks `talkToHartin` and `solveThePuzzle` to proceed to next checkpoint.

***

#### Header:`gameStartActions`

#### Body

Point-listing of actions that execute whenever student revisits the checkpoint.

Example:
```
gameStartActions
    show_dialogue*(welcomeBack)
```

Explanation: Whenever the student comes back to play this game, he is shown the `welcomeBack` message.

***

#### Header:`checkpointCompleteActions`

#### Body

Point-listing of actions that execute once student has completed the checkpoint.

Example:

```
checkpointCompleteActions
    show_dialogue(wellDone)
```

Explanation: When the student has completed the checkpoint, he is shown the `welcomeDone` message.

***

#### Header: `locations`

#### Body

Point-listing of CSVs to declare locations present in the checkpoint.

CSV format: `locationId, location asset path, location name, asset type(optional), number of frames(optional)`

| Property | Meaning |
|:--|:--|
| locationId | Declaration of new location with that `locationId`
| location asset path | The path to an image file to be used in painting the background of the location |
| location name | display for that location when players click on "Move" |
| asset type | `Image` or `Sprite` type. Asset type can be omitted if type is `Image` but is mandatory for `Sprite` |
| number of frames | Number of frames of a spritesheet. Required only for `Sprite` assets |

 Example:
```
locations
    studentRoom, /locations/room.png, Student Room, Sprite, 11
    hallway, /locations/hallaway.png, Hallway
```

Explanation: We create two locations, one with **locationId** `studentRoom` that uses the spritesheet `/locations/room.png` with 11 frames as an animated background and has the display name `Student Room`, and another with **locationId** `hallway` that uses `/locations/hallway.png` as background and has the display name `Hallway`

Note: Animated background spritesheets must have frames with dimensions 1920x1080

***

#### Header:`<specific-locationId>`

Use one of the **locationIds** you defined previously under `locations` paragraph

#### Body

Nested paragraph describing a specific location

Example:

```
studentRoom
    modes: ...
    objects
        ...
    characters
        ...
```
Refer to [Location Guide](#location-guide) for more information on writing location paragraphs.

***

#### Header:`dialogues`

#### Body

Point-listing of dialogue (paragraphs) that can be brought up

Example:

```
dialogues
    uninhabited, The Uninhabited Planet
        ...
    newBegin, A new beginning
        ...
```

Refer to [Dialogue Guide](#dialogue-guide) for more information on writing dialogues.

## Location Guide

How to write paragraphs describing a certain location.

Each location paragraph is headed by the `locationId` of the location that you want to describe.

The following must be specified in the body of a location:

### Configuration lines

|Key|Value(s)| Requirements |
|:--|:--|:--|
|`modes`| Comma-listing of the [game modes](#mode) available in the chapter | Every mode must be one of the following: `talk`, `explore`, `move`|
|`talkTopics`| Comma-listing of [talk topics](#talkTopic) available in a location, i.e. dialogues that players can choose from when click on "Talk" | Must be valid **dialogueIds** |
|`nav`| Comma-listing of other locations that this location is connected to. |Must be valid **locationIds** |

### Paragraphs

Paragraphs in the body of the location paragraph.

***

#### Header:`actions`

#### Body
Point-listing of actions 🎬 that occur when location is visited.

Example:

```
actions
   show_dialogue(youveArrived)
   make_object_glow(pen)
```

Refer to [Action Guide](#action-guide) for more details on how to describe an action.

***

#### Header:`objects`

#### Body

Point-listing of CSVs of [objects](#object) in the location.

CSV format: `+objectId, object asset path, x, y, width(optional), height(optional), asset type(optional), number of frames(optional)`

| Property | Meaning |
|:--|:--|
| + | Presence of object at the start of the checkpoint. Put a plus sign if you want the object to appear initially when checkpoint begins, drop the plus sign if you'd like to add it later.
| objectId | Declaration of new [object](#object) with objectId of `objectId` |
| object asset path | The path to the image file to be used in rendering the object in the location |
| x | x-coordinate of centre of the image |
| y | y-coordinate of centre of image |
| width | width of the image, required for `Sprite` assets |
| height | height of the image, required for `Sprite` assets |
| asset type | `Image` or `Sprite` type. Asset type can be omitted if type is `Image` but is required for `Sprite`  |
| number of frames | Number of frames of a spritesheet. Required only for `Sprite` assets |

Example: `+mainDoor1, /objects/door.png, 10, 20, 30, 40, Sprite, 11`

Explanation: In the location, we display an animated object with **objectId** `mainDoor1` that uses the spritesheet `/objects/door.png`, with 11 frames and each frame of size 30 by 40 pixels, as its texture and is rendered using the coordinates (10, 20).

Each object CSV line may have nested point-listed [actions](#action-guide) 🎬 underneath, specifying what actions are triggered when object is clicked.

***

#### Header:`boundingBoxes

#### Body

Point-listing of CSVs of [bounding boxes](#boundingBox) in the location.

CSV format: `+bboxId, x, y, width, height`

| Property | Meaning |
|:--|:--|
| + | Presence of boundingbox at the start of the checkpoint. Put a plus sign if you want the [bounding box](#boundingBox) to appear initially when checkpoint begins, drop the plus sign if you'd like to add it later.
| bboxId | Declaration of new boundingBox with bboxId of `bboxId` |
| x | x-coordinate of centre of the rectangle |
| y | y-coordinate of centre of rectangle |
| width | width of the rectangle |
| height | height of the rectangle |

Example: `+bbox1, 10, 20, 30, 40`

Explanation: In the location, we draw an invisible, possibly clickable rectangle with **bboxId** `bbox1` with coordinates (10, 20) and size 30 by 40 pixels.

Each object CSV line may have nested point-listed [actions](#action-guide) 🎬 underneath, specifying what actions are triggered when boundingBox is clicked.

***

#### Header:`characters`

#### Body

Point-listing of CSVs of [characters](#character) in the location.

CSV format: `+characterId and assetPath, character name, default expression, default position`

| Property | Meaning |
|:--|:--|
| + | Presence of character at the start of the checkpoint. Put a plus sign if you want the [character](#character) to appear initially when checkpoint begins, drop the plus sign if you'd like to hide the character or add him later.
| characterId and assetPath | Declaration of new characterId. Since characters are unique and cannot have multiple instances, the id has to match the asset path (which is the folder name inside `/characters` folder)
| character name | The display name to show in the dialogue speaker box if ever that character has dialogues |
| default expression | The expression for this character initially. Must match one of the filenames
| default position | Position to render the character originally. May be one of `left`, `right`, or `center` |

Example: `+chieftain, Chieftain, happy, left`

Explanation: In the location, we display `Chieftain` with **characterId** `chieftain` that uses `/characters/chieftain` folder as source of images, with `happy.png` being his original expression and is rendered on the left side of the screen.

***

#### Header:`bgm`

#### Body

Point-listing of CSVs of [bgm](#bgm) in the location.

CSV format: `bgmId, bgm asset path, volume`

| Property | Meaning |
|:--|:--|
| bgmId | Declaration of new bgmId |
| asset path | The path to the background music to be used in rendering the object in the location |
| volume | Volume of this background music |

Example: `heavyHitter, /bgm/HeavyHitter.mp3, 0.5`

Explanation: We declare the background music with **bgmId** `heavyHitter` that uses `/bgm/HeavyHitter.mp3` as source of background music and volume of 0.5.

Note: The first background music declared is made default for the location.

***

#### Header:`sfx`

#### Body

Point-listing of CSVs of [sfx](#sfx) in the location.

CSV format: `sfxId, sfx asset path, volume`

| Property | Meaning |
|:--|:--|
| sfxId | Declaration of new sfxId |
| asset path | The path to the sound effect to be used in rendering the object in the location |
| volume | Volume of this sound effect |

Example: `card, /sfx/card.mp3, 0.5`

Explanation: We declare the sound effect with **sfxId** `card` that uses `/sfx/card.mp3` as source of background music and volume of 0.5.

## Dialogue Guide

Crafting individual dialogue paragraphs under the `dialogues` paragraph

### Dialogue Header

The header for a dialogue is a CSV with format: `dialogueId, title(optional)`

Example:

```
planetXrk, What happened to the planet, Scottie?
    <dialogue body goes here>
```

Explanation: We define a dialogue with the **dialogueId** `planetXrk`. It has the title `What happened to the planet, Scottie?`. The title is  will appear under the "Talk" menu if the dialogue is a [talk topic](#talkTopic) under a location.

Note: Although this is a CSV, you can put commas in the title, this is fine.

### Dialogue Body

Writing dialogue bodies are quite simple, much like writing a script for a play.

Example:

```
@scottie, happy, left
It has been a while since you last set foot on the ship.
I'm so happy you've arrived.
@you
Thanks, Scottie! It was my pleasure.
```

#### Basic rule

The simplest body is just a point-listing of lines to be spoken in the dialogue.

#### Dialogue Actions 🎬

You may nest point-listed actions underneath any line in the dialogue. This causes the actions to be triggered when dialogue line is spoken.

❗You may not perform the following actions during a dialogue.

* `show_dialogue` - Dialogue is already playing. Use [goto](#parts-and-gotos)'s instead.

#### Speakers

You can change the speaker by writing a change-speaker CSV which starts with an `@` symbol

CSV Format: `@characterId, expression(optional), position(optional)`

|Property|Meaning|
|:--|:--|
| characterId | characterId of the speaker |
| expression | Expression of the speaker. Has to be one of the expressions in the folder. Defaults to speaker's default expression if not specified |
| position | Position of speaker on the map. One of `left`, `right`, `center`. Defaults to speaker's default position if not specified.|

Example: `@beat, happy, left`

Explanation: This line changes the speaker to a `beat` as described in any location's `characters` list, whose expression is happy and rendered on the left of the screen.

❗Make sure you have the character somewhere on the map first before making him a speaker, even if he's hidden or in another location.

Note: You may use the following special speakers in place of the speaker CSV

* `@you` - will make students' name appear
* `@narrator` - will speaker box and speaker avatar disappear

#### Parts and goto's

You can specify parts of a dialogue by numbering lines using integers.

Example:

```
9
Welcome to Source Academy
goto 2

1
You will never reach this part.

2
You have reached part two.
```

Explanation: This dialogue has three parts. Part `9`, being listed first, will be played first. After playing `Welcome to Source Academy`, it reaches `goto 2`. This means the dialogue will now play part `2` - `You have reached part two`. That's the end. Part `1` will never be reached.

#### Prompts

You can also use user prompts to jump between dialogue parts, based on player choice.

Example:

```
1
@narrator
Welcome to Source Academy
    prompt: Which part next?
        Part 2 -> goto 2
        Part 3 -> goto 3
        Again! -> goto 1
2
@narrator
This is part 2.
3
@narrator
This is part 3.
```

Explanation: This dialogue again has 3 parts. After playing `Welcome to Source Academy`, the player sees a user prompt with the title `Which part next?` and 3 options: `Part 2`, `Part 3` and `Again!`. If the user selects `Part 2` or `Part 3`, parts `2` or `3` of the dialogue will be played respectively. If the user selects  `Again!`, all of part `1` (including the prompt) will be played again.

Note: Prompt boxes have fixed size. Although no hard character limits are enforced, the following recommended limits should be adhered to, to avoid overflow:

* Prompt title text: 70 characters
* Prompt choice text: 15 characters
* Number of prompt choices: 5

#### Interpolation

You may interpolate student's name into the script using `{name}` which gets replaced with their Luminus names.

## Action Guide

As we've seen, actions can be performed by various entities, such as locations, objects, bounding boxes, and even during dialogues.

### Action execution order

If there is a listing of many actions, they are performed sequentially.

### Entities that can trigger actions

| Entity | How to specify/attach action | When action is triggered|
|:--|:--|:--|
|Locations | Nest point-listed actions under the location paragraph's `action` header | When location is visited|
|Objects | Nest point-listed actions under an object CSV line | When object is clicked during Explore mode |
|BoundingBoxes | Nest point-listed under a boundingBox CSV line | When boundingBox is clicked during Explore mode |
|Dialogue | Nest point-listed actions under any line in a dialogue body | Right after dialogue line is played |

### Actions API

***

##### `complete_objective(taskId)`

Marks task with `taskId` in the checklist as accomplished

Example: `complete_objective(talkToChieftain)`

Explanation: When this action is executed, the task `talkToChieftain` in Objectives is marked as completed

***

##### `preview_location(locationId)`

Previews a different location on the map

Example: `preview_location(crashsite)`

Explanation: When this action is executed, players will be shown the background and objects inside the `crashsite` location.

❗Warning: This is only a preview used for effect purposes. Internally, the player still is inside the original location.

***

##### `add_item(category, locationId, entityId)`

##### `remove_item(category, locationId, entityId)`

Adds or removes an item with id `entityId` of category `category` from a location with id `locationId`

List of possible categories and corresponding entity id.

| Category | Entity Id |
|:--|:--|
| `objects` | objectId |
| `dialogues` | dialogueId |
| `boundingBoxes` | bboxId |
| `navigation` | locationId |

***

##### `start_animation(objectId / locationId, startFrame, frameRate)`

##### `stop_animation(objectId / locationId)`

Start and stop an object or background animation.

Examples:
`start_animation(yourRoom, 3, 20)`
`stop_animation(yourRoom)`

Explanation:

* The first action acts on an entity with id `yourRoom` (entity must be an `objectId` or `locationId`). It plays an animation starting at the 3rd frame in the spritesheet and runs it at 20 frames per second. `yourRoom` must have been declared to be a  `Sprite`.
* The second action acts on the same entity with id `yourRoom` (entity must be an `objectId` or `locationId`). It stops the `yourRoom` animation if it was playing.

***

##### `change_background(locationId)`

Changes the background of the scene to the background of a particular locationId as specified under main .txt's `locations` paragraph.

***

##### `show_object_layer(show?)`

Shows or hides the object layer depending on whether show is 'true' or 'false'. Usually used to accompany `change_background` to create an effect.

***

##### `show_dialogue(dialogueId)`

Brings up the dialogue with dialogueId `dialogue` as specified under the main `dialogues` paragraph.

***

##### `add_mode(locationId, mode)`

##### `remove_mode(locationId, mode)`

Adds or removes a [game mode](#mode) from a location with locationId `locationId`.

***

##### `add_popup(objectId, position, duration?, size?)`

Adds a popup showing the image of `objectId` on a position (`left`, `right`, or `middle`) for an optional duration of `duration` milliseconds with an optional size (`small`, `medium`, or `large`)

***

##### `delay(seconds)`

Delay the execution of subsequent actions and dialogue by the specified number of seconds.

***

##### `make_object_glow(objectId, turnOn?)`

If an object with objectId `objectId` is visible on screen, then it will glow yellow to draw players' attention. Optional parameter `turnOn` can either be true or false to turn on or turn off the glow respectively.

***

##### `make_object_blink(objectId, turnOn?)`

If an object with objectId `objectId` is visible on screen, then it will blink to draw players' attention. Optional parameter `turnOn` can either be true or false to turn on or turn off the glow respectively.

***

##### `play_bgm(bgmId)`

Changes background music to bgmId `bgmId`.

***

##### `play_sfx(sfxId)`

Plays background music with sfxId `sfxId`.

***

##### `move_character(characterId, locationId, position?)`

Moves a character from its original location to a new location. You may also change its default position (left, right, center).

***

##### `navigate_to_assessment(number)`

Opens a assessment (or minigame) in another tab based on the unique string "number" provided.

***

##### `update_character(characterId, expression)`

Changes the default expression of a character.

***

### Action Repeatability

By default, actions are only performed once the first time they are triggered. Say, you want `show_dialogue(hi)` to occur every time an object is clicked. You can use the asterisk symbol to mark them as repeatable `*`

Example: `show_dialogue*(hi)`

Explanation: The action `show_dialogue*(hi)` will be performed every time object is clicked.

### Action Conditions

Action Conditions are used to specify under which precondition an action can performed. They can be written as if clauses right next to the actions.

Example: `show_dialogue(welcome) if checklist.talkToChieftain`

Explanation: The action will be the first time it's triggered and `talkToChieftain` is completed.

Example: `show_dialogue*(welcome) if checklist.talkToChieftain`

Explanation: The action will be performed every time it's triggered as long as `talkToChieftain` task has been completed.

You may chain multiple conditions using the keyword `AND`

Example: `show_dialogue*(welcome) if checklist.talkToChieftain AND userState.collectibles.cookie`

Explanation: The action will be performed only if `talkToChieftain` task has been completed, and user has the `cookie` under his collectibles list.

#### Conditions API

##### `checklist.<objectiveId>`

##### `userstate.collectibles.<collectibleId>`

## Entity Guide

What do we mean when we refer to...?

### objective

One of the tasks the player has to complete to finish the checkpoint

### location

One of the places players can to navigate to

### mode

Choices of what activities players can do upon visiting a location.

One of the following: `talk`, `explore`, `move`

### dialogue

A list of lines containing speaker and sequence of spoken lines.

### object

An image that is drawn in a certain location. This may or not be interactive in Explore Mode.

### character

A character (avatar) present in a certain location. May also be used as speaker in the dialogue.

### boundingBox

An invisible rectangle that players may possibly be able to interact with in Explore Mode.

### talkTopic

One of the topics the player can talk about in Talk Mode. Specified using a dialogue.

## Sample Checkpoint

```
startingLoc: normal

objectives
    talk

gameStartActions
    show_dialogue*(unwelcome) if !userstate.assessments.301
    show_dialogue(welcome) if userstate.assessments.301

checkpointCompleteActions
    show_dialogue(done)

locations
    normal, /locations/yourRoom-dim/normal.png, Locations Your Room Dim Normal Png

normal
    modes: explore, talk
    talkTopics: whatToDo
    actions
        show_dialogue(welcome)
    objects
        +emergency1, /objects/cmd-chair03/emergency.png, 781, 531, 318, 398
        +chieftain2, /avatars/chieftain/chieftain.angry.png, 1400, 526, 697, 744
    boundingBoxes
        +bbox#0, 536, 420, 373, 402
            show_dialogue(click)

    characters
        hartin-menz, Hartin, happy, left

dialogues
    welcome
        Congrats on creating your scene
    click
        Invisible bounding box is right here

    whatToDo, Are you wondering what to do, Cadet?
        @you
        Hmm, there's nothing to do around here.

        @hartin-menz
        There's plenty of things you can do!
```
