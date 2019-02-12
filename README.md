# Adventure Quest

Build an app that allows the user to complete a series of quests. After all quests have been completed, or if the user dies, a final outcome is determined based on how they did on their quests.

Overview

## Overview

There are four main pages:

* `index.html` - user sign-in page 
* `map.html` - the page where the user chooses an available quest
* `quest.html` - the page where the quest is presented and the
user chooses a response
* `end.html` - the page where the user's final fate as an adventurer is revealed

## Quests Data

The information about each quests should be specified via data. This is an array of object where each object describes a particular quest. Each quest object should have the same properties and specified information, though the values are different. Each object needs to have properties for:

1. Its `id`. Some up with a short string unique to that quest, like `monsters`, `dragons`, `treasure`.
1. Information to be displayed about the quest on the `quest.html` page. Could be things like title, description, image file to display, audio file to play.
1. Choice list. The question object will need to have a property that describes the choices available to the user with which to respond. Each choice is itself an object. It needs properties for:
    1. Its `id`. Short string unique to that choice
    1. Display information such as description, and what text to display if that choices is chosen
    1. Information about how to modify the user object if that choice is made.

An example quest data is provided below.

## User Sign-in Page

Prompt the user for a name. You can prompt for additional info as well (like an avatar or what kind of adventurer they are).

When the user submits their info, it should be saved. *In addition, you also need to initialize:
1. Any user properties you need to track, for example `hp` and `gold`
1. An empty `completed` object that will be used to track which quests the user has completed. 

The user should then be navigated to `map.html`.

## Map Page

This page lists the available quests. It needs the `user`, `quests`, and `completed` data. 

The quests list should be generated from the `quests` data. The user should be able to click on the quest which navigates them to the quest page, included the query string with the id: `quests.html?id=dragon`

Display user profile information including stats (like hp and gold)

This page also needs to check current state of data and adjust itself accordingly:
1. Check the `completed` object and skip generating a link for any quest the user has already completed.
1. Check if the user is dead (or other premature end condition), and if so navigate to `end.html`
1. Check if all quests have been completed, and if so navigate to `end.html`

## Quest Page

This page runs a quest.  It needs the `user`, `quests`, and `completed` data. Find the quest in `quests` data based on the query `id`/

Display user profile information including stats (like hp and gold)

Populate the display in `quest.html` based on the properties of the quest. Loop through the `quest.choices` and create the form controls for each choice (use the choice `id` as the `input` `value`).

When the user submits their choice, find the corresponding choice in `quest.choices`. Update the display with result, change the user object according to the selected `choice`,
and (re)save (you might want to reload the user profile info as well). The `completed` object needs to be updated by setting a property whose key is the `quest.id` and whose value is `true`

## End Page

This page shows the final results.  It needs the `user` data.

Display user profile information including stats (like hp and gold).

TDD one or more functions for evaluating the user object (which has status properties like `hp` and `gold`). These should be used to create a textual summary that is then presented to the user.

## Example Quest Data

Here is an example single `quest` object. Multiple objects like this would be combined to be the `quests` array.

```js
const dragon = {
    id: 'dragon',
    title: 'A Problem Dragon',
    image: 'dragon.jpg',
    audio: 'dragon.wav',
    action: 'dragon-growl.aiff',
    description: `
        You travel to a nearby village you have heard is being
        terrorized by a dragon. Sure enough as you rent a room
        in a local inn, you go outside and see the dragon about
        to lay seige! What do you do?
    `,
    choices: [{
        id: 'run',
        description: 'Get the hell out of the village',
        result: `
            You high tail it in the opposite direction. Luckily,
            in the panic you find a bag on the ground with 15 gold.
            Unluckily, you trip over a discarded wagon wheel on your
            way out of town and take 40 hp damage. 
        `,
        hp: -40,
        gold: 35
    }, {
        id: 'fight',
        description: 'Fiiiiiggghhhttt!',
        result: `
            You attempt to charge towards the dragon, who sees you approach
            and let's loose a fireball. You wake up the next morning and the
            village has been completely burned to the ground.
            Oh, and you take 45 hp damage.
        `,
        hp: -45,
        gold: 0
    }, {
        id: 'archer',
        description: 'Emulate that guy from LOR who shot an arrow',
        result: `
            Inspired by the legend of Bard the Bowman, you notice a
            stunned archer standing nearby and take their bow and quiver,
            climb to the top of a tall tower and take aim. On the dragon's
            next pass you steady your aim and let one fly. Amazingly,
            you strike the dragon in the eye, piercing into the brain and
            killing the dragon instantly. The villagers declare you their hero
            and award you 90 gold.
        `,
        hp: 0,
        gold: 90
    }]
};
```