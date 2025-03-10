### Excalidraw

Excalidraw exported as a component to directly embed in your projects.

### Installation

You can use npm

```
npm install react react-dom @excalidraw/excalidraw
```

or via yarn

```
yarn add react react-dom @excalidraw/excalidraw
```

After installation you will see a folder `excalidraw-assets` in `dist` directory which contains the assets needed for this app.

Move the folder `excalidraw-assets` to the path where your assets are served.

By default it will try to load the files from `https://unpkg.com/@excalidraw/excalidraw/{currentVersion}/dist/`

If you want to load assets from a different path you can set a variable `window.EXCALIDRAW_ASSET_PATH` to the url from where you want to load the assets.

### Demo

[Try here](https://codesandbox.io/s/excalidraw-ehlz3).

### Usage

#### Using Web Bundler

If you are using a Web bundler (for instance, Webpack), you can import it as an ES6 module as shown below

<details><summary><strong>View Example</strong></summary>

```js
import React, { useEffect, useState, useRef } from "react";
import Excalidraw from "@excalidraw/excalidraw";
import InitialData from "./initialData";

import "./styles.scss";

export default function App() {
  const excalidrawRef = useRef(null);
  const excalidrawWrapperRef = useRef(null);
  const [dimensions, setDimensions] = useState({
    width: undefined,
    height: undefined,
  });

  const [viewModeEnabled, setViewModeEnabled] = useState(false);
  const [zenModeEnabled, setZenModeEnabled] = useState(false);
  const [gridModeEnabled, setGridModeEnabled] = useState(false);

  useEffect(() => {
    setDimensions({
      width: excalidrawWrapperRef.current.getBoundingClientRect().width,
      height: excalidrawWrapperRef.current.getBoundingClientRect().height,
    });
    const onResize = () => {
      setDimensions({
        width: excalidrawWrapperRef.current.getBoundingClientRect().width,
        height: excalidrawWrapperRef.current.getBoundingClientRect().height,
      });
    };

    window.addEventListener("resize", onResize);

    return () => window.removeEventListener("resize", onResize);
  }, [excalidrawWrapperRef]);

  const updateScene = () => {
    const sceneData = {
      elements: [
        {
          type: "rectangle",
          version: 141,
          versionNonce: 361174001,
          isDeleted: false,
          id: "oDVXy8D6rom3H1-LLH2-f",
          fillStyle: "hachure",
          strokeWidth: 1,
          strokeStyle: "solid",
          roughness: 1,
          opacity: 100,
          angle: 0,
          x: 100.50390625,
          y: 93.67578125,
          strokeColor: "#c92a2a",
          backgroundColor: "transparent",
          width: 186.47265625,
          height: 141.9765625,
          seed: 1968410350,
          groupIds: [],
        },
      ],
      appState: {
        viewBackgroundColor: "#edf2ff",
      },
    };
    excalidrawRef.current.updateScene(sceneData);
  };

  return (
    <div className="App">
      <h1> Excalidraw Example</h1>
      <div className="button-wrapper">
        <button className="update-scene" onClick={updateScene}>
          Update Scene
        </button>
        <button
          className="reset-scene"
          onClick={() => {
            excalidrawRef.current.resetScene();
          }}
        >
          Reset Scene
        </button>
        <label>
          <input
            type="checkbox"
            checked={viewModeEnabled}
            onChange={() => setViewModeEnabled(!viewModeEnabled)}
          />
          View mode
        </label>
        <label>
          <input
            type="checkbox"
            checked={zenModeEnabled}
            onChange={() => setZenModeEnabled(!zenModeEnabled)}
          />
          Zen mode
        </label>
        <label>
          <input
            type="checkbox"
            checked={gridModeEnabled}
            onChange={() => setGridModeEnabled(!gridModeEnabled)}
          />
          Grid mode
        </label>
      </div>
      <div className="excalidraw-wrapper" ref={excalidrawWrapperRef}>
        <Excalidraw
          ref={excalidrawRef}
          width={dimensions.width}
          height={dimensions.height}
          initialData={InitialData}
          onChange={(elements, state) =>
            console.log("Elements :", elements, "State : ", state)
          }
          onPointerUpdate={(payload) => console.log(payload)}
          onCollabButtonClick={() =>
            window.alert("You clicked on collab button")
          }
          viewModeEnabled={viewModeEnabled}
          zenModeEnabled={zenModeEnabled}
          gridModeEnabled={gridModeEnabled}
        />
      </div>
    </div>
  );
}
```

To view the full example visit :point_down:

[![Edit excalidraw](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/excalidraw-ehlz3?fontsize=14&hidenavigation=1&theme=dark)

Since Excalidraw doesn't support server side rendering yet so you will have to make sure the component is rendered once host is mounted.

```js
import { useState, useEffect } from "react";
export default function IndexPage() {
  const [Comp, setComp] = useState(null);
  useEffect(() => {
    import("@excalidraw/excalidraw").then((comp) => setComp(comp.default));
  });
  return <>{Comp && <Comp />}</>;
}
```

</details>

#### In Browser

To use it in a browser directly:

You will need to make sure `react`, `react-dom` is available as shown below.

<details><summary><strong>View Example</strong></summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Excalidraw in browser</title>
    <meta charset="UTF-8" />
    <script src="https://unpkg.com/react@16.14.0/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16.13.1/umd/react-dom.development.js"></script>

    <script
      type="text/javascript"
      src="https://unpkg.com/@excalidraw/excalidraw@0.5.0/dist/excalidraw.min.js"
    ></script>
  </head>

  <body>
    <div class="container">
      <h1>Excalidraw Embed Example</h1>
      <div id="app"></div>
    </div>
    <script type="text/javascript" src="src/index.js"></script>
  </body>
</html>
```

```js
/*eslint-disable */
import "./styles.css";
import InitialData from "./initialData";

const App = () => {
  const excalidrawRef = React.useRef(null);
  const excalidrawWrapperRef = React.useRef(null);
  const [dimensions, setDimensions] = React.useState({
    width: undefined,
    height: undefined,
  });

  const [viewModeEnabled, setViewModeEnabled] = React.useState(false);
  const [zenModeEnabled, setZenModeEnabled] = React.useState(false);
  const [gridModeEnabled, setGridModeEnabled] = React.useState(false);

  React.useEffect(() => {
    setDimensions({
      width: excalidrawWrapperRef.current.getBoundingClientRect().width,
      height: excalidrawWrapperRef.current.getBoundingClientRect().height,
    });
    const onResize = () => {
      setDimensions({
        width: excalidrawWrapperRef.current.getBoundingClientRect().width,
        height: excalidrawWrapperRef.current.getBoundingClientRect().height,
      });
    };

    window.addEventListener("resize", onResize);

    return () => window.removeEventListener("resize", onResize);
  }, [excalidrawWrapperRef]);

  const updateScene = () => {
    const sceneData = {
      elements: [
        {
          type: "rectangle",
          version: 141,
          versionNonce: 361174001,
          isDeleted: false,
          id: "oDVXy8D6rom3H1-LLH2-f",
          fillStyle: "hachure",
          strokeWidth: 1,
          strokeStyle: "solid",
          roughness: 1,
          opacity: 100,
          angle: 0,
          x: 100.50390625,
          y: 93.67578125,
          strokeColor: "#c92a2a",
          backgroundColor: "transparent",
          width: 186.47265625,
          height: 141.9765625,
          seed: 1968410350,
          groupIds: [],
        },
      ],
      appState: {
        viewBackgroundColor: "#edf2ff",
      },
    };
    excalidrawRef.current.updateScene(sceneData);
  };

  return React.createElement(
    React.Fragment,
    null,
    React.createElement(
      "div",
      { className: "button-wrapper" },
      React.createElement(
        "button",
        {
          className: "update-scene",
          onClick: updateScene,
        },
        "Update Scene",
      ),
      React.createElement(
        "button",
        {
          className: "reset-scene",
          onClick: () => excalidrawRef.current.resetScene(),
        },
        "Reset Scene",
      ),
      React.createElement(
        "label",
        null,
        React.createElement("input", {
          type: "checkbox",
          checked: viewModeEnabled,
          onChange: () => setViewModeEnabled(!viewModeEnabled),
        }),
        "View mode",
      ),
      React.createElement(
        "label",
        null,
        React.createElement("input", {
          type: "checkbox",
          checked: zenModeEnabled,
          onChange: () => setZenModeEnabled(!zenModeEnabled),
        }),
        "Zen mode",
      ),
      React.createElement(
        "label",
        null,
        React.createElement("input", {
          type: "checkbox",
          checked: gridModeEnabled,
          onChange: () => setGridModeEnabled(!gridModeEnabled),
        }),
        "Grid mode",
      ),
    ),
    React.createElement(
      "div",
      {
        className: "excalidraw-wrapper",
        ref: excalidrawWrapperRef,
      },
      React.createElement(Excalidraw.default, {
        ref: excalidrawRef,
        width: dimensions.width,
        height: dimensions.height,
        initialData: InitialData,
        onChange: (elements, state) =>
          console.log("Elements :", elements, "State : ", state),
        onPointerUpdate: (payload) => console.log(payload),
        onCollabButtonClick: () => window.alert("You clicked on collab button"),
        viewModeEnabled: viewModeEnabled,
        zenModeEnabled: zenModeEnabled,
        gridModeEnabled: gridModeEnabled,
      }),
    ),
  );
};

const excalidrawWrapper = document.getElementById("app");

ReactDOM.render(React.createElement(App), excalidrawWrapper);
```

To view the full example visit :point_down:

[![Edit excalidraw-in-browser](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/excalidraw-in-browser-tlqom?fontsize=14&hidenavigation=1&theme=dark)

</details>

### Props

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| [`width`](#width) | Number | `window.innerWidth` | The width of Excalidraw component |
| [`height`](#height) | Number | `window.innerHeight` | The height of Excalidraw component |
| [`onChange`](#onChange) | Function |  | This callback is triggered whenever the component updates due to any change. This callback will receive the excalidraw elements and the current app state. |
| [`initialData`](#initialData) | <pre>{elements?: <a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a>, appState?: <a href="https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L37">AppState<a> } </pre> | null | The initial data with which app loads. |
| [`ref`](#ref) | [`createRef`](https://reactjs.org/docs/refs-and-the-dom.html#creating-refs) or [`callbackRef`](https://reactjs.org/docs/refs-and-the-dom.html#callback-refs) or <pre>{ current: { readyPromise: <a href="https://github.com/excalidraw/excalidraw/blob/master/src/utils.ts#L317">resolvablePromise</a> } }</pre> |  | Ref to be passed to Excalidraw |
| [`onCollabButtonClick`](#onCollabButtonClick) | Function |  | Callback to be triggered when the collab button is clicked |
| [`isCollaborating`](#isCollaborating) | `boolean` |  | This implies if the app is in collaboration mode |
| [`onPointerUpdate`](#onPointerUpdate) | Function |  | Callback triggered when mouse pointer is updated. |
| [`onExportToBackend`](#onExportToBackend) | Function |  | Callback triggered when link button is clicked on export dialog |
| [`langCode`](#langCode) | string | `en` | Language code string |
| [`renderFooter `](#renderFooter) | Function |  | Function that renders custom UI footer |
| [`viewModeEnabled`](#viewModeEnabled) | boolean |  | This implies if the app is in view mode. |
| [`zenModeEnabled`](#zenModeEnabled) | boolean |  | This implies if the zen mode is enabled |
| [`gridModeEnabled`](#gridModeEnabled) | boolean |  | This implies if the grid mode is enabled |
| [`libraryReturnUrl`](#libraryReturnUrl) | string |  | What URL should [libraries.excalidraw.com](https://libraries.excalidraw.com) be installed to |
| [`theme`](#theme) | `light` or `dark` |  | The theme of the Excalidraw component |
| [`name`](#name) | string |  | Name of the drawing |

#### `width`

This props defines the `width` of the Excalidraw component. Defaults to `window.innerWidth` if not passed.

#### `height`

This props defines the `height` of the Excalidraw component. Defaults to `window.innerHeight` if not passed.

#### `onChange`

Every time component updates, this callback if passed will get triggered and has the below signature.

```js
(excalidrawElements, appState) => void;
```

1.`excalidrawElements`: Array of [excalidrawElements](https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78) in the scene.

2.`appState`: [AppState](https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L37) of the scene

Here you can try saving the data to your backend or local storage for example.

#### `initialData`

This helps to load Excalidraw with `initialData`. It must be an object or a [promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise) which resolves to an object containing the below optional fields.

| Name | Type | Descrption |
| --- | --- | --- |
| `elements` | [ExcalidrawElement[]](https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78) | The elements with which Excalidraw should be mounted. |
| `appState` | [AppState](https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L37) | The App state with which Excalidraw should be mounted. |
| `scrollToContent` | boolean | This attribute implies whether to scroll to the nearest element to center once Excalidraw is mounted. By default, it will not scroll the nearest element to the center. Make sure you pass `initialData.appState.scrollX` and `initialData.appState.scrollY` when `scrollToContent` is false so that scroll positions are retained |

```json
{
  "elements": [
    {
      "type": "rectangle",
      "version": 141,
      "versionNonce": 361174001,
      "isDeleted": false,
      "id": "oDVXy8D6rom3H1-LLH2-f",
      "fillStyle": "hachure",
      "strokeWidth": 1,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 100.50390625,
      "y": 93.67578125,
      "strokeColor": "#000000",
      "backgroundColor": "transparent",
      "width": 186.47265625,
      "height": 141.9765625,
      "seed": 1968410350,
      "groupIds": []
    }
  ],
  "appState": { "zenModeEnabled": true, "viewBackgroundColor": "#AFEEEE" }
}
```

You might want to use this when you want to load excalidraw with some initial elements and app state.

#### `ref`

You can pass a `ref` when you want to access some excalidraw APIs. We expose the below APIs:

| API | signature | Usage |
| --- | --- | --- |
| ready | `boolean` | This is set to true once Excalidraw is rendered |
| readyPromise | [resolvablePromise](https://github.com/excalidraw/excalidraw/blob/master/src/utils.ts#L317) | This promise will be resolved with the api once excalidraw has rendered. This will be helpful when you want do some action on the host app once this promise resolves. For this to work you will have to pass ref as shown [here](#readyPromise) |
| updateScene | <pre>(<a href="https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L192">sceneData</a>)) => void </pre> | updates the scene with the sceneData |
| resetScene | `({ resetLoadingState: boolean }) => void` | Resets the scene. If `resetLoadingState` is passed as true then it will also force set the loading state to false. |
| getSceneElementsIncludingDeleted | <pre> () => <a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a></pre> | Returns all the elements including the deleted in the scene |
| getSceneElements | <pre> () => <a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a></pre> | Returns all the elements excluding the deleted in the scene |
| getAppState | <pre> () => <a href="https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L37">AppState</a></pre> | Returns current appState |
| history | `{ clear: () => void }` | This is the history API. `history.clear()` will clear the history |
| setScrollToContent | <pre> (<a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a>) => void </pre> | Scroll to the nearest element to center |
| setCanvasOffsets | `() => void` | Updates the offsets for the Excalidraw component so that the coordinates are computed correctly (for example the cursor position). You should call this API when your app changes the dimensions/position of the Excalidraw container, such as when toggling a sidebar. You don't have to call this when the position is changed on page scroll (we handled that ourselves). |
| importLibrary | `(url: string, token?: string) => void` | Imports library from given URL. You should call this on `hashchange`, passing the `addLibrary` value if you detect it. Optionally pass a CSRF `token` to skip prompting during installation (retrievable via `token` key from the url coming from [https://libraries.excalidraw.com](https://libraries.excalidraw.com/)). |

#### `readyPromise`

<pre>
const excalidrawRef = { current: { readyPromise: <a href="https://github.com/excalidraw/excalidraw/blob/master/src/utils.ts#L317">resolvablePromise</a>}}
</pre>

#### `onCollabButtonClick`

This callback is triggered when clicked on the collab button in excalidraw. If not supplied, the collab dialog button is not rendered.

#### `isCollaborating`

This prop indicates if the app is in collaboration mode.

#### `onPointerUpdate`

This callback is triggered when mouse pointer is updated.

```js
({ x, y }, button, pointersMap}) => void;
```

1.`{x, y}`: Pointer coordinates

2.`button`: The position of the button. This will be one of `["down", "up"]`

3.`pointersMap`: [`pointers map`](https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L131) of the scene

#### `onExportToBackend`

This callback is triggered when the shareable-link button is clicked in the export dialog. The link button will only be shown if this callback is passed.

```js
(exportedElements, appState, canvas) => void
```

1. `exportedElements`: An array of [non deleted elements](https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L87) which needs to be exported.
2. `appState`: [AppState](https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L37) of the scene.
3. `canvas`: The `HTMLCanvasElement` of the scene.

#### `langCode`

Determines the language of the UI. It should be one of the [available language codes](https://github.com/excalidraw/excalidraw/blob/master/src/i18n.ts#L14). Defaults to `en` (English). We also export default language and supported languages which you can import as shown below.

```js
import { defaultLang, languages } from "@excalidraw/excalidraw";
```

| name | type |
| --- | --- |
| defaultLang | string |
| languages | [Language[]](https://github.com/excalidraw/excalidraw/blob/master/src/i18n.ts#L8) |

#### `renderFooter`

A function that renders (returns JSX) custom UI footer. For example, you can use this to render a language picker that was previously being rendered by Excalidraw itself (for now, you'll need to implement your own language picker).

#### `viewModeEnabled`

This prop indicates whether the app is in `view mode`. When supplied, the value takes precedence over `intialData.appState.viewModeEnabled`, the `view mode` will be fully controlled by the host app, and users won't be able to toggle it from within the app.

#### `zenModeEnabled`

This prop indicates whether the app is in `zen mode`. When supplied, the value takes precedence over `intialData.appState.zenModeEnabled`, the `zen mode` will be fully controlled by the host app, and users won't be able to toggle it from within the app.

#### `gridModeEnabled`

This prop indicates whether the shows the grid. When supplied, the value takes precedence over `intialData.appState.gridModeEnabled`, the grid will be fully controlled by the host app, and users won't be able to toggle it from within the app.

#### `libraryReturnUrl`

If supplied, this URL will be used when user tries to install a library from [libraries.excalidraw.com](https://libraries.excalidraw.com). Defaults to `window.location.origin + window.location.pathname`. To install the libraries in the same tab from which it was opened, you need to set `window.name` (to any alphanumeric string) — if it's not set it will open in a new tab.

#### `theme`

This prop controls Excalidraw's theme. When supplied, the value takes precedence over `intialData.appState.theme`, the theme will be fully controlled by the host app, and users won't be able to toggle it from within the app.

#### `name`

This prop sets the name of the drawing which will be used when exporting the drawing. When supplied, the value takes precedence over `intialData.appState.name`, the `name` will be fully controlled by host app and the users won't be able to edit from within Excalidraw.

### Does it support collaboration ?

No Excalidraw package doesn't come with collaboration, since this would have different implementations on the consumer so we expose the API's which you can use to communicate with Excalidraw as mentioned above. If you are interested in understanding how Excalidraw does it you can check it [here](https://github.com/excalidraw/excalidraw/blob/master/src/excalidraw-app/index.tsx).

### Extra API's

#### `getSceneVersion`

**How to use**

<pre>
import { getSceneVersion } from "@excalidraw/excalidraw";
getSceneVersion(elements:  <a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a>)
</pre>

This function returns the current scene version.

#### `getSyncableElements`

**_Signature_**

<pre>
getSyncableElements(elements:  <a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a>):<a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a>
</pre>

**How to use**

```js
import { getSyncableElements } from "@excalidraw/excalidraw";
```

This function returns all the deleted elements of the scene.

#### `getElementMap`

**_Signature_**

<pre>
getElementsMap(elements:  <a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a>): {[id: string]: <a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement</a>}
</pre>

**How to use**

```js
import { getElementsMap } from "@excalidraw/excalidraw";
```

This function returns an object where each element is mapped to its id.

### Restore utilities

#### `restoreAppState`

**_Signature_**

<pre>
restoreAppState(appState:  <a href="https://github.com/excalidraw/excalidraw/blob/master/src/data/types.ts#L17">ImportedDataState["appState"]</a>, localAppState: Partial<<a href="https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L37">AppState</a>> | null): <a href="https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L37">AppState</a>
</pre>

**_How to use_**

```js
import { restoreAppState } from "@excalidraw/excalidraw";
```

This function will make sure all the keys have appropriate values in [appState](https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L37) and if any key is missing, it will be set to default value. If you pass `localAppState`, `localAppState` value will be preferred over the `appState` passed in params.

#### `restoreElements`

**_Signature_**

<pre>
restoreElements(elements:  <a href="https://github.com/excalidraw/excalidraw/blob/master/src/data/types.ts#L16">ImportedDataState["elements"]</a>): <a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a>
</pre>

**_How to use_**

```js
import { restoreElements } from "@excalidraw/excalidraw";
```

This function will make sure all properties of element is correctly set and if any attribute is missing, it will be set to default value.

#### `restore`

**_Signature_**

<pre>
restoreElements(data:  <a href="https://github.com/excalidraw/excalidraw/blob/master/src/data/types.ts#L12">ImportedDataState</a>): <a href="https://github.com/excalidraw/excalidraw/blob/master/src/data/types.ts#L4">DataState</a>
</pre>

**_How to use_**

```js
import { restore } from "@excalidraw/excalidraw";
```

This function makes sure elements and state is set to appropriate values and set to default value if not present. It is combination of [restoreElements](#restoreElements) and [restoreAppState](#restoreAppState)

### Export utilities

#### `exportToCanvas`

**_Signature_**

<pre
>exportToCanvas({
  elements,
  appState
  getDimensions,
}: <a href="https://github.com/excalidraw/excalidraw/blob/master/src/packages/utils.ts#L10">ExportOpts</a>
</pre>

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| elements | [Excalidraw Element []](https://github.com/excalidraw/excalidraw/blob/master/src/element/types) |  | The elements to be exported to canvas |
| appState | [AppState](https://github.com/excalidraw/excalidraw/blob/master/src/packages/utils.ts#L12) | [defaultAppState](https://github.com/excalidraw/excalidraw/blob/master/src/appState.ts#L11) | The app state of the scene |
| getDimensions | `(width: number, height: number) => {width: number, height: number, scale: number)` | `(width, height) => ({ width, height, scale: 1 })` | A function which returns the width, height and scale with which canvas is to be exported. |

**How to use**

```js
import { exportToCanvas } from "@excalidraw/excalidraw";
```

This function returns the canvas with the exported elements, appState and dimensions.

#### `exportToBlob`

**_Signature_**

<pre>
exportToBlob(
  opts: <a href="https://github.com/excalidraw/excalidraw/blob/master/src/packages/utils.ts#L10">ExportOpts</a> & {
  mimeType?: string,
  quality?: number;
})
</pre>

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| opts |  |  | This param is passed to `exportToCanvas`. You can refer to [`exportToCanvas`](#exportToCanvas) |
| mimeType | string | "image/png" | Indicates the image format |
| quality | number | 0.92 | A value between 0 and 1 indicating the [image quality](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toBlob#parameters). Applies only to `image/jpeg`/`image/webp` MIME types. |

**How to use**

```js
import { exportToBlob } from "@excalidraw/excalidraw";
```

Returns a promise which resolves with a [blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob). It internally uses [canvas.ToBlob](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement/toBlob).

#### `exportToSvg`

**_Signature_**

<pre>
exportToSvg({
  elements: <a href="https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78">ExcalidrawElement[]</a>,
  appState: <a href="https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L42">AppState</a>,
  exportPadding?: number,
  metadata?: string,
}
</pre>

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| elements | [Excalidraw Element []](https://github.com/excalidraw/excalidraw/blob/master/src/element/types.ts#L78) |  | The elements to exported as svg |
| appState | [AppState](https://github.com/excalidraw/excalidraw/blob/master/src/types.ts#L42) | [defaultAppState](https://github.com/excalidraw/excalidraw/blob/master/src/appState.ts#L11) | The app state of the scene |
| exportPadding | number | 10 | The padding to be added on canvas |
| metadata | string | '' | The metadata to be embedded in svg |

This function returns a svg with the exported elements.

##### Additional attributes of appState for `export\*` APIs

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| exportBackground | boolean | true | Indicates whether background should be exported |
| viewBackgroundColor | string | #fff | The default background color |
| shouldAddWatermark | boolean | false | Indicates whether watermark should be exported |
| exportWithDarkMode | boolean | false | Indicates whether to export with dark mode |
