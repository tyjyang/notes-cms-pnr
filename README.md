This is a repo of markdown notes, that can be directly viewed on GitHub by simply clicking on them.  
They are collected from discussions with folks in the PNR team,
and are not meant to be used as official documentation.  
Plans, however, exist to eventually convert them to polished, reviewed twiki pages as official documentation.  
The official twiki page for the PNR team is [here](https://twiki.cern.ch/twiki/bin/view/CMS/CompOpsProductionReprocessing) (CMS credentials required to view).

# How to contribute
You only need to know basic markdown and git to be able to properly contribute to this repo.

Fork this repo to your GitHub account, make edits as you see fit, and make a Pull Request (PR)
for your edits to be merged.

# Convert to twiki
You can compose your notes in lightweight markdown,
and later convert it to html using pandoc:
```
pandoc -f markdown-auto_identifiers -t html -o output.html input.md
```

Then just directly paste the html text to the twiki page in the **raw edit** view.
Twiki is fully compatible with html.
