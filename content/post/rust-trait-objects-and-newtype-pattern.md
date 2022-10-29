+++
title = "Rust: Trait Objects and the Newtype Pattern"
draft = false
+++

## Introduction {#introduction}

I built a simple Hacker News wrapper in Rust to learn about API design principles. Along the way I also discovered the [Newtype pattern](https://rust-unofficial.github.io/patterns/patterns/behavioural/newtype.html) which provides type safety and encapsulation. The following post is a summary of learnings:

I set a small challenge of building a wrapper around the Hacker News API. To keep the project simple and focus on the goal of studying idiomatic Rust, I only aggregated the top stories (i.e. frontpage matter) and ignored all other site categories (polls, jobs, etc.).


## Understanding the data structure {#understanding-the-data-structure}

-   Stories can have _many_ children
-   Comments can have _many_ children
-   Comments can have _only one_ parent

Both sotries and comments are "items". You may be able to spot a simple rule here:

-   Stories are _always_ parents. Comments can be either parents _or_ children.


## Defining the item type {#defining-the-item-type}
