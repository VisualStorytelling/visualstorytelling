# visualstorytelling

## Modules
|    |    |
| -- | -- |
| [provenance-core](/../../../provenance-core) | Data structures and tracking for interaction provenance in web applications |
| [provenance-tree-visualization](/../../../provenance-tree-visualization)  | D3 based interactive visualization of provenance graph |
| [provenance-tree-visualization-react](/../../../provenance-tree-visualization-react) | React wrapper for [provenance-tree-visualization](/../../../provenance-tree-visualization) |
| [brainvis](/../../../brainvis) (private) | Brainvis demo |



## Development
The different modules are dependent on one another, to be able to work on several modules at the same time use [yarn workspaces](https://yarnpkg.com/lang/en/docs/workspaces/). Make sure you have node installed (8.x), and yarn. Then (paste in terminal as a whole, or per instruction):

```bash
mkdir prov && cd prov

# clone repos that you want:
git clone git@github.com:VisualStorytelling/provenance-core.git
git clone git@github.com:VisualStorytelling/slide-deck-visualization.git
git clone git@github.com:VisualStorytelling/provenance-tree-visualization.git
git clone git@github.com:VisualStorytelling/brainvis.git

# create a package.json for the workspace:
# Angular doesn't play nice in a yarn workspace, the nohoist ensures packages are installed in the subfolders' node_modules instead of the root.
cat <<EOT > package.json
{
  "private": true,
  "workspaces": {
    "packages": [
      "provenance-core",
      "provenance-tree-visualization",
      "slide-deck-visualization",
      "brainvis"
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


# to auto re-build dependencies, start watchers:
PIDS=()
(cd provenance-core && yarn start) &; PIDS+=$!
(cd provenance-tree-visualization && yarn start) &; PIDS+=$!
(cd slide-deck-visualization && yarn start) &; PIDS+=$!
(cd brainvis && yarn start) &; PIDS+=$!

# sleep long time
sleep 1e10 &; SLEEP_PID=$!; PIDS+=$SLEEP_PID

# kill watchers when CTRL-C:
trap "kill -9 $PIDS" SIGINT

wait $SLEEP_PID
trap - SIGINT
echo "done"
```

