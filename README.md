# Music Universe 🎵

> A music streaming web application — a project for the **Advanced Databases** course, built on a **Neo4j graph database** with **Redis** caching

![.NET](https://img.shields.io/badge/.NET-5.0-512BD4?logo=dotnet&logoColor=white)
![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=black)
![Neo4j](https://img.shields.io/badge/Neo4j-Graph_DB-008CC1?logo=neo4j&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-Cache-DC382D?logo=redis&logoColor=white)
![JWT](https://img.shields.io/badge/Auth-JWT-000000?logo=jsonwebtokens&logoColor=white)
![Material UI](https://img.shields.io/badge/MUI-5-007FFF?logo=mui&logoColor=white)

## 📋 About

**Music Universe** is a full-stack music streaming web application. Users can sign up, log in, browse and
search songs and artists, listen to tracks through a built-in player, like songs, subscribe to artists,
and explore charts, featured tracks, and personalized "For You" recommendations.

The project was built for the *Advanced Databases* course, with a focus on the **graph database Neo4j** —
modeling entities (songs, artists, albums, users) as nodes and the relationships between them as edges,
and querying that graph through Cypher via a REST API. A **Redis** layer is used for caching, and
authentication is handled with **JWT** tokens.

## ✨ Features

- User **sign up / login** with **JWT** authentication
- **Browse and search** songs and artists
- Built-in **music player** (play, play count tracking)
- **Like** songs and **subscribe / unsubscribe** to artists
- **Charts** (top songs), **Featured**, and personalized **"For You"** sections
- **Artist pages** with biography, stats, songs, and most popular tracks
- Add new **songs, albums, artists, and songwriters**
- **Redis caching** for faster repeated reads
- Auto-generated **Swagger** API documentation

## 🛠️ Tech stack

| Layer | Technology |
|-------|------------|
| Frontend | React 18, React Router 6, Material UI (MUI) 5 |
| Backend | ASP.NET Core 5.0 Web API (C#) |
| Database | Neo4j (graph database, via Neo4jClient / Bolt) |
| Cache | Redis (StackExchange.Redis + ServiceStack.Redis) |
| Auth | JWT (JSON Web Tokens) |
| API docs | Swagger / Swashbuckle |

## 🏗️ Architecture

```
┌──────────────┐     REST / JWT      ┌──────────────┐  Bolt   ┌──────────────┐
│   React SPA  │ ──────────────────► │  ASP.NET API │ ──────► │    Neo4j     │
│  (port 3000) │ ◄────────────────── │ (port 5001)  │ ◄────── │ (bolt 7687)  │
└──────────────┘        JSON         └──────┬───────┘         └──────────────┘
                                            │
                                            ▼
                                     ┌──────────────┐
                                     │    Redis     │
                                     │ (port 6379)  │
                                     └──────────────┘
```

The backend is organized into **Controllers** (REST endpoints), **Services** (caching), and **Auth**
(JWT issuing/validation), with **Models** describing the graph entities.

## 🗃️ Data model (graph entities)

Entities are stored as nodes in Neo4j and connected by relationships:

- **User** — username, email, password; likes songs and subscribes to artists
- **Song** — title, lyrics, year, genre, audio file, image, stream count
- **Singer** — name, birthplace, birthday, biography
- **Songwriter** — name; writes songs
- **Album** — title, year, cover
- **Rating** — a like relationship between a user and a song

Typical relationships include *user → likes → song*, *user → subscribes → singer*,
*singer → performs → song*, *songwriter → wrote → song*, and *song → belongs to → album*.

## 📡 API overview

**UserController** — `/User`
- `POST /SignUp` — register a new user
- `POST /Login` — log in (returns a JWT)
- `GET /GetuserName/{jwt}` — current user's username
- `POST /Subscribe/{singerID}/{jwt}` — subscribe to an artist
- `DELETE /UnSubscribe/{singerID}/{jwt}` — unsubscribe from an artist

**SongController** — `/Song`
- `POST /AddSong/{jwt}/{pevacID}/{albumID}/{songwriterID}` — add a song
- `POST /AddAlbum/jwt` — add an album
- `GET /GetSongsFeatured/{jwt}` — featured songs
- `GET /GetSongsForYou/{jwt}` — personalized recommendations
- `GET /GetSongsTopCharts` — top charts
- `GET /SearchSongs/{name}` — search songs
- `GET /GetSongView/{songID}` — single song details
- `PUT /LikeTheSong/{jwt}/{songID}` — like a song
- `PUT /IncreasePlaysNumber/{songID}` — increment play count
- `GET /GetAlbum/{albumName}` — album details
- `DELETE /DeleteSong/{songID}/{jwt}` — delete a song

**SingerController** — `/Singer`
- `POST /AddSinger/jwt` — add an artist
- `POST /AddSongWritter/jwt` — add a songwriter
- `GET /GetSinger/{singerName}` — artist details
- `GET /GetSingerStats/{singerName}` — artist statistics
- `GET /GetSingerSongs/{singerName}` — songs by an artist
- `GET /GetSingerPopularSongs/{singerName}` — most popular songs
- `GET /GetSongwriter/{songwriterName}` — songwriter details
- Redis cache endpoints: `GetCacheSongList`, `SetCacheSongList`, `GetCacheSong`, `SetCacheSong`

> Full interactive documentation is available through the Swagger UI.

## 📁 Project structure

```
Music-Universe/
├── backend/                 # ASP.NET Core 5 Web API
│   ├── Controllers/         # REST endpoints (Song, Singer, User)
│   ├── Services/            # Redis cache service
│   ├── Auth/                # JWT service
│   ├── Models/              # Graph entities (Song, Singer, Album, ...)
│   ├── appsettings.json     # Redis configuration
│   ├── Program.cs
│   └── Startup.cs           # Neo4j, Redis, CORS, JWT setup
└── frontend/                # React application
    ├── public/              # Images and audio files
    └── src/
        ├── Components/       # React components (Main, Charts, PlayBar, Artist, ...)
        ├── Styles/           # CSS styles
        └── App.js
```

## 👥 Authors

Team project for the *Advanced Databases* course.

**Pavle Živanović**
**Nikola Jovanović**
**Andrija Ivković**
**Jovana Stojković**
