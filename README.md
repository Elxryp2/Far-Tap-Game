// Farcaster Mini-Game: Tap to Score with Leaderboard (Ready-to-Deploy Template)

// File: pages/index.tsx
import Head from 'next/head';

export default function Home() {
  return (
    <>
      <Head>
        <meta property="fc:frame" content="vNext" />
        <meta property="fc:frame:image" content="https://placekitten.com/640/336" />
        <meta property="fc:frame:button:1" content="TAP!" />
        <meta property="fc:frame:button:2" content="Leaderboard" />
        <meta property="fc:frame:post_url" content="https://your-vercel-deployment.vercel.app/api/frame" />
        <title>Tap to Score Game</title>
      </Head>
      <main>
        <h1>Tap to Score Mini-Game</h1>
        <p>Use the Frame buttons inside Farcaster to increase your score and view the leaderboard!</p>
      </main>
    </>
  );
}

// File: pages/api/frame.ts
import type { NextApiRequest, NextApiResponse } from 'next';

let scores: Record<string, number> = {};

export default async function handler(req: NextApiRequest, res: NextApiResponse) {
  const fid = req.headers['fc-user-id'] as string;
  const button = req.body?.untrustedData?.buttonIndex;

  if (!fid) return res.status(400).send('Missing FID');

  if (button === 1) {
    scores[fid] = (scores[fid] || 0) + 1;
  }

  const topPlayers = Object.entries(scores)
    .sort((a, b) => b[1] - a[1])
    .slice(0, 5)
    .map(([user, score]) => `${user}: ${score}`);

  const leaderboard = topPlayers.join('\n');

  return res.status(200).json({
    image: {
      url: 'https://placekitten.com/640/336'
    },
    text: button === 2 ? `🏆 Top Players:\n${leaderboard}` : `You tapped! Current score: ${scores[fid]}`,
    buttons: [
      { label: 'TAP!' },
      { label: 'Leaderboard' }
    ]
  });
}

