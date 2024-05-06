---
author: 'Alex'
title: 'scooze - Building Better Tooling for Magic: the Gathering'
date: 2024-05-02
draft: false
tags:
    - magic
    - scooze
---

Since last summer, I have been working with some friends to create better tools for developers working with Magic: the Gathering data. Card and deck data has historically been unapproachable for independent projects because of the amount of set up required just to represent these game objects. [scooze](https://github.com/arcavios/scooze) aims to fix that!

``` python
from scooze import Card, Deck, Color

card = Card("Python", colors=[Color.Black])
deck = Deck(archetype="My Deck", cards={card: 2})
deck.add_card(card, 2)
deck.export()
```

We also support developing with scooze using Docker. With scooze running in Docker, you can take advantage of all the data Scryfall has to offer, but without the hassle of making external requests and dealing with rate limits.

``` shell
scooze setup docker
scooze load cards --oracle
```

Our database models and dataclasses will automatically validate Scryfall data for you and take care of all the annoying parts of building Magic apps.

``` python
from scooze import Color, CardList
from scooze.api import ScoozeApi

with ScoozeApi() as s:
    green_cards = s.get_cards_by("colors", [Color.GREEN])
    for c in green_cards:
        print(f"{c.name} {c.mana_cost}")
```
