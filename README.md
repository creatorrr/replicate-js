# Replicate Javascript client

This is a Javacript client for Replicate. It lets you run models from your browser or node.

WIP, needs to be bundled and built for both.

You can run a model and get its output:

**From The Browser**
```javascript
import Replicate from './replicate.js'
// NEVER put your token in any publically accessible client-side Javascript
// Instead, use a proxy-- see cors-proxy.js
let replicate = new Replicate({proxy_url: "http://localhost:3000/api"});
let model = await replicate.models.get('replicate/hello-world');
let prediction = await model.predict({ text: "why"});
```

**From Node**
```javascript
import Replicate from './replicate.js'
let replicate = new Replicate({token: "YOUR_TOKEN"});
let model = await replicate.models.get('replicate/hello-world');
let prediction = await model.predict({ text: "why"});
```

You can run a model and feed the output into another model:

```javascript
let image = await replicate.models.get('afiaka87/laionide-v4').predict({prompt: "avocado armchair"});
let upscaledImage = replicate.models.get("jingyunliang/swinir").predict({image: image})
```

Run a model and get its output while it's running:

```javascript
let model = replicate.models.get("pixray/text2image")
let predictor = model.predictor({ prompts: "san francisco sunset"})
for await(let prediction of predictor){
    console.log(prediction);
}
```

By default, `model.predict()` uses the latest version. If you want to pin to a particular version, you can get a version with its ID:

```javascript
let model = replicate.models.get("replicate/hello-world")
let versionedModel = replicate.models.get("replicate/hello-world","5c7d5dc6dd8bf75c1acaa8565735e7986bc5b66206b55cca93cb72c9bf15ccaa");
```

## Install

TBD

```sh
npm install replicate
```

## Authentication

In a Node.js environment, set the `REPLICATE_API_TOKEN` environment variable to your API token. For example, run this before running any Javascript that uses the API:

```
export REPLICATE_API_TOKEN=<your token>
```

In either a Node.js or browser environment, you can pass in your API token while creating a replicate instance: `let replicate = Replicate({token: 'YOUR_TOKEN'})`.
