# 🚀 Power BI Custom Visuals – Starter Setup

> Everything you need to start building powerful and interactive Power BI custom visuals using the `pbiviz` CLI.


## 🌐 Step 1: Install Node.js

Node.js is a JavaScript runtime built on Chrome’s V8 engine, allowing you to run JavaScript code outside the browser.

### ✅ To install Node.js:

1. Visit 👉 [nodejs.org](https://nodejs.org/en)
2. Download the **latest LTS version** (MSI Installer)
3. Run the installer:
   - Accept the license agreement
   - Use default settings
4. Restart your computer once installation is complete


## 🔧 Step 2: Install `pbiviz`

Use the command below in **Windows PowerShell** to install the Power BI Visual Tools globally:

```powershell
npm i -g powerbi-visuals-tools@latest
```


## 🔧 Step 3: Enable Developer Mode in Power BI

1. Go to 👉 [Power BI Developer Settings](https://app.powerbi.com/user/user-settings/developer-settings?experience=power-bi)
2. Enable the toggle for Power BI Developer Mode
![Alt Text](https://raw.githubusercontent.com/munggonegg/pbiviz-setup/refs/heads/main/assets/image1.png)


## ✨ Step 4: Create a new visual

To create a new visual, navigate to the directory you want the Power BI visual to reside in, and run the command:

```powershell
pbiviz new <visual project name>
```

After running the above command, it will create a Power BI visual folder that contains the following files:

```markdown
project
├───.vscode
│   ├───launch.json
│   └───settings.json
├───assets
│   └───icon.png
├───node_modules
├───src
│   ├───settings.ts
│   └───visual.ts
├───style
│   └───visual.less
├───capabilities.json
├───package-lock.json
├───package.json
├───pbiviz.json
├───tsconfig.json
└───tslint.json
```

[Read more about the Power BI visual project structure.](https://learn.microsoft.com/en-us/power-bi/developer/visuals/visual-project-structure)


## 🧪 Step 5: Set up React in your project

Install React:

```powershell
npm i react react-dom
```

Install React type definitions:

```powershell
npm i @types/react @types/react-dom
```


## 📦 Step 6: Create a React component class

1. Create a folder named **`components`** inside the **`src`** folder.
2. Create a new file in the `components` folder, such as `App.tsx`.
3. Copy the following code into the new file.

```typescript
import * as React from "react";

export class ChatBot extends React.Component<{}>{
    render(){
        return (
            <div className="circleCard">
                Hello, React!
            </div>
        )
    }
}

export default ChatBot;
```

## 💡 Step 7: Add React to the `visual.ts` file

Add following code:
```typescript
// Import React dependencies and the added component
import * as React from "react";
import { createRoot } from "react-dom/client";
import { ChatBot } from "./components/App";
```

```typescript
export class Visual implements IVisual {
    private target: HTMLElement;
    private updateCount: number;
    private textNode: Text;
    private formattingSettings: VisualFormattingSettingsModel;
    private formattingSettingsService: FormattingSettingsService;
    private reactRoot: React.ReactElement; // Add Root React element for rendering the visual's UI
```

```typescript
public update(options: VisualUpdateOptions) {
    this.formattingSettings = this.formattingSettingsService.populateFormattingSettingsModel(VisualFormattingSettingsModel, options.dataViews[0]);

    console.log('Visual update', options);
    if (this.textNode) {
        this.textNode.textContent = (this.updateCount++).toString();
    }

    // React: create and render the component into the visual's target
    const root = createRoot(this.target);
    this.reactRoot = React.createElement(ChatBot);
    root.render(this.reactRoot);
}
```

## ⚠️ Step 8: Edit the tsconfig.json file

Open tsconfig.json and add two lines to the beginning of the compilerOptions item:

```typescript
"jsx": "react",
"types": ["react", "react-dom"],
```

Your tsconfig.json should look like this:

```typescript
{
    "compilerOptions": {
        "jsx": "react",
        "types": ["react", "react-dom"],
        "allowJs": false,
        "emitDecoratorMetadata": true,
        "experimentalDecorators": true,
        "target": "es2022",
        "sourceMap": true,
        "outDir": "./.tmp/build/",
        "moduleResolution": "node",
        "declaration": true,
        "lib": [
            "es2022",
            "dom"
        ]
    },
    "files": [
        "./src/visual.ts"
    ]
}
```

## 📝 Step 9: Test your visual

Open terminal and typing this command:

```powershell
pbiviz start
```

1. Navigate to `https://localhost:8080/assets` : click Advance → Proceed to localhost (unsafe)
<img src="https://raw.githubusercontent.com/munggonegg/pbiviz-setup/refs/heads/main/assets/image2.png" alt="Step 1" width="600"/>

2. Navigate to powerbi.com

3. Create a new report and open the Developer Visual:  
<img src="https://raw.githubusercontent.com/munggonegg/pbiviz-setup/refs/heads/main/assets/image3.png" alt="Step 3" width="250"/>


4. Add data:  
<img src="https://raw.githubusercontent.com/munggonegg/pbiviz-setup/refs/heads/main/assets/image4.png" alt="Step 4" width="250"/>


5. When you add a new Developer Visual and Data to your report, it looks like this:  
<img src="https://raw.githubusercontent.com/munggonegg/pbiviz-setup/refs/heads/main/assets/image5.png" alt="Step 5" width="250"/>

## 📋 Summary

This guide walks you through the complete process of setting up and building custom Power BI visuals using the pbiviz CLI and React:

1. Install Node.js – Required runtime environment for building visuals.

2. Install pbiviz – CLI tool for creating and managing Power BI visuals.

3. Enable Developer Mode – Allows testing custom visuals in Power BI service.

4. Create a Visual Project – Scaffold a new visual with the CLI.

5. Set Up React – Integrate React for building component-based UIs.

6. Create & Use a Component – Build and render a simple React component.

7. Configure TypeScript – Update tsconfig.json for JSX and React types.

8. Test the Visual – Run the visual locally and load it into Power BI.

By the end, you'll have a working custom Power BI visual powered by React, ready for further development and styling.