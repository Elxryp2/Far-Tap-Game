'use client';
import { useState } from 'react';
import { useMiniAppContext } from '@farcaster/frames';

export default function Home() {
  const { context, actions } = useMiniAppContext();
  const [score, setScore] = useState(0);

  const handleTap = () => {
    setScore(score + 1);
  };

  const shareScore = () => {
    actions.composeCast({
      text: `🔥 I scored ${score} in TapTap Game! Can you beat me?`,
      url: context.url,
    });
  };

  return (
    <main className="flex flex-col items-center justify-center h-screen bg-black text-white">
      <h1 className="text-3xl font-bold mb-6">🎮 TapTap Game</h1>
      <p className="text-xl mb-4">Score: {score}</p>

      <button
        onClick={handleTap}
        className="bg-purple-600 text-white px-8 py-4 rounded-full text-2xl shadow-md mb-4"
      >
        Tap Me!
      </button>

      <button
        onClick={shareScore}
        className="bg-green-500 text-white px-6 py-2 rounded shadow"
      >
        Share Score
      </button>
    </main>
  );
}
