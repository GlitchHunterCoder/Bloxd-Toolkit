# Maintenance Steps
## Check Back Here every so often
Below are the steps to get the game data ready to be set up
### Get Secrets
- Go to network and find `get-published-game-previews`
- Get `Link` and `Payload`
### Run Chunks
- Open an `about:blank` page, and paste code
- Insert `Link` and `Payload`
```js
var allResults=[]
function logInChunks(obj, chunkSize = 1000) {
  const text = JSON.stringify(obj); 
  for (let i = 0; i < text.length; i += chunkSize) {
    console.log(text.slice(i, i + chunkSize));
  }
}
let n = 1;
const payload = /* payload */
async function getGameData() {
  try {
    const link = /* link */";
    payload.contents.pageNumber = n;
    const response = await fetch(link, {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify(payload)
    });
    const result = await response.json();
    allResults.push(result);
    console.log(n)
    n += 1;
  } catch (error) {
    console.error("Request failed:", error);
  }
  setTimeout(getGameData, 200); // Poll every 200ms
}
getGameData(); // Start polling
```
- Wait until the logger says `200` at that point, run code below
```js
logInChunks(allResults)
```
- Then quickly right click and `Copy Console`
### Clean Data
- take data and clean up JSON, removing any errored logs
- use Find and Replace on 
  - `VM??:? ` -> "" //nothing
  - `\n` -> "" //regex match on, nothing
- enter code into below
```js
data = /* Data */

console.log(JSON.stringify(
data.map(e=>e.results).flat()
))
```
- Copy Formatted Data into `Custom-Games.json`
## And you are done !!!
With that, the Data is formatted and usable, and thus the Search Engine can use it as lookup
