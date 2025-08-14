# Setting up TypeScript in a Node.js Project (Mac + VS Code)

This guide walks you through creating a modern, auto-reloading TypeScript + Node.js setup on macOS using VS Code.  
It’s optimized for fast feedback while coding (`tsc --watch` + `nodemon`).

---

## Step 1: Initialize a New Node.js Project

```bash
# Navigate to the folder where you want your project
cd /path/to/your/projects

# Create a new folder for your project
mkdir project-name
cd project-name

# Initialize a package.json with default settings
npm init -y

```

💡 Tip: Avoid spaces in your project name. Use hyphens or camelCase.

## Step 2: Install TypeScript

```bash
npm install --save-dev typescript
```

💡 Installs TypeScript locally so the project is portable.

## Step 3: Create a TypeScript Config

```bash
npx tsc --init
```

Update the generated tsconfig.json to:

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

## Step 4: Create Project Folders

```bash
mkdir src
mkdir dist
touch src/index.ts
```

💡 src/ is where you write .ts files.
💡 dist/ is where compiled .js files go.

## Step 4.5: Add Your First File

In src/index.ts:

```typescript
console.log("Hello World!");
```

## Step 5: Compile & Run

Compile:

```bash
npx tsc
```

Run:

```bash
node dist/index.js
```

## Step 5.5: Verify

You should see:

```bash
Hello World!
```

## Step 6: Install Dev Tools

```bash
npm install --save-dev nodemon concurrently
```

## Step 7: Create nodemon.json

In the project root:

```json
{
  "watch": ["dist"],
  "ext": "js,json",
  "exec": "node dist/index.js",
  "delay": "200"
}
```

💡 This makes nodemon watch the compiled JS, not your TS.

## Step 8: Turn On Source Maps

So stack traces point back to .ts lines.

Edit tsconfig.json to include:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "DOM"],
    "module": "commonjs",
    "moduleResolution": "node",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "sourceMap": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

## Step 8.5: Install Node.js Type Definitions

TypeScript doesn’t automatically know about Node.js globals (like `process`, `__dirname`, or `require`) unless you install the Node type definitions.

Run:

```bash
npm install --save-dev @types/node
```

💡 Note:
This package doesn’t install any actual code — it just gives TypeScript the “blueprints” for all the built-in features of Node.js so you get autocompletion, error-checking, and IntelliSense in VS Code.
Think of it as teaching TypeScript what Node.js can do.

## Step 9: Add Scripts to package.json

```json
"scripts": {
  "build": "tsc",
  "watch": "tsc --watch",
  "serve": "nodemon",
  "dev": "concurrently -k -n TSC,NODE -c blue,green \"npm:watch\" \"npm:serve\""
}
```

- watch → compiles .ts to dist/ on save
- serve → runs/restarts Node from dist/
- dev → runs both together

## Step 10: Run It

```bash
npm run dev
```

You should see:

- The TypeScript watcher running
- Nodemon starting node dist/index.js

## Step 11: Smoke Test

Edit src/index.ts:

```typescript
console.log("Hello World! (edited)");
```

Save → tsc recompiles → nodemon auto-restarts → new output prints.

## Optional: Git Setup

If using git create a .gitignore file in the root directory. Manually or via terminal:

```bash
touch .gitignore
```

Then add this to your new .gitignore file:

```bash
node_modules/
dist/
```
---

## 👋 About the Author

Hi! I’m Phillip Cantu, an American digital nomad, a Full Sail University web development student, and a [4Geeks Academy Full Stack Development with AI Bootcamp](https://4geeksacademy.com/us/apply?ref=REFERRALQEZPTJCK-17696). I’m passionate about creating modern, user-friendly, and practical applications that help people learn and build faster.

You can find me here:

- **GitHub:** [hereisphil](https://github.com/hereisphil)
- **LinkedIn:** [PhillipCantu](https://www.linkedin.com/in/phillipcantu/)
- **Instagram:** [@philtheotaku](https://www.instagram.com/philtheotaku/)

If this guide helped you, feel free to share it with others who are getting started with TypeScript!
✅ You’re now running the battle-tested dev loop:
TS compiles on save → Node restarts automatically.
