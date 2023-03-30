# BlockadeLabs SDK

BlockadeLabs SDK for TypeScript/JavaScript

## Envs Supported

- Node.js
- Modern browsers

## How to Install

```
$ yarn add @blockadelabs/sdk

# or

$ npm i @blockadelabs/sdk
```

## Basic Usage

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

const generators = await sdk.getGenerators();

console.log(generators);
```

## Methods Doc

#### getSkyboxStyles

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

const styles = await sdk.getSkyboxStyles();
```

#### generateSkybox

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

const generation = await sdk.generateSkybox({
  prompt: 'PROMPT_GOES_HERE', // Required
  skybox_style_id: 2, // Required
  webhook_url: 'YOUR_WEBHOOK_URL', // Optional
});
```

#### getGenerators

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

const generators = await sdk.getGenerators();
```

#### generateImagine

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

const generation = await sdk.generateImagine({
  generator: 'stable', // REQUIRED
  generator_data: {
    prompt: 'PROMPT_GOES_HERE', // REQUIRED
    ...others_generator_data, // Optional
  }, // REQUIRED
  webhook_url: 'YOUR_WEBHOOK_URL', // Optional
});

/* 
    The generateImagine method can accept different types of generator_data, which may include files. 
    In this case, we've multiple ways of passing these files to the method, which can change depending on your environment.

    # Node Environment

    In Node, you can pass these params as a string URL, as a Buffer, or as a Uint8Array

    # Browser Environment

    In a browser environment, you can pass these params as a string URL, as a Blob, or as a Uint8Array
*/

// init_image param node environment example
import * as fs from 'fs';
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const readFileAsBuffer = (filePath: string): Promise<Buffer> => {
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, (error, data) => {
      if (error) {
        reject(error);
      } else {
        resolve(data);
      }
    });
  });
};

const buffer = await readFileAsBuffer('IMAGE_PATH HERE');

const generation = await sdk.generateImagine({
  generator: 'stable',
  generator_data: { prompt: 'PROMPT_GOES_HERE', init_image: buffer },
});

// init_image param browser environment example with React
import { useState, useCallback } from 'react';
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const fileToBlob = (file: File): Blob => {
  const blob = file.slice(0, file.size, file.type);

  return new Blob([blob], { type: file.type });
};

const sdk = new BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

function Component() {
  const [file, setFile] = useState<File | null>(null);

  const generateSkybox = useCallback(async (file: File) => {
    const blob = fileToBlob(file);

    const generation = await sdk.generateImagine({
      generator: 'stable',
      generator_data: {
        prompt: 'PROMPT_GOES_HERE',
        init_image: blob,
      },
    });

    console.log(generation);

    return generation;
  }, []);

  const handleFileChange = useCallback(
    (event: React.ChangeEvent<HTMLInputElement>) => {
      const selectedFile = event.target.files && event.target.files[0];

      if (selectedFile) {
        setFile(selectedFile);
      }
    },
    [file],
  );

  return (
    <div className="App">
      <input type="file" accept="image/*" onChange={handleFileChange} />
      {file && <button onClick={() => generateSkybox(file)}>Generate</button>}
    </div>
  );
}

export default Component;
```

#### getImagineById

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

const imagine = await sdk.generateImagine({
  generator: 'stable',
  generator_data: {
    prompt: 'PROMPT_GOES_HERE',
  },
});

const imagineResult = await sdk.getImagineById({
  id: imagine.request.id, // REQUIRED
});
```

#### getImagineByObfuscatedId

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

const imagine = await sdk.generateImagine({
  generator: 'stable',
  generator_data: {
    prompt: 'PROMPT_GOES_HERE',
  },
});

const imagineResult = await sdk.getImagineByObfuscatedId({
  obfuscated_id: imagine.request.obfuscated_id, // REQUIRED
});
```

#### getImagineHistory

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

const imagineHistory = await sdk.getImagineHistory();

const imagineHistoryWithFilters = await sdk.getImagineHistory({
  status: 'STATUS', // OPTIONAL
  limit: 10, // OPTIONAL
  offset: 0, // OPTIONAL
  order: 'ASC', // OPTIONAL
  imagine_id: 1, // OPTIONAL
  query: 'PROMPT', // OPTIONAL
  generator: 'GENERATOR', // OPTIONAL
});
```

#### cancelImagine

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

const generateImagine = await sdk.generateImagine({
  generator: 'stable',
  generator_data: { prompt: 'PROMPT_GOES_HERE' },
});

const cancelRequest = await sdk.cancelImagine({
  id: generateImagine.request.id, // REQUIRED
});
```

#### cancelAllPendingImagines

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

await sdk.cancelAllPendingImagines();
```

#### deleteImagine

```ts
import { BlockadeLabsSdk } from '@blockadelabs/sdk';

const sdk = BlockadeLabsSdk({
  api_key: YOUR_API_KEY,
});

await sdk.deleteImagine({
  id: IMAGINE_ID_GOES_HERE, // REQUIRED
});
```
