# visualstorytelling

## Modules
|    |    |
| -- | -- |
| [provenance-core](/../../../provenance-core) | Data structures and tracking for interaction provenance in web applications |
| [provenance-tree-visualization](/../../../provenance-tree-visualization)  | D3 based interactive visualization of provenance graph |
| [provenance-tree-visualization-react](/../../../provenance-tree-visualization-react) | React wrapper for [provenance-tree-visualization](/../../../provenance-tree-visualization) |
| [slide-deck-visualization](/../../../slide-deck-visualization) | D3 based slide deck GUI |
| [provenance-tree-calculator-demo](/../../../provenance-tree-calculator-demo) | Simple calculator demo to show provenance features |
| [brainvis](/../../../brainvis) (private) | Brainvis demo |
| [provenance-task-list](/../../../provenance-task-list) (private) | Task list (Brainvis dependency) |




## Development
The different modules are dependent on one another, to be able to work on several modules at the same time use [yarn workspaces](https://yarnpkg.com/lang/en/docs/workspaces/). Make sure you have node installed (8.x), and yarn. Then (paste in terminal as a whole, or per instruction):

```bash
mkdir prov && cd prov

# clone repos that you want:
git clone git@github.com:VisualStorytelling/provenance-core.git
git clone git@github.com:VisualStorytelling/slide-deck-visualization.git
git clone git@github.com:VisualStorytelling/provenance-tree-visualization.git
git clone git@github.com:VisualStorytelling/provenance-tree-calculator-demo.git

# if working on brainvis:
git clone git@github.com:VisualStorytelling/brainvis.git
git clone git@github.com:VisualStorytelling/provenance-task-list.git

# create a package.json for the workspace:
# Angular doesn't play nice in a yarn workspace, the nohoist ensures packages are installed in the subfolders' node_modules instead of the root.
# Add `brainvis` and `provenance-task-list` to packages if working on brainvis
cat <<EOT > package.json
{
  "private": true,
  "workspaces": {
    "packages": [
      "provenance-core",
      "provenance-tree-visualization",
      "slide-deck-visualization",
      "provenance-tree-calculator-demo"
    ],
    "nohoist": [
        "**/@angular*",
        "**/@angular*/**",
        "**/@ngtools*",
        "**/@ngtools*/**"
      ] 
  }
}
EOT

# install dependencies:
yarn install

# Do initial builds, to setup dependencies
(cd provenance-core && yarn build)
(cd provenance-tree-visualization && yarn build)
(cd slide-deck-visualization && yarn build)
# if brainvis:
# (cd provenance-task-list && yarn build)
# Use `yarn watch` in each directory to auto build after updates.

# Run demo or brainvis:
cd provenance-tree-calculator-demo
yarn start
# Now you can open your browser on http://localhost:8080/
```
