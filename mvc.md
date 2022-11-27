### Data flow graph

```mermaid
flowchart TB
    view
    C[non-user facing side-effect]

    subgraph controller
        store
        A[state transition]
    end

    store -->|current model| A
    view -->|event| A[state transition]
    A -->|updated model| view
    A -->|updated model| store
    A -->|updated model| C
    C -->|event| A
```

### Sequence diagram

```mermaid
sequenceDiagram
    loop
        alt
            side-effect.view->>controller.state transition: event
        else
            side-effect.other->>controller.state transition: event
        end
        controller.store->>controller.state transition: current state
        controller.state transition->>controller.store: updated state
        controller.state transition->>side-effect.view: updated state
        controller.state transition->>side-effect.other: updated state
    end
```
