---
title: "Thinking Things Through and Edge Cases"
slug: edge-cases
---     

The most important thing to keep in mind is that the user has to be given constant feedback. Even if you've told them that aligning three red tiles in a row is how you beat a level, you still need to give them a visual cue when they've done it right or wrong. Your game should never change state in any way without there being explicit feedback to user as to what's going on. Try and think of all the ways a user could not understand what's going on.

People will interact with your game in ways you don't expect, without the knowledge you have, in ways that aren't rational.

This means that, guaranteed, edge and corner cases will be uncovered.

An edge case is when a certain condition reaches an extreme. What happens when a player taps really really fast, or swipes to the very corner of the screen, or tries to kill every single enemy before going to the next level, etc...?

A corner case is when two or more conditions are in play or at their extremes at once. What happens when someone tries to jump and punch at the same time, or use two powerups at once, or use a hint after they've already solved a level, etc...?

Please think these things through and structure your code accordingly. The more edge and corner cases you can foresee, the easier it will be to deal with them. Playtesting often will also help uncover issues with edge cases, corner cases, and how much feedback you give users.
