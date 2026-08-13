# Hello

This JavaScript script demonstrates client-side manipulation by injecting code into Discord's internal Webpack modules.

It works by scanning HTTPUtils to hijack the application's custom API handler, allowing it to send a direct POST request to the /hypesquad/online endpoint. This effectively bypasses the standard user interface (personality test) to manually update the user's profile badge directly on the server.

-----------------------------------------------------------------------------------
```
let wreq = webpackChunkdiscord_app.push([[Symbol()],{},r=>r]);
webpackChunkdiscord_app.pop();
const chunks = Object.entries(wreq.m)
const findChunkByCode = (...codes) => { 
for (let i = 0; i < chunks.length; i++) { 
const [id,func] = chunks[i] 
const chunkCode = func.toString() 

if (codes.every(code=>chunkCode.includes(code))) return wreq(id) 
}
}

const api = Object.values(findChunkByCode("HTTPUtils")).find(e=>e?.get)

api.post({url: "/hypesquad/online",body:{house_id: 1}})

```
---------------------------------------------------------------------

House Selection: house_id determines the badge:

[1] House of Bravery (Purple).

[2] House of Brilliance (Orange).

[3] House of Balance (Green).

**Example:** api.post({url: ```"/hypesquad/online",body:{house_id: 2}})```

### Warning: Running scripts in the console may violate Discord's Terms of Service and pose a security risk if the code comes from an untrusted source.
