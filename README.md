# merge madness git tool
  
### installation  
`git clone https://github.com/advectus/merge-madness.git ~/merge-madness`  
add merge-madness/bin to your PATH variable  
`export PATH=~/merge-madness/bin/git-merge-madness:$PATH`  
or copy the bin script from merge-madness/bin to a known bin path  
`cp ~/merge-madness/bin/git-merge-madness ~/bin/git-merge-madness`  
or set up symlink
`ln -s ~/merge-madness/bin/git-merge-madness ~/bin/git-merge-madness`
  
### how to use
move to WEM  
`cd wem`  
create a hidden manifest file in the root of WEM clone  
`vi .merge-madness-manifest`  
the manifest is a JSON structured branch list
```
{
  "branch-list": [
    "feature1",
    "feature2",
    "develop"
  ]
}
```   
or just copy an example branch list manifest  
`cp ~/merge-madness/examples/bisque-release-manifest .merge-madness-manifest`   
run merge-madness  
`git merge-madness`  

### understanding the output
```
{
  "feature1": {
    "feature2": {
      "status": "merged-ok"
    },
    "develop": {
      "status": "merged-ok"
    }
  },
  "feature2": {
    "feature1": {
      "status": "merged-ok" #pulled from hash["feature1"]["feature2"]
    },
    "develop": {
      "status": "merged-ok"
    }
  },
  "develop": {
    "feature1": {
      "status": "merged-ok" #pulled from hash["feature1"]["develop"]
    },
    "feature2": {
      "status": "merged-ok" #pulled from hash["feature2"]["develop"]
    }
  }
}
```