# DiscoElysiumTwineMacros
A copy of a fangame I created for Disco Elysium in Twine using Sugarcube 2.30.0 and a set of custom macros. The intent is that you can use this story as a jumping point for your own project, with the same macros.

A short set of instructions follow on how to use the macros. The game itself can be played on itch at: https://apepers.itch.io/re-hearsed-a-disco-elysium-fanwork

# Setting up your own Disco Elysium Twine Fangame
You will need a recent version of Twine (this was created using 2.3.5).
You can then download Re-Hearsed.html from this repo and import it as a new story into Twine.

There are a couple options at this point:
- Delete the story content while keeping the macros, rename the story to your own title, and work from there
- Re-create all the macros and javascript/CSS by copy-pasting their internals to your own new story, which must be using the Sugarcube 2.30.0 format.

The relevant passages are grouped together in the top-left but to list them, you will need: StoryInit, PassageHeader, PassageFooter, Speaker, PassiveSkill, DisplayOptions, SetSpeaker, AddOption, DisplayParagraph, and AddParagraph.

You will also need the custom story javascript and story CSS.

(Ideally I will have the time to convert all of this into Twee to make this workflow easier, but for now, it's the best I'm aware of as you can't copy-paste passages between stories)

# Using the macros
Once your project is set up, most of the functionality around continue buttons, displaying previously chosen options, and formatting passive skill checks is handled for you - as long as you use the provided macros. There is a small set that provides the full functionality of this game.

1. SetSpeaker and AddParagraph
These two macros make up the bulk of the writing. SetSpeaker is used to set up who or what the dialogue will belong to, such as "Kim Kitsuragi" or "Sunken Motor Carriage". This will overwrite whoever the currently set speaker is - you should always make sure to use at the start of a new passage to ensure it's set up correctly no matter where you're coming from.

So an example beginning of a passage would be:
<<SetSpeaker "Alexei Pepers">>
<<AddParagraph "\"I hope you have an easy time using this plugin!\" she says, cheerfully.">>

Note that as the text inside of macros is specified with double-quotes, you will need to escape any double-quotes you wish to appear in the text itself when a character is speaking, \"Like so \"

Also note that if you want to break up multiple paragraphs with the same speaker, it is unnecessary to set the speaker each time - it will stay whatever it was last set to.

2. PassiveSkill
The other macro used for dialogue is the PassiveSkill macro, which represents skill checks appearing inside of dialogue. It is basically a special kind of SetSpeaker macro that sets your speaker to be a particular skill. You follow it up with your usual AddParagraph macro containing the actual text of the skill check. You can reference any of the defined Disco Elysium skills and it will use an appropriate colour for the text. You can add your own skills too - see later in this guide.

There are two ways to include a passive skill:
<<PassiveSkill "Logic">>
<<AddParagraph "Like this, as a free aside.">>

<<PassiveSkill "Logic" "Easy" "Success">>
<<AddParagraph "Or like this, as a check with a difficult and success or failure.">>
<<PassiveSkill "Volition" "Legendary" "Failure">>
<<AddParagraph "Currently you as the write specify success or failure - real dice rolls may be incoming, but not at the moment.">>

Remember that the PassiveSkill is setting the current speaker, so if after a skill check you return to a previously speaking character, you will need to use SetSpeaker again.

3. AddOption
The final macro is how we give the player options. The AddOption macro takes in the text to be displayed for the option, and the name of the passage that it should link to, like so:
<<AddOption "Move to the next part of the tutorial" "NextTutorialPassage">>
<<AddOption "\"Remember, you need to escape double-quotes if this is spoken word!\"" "FriendlyReminderPassage">>

However many options you add, they will all be listed at the bottom of the page without any further calls on your part.
The downside of using macros for this is that you will not automatically get a visual link between passages in Twine. For my own convenience, what I do is always create a matching empty link to the correct passage immediately after the option, like so:
<<AddOption "It's annoying, but it works!" "SorryDevPassage">>[[|SorryDevPassage]]

Now you will see the link in Twine, but because the text is empty it will not mess with any clicking.

4. Whitespace
Because this format is attempting to replicate such a specific visual style, the macros are responsible for setting up the specific layout and whitespace of the text. This means that our passage itself is just a series of macro calls, and any whitespace inside of it should be ignored. You can do this a few ways, but the easiest is that for any passage of your story, add the tag "nobr". You can then use as many newlines as required to make the passage legible, without impacting the formatting of the final story.

(If you know a better way to handle this, feel free to Tweet at me or submit a pull request, I've only used Twine once before so my solutions work but are I'm sure not ideal!)

5. Tips and Tricks
Those are all the macros you need in order to write with this tool. As some parting notes:
* This is still running as Sugarcube, and there is nothing stopping you from using variables or any other default Twine features, even though I used them lightly in my own game. You may get into trouble if you try to output text in a way that doesn't mesh with the current macros, but any kind of conditions or logic should work as normal.
* The page is scrolling to handle mobile support, so nothing is stopping you from adding as many paragraphs to a given passage as you want - however, any time you want a player response, it will have to link to a new passage barring some extra work on your part.
* In my experience writing this, the most common failure points are un-escaped double-quotes and missed 'nobr' tags. Both will be immediately evident when you view the game, so test often and if you see something funky with your spacing or missing text, check your escaped characters and your whitespace.

6. Adding your own skill checks
Inside of the StoryInit passage there are arrays of strings defining the skills associated with each attribute in Disco Elysium. The PassiveSkill macro then looks for a match between the skill name it's given and each array, and then adds a CSS class to the speaker based on which attribute it matches. 
So if you want to modify or add skills and still use the same attributes, simply change the arrays in StoryInit. You can now specify your custom skills in PassiveSkill macros and will see the correct text colour.
If you want to also add a new attribute, there are a few more steps:
1. Open up the Story Stylesheet and find the existing attribute definitions and add a new definition with your new attribute name and the new colour. 
2. Add a new array of the skill names for your new attribute inside of StoryInit. 
3. Inside of the PassiveSkill macro, add another set of logic checking for your new attribute just like how the current attributes work, and apply your new tag you set up in the CSS.

I hope some people enjoy this, if you make something fun please share it!
