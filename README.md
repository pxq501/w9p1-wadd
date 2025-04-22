# Week 9 Practical 1: Vite Tutorial
This practical will walk you through the steps of creating, editing, building, and publishing a Vite-built website on GitHub pages.

NOTE: You are not required to use this approach in your assessment but you may need to if you want to use libraries. If you are confident you don't need to use this approach and it feels like too much to tackle right now, you can choose to spend this time working on your assessment or your portfolio instead.

### Pre-requisite (personal computers only, skip if using an LMB/008 computer!) - install Node
[Download and install Node.js](https://nodejs.org/en). To verify that it has installed correctly, open up the command line ([Mac](https://support.apple.com/en-gb/guide/terminal/apd5265185d-f365-44cb-8b09-71a064a42125/mac) | [Windows](https://www.lifewire.com/how-to-open-command-prompt-2618089) | [Linux](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview)) and run:

```node -v```

If you see a version number, e.g. v20.5.0, Node has installed correctly. If you see anything else, installation has failed. Seek help!
### Step 1 - create a Vite project
Open this repo folder in VS Code and open the built-in Terminal (Terminal menu in VS Code > New Terminal).

Decide what you will call your project folder (remember, folder names cannot contain spaces). Then, run the following command but replace `my-vite-app` with your folder name.

```npm create vite@latest my-vite-app -- --template vanilla```

If you are asked if you should install something, enter `y`.

When installation is complete, you will see a message that gives you three commands to run. Run each command in the VS Code terminal. Wait for each command to finish running before entering the next command. 

If all goes well, you will get output that includes a "local" server URL, likely `http://localhost:5173`. Copy the URL shown in your terminal and open it in Chrome.

Explanation of the commands:
- `cd my-vite-app` The "cd" command changes the working folder of the terminal. In this case it moves the terminal inside the folder created when the Vite project was created.
- `npm install` This command installs dependencies (libraries, packages) needed to run a Vite project. Look inside your app folder and you should see, among other things, a folder called `node-modules`. This folder contains all the installed dependencies. Do not edit or move it!
- `npm run dev` This command starts the Vite development server so you can view your website in your browser as you work on it.

### Step 2 - explore the files created in the previous step
Open the newly created project folder in VS Code and take a few minutes to investigate the files and folders created by Vite in the previous step, ignoring `node_modules`.

- The `public` folder contains assets that should be available to index.html.
- The `src` folder stores the main code files that you will edit. Do not rename or move this folder or you will break the project! The template files include counter.js and main.js, which both use module syntax. Read through this code to understand how the template code works.
- The `.gitignore` file lists files and folders that should not be committed to GitHub. You should leave this file as is (you can add things to it but don't delete things!)
- `index.html` is the "entry point" to the website, just like you're used to with vanilla web development. Try changing the title of the page to something other than Vite App and check that you see the change reflected in your browser.
- `package-lock.json` is automatically managed by Vite. It contains important information for the build process. Do not edit it!
- `package.json` is the editable version of `package-lock.json`. We will come back to this file in future! For now, you can ignore it.

### Step 3 - remove unnecessary template code
Open main.js, inside the src folder. This file is linked in index.html and runs when the site first loads.

When you're happy you understand how the existing code in this file works, modify it so that it displays a paragraph that says "Hello, World!". There should be no other content!

Remove any code that is no longer being used, including import statements. You should be able to: 
- delete the counter.js file,
- delete the `setupCounter()` function call at the bottom of the file,
- remove all import statements EXCEPT the stylesheet import.

Delete all code in style.css and add a few styles of your choosing e.g. set the background colour and font family.

### Step 4 - build the code
Next, you're going to build the code so that it can be published on GitHub pages

In the VS Code terminal, stop the development server by typing `CTRL C`.

Run the build command:
```npm run build```

When this command finishes, a `dist` folder should appear, which contains the compiled website.

### Step 5 - commit and push to GitHub

### Step 6 - publishing with GitHub pages, pt 1
Enable GitHub Pages on your repo and open the website URL once it's ready. Spoiler alert: it won't work yet, you'll just see these instructions! You'll fix that in the next step.

Here's a reminder of how to enable GitHub Pages.
1. Open your repo in your browser if you haven't already done so. Use GitHub Desktop or login to github.com and find it in the repositories area of your account.
2. Go to the Settings tab and check that your repo visibility is public: scroll all the way down to Danger Zone, repo visibility is the first setting.
3. In the left menu, go to Pages. In the "branch" dropdown, select "main". Click Save. Refresh the page in a minute or two to get the URL. Tip: go to the Actions tab in the GitHub repo to see the status of the build. It will turn green when it's ready.

### Step 7 - configuring Vite to work with GitHub pages
You won't see your webpage in GitHub Pages because Vite assumes only the contents of the `dist` folder will be put online and that they are at the root of the website (that would be github.com in this case). To fix this, you need to configure Vite to work with the Pages structure.

1. Create a file called `vite.config.js` at the top level of your project folder. For example, if your folder is called `my-vite-app`, `vite.config.js` should be immediately inside this folder, not the `src` folder.
2. Copy the code below into `vite.config.js` but replace "YOUR_GITHUB_URL" with the GitHub Pages URL from Step 6:

```
import { defineConfig } from 'vite';


export default defineConfig({
    base: "YOUR_GITHUB_URL",
    build: {
        outDir: '../docs',
        emptyOutDir: true,
    }
})
```
The "base" setting tells Vite where your website will be hosted. The "outDir" setting tells Vite to put the compiled website into a folder called "docs", which will be created in the top level of the repo instead of the default "dist" folder inside the project folder.

3. Build your site (`npm run build`) and check the docs folder is created.
4. Commit and push your changes.

### Step 8 - publishing with GitHub Pages, pt 2
Now that your Vite project is properly configured, go back to the GitHub Pages settings in your browser. In the dropdown that says "/root", select "/docs" instead, then click "Save". Wait a few minutes then open the URL that you put as "base" in `vite.config.js`. You should see your site instead of these instructions.

### Notes on the command line
#### Running commands
Vite commands like `npm run dev` and `npm run build` must **always be run from the folder that contains `package.json`**. 

With the current folder structure, you would have to use the `cd` command (see end of Step 1) to move the VS Code Terminal inside the project folder whenever you open the repo. To avoid this, move the project files out of the folder into the top level of the repo. 
- After moving the files, set the "outDir" in `vite.config.js` to "docs" instead of "../docs" to reflect it's new position relative to `package.json`.

#### Cloning an existing repo that uses a build tool onto another computer
Dependencies (i.e. `node_modules`) need to be installed before running the code.
1. Open the VS Code Terminal
2. Check the name of the folder that precedes the prompt in the terminal. It should match the folder that contains `package.json`. If the terminal is not in the right place, use the `cd` command to move. `cd folder_name` moves into a folder of the same name, `cd ../` moves up one folder.
3. Run `npm install`. This will install all dependencies listed under "dependencies" or "devDependencies" in `package.json`.

#### Using Git from the command line
Now that you've been using the command line to work with Vite, you may like to try using it to work with GitHub too. Most professionals use the terminal rather than GitHub Desktop. Here are the steps:
1. Run `git add .` to prepare all changed files to be committed. The . is important!
2. Run `git commit -m "useful message here"` to commit the changes.
3. Run `git push` to send the commit to the remote repo.

Note: **git commands should always be run from the top level of the repo**. If your terminal is in a nested folder (look at the folder name before the prompt in the terminal), run the command `cd ../` to move one folder up.
