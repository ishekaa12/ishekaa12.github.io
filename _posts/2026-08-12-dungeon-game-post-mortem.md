---
layout: post
title: "I Built a Dungeon Game in C and Finally Understood Pointers"
date: 2026-08-12
categories: [tech, c, dsa]
---

Remember how I said I still don't know what a pointer is? Lied a little. I built an entire dungeon game to find out.

It's called **Dungeon of the Lost King** — a text-adventure where you walk through rooms, pick things up, and hunt down an Ancient Amulet. The game itself is not the point. The point is that to make it work, I had to actually use pointers, structs, linked lists, and 2D arrays instead of just reading about them.

## The Map: Rooms That Point to Other Rooms

Every room in the game is a struct, and every struct has four pointers in it — one for each direction:

```c
typedef struct Room {
    char name[50];
    char description[200];
    int is_visited;
    int map_row;
    int map_col;

    struct Room *north;
    struct Room *south;
    struct Room *east;
    struct Room *west;

    Item *items;
} Room;
```

That `struct Room *north` sitting inside `Room` is the self-referential part — a room holding a pointer to another room of the same type. Before this, "self-referential struct" was just a phrase in a slide. Here it's just... how you tell a room where the exits go.

Connecting them ended up being three lines:

```c
entrance->north = corridor;
corridor->south = entrance;
corridor->north = chamber;
chamber->south  = corridor;
```

Right now my dungeon is basically a straight line — Entrance → Corridor → Chamber, north/south only. I never used east/west, they're just sitting there unused in every room. That's the honest version: I built four directions and only needed one axis. Next version, I want an actual branching layout, which is the whole reason I left east/west in the struct instead of ripping them out.

There's also an `all_rooms[MAP_ROWS][MAP_COLS]` grid sitting alongside the pointer connections — a 2D array of `Room *`, indexed by `map_row` and `map_col`, that only exists to draw the ASCII map (`[YOU]`, `[X]` for visited, `[?]` for unknown). So there are technically two systems tracking the dungeon layout at once: the pointers for actual navigation, and the grid for drawing it. That felt redundant while I was writing it, but it's also the cleanest way I found to separate "how the player moves" from "how the map gets displayed."

## Inventory: Why a Linked List

This is the decision I actually had to think about, because an array would've been simpler to type out.

The problem: I didn't know how many items a room would hold, or how many the player would end up carrying. A linked list doesn't care — you just allocate a new node and point the old head at it:

```c
void add_item(Item **head, char *name, int weight) {
    Item *new_item = malloc(sizeof(Item));
    strcpy(new_item->name, name);
    new_item->weight = weight;
    new_item->next = *head;
    *head = new_item;
}
```

The part I'm actually proud of is `move_item`. My first instinct for "take an item" was: remove it from the room's list and free it, then malloc a fresh node for the inventory list. Which works, but it's wasteful — you're destroying a perfectly good node just to rebuild an identical one two lines later.

What I did instead:

```c
void move_item(Item **from, Item **to, char *name) {
    Item *current = *from;
    Item *prev = NULL;

    while (current) {
        if (strcmp(current->name, name) == 0) {
            if (prev == NULL)
                *from = current->next;
            else
                prev->next = current->next;

            current->next = *to;
            *to = current;
            return;
        }
        prev = current;
        current = current->next;
    }
    printf("  '%s' not found.\n", name);
}
```

No malloc, no free. Same node, unhooked from one list and re-hooked into another. `take` and `drop` are the same function with the arguments flipped. That's the moment linked lists stopped being "a thing with `next` pointers I get tested on" and started being "a thing I can actually restructure on the fly without copying data around."

## The Win Condition Is Just a List Search

Once I had `has_item()`, the entire win condition was one check tucked into the `take` command:

```c
if (has_item(player.inventory, "Amulet")) {
    printf("YOU HAVE WON THE DUNGEON!\n");
    running = 0;
}
```

No separate "check for win" system, no flags scattered around. If the Amulet is in your inventory linked list, you've won — because you can only get it there by walking the whole dungeon and typing `take Amulet`. The data structure was already doing the game logic for me.

## Cleaning Up After Myself

The unglamorous but important part — freeing everything before the program exits:

```c
free_items(player.inventory);
free_items(entrance->items);
free_items(corridor->items);
free_items(chamber->items);
free(entrance);
free(corridor);
free(chamber);
```

Every `malloc` in this game — for rooms and for items — gets a matching `free`. I didn't get that right on the first try. Early on I was freeing item nodes inside `drop_item` while also trying to reuse them elsewhere in a different version of the same function, which is exactly the kind of use-after-free bug that C will let you write without complaint and then punish you for silently later. Splitting the logic into "move between lists" (no free) versus "actually done with this item" (free) is what fixed it.

## What I'd Do Differently Now

- Actually use east/west, not just declare them
- Get rid of the redundant `all_rooms` grid and derive the map purely from the pointer structure
- Add a `capacity` or weight limit check using the `weight` field I'm already storing on every item but never actually using for anything

## Do I Know What a Pointer Is Now?

Enough to build with one. Enough to know that `struct Room *north` is not scary, it's just a room telling you where the next room is. Still not enough to explain a segfault to a confused roommate at 2 AM without drawing a diagram first. Progress, not mastery.

Next up: probably something DSA-related, since that's what's eating most of my brain space right now.
