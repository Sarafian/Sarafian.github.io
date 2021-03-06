---
title: PlantUML tips and tricks
excerpt: Tips and tricks for PlantUML
tags: 
  - plantuml
categories: Tips
---

I love [PlantUML]. It has it's limitations but if the size and the complexity of the diagrams can be contained, then it offers great benefits. The fact that the format is in text is so powerful in software engineering because it can be attached to any process with change management over source control. Also, it is great for [Confluence] when the [PlantUML Diagrams for Confluence] add-on is enabled.

With this post I want to share some tips and tricks from my experience using it.

# Layout orientation in component diagram
When using the component diagram, often the layout seems to have a mind of it's own. You can try to provide some guidelines to the rendering component by adding directions in the arrows (e.g. `-l->`) but when that doesn't work, I try to achieve an optimal layout by reorganizing the appearance of the components, especially when they are nested with e.g. `package`. It is a bit strange, but order in rendering seems to follow a reverse order of appearance in the script.

# Multiple and options flows in a component diagram
Sometimes, I want to have multiple flows within a component diagram. In this case and depending on the case, I use color coding with each arrow (e.g. `A -> B #Green : text`) and possibly add a legend at the end

```
@startuml

package "Some Group" {
  HTTP - [First Component]
  [Another Component]
}

node "Other Groups" {
  FTP - [Second Component]
  [First Component] --> FTP
}

cloud {
  [Example 1]
}


database "MySql" {
  folder "This is my folder" {
    [Folder 3]
  }
  frame "Foo" {
    [Frame 4]
  }
}

[Another Component] --> [Example 1] #Blue
[Example 1] --> [Folder 3] #Blue
[Folder 3] --> [Frame 4] #Blue


legend
    | Color | Flow |
    |<#Red>|  Flow 1 |
    |<#Blue>|  Flow 2 |
endlegend

@enduml
```

![Color coded legend](/assets/images/posts/plantuml/2021-02-24-plantuml-tips-tricks-1/legend.svg "Color coded legend")

When sequence is important then I use a numbering scheme like this

- Flow 1
  - `1.1 action`
  - `1.2 action`
- Flow 2
  - `2.1 action`
  - `2.2 action`

# Autonumber events in sequence diagram

One of the best features of sequence diagrams is the `autonumber` feature which will add a number before the text of each event as explained in the documentation

```
@startuml

autonumber
Bob -> Alice : Authentication Request
Bob <- Alice : Authentication Response

@enduml
```

Autonumbers can be formatted. In the following example the number is 2-digit padded and highlighted.


```
@startuml

autonumber "<B>[00]"
Bob -> Alice : Authentication Request
Bob <- Alice : Authentication Response

@enduml
```

| Mode | Diagram |
| ---- | ------- |
| simple | ![Simple autonumber](/assets/images/posts/plantuml/2021-02-24-plantuml-tips-tricks-1/autonumber.svg "Simple autonumber") |
| formatted | ![Formatted autonumber](/assets/images/posts/plantuml/2021-02-24-plantuml-tips-tricks-1/autonumber-formatted.svg "Formatted autonumber") |

I prefer the `autonumber "<B>[00]"` the most because I've never had to exceed 99 events in a diagram. Besides, when my diagrams become big, they are usuall split by a deviders and seperators (`==`) and for each I restart the the numbering in the sequence.

# Text alignment in sequence diagram

In the above example, the lines can be brought closer together to make the diagram more dense with this skin parameter `skinparam responseMessageBelowArrow true`.

```
@startuml

skinparam responseMessageBelowArrow true

autonumber "<B>[00]"
Bob -> Alice : Authentication Request
Bob <- Alice : Authentication Response

@enduml
```

| Mode | Diagram |
| ---- | ------- |
| default | ![Default](/assets/images/posts/plantuml/2021-02-24-plantuml-tips-tricks-1/autonumber-formatted.svg "Default") |
| responseMessageBelowArrow | ![Response below arrow](/assets/images/posts/plantuml/2021-02-24-plantuml-tips-tricks-1/response-below-arrow.svg "Response below arrow") |

# Semi transparent group backgrounds in sequence diagrams

I like to group my participants in boxes and color code them because when the sequence is long, you can quickly identify the scope of the vertical you are looking at even when the participants are out of view. Problem is that every one of the `Grouping message` features (`group`,`alt/else`,`opt`,`loop`,`par`,`break`,`critical` and `group`) are rendered by default with a white solid background which hides the color of the box. This is annoying when the group overlaps many verticals and becomes one massive big white background. A solution for this is not easy. You could use the `#Transparent` but then the entire group's background becomes transparent and when big the context is lost.

This was not easy to find, but [PlantUML] skinning supports alpha channel and the background can be set through the skin. [Skin Parameters] are not  documented extensively and I was able to find the proper one `SequenceGroupBodyBackgroundColor` through this [page][All Skin Parameters]. After some experimentation, I've decided to use white with some tranparency using this value `#FFFFFF90`.


```
@startuml

skinparam SequenceGroupBodyBackgroundColor #FFFFFF90

box "Internal Service" #LightBlue
    participant Bob
    participant Alice
end box

box "Other" #LightGreen
    participant Other
end box

group group
    Bob -> Alice : hello
    Alice -> Other : hello
end

@enduml
```

| Mode | Diagram |
| ---- | ------- |
| default | ![Default group background](/assets/images/posts/plantuml/2021-02-24-plantuml-tips-tricks-1/solid-group.svg "Default group background") |
| transparent | ![Transparent group background](/assets/images/posts/plantuml/2021-02-24-plantuml-tips-tricks-1/transparent-group.svg "Transparent group background") |
| semi-transparent | ![Semi-transparent group background](/assets/images/posts/plantuml/2021-02-24-plantuml-tips-tricks-1/semi-transparent-group.svg "Semi-transparent group background") |

# Solve cut off issue

When using the default [PlantUML] server to render the diagrams in `png`, often the diagrams are cut off because [PlantUML] limits image width and height to 4096. This issue is discussed on github [issue][GH-136] and the solution is to either use command line parameters or use the `skinparam dpi X` parameter. When using the [VSCode PlantUML] add-on with the default server, this is the only solution as current the add-on doesn't support command line parameters. Unfortunately, the `X` for the dpi needs to be found by experimentation to make sure the diagram fits. As the diagram grows horizontally, the parameter needs to be adjusted accordingly.


# Help with skinning

[PlantUML] supports many different colors by name but it is kind of a maze. Use the `colors` command to render a picture with all colors

```
@startuml

colors

@enduml
```

![PlantUML Colors](/assets/images/posts/plantuml/2021-02-24-plantuml-tips-tricks-1/colors.svg "PlantUML Colors")

Unfortunately the [Skin Parameters] are not very well documented and the best comprehensive list is in the [All Skin Parameters] page.


[VSCode PlantUML]: https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml
[PlantUML]: https://plantuml.com/
[Confluence]: https://www.atlassian.com/software/confluence
[PlantUML Diagrams for Confluence]: https://marketplace.atlassian.com/apps/1215115/plantuml-diagrams-for-confluence?hosting=cloud&tab=overview
[Skin Parameters]: https://plantuml.com/skinparam
[All Skin Parameters]: https://plantuml-documentation.readthedocs.io/en/latest/formatting/all-skin-params.html
[GH-136]: https://github.com/qjebbs/vscode-plantuml/issues/136