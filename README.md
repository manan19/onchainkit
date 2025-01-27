# [OnchainKit](https://github.com/coinbase/onchainkit/)

> OnchainKit is a collection of CSS, React components and Core utilities specifically crafted to enhance your creativity when building onchain applications.

## Getting Started

Add OnchainKit to your project, install the required packages.

```bash
# Use Yarn
yarn add @coinbase/onchainkit

# Use NPM
npm install @coinbase/onchainkit

# Use PNPM
pnpm add @coinbase/onchainkit
```

<br />
<br />

## FrameKit 🖼️
A Frame transforms any cast into an interactive app. 

Creating a frame is easy: select an image and add clickable buttons. When a button is clicked, you receive a callback and can send another image with more buttons. To learn more, check out "[Farcaster Frames Official Documentation](https://warpcast.notion.site/Farcaster-Frames-4bd47fe97dc74a42a48d3a234636d8c5)".

### getFrameAccountAddress()

When a user interacts with your Frame, you will receive a JSON message called `Frame Signature Packet`. From this message, you can extract the Account Address using the `getFrameAccountAddress()` function.

This Account Address can then be utilized for subsequent operations, enhancing the personalized experience of using the Frame for each individual.

Note: To utilize this function, we rely on [Neynar APIs](https://docs.neynar.com/reference/user-bulk). In order to avoid rate limiting, please ensure that you have your own API KEY.

```ts
// Steps 1. import getFrameAccountAddress from @coinbase/onchainkit
import { getFrameAccountAddress } from '@coinbase/onchainkit';
import { NextRequest, NextResponse } from 'next/server';

async function getResponse(req: NextRequest): Promise<NextResponse> {
  let accountAddress = '';
  try {
    // Step 2. Read the body from the Next Request
    const body = await req.json();
    // Step 3. Get from the body the Account Address of the user using the Frame 
    accountAddress = await getFrameAccountAddress(body, { NEYNAR_API_KEY: 'NEYNAR_API_DOCS' });
  } catch (err) {
    console.error(err);
  }

  // Step 4. Use the account address for your next step
  return new NextResponse(`
    <!DOCTYPE html>
    <html>
      <head>
        <title>BOAT</title>
        <meta name="fc:frame" content="vNext">
        <meta name="fc:frame:image" content="https://build-onchain-apps.vercel.app/release/v-0-17.png">
        <meta name="fc:frame:post_url" content="post_url_test">
        <meta name="fc:frame:button:1" content="${accountAddress}">
      </head>
      <body>
        <p>BOAT Text</p>
      </body>
    </html>
  `);
}

export async function POST(req: NextRequest): Promise<Response> {
  return getResponse(req);
}

export const dynamic = 'force-dynamic';
```

`getFrameAccountAddress` params

- `body`: The Frame Signature Packet body
- `options`:
  - `NEYNAR_API_KEY`: The NEYNAR_API_KEY used to access [Neynar Farcaster Indexer](https://docs.neynar.com/reference/user-bulk)

<br />

### getFrameMetadata()

With Next.js App routing, use the `getFrameMetadata` inside your `page.ts` to get the metadata need it for your Frame.

```ts
// Steps 1. import getFrameMetadata from @coinbase/onchainkit
import { getFrameMetadata } from '@coinbase/onchainkit';
import type { Metadata } from 'next';
import HomePage from './home';

// Step 2. Use getFrameMetadata to shape your Frame metadata
const frameMetadata = getFrameMetadata({
  buttons: ['boat'],
  image: 'https://build-onchain-apps.vercel.app/release/v-0-17.png',
  post_url: 'https://build-onchain-apps.vercel.app/api/frame',
});

// Step 3. Add your metadata in the Next.js metadata utility
export const metadata: Metadata = {
  manifest: '/manifest.json',
  other: {
    ...frameMetadata
  },
};

export default function Page() {
  return <HomePage />;
}
```

`getFrameMetadata` params

- `buttons`: A list of strings which are the label for the buttons in the frame (max 4 buttons).
- `image`: An image which must be smaller than 10MB and should have an aspect ratio of 1.91:1
- `post_url`: A valid POST URL to send the Signature Packet to.

<br />

## The Team and Our Community ☁️ 🌁 ☁️

OnchainKit is all about community; for any questions, feel free to:

1. Reach out to the core maintainers on Twitter or Farcaster
<table>
  <tbody>
    <tr>
      <td align="center" valign="top">
        <a href="https://twitter.com/Zizzamia">
          <img width="80" height="80" src="https://github.com/zizzamia.png?s=100">
        </a>
        <br />
        <a href="https://twitter.com/Zizzamia">Leonardo Zizzamia</a>
      </td>
      <td align="center" valign="top">
        <a href="https://twitter.com/alvaroraminelli">
          <img width="80" height="80" src="https://github.com/alvaroraminelli.png?s=100">
        </a>
        <br />
        <a href="https://twitter.com/alvaroraminelli">Alvaro Raminelli</a>
      </td>
      <td align="center" valign="top">
        <a href="https://twitter.com/0xr0b_eth">
          <img width="80" height="80" src="https://github.com/robpolak.png?s=100">
        </a>
        <br />
        <a href="https://twitter.com/0xr0b_eth">Rob Polak</a>
      </td>
    </tr>
  </tbody>
</table>

<br>

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details