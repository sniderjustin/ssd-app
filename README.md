# ssd-app
ssd-app

## Implementation Steps

### Setup Web App Locally

1. Navigate to the directory where you want the repo directory to be.

2. Create ssd-app using `npm create vite@latest`

3. Use name `ssd-app`, select React,  and select Typescript.

4. Move into folder `cd ssd-app`

4. Install using `npm i`

5. To test and see the application run `npm run dev` and view the app in your browser

### Setup GH Pages Deployment

1. Run `npm install gh-pages --save-dev`

2. Install using `npm i`

3. Create GitHub repo and link up with our local repo:
* `git init`
* `git add .`
* `git commit -m "first-commit"`
* `git branch -M main`
* `git remote add origin https://github.com/username/ssd-app.git`
* `git push -u origin main`

4. Update `vite.config.js` to include our repo name:
```
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  base: '/ssd-app/',
  plugins: [react()],
})

```

5. Run `npm run build` in your terminal.

6. Add `/dist` folder into your repo by running `git add dist -f`

7. Run `git commit -m "Adding dist`

8. Run `git subtree push --prefix dist origin gh-pages`

9. Navigate to your GH Pages live page to verify your site is live.

### Setup GitHub Actions to rebuild your site on commit to main

1. To allow GitHub action to update our build go to your repo Settings > Actions > General > Workflow permisions to select `Read and write permissions`. Our change will allow the action to update the build in the gh-pages branch with each push.

2. Add `"homepage": "/ssd-app",` to the `package.json` file. The build will be configured for URL `user.github.io/ssd-app`, which is what GitHub pages will expect.

3. Add `.github/workflows/node.js.yml` the root project folder. Populate the file with the following text:
```
# This workflow will do a clean installation of node dependencies, cache/restore them, build the source code and run tests across different versions of node
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-nodejs

name: Node.js CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x]
        # See supported Node.js release schedule at https://nodejs.org/en/about/releases/

    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version: '18.x'
      - run: npm install
      - run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

4. Make a visible change to the web app you can use to verify the update.

5. Run `git add .`

6. Run `git commit -m "add GH Action"`

7. Open your GH Pages web app to veriy your change has been made. 