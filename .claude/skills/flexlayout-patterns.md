# FlexLayout-react Patterns

## Installation
```bash
npm install flexlayout-react
```

## Core Concepts

FlexLayout uses a JSON model to describe the layout. The model contains rows, tabsets, and tabs. You create a `Model` from JSON, render it with `<Layout>`, and provide a factory function to map tab components.

## Model JSON Structure

```typescript
import { Model, Layout, TabNode, Actions, IJsonModel } from 'flexlayout-react';

const defaultLayout: IJsonModel = {
  global: {
    tabEnableClose: true,
    tabEnableRename: false,
    splitterSize: 4,
    tabSetEnableMaximize: true,
  },
  borders: [],
  layout: {
    type: 'row',
    weight: 100,
    children: [
      {
        // Left panel (Topic Browser)
        type: 'tabset',
        weight: 20,
        children: [
          {
            type: 'tab',
            name: 'Topic Browser',
            component: 'topic_browser',  // matches panel registry type
            id: 'topic_browser_1',
          },
        ],
      },
      {
        // Right column
        type: 'row',
        weight: 80,
        children: [
          {
            // Top right (3D Viewer)
            type: 'tabset',
            weight: 60,
            children: [
              {
                type: 'tab',
                name: '3D Viewer',
                component: 'viewer_3d',
                id: 'viewer_3d_1',
              },
            ],
          },
          {
            // Bottom right (Chart + Image tabs)
            type: 'tabset',
            weight: 40,
            children: [
              {
                type: 'tab',
                name: 'Chart',
                component: 'chart',
                id: 'chart_1',
              },
              {
                type: 'tab',
                name: 'Image',
                component: 'image',
                id: 'image_1',
              },
            ],
          },
        ],
      },
    ],
  },
};
```

## Layout Component

```typescript
import { Layout, Model, TabNode, Actions, ITabSetRenderValues } from 'flexlayout-react';
import 'flexlayout-react/style/dark.css'; // base theme

function PanelContainer() {
  const [model, setModel] = useState(() => Model.fromJson(defaultLayout));

  // Factory maps component strings to React components
  const factory = (node: TabNode) => {
    const component = node.getComponent(); // returns the 'component' string from JSON
    const panelId = node.getId();
    
    switch (component) {
      case 'viewer_3d': return <Viewer3DPanel panelId={panelId} />;
      case 'chart': return <ChartPanel panelId={panelId} />;
      case 'image': return <ImagePanel panelId={panelId} />;
      case 'topic_browser': return <TopicBrowserPanel panelId={panelId} />;
      default: return <div>Unknown panel: {component}</div>;
    }
  };

  return (
    <Layout
      model={model}
      factory={factory}
      onModelChange={() => {
        // Save layout to localStorage
        localStorage.setItem('jig-layout', JSON.stringify(model.toJson()));
      }}
    />
  );
}
```

## Adding Panels Programmatically

```typescript
// Add a new tab to the active tabset
function addPanel(model: Model, panelType: string, name: string) {
  const uniqueId = `${panelType}_${Date.now()}`;
  model.doAction(
    Actions.addNode(
      {
        type: 'tab',
        component: panelType,
        name: name,
        id: uniqueId,
      },
      // Add to the first tabset found, or specify a tabset ID
      model.getActiveTabset()?.getId() ?? model.getRoot().getChildren()[0].getId(),
      DockLocation.CENTER,
      -1, // index: -1 = append
      true // select the new tab
    )
  );
}
```

## Saving and Restoring Layout

```typescript
// Save
const json = model.toJson();
localStorage.setItem('jig-layout', JSON.stringify(json));

// Restore
const saved = localStorage.getItem('jig-layout');
if (saved) {
  const model = Model.fromJson(JSON.parse(saved));
}
```

## CSS Theming

FlexLayout uses these key CSS classes (override in globals.css):

```css
/* Tab bar background */
.flexlayout__tabset_header { background: var(--surface); }

/* Individual tabs */
.flexlayout__tab_button { color: var(--text-secondary); border-radius: 6px 6px 0 0; }
.flexlayout__tab_button--selected { 
  color: var(--text-primary); 
  border-bottom: 2px solid var(--accent); 
}

/* Tab close button */
.flexlayout__tab_button_trailing { opacity: 0; transition: opacity 0.15s; }
.flexlayout__tab_button:hover .flexlayout__tab_button_trailing { opacity: 1; }

/* Splitter handles */
.flexlayout__splitter { background: var(--border); }
.flexlayout__splitter:hover { background: var(--accent); }

/* Tab content area */
.flexlayout__tab { background: var(--bg); overflow: hidden; }

/* Drag indicator */
.flexlayout__outline_rect { border: 2px solid var(--accent); }
```

## Important Notes

- FlexLayout requires a container with explicit width and height (use `flex-1` + `min-h-0` in a flex column).
- Import the base CSS (`flexlayout-react/style/dark.css`) then override with your theme.
- The `model` is mutable — `doAction` mutates it. React won't re-render unless you force it or use the `onModelChange` callback.
- Tab component strings must match exactly between JSON model and factory function.

## UNVERIFIED — Written from training knowledge. CC should verify Model.fromJson format and Actions API against flexlayout-react .d.ts types after npm install.
