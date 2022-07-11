# Defining structure for content authoring

Refers to [#1](https://github.com/powerd6/experiments/issues/1).

> When writing content, it is important to take into account the following things:
> 
> - Each piece of content should be referable by some unique identifier
> - Contents can be written by one or multiple authors
> - Contents can contain:
>     - text
>     - images
>     - files
>     - links
> - Contents of different types may need to be displayed differently (but assume most of them won't)


## Options

### Option A

A single self-containing file with markdown text inlined.

Some markdown syntax extensions can be expected (like github's table syntax).

### Option B

A collection of files that get combined before shipping.

This would allow each part of the content to be written on the most appropriate format (like `body` being a markdown file).

### Option C

A copy of option B, but with standard file names for specific fields.

In this case `metadata.yaml` is where content-adjacent information is stored. While `body` is the actual content body, in the format that makes most sense.

In this example, `data.json` is an arbitrary object that should be provided with the content to the mechanism that will render it.

This example also has a `file.bin`, a fake binary file, showcasing how you could attack arbitrary files and provide them as needed (to rendering, as part of the module output, as part of the book, etc). This would probably be done by encoding the file into a format that can be serialized with json or yaml, `base64` is the most likely contender.

## Conclusions

`Explain what were the things that were noticed and what conclusions were drawn`