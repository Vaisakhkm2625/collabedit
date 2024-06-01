# Collab edit


Simple collaborative editing software based with Svelte and [tiptap](https://tiptap.dev/)

## demo



https://github.com/Vaisakhkm2625/collabedit/assets/68694876/74dab6a5-093f-42ca-8735-2d7d4fea3ce7




## Installation & usage
```bash

git clone git@github.com:Vaisakhkm2625/collabedit.git
cd collabedit
npm install

# start websocket socker server
node ./bin/hocuspocserver.js

# start the server
npm run dev

```
got to `http://localhost:5173/collab/<document id>` in your browser 
 eg: [collabedit - dog](http://localhost:5173/collab/dog) 

open same in another browser


## features

- real-time collaborative editing
- real-time cursor position
- real-time selection
- change color and name of cursor
