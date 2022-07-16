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

A copy of option B, but without standard file names for specific fields.

In this case `metadata.yaml` is where content-adjacent information is stored. While `body` is the actual content body, in the format that makes most sense.

In this example, `data.json` is an arbitrary object that would be provided with the content to the mechanism that will render it.

This example also has a `file.bin`, a fake binary file, showcasing how you could attach arbitrary files and provide them as needed (to rendering, as part of the module output, as part of the book, etc). This would probably be done by encoding the file into a format that can be serialized with json or yaml, `base64` is the most likely contender.

### Option D

Similar to option B, but each file mapping to a single property.

Each file name maps to a single property of the content structure. This means that the contents of the `id.*` file will be set as the values of the content `.id` property.

### Option E

A mix between option B and C. With the caveat that the `_` filename is special and places the content at the root of the output.

## Conclusions

From the three options listed, option A is the most self-contained, but option C is the most flexible one.

A big advantage of option A, is that since each content piece is self-contained, organizing them is straightforward. However, this means that other mature tools to manage and create content types (markdown-lint, for example), would not work out of the box. This is a big disadvantage, since it would require development to make working with these files smooth. Therefore, option A is discarded.

Option B seems to be a great way of managing things we already know, but would cripple flexibility. Since it requires standard files to contain specific parts of the content, handling this definition would have to be dynamic and extensible, making the initial "standard" pretty flimsy.

Option C and D are more flexible, but they suffer from other problems. Option C is hard to manage, since any filename would be accepted, but there is no definition of what the bare minimum are. Option D on the other hand, by mapping file contents to a single file suffer from overly distributed content (files with a single line of text, for example).

To this end, the chosen option is option E: A modification of option D with the following rules:

- The content of a file named `_` will be interpreted as the root of the content.
  - This means that a file `_.yaml` with contents:
    ```yaml
    id: a
    title: b
    data:
      - a
      - b
      - c
    ```
     will produce an output as follows:
    ```json
    {
      "id": "a",
      "title": "b",
      "data": [
        "a",
        "b",
        "c"
      ]
    ```
- The contents of any file will be placed on the root of the content document under a field with the same name as the filename
  - This means a file `test.json` will create a `test` field in the output with it's contents copied into it.