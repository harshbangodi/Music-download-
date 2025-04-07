import React, { useState, useEffect } from "react"; import { Input } from "@/components/ui/input"; import { Button } from "@/components/ui/button"; import { Card, CardContent } from "@/components/ui/card"; import { AudioLines, Download, LogIn } from "lucide-react";

const API_URL = "http://localhost:5000";

export default function MusicDownloadApp() { const [filter, setFilter] = useState(""); const [user, setUser] = useState(null); const [token, setToken] = useState(""); const [songs, setSongs] = useState([]);

const fetchSongs = async () => { if (!token) return; const params = new URLSearchParams(); if (filter) params.append("title", filter);

const res = await fetch(`${API_URL}/songs?${params.toString()}`, {
  headers: { Authorization: `Bearer ${token}` },
});
const data = await res.json();
setSongs(data);

};

useEffect(() => { fetchSongs(); }, [filter, token]);

const handleLogin = async () => { const res = await fetch(${API_URL}/auth/login, { method: "POST", headers: { "Content-Type": "application/json" }, body: JSON.stringify({ username: "test", password: "1234" }), }); if (res.ok) { const data = await res.json(); setToken(data.token); setUser({ name: "test" }); } };

return ( <div className="min-h-screen bg-gray-100 p-6"> <header className="flex justify-between items-center mb-6"> <h1 className="text-3xl font-bold flex items-center gap-2"> <AudioLines /> Music Download </h1> {!user ? ( <Button onClick={handleLogin} className="flex items-center gap-2"> <LogIn size={18} /> Login </Button> ) : ( <div className="text-gray-700">Welcome, {user.name}</div> )} </header>

<Input
    placeholder="Search by title, artist or genre..."
    value={filter}
    onChange={(e) => setFilter(e.target.value)}
    className="mb-6"
  />

  <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
    {songs.map((song) => (
      <Card key={song.id} className="bg-white p-4 shadow rounded-2xl">
        <CardContent>
          <h2 className="text-xl font-semibold">{song.title}</h2>
          <p className="text-gray-600">{song.artist} | {song.genre}</p>
          <audio controls className="w-full mt-2">
            <source src={song.audioUrl} type="audio/mpeg" />
          </audio>
          <a
            href={song.audioUrl}
            download
            className="mt-3 inline-flex items-center text-blue-600 hover:underline"
          >
            <Download size={16} className="mr-1" /> Download
          </a>
        </CardContent>
      </Card>
    ))}
  </div>
</div>

); }

# Music-download-
