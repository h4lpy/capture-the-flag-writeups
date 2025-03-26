# Challenge 01 | Hacking Chess

## Overview

A clean and fair game of chess. Careful though, this is not a game for grandmasters to win.

Hint: Don't make this game harder than it needs to be.

## Walkthrough

The challenge here is to beat the AI at chess and retrieve the flag.

![[ep000_01_hacker_chess.png]]

However, as we play the game, the AI cheats and changes some of its pieces to queens:

![[ep000_01_cheating_ai.png]]

As shown on the bottom-right of the screen, there is a "**Master Login**" section:

![[ep000_01_admin_panel.png]]

We can bypass this login via SQLi (SQL Injection) using `admin' or 1=1 -- -` as our payload and entering a dummy password.  Once bypassed, we are presented with a configuration form:

![[ep000_01_chess_config.png]]

We can therefore **disable** the AI queen cheats and use a chess solver, such as [NextChessMove](https://nextchessmove.com/), to beat the AI and retrieve the flag:

![[ep000_01_flag.png]]

Flag: `https://h4ck1ng.google/solve/h4cker_a1_d3f34t3d_n1c3`
