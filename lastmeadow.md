# Hello

This script connects to Discord's internal Webpack system to access hidden modules that aren't publicly accessible. It scans all loaded modules to find functions tied to activity events by matching internal identifiers, then extracts handlers to start and complete activities—essentially bypassing normal APIs and directly calling Discord's internal logic.

After that, it automates activity execution using infinite asynchronous loops: each loop repeatedly starts an activity, waits for a specified time, completes, and then pauses for a waiting period before repeating. Multiple loops run simultaneously (gathering, crafting, fighting), effectively simulating continuous activity progress without user input, which relies on internal behavior and can break or trigger constraints.

-----------------------------------------------------------------------------------
```
let wpRequire = window.webpackChunkdiscord_app.push([[Symbol()], {}, r => r]);
const findModule = (filter) => { 
for (let i in wpRequire.c) { 
let m = wpRequire.c[i].exports; 
if (!m) continue; 
if (filter(m)) return m; 
if (m.default && filter(m.default)) return m.default; 
for (let j in m) { 
if (m[j] && filter(m[j])) return m[j]; 
} 
}
};

const startActivity = findModule(m => typeof m === 'function' && m.toString().includes('"GORILLA_START_ACTIVITY_SUCCESS"'));
const completeActivity = findModule(m => typeof m === 'function' && m.toString().includes('"GORILLA_COMPLETE_ACTIVITY_SUCCESS"'));
const sleep = async ms => await new Promise(r => setTimeout(r, ms));

async function runActivityLoop(name, duration, cooldown) { 
while (true) { 
startActivity({ activity: name }); 
await sleep(duration); 
completeActivity({ activity: name }); 
await sleep(cooldown); 
}
}

runActivityLoop("gathering", 3000, 1000);
await sleep(500);
runActivityLoop("crafting", 5000, 128_000);
await sleep(500);
runActivityLoop("combat", 5000, 188_000);
```
---------------------------------------------------------------------

### Warning: Running scripts in the console may violate Discord's Terms of Service and pose a security risk if the code comes from an untrusted source.
